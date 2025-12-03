sap.ui.define([
    "sap/m/MessageToast",
    "sap/m/MessageBox"
], function (MessageToast, MessageBox) {
    "use strict";

    var oExtAPI;           // sap.fe.core.ExtensionAPI
    var oDialog;           // ONE dialog instance
    var sSelectedTourId;   // TourId from selected ZC_PDTOUR row

    function applyTourFilters() {
        if (!oExtAPI) {
            return;
        }

        // --- SERVICE WR TABLE: filter by TourId and trigger GO ---
        var oServiceFB = oExtAPI.byId("ServiceWRFilterBar");
        if (oServiceFB && oServiceFB.setFilterConditions) {
            var mCond = oServiceFB.getFilterConditions() || {};

            // Hidden filter on property TourId (ServiceWRType.TourId)
            mCond.TourId = [{
                operator: "EQ",
                values: [sSelectedTourId],
                isEmpty: false
            }];

            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();   // behaves like pressing GO
        }

        // --- ATTACHMENTS TABLE: just trigger GO (no special filter) ---
        var oAttachFB = oExtAPI.byId("AttachmentFilterBar");
        if (oAttachFB && oAttachFB.search) {
            oAttachFB.search();    // loads attachments / print config
        }
    }

    var oActionHandlers = {

        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;   // ExtensionAPI in FE V4

            // Take the first selected tour
            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }

            var oTourObject = oTourCtx.getObject();
            sSelectedTourId = oTourObject.TourId;  // field from ZC_PDTOUR

            // Dialog already created → just open; afterOpen will apply filters
            if (oDialog) {
                oDialog.open();
                return;
            }

            // First time: load fragment and wire up afterOpen
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                // Every time the dialog opens, apply the filters
                oDialog.attachAfterOpen(function () {
                    applyTourFilters();
                });

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

            var sAttachmentJson = JSON.stringify(aTopSelected);
            var sServiceWRJson  = JSON.stringify(aBottomSelected);

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

          <!-- ========= SERVICE WR “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="ServiceWRFilterBar"
                    contextPath="/ServiceWR"
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />
                <macros:Table
                    id="ServiceWRTable"
                    contextPath="/ServiceWR"
                    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                    filterBar="ServiceWRFilterBar"
                    selectionMode="ForceMulti"
                    header="Service WR" />

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

    </Dialog>
</core:FragmentDefinition>



