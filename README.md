sap.ui.define([
    "sap/ui/core/mvc/ControllerExtension",
    "sap/m/MessageToast",
    "sap/m/MessageBox",
    "sap/base/Log"
], function (ControllerExtension, MessageToast, MessageBox, Log) {
    "use strict";

    return ControllerExtension.extend("zpdattachment.ext.controller.ListReportExt", {

        // --------------------------------------------------------------------
        // Helper to access extensionAPI
        // --------------------------------------------------------------------
        _getExtensionAPI: function () {
            return this.base.getExtensionAPI();
        },

        // --------------------------------------------------------------------
        // Custom action handler (wired via Guided Development / manifest)
        // --------------------------------------------------------------------
        manuallyGenerateDocs: function () {
            var oExtAPI = this._getExtensionAPI();
            var oView   = this.base.getView();
            var that    = this;

            // Function to actually open the dialog once fragment is available
            var fnOpenDialog = function (oDialog) {
                that._oAttachmentDialog = oDialog;

                // Add as dependent so models / busy / lifecycle work correctly
                oView.addDependent(oDialog);

                oDialog.open();
            };

            // If we already created the dialog once, just reopen it
            if (this._oAttachmentDialog) {
                this._oAttachmentDialog.open();
                return;
            }

            // First time → load fragment
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragment.GenerateDocDialog", // adjust name if different
                controller: this
            }).then(fnOpenDialog).catch(function (oError) {
                Log.error("Error loading GenerateDocDialog fragment", oError);
                MessageBox.error("The attachment dialog could not be opened. Please contact IT.");
            });
        },

        // --------------------------------------------------------------------
        // Dialog: Cancel
        // --------------------------------------------------------------------
        onAttachmentDialogCancel: function () {
            if (this._oAttachmentDialog) {
                this._oAttachmentDialog.close();
            }
        },

        // --------------------------------------------------------------------
        // Dialog: OK / Confirm
        // --------------------------------------------------------------------
        onAttachmentDialogOk: function () {
            var oView = this.base.getView();
            var oTable = oView.byId("AttachmentTable"); // macros:Table

            var iCount = 0;

            // In many UI5 versions, sap.fe.macros.Table / mdc.Table exposes getSelectedContexts().
            // If it's not available in your version, this will just return 0 and NOT crash.
            if (oTable && typeof oTable.getSelectedContexts === "function") {
                var aContexts = oTable.getSelectedContexts() || [];
                iCount = aContexts.length;

                // TODO: Here you can send the selected attachment IDs to the backend
                // Example:
                // var aAttachmentIds = aContexts.map(function (oCtx) {
                //     return oCtx.getObject().AttachmentID;
                // });
                // ... call an action, function import, etc.
            }

            MessageToast.show(iCount + " attachment(s) selected.");

            this.onAttachmentDialogCancel();
        }
    });
});





<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:macros="sap.fe.macros">

    <!-- Popup dialog -->
    <Dialog
        id="TwoSmartTablesDialog"
        title="Choose Attachments"
        stretch="true"
        contentWidth="1200px"
        contentHeight="600px"
        class="sapUiResponsivePadding">

        <content>
            <VBox id="vbMain" width="100%" height="100%" renderType="Div">

                <!-- ========= ATTACHMENT FILTER BAR ========= -->
                <macros:FilterBar
                    id="AttachmentFilterBar"
                    contextPath="/PrintConfiguration"
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                <!-- ========= ATTACHMENT TABLE ========= -->
                <macros:Table
                    id="AttachmentTable"
                    contextPath="/PrintConfiguration"
                    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                    filterBar="AttachmentFilterBar"
                    type="GridTable"
                    selectionMode="Multi"
                    header="Attachments" />

            </VBox>
        </content>

        <!-- Buttons -->
        <beginButton>
            <Button
                text="OK"
                type="Emphasized"
                press=".onAttachmentDialogOk" />
        </beginButton>

        <endButton>
            <Button
                text="Cancel"
                press=".onAttachmentDialogCancel" />
        </endButton>

    </Dialog>
</core:FragmentDefinition>

