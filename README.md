<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:macros="sap.fe.macros">

    <Dialog
        id="TwoSmartTablesDialog"
        title="Choose Items"
        stretch="true"
        contentWidth="1200px"
        contentHeight="600px"
        class="sapUiResponsivePadding">

        <content>
            <VBox id="vbMain" width="100%" height="100%" renderType="Div">

                <!-- ========= ATTACHMENT “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="AttachmentFilterBar"
                    contextPath="/PrintConfiguration"
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                    <macros:Table
                        id="AttachmentTable"
                        contextPath="/PrintConfiguration"
                        metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                        filterBar="AttachmentFilterBar"            
                        selectionMode="ForceMulti"    
                        header="Attachments" />



                <Toolbar id="tbSpacer1" design="Transparent">
                    <ToolbarSpacer id="tb1" />
                </Toolbar>

                <!-- ========= SERVICE WR (USE CHILD NAVIGATION) ========= -->
<!-- SERVICE WR – absolute but filtered by selected TourId -->
                <macros:FilterBar
                    id="ServiceWRFilterBar"
                    contextPath="/ServiceAssignment"
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                <macros:Table
                    id="ServiceWRTable"
                    contextPath="/ServiceAssignment"
                    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                    filterBar="ServiceWRFilterBar"
                    selectionMode="ForceMulti"
                    header="Service WR"
                    
                     />

            </VBox>
        </content>

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




sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI;        // sap.fe.core.ExtensionAPI
    var oDialog;        // ONE dialog instance

    function applyFiltersAndSearch(sTourId) {
        // --- Service WR: Filter by TourId ---
        var oServiceFB = oExtAPI.byId("ServiceWRFilterBar");
        if (oServiceFB && oServiceFB.setFilterConditions) {
            var mCond = oServiceFB.getFilterConditions() || {};
            mCond.TourId = [{
                operator: "EQ",
                values: [sTourId],
                isEmpty: false
            }];
            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();  // Triggers immediate load
        }

        // --- Attachments: load all ---
        var oAttachFB = oExtAPI.byId("AttachmentFilterBar");
        if (oAttachFB && oAttachFB.search) {
            oAttachFB.search();  // Triggers immediate load
        }
    }

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }
            var sTourId = oTourCtx.getProperty("TourId");

            if (oDialog) {
                // Reopen: Detach old handler, attach new one with current sTourId (closure)
                oDialog.detachAfterOpen(oDialog._afterOpenHandler);
                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);  // Closure captures current sTourId
                };
                oDialog.attachAfterOpen(oDialog._afterOpenHandler);

                oDialog.open();
                return;
            }

            // Initial open
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                // Attach handler with closure (captures current sTourId)
                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);  // Always the correct sTourId
                };
                oDialog.attachAfterOpen(oDialog._afterOpenHandler);

                oDialog.open();
            });
        },

               onDialogChoose: function () {
            var oTopTable    = oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId("ServiceWRTable");

            function getSelectedObjects(oTable) {
                if (!oTable || !oTable.getSelectedContexts) { return []; }
                return (oTable.getSelectedContexts() || []).map(function (oCtx) {
                    return oCtx.getObject();
                });
            }

            var aTopSelected    = getSelectedObjects(oTopTable);
            var aBottomSelected = getSelectedObjects(oBottomTable);

            if (!aTopSelected.length && !aBottomSelected.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            var aAttachmentItems = aTopSelected;
            var aServiceWRItems  = aBottomSelected;

            var sAttachmentJson = JSON.stringify(aAttachmentItems);
            var sServiceWRJson  = JSON.stringify(aServiceWRItems);

            var oModel = oExtAPI.getModel();

            var oActionBinding = oModel.bindContext(
                "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
            );

            oActionBinding.setParameter("AttachmentItemsjson", sAttachmentJson);
            oActionBinding.setParameter("ServiceWRItemsjson",  sServiceWRJson);

            oActionBinding.execute("$auto").then(function () {
                MessageToast.show("Documents were generated successfully.");
                oModel.refresh();
            }).catch(function (oError) {
                sap.m.MessageBox.error(oError.message || "Error while generating documents.");
            });

            if (oDialog) {
                oDialog.close();
            }
        },

        onDialogCancel: function () {
            if (oDialog) {
                oDialog.close();
            }
        }
    };


    return oActionHandlers;
});

    </Dialog>
</core:FragmentDefinition>
