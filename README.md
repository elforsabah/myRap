sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    // Keep dialog + ExtensionAPI in module scope
    var oDialog;
    var oExtAPI;   // sap.fe.core.ExtensionAPI

    return {
        /**
         * Custom action handler, wired in manifest.json ("press").
         *
         * @param {object} oContext          – FE context (not needed here)
         * @param {sap.ui.model.Context[]} aSelectedContexts – selected rows of the LR table
         */
        manualattachments: function (oContext, aSelectedContexts) {

            // In V4 FE, "this" is the ExtensionAPI, not a controller
            // See sap.fe.core.ExtensionAPI docs (loadFragment, addDependent, getModel, getEditFlow). 
            oExtAPI = this;

            // If dialog already created -> just open
            if (oDialog) {
                oDialog.open();
                return;
            }

            // Use ExtensionAPI.loadFragment so FE wires the models correctly
            this.loadFragment({
                id: "TwoSmartTablesDialog",                   // fragment root ID
                name: "zsb_attachment.ext.fragments.GenerateDocDialog", // <app id>.ext.fragment.<fileName without .xml>
                controller: {
                    // called when user presses Button A
                    onDialogChoose: function () {
                        // Access the inner tables via ExtensionAPI.byId (no view.getId() needed)
                        var oTopTable    = oExtAPI.byId("InnerTopTable");
                        var oBottomTable = oExtAPI.byId("InnerBottomTable");

                        var aTopSelected = oTopTable.getSelectedContexts().map(function (oCtx) {
                            return oCtx.getObject();
                        });
                        var aBottomSelected = oBottomTable.getSelectedContexts().map(function (oCtx) {
                            return oCtx.getObject();
                        });

                        if (!aTopSelected.length && !aBottomSelected.length) {
                            MessageToast.show("Please select at least one row in one of the tables.");
                            return;
                        }

                        // ===== Call backend action with TWO separate inputs =====
                        // Example: RAP unbound action MyService.ProcessTwoSets( TopItems : TopType[], BottomItems : BottomType[] )
                        // Use EditFlow.invokeAction from ExtensionAPI.getEditFlow(). 

                        var oEditFlow = oExtAPI.getEditFlow();
                        var oMainModel = oExtAPI.getModel(); // main OData V4 model

                        oEditFlow.invokeAction("MyService.ProcessTwoSets", {
                            model: oMainModel,               // unbound action -> provide model
                            parameterValues: [
                                { name: "TopItems",    value: aTopSelected },
                                { name: "BottomItems", value: aBottomSelected }
                            ],
                            label: "Process selected items"
                        }).then(function () {
                            MessageToast.show("Processing started.");
                            oDialog.close();
                        }).catch(function (oError) {
                            MessageToast.show(oError.message || "Error while calling backend action.");
                        });
                    },

                    // cancel button
                    onDialogCancel: function () {
                        oDialog.close();
                    }
                }
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;

                // Register dialog as dependent of the FE page
                oExtAPI.addDependent(oDialog);

                oDialog.open();
            });
        },

        // you can keep your second handler as-is
        manualattachments2: function (oContext, aSelectedContexts) {
            MessageToast.show("Custom handler invoked 2.");
        }
    };
});




<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:smartTable="sap.ui.comp.smarttable">

    <!-- Popup dialog -->
    <Dialog
        id="TwoSmartTablesDialog"
        title="Choose items"
        stretch="true"
        contentWidth="900px"
        contentHeight="500px"
        class="sapUiResponsivePadding">

        <content>
            <VBox width="100%" height="100%" renderType="Div">

                <!-- TOP TABLE (blue area) -->
                <smartTable:SmartTable
                    id="SmartTableTop"
                    entitySet="Attachment"
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    useExportToExcel="false"
                    showRowCount="true"
                    header="Top items"
                    enableAutoBinding="true"
                    persistencyKey="TopTable">

                    <!-- underlying sap.m.Table with multi-select -->
                    <smartTable:table>
                        <Table
                            id="InnerTopTable"
                            mode="MultiSelect"
                            growing="true"
                            growingScrollToLoad="true" />
                    </smartTable:table>
                </smartTable:SmartTable>

                <!-- BOTTOM TABLE (yellow area) -->
                <smartTable:SmartTable
                    id="SmartTableBottom"
                    entitySet="ServiceWR"
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    useExportToExcel="false"
                    showRowCount="true"
                    header="Bottom items"
                    enableAutoBinding="true"
                    persistencyKey="BottomTable">

                    <smartTable:table>
                        <Table
                            id="InnerBottomTable"
                            mode="MultiSelect"
                            growing="true"
                            growingScrollToLoad="true" />
                    </smartTable:table>
                </smartTable:SmartTable>

            </VBox>
        </content>

        <!-- "Button A" -->
        <beginButton>
            <Button
                id="btnChoose"
                text="Choose"
                type="Emphasized"
                press=".onDialogChoose" />
        </beginButton>

        <endButton>
            <Button
                id="btnCancel"
                text="Cancel"
                press=".onDialogCancel" />
        </endButton>

    </Dialog>
</core:FragmentDefinition>
