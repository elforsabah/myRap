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
                    contextPath="/ServiceAsignment"
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

    </Dialog>
</core:FragmentDefinition>
sap.ui.define([
    "sap/m/MessageToast",
    "sap/ui/core/Fragment"
], function (MessageToast, Fragment) {
    "use strict";

    var oExtAPI;
    var oDialog;

    // --------------------------------------------------------------
    // NEW: Robust function that waits until FilterBars are ready
    // --------------------------------------------------------------
    function waitForFilterBarAndSearch(oFilterBar, fnApply) {
        if (!oFilterBar) return;

        // Case 1: Already initialized → run immediately
        if (oFilterBar.getMetadata().getName() === "sap.ui.mdc.FilterBar" && oFilterBar.isInitialised?.()) {
            fnApply();
            return;
        }

        // Case 2: Not yet → listen to the real initialization event
        var fnHandler = function () {
            oFilterBar.detachEvent("initialized", fnHandler);   // one-time
            oFilterBar.detachEvent("initialise", fnHandler);    // fallback
            fnApply();
        };

        oFilterBar.attachInitialized(fnHandler);
        oFilterBar.attachInitialise(fnHandler);   // older UI5 versions use this
    }

    function applyFiltersAndSearch(sTourId) {
        // Use the dialog's own view to resolve IDs correctly (no prefix issue!)
        var oView = oDialog.getParent(); // oDialog is the Dialog → parent is the Fragment view

        var oServiceFB = oView.byId("ServiceWRFilterBar");
        var oAttachFB   = oView.byId("AttachmentFilterBar");

        // Service WR table – filter by TourId
        waitForFilterBarAndSearch(oServiceFB, function () {
            var mCond = oServiceFB.getFilterConditions() || {};
            mCond.TourId = [{
                operator: "EQ",
                values: [sTourId],
                isEmpty: false
            }];
            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();               // this now really works
        });

        // PrintConfiguration table – just load everything
        waitForFilterBarAndSearch(oAttachFB, function () {
            oAttachFB.search();
        });
    }

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;

            var oTourCtx = aSelectedContexts?.[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }
            var sTourId = oTourCtx.getProperty("TourId");

            if (oDialog) {
                // REOPEN case
                oDialog._currentTourId = sTourId;                    // remember for later
                oDialog.detachAfterOpen(oDialog._afterOpenHandler);
                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);
                };
                oDialog.attachAfterOpen(oDialog._afterOpenHandler);
                oDialog.open();
                return;
            }

            // FIRST OPEN
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog"
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);
                oDialog._currentTourId = sTourId;

                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);
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
