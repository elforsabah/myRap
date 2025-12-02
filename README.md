<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:macros="sap.fe.macros">

    <!-- Popup dialog -->
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
                    metaPath="/PrintConfiguration/@com.sap.vocabularies.UI.v1.SelectionFields"
                    contextPath="/PrintConfiguration" />

                <macros:Table
                    id="AttachmentTable"
                    metaPath="/PrintConfiguration/@com.sap.vocabularies.UI.v1.LineItem"
                    contextPath="/PrintConfiguration"
                    filterBar="AttachmentFilterBar"
                    selectionMode="Multi"
                    header="Attachments" />

                <!-- Spacer between the two areas -->
                <Toolbar id="tbSpacer1" design="Transparent">
                    <ToolbarSpacer id="tb1" />
                </Toolbar>

                <!-- ========= SERVICE WR “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="ServiceWRFilterBar"
                    metaPath="/ServiceWR/@com.sap.vocabularies.UI.v1.SelectionFields"
                    contextPath="/ServiceWR" />

                <macros:Table
                    id="ServiceWRTable"
                    metaPath="/ServiceWR/@com.sap.vocabularies.UI.v1.LineItem"
                    contextPath="/ServiceWR"
                    filterBar="ServiceWRFilterBar"
                    selectionMode="Multi"
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



sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI;   // sap.fe.core.ExtensionAPI
    var oDialog;   // will hold the ONE dialog instance

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // in FE V4 this is ExtensionAPI

            // 1) Dialog already created once 
            if (oDialog) {
                oDialog.open();
                return;
            }
            // 2) First time: load fragment once
            oExtAPI.loadFragment({
                name: "zsbattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers   // .onDialogChoose / .onDialogCancel
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;          
                oExtAPI.addDependent(oDialog);    // clean lifecycle
                oDialog.open();
            });
        },
        /**
         * "Choose" button in the dialog
         */
        onDialogChoose: function () {
            // Tables are building-block tables (mdc.Table) – use ExtensionAPI.byId
            var oTopTable    = oExtAPI.byId("TopTable");
            var oBottomTable = oExtAPI.byId("BottomTable");

            function getSelectedObjects(oTable) {
                if (!oTable || !oTable.getSelectedContexts) { return []; }
                var aCtx = oTable.getSelectedContexts() || [];
                return aCtx.map(function (oCtx) { return oCtx.getObject(); });
            }

            var aTopSelected    = getSelectedObjects(oTopTable);
            var aBottomSelected = getSelectedObjects(oBottomTable);

            if (!aTopSelected.length && !aBottomSelected.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }
          
            var aAttachmentItems = aTopSelected;   // your array of attachment objects
            var aServiceWRItems  = aBottomSelected;     // your array of service WR objects

            // 1) Build JSON strings (as required by action parameters AttachmentItemsjson, ServiceWRItemsjson)
            var sAttachmentJson = JSON.stringify(aAttachmentItems);
            var sServiceWRJson  = JSON.stringify(aServiceWRItems);

            // 2) Get the OData V4 model 
            var oModel = this.getView().getModel();  // sap.ui.model.odata.v4.ODataModel

            // 3) Bind the collection-bound action to the Tour entity set
            //    Path pattern from metadata: /Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)
            var oActionBinding = oModel.bindContext(
                "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
            );

            // 4) Set non-binding parameters exactly as defined in the metadata
            oActionBinding.setParameter("AttachmentItemsjson", sAttachmentJson);
            oActionBinding.setParameter("ServiceWRItemsjson", sServiceWRJson);

            // 5) Execute the action
            //    Optional: pass a groupId like "$auto" if you use batching conventions.
            oActionBinding.execute("$auto").then(function (oResultContext) {
                // oResultContext is a V4 context pointing to the returned TourType
                // e.g. you can read the returned data if needed:
                var oReturnedTour = oResultContext.getObject();

             sap.m.MessageToast.show("Documents were generated successfully.");

                // Optionally refresh the list / tables if attachments changed
                // This refreshes all bindings of the model (simple & safe):
                oModel.refresh();
            }).catch(function (oError) {
                // Handle backend or network errors
                sap.m.MessageBox.error(oError.message || "Error while generating documents.");
            });

            MessageToast.show(
                "Selected: " + aTopSelected.length + " top, " +
                aBottomSelected.length + " bottom"
            );

            // Close after processing
            if (oDialog) {
                oDialog.close();
            }
        },

        /**
         * "Cancel" button in the dialog
         */
        onDialogCancel: function (oEvent) {
            // Robust: close the parent dialog of the button
            if (oEvent && oEvent.getSource) {
                var oSource = oEvent.getSource();
                var oParent = oSource.getParent();  // for beginButton/endButton this is the Dialog
                if (oParent && oParent.close) {
                    oParent.close();
                    return;
                }
            }

            // fallback – use stored reference
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});


LoaderExtensions-dbg.js:331 Uncaught (in promise) Error: resource zsbattachment/ext/fragments/GenerateDocDialog.fragment.xml could not be loaded from ../resources/zsbattachment/ext/fragments/GenerateDocDialog.fragment.xml. Check for 'file not found' or parse errors. Reason: 
    at LoaderExtensions-dbg.js:331:19
    at SyncPromise-dbg.js:314:14
    at e (SyncPromise-dbg.js:63:4)
    at new r (SyncPromise-dbg.js:230:3)
    at r.then (SyncPromise-dbg.js:313:7)
    at i.loadResource (LoaderExtensions-dbg.js:323:5)
    at sap.ui.predefine.U.loadTemplate (XMLTemplateProcessor-dbg.js:268:27)
    at Object.Ee [as templateControlFragment] (CommonUtils.ts:1599:25)
    at e.n [as loadFragment] (ExtensionAPI.ts:289:40)
    at e.manualattachments (ListReportExt.js:19:21)



