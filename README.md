// webapp/ext/controller/ListReportExt.js
sap.ui.define([
    "sap/m/MessageToast",
    "sap/ui/core/Fragment"
], function (MessageToast, Fragment) {
    "use strict";

    return {

        // keep dialog instance on the extension object
        _oGenerateDocDialog: null,

        /**
         * Custom action "Manual Attachments"
         *
         * @param oContext          context of the page (undefined on LR)
         * @param aSelectedContexts selected rows of the table (if requiresSelection = true)
         */
        manualattachments: function (oContext, aSelectedContexts) {
            var oView = this.getView();   // List Report view

            // lazy-load the fragment once
            if (!this._oGenerateDocDialog) {
                var that = this;

                Fragment.load({
                    id: oView.getId(),   // make fragment IDs view-local
                    name: "zsb_attachment.ext.fragment.GenerateDocDialog", // <== your fragment name
                    controller: this     // handlers inside fragment use this object
                }).then(function (oDialog) {
                    that._oGenerateDocDialog = oDialog;
                    oView.addDependent(oDialog); // lifecycle + models
                    oDialog.open();
                });

            } else {
                this._oGenerateDocDialog.open();
            }
        },

        // second action – still free to use or remove
        manualattachments2: function (oContext, aSelectedContexts) {
            MessageToast.show("Custom handler invoked 2.");
        },

        // called from the dialog's Close/Cancel button
        onGenerateDocDialogClose: function () {
            if (this._oGenerateDocDialog) {
                this._oGenerateDocDialog.close();
            }
        }
    };
});
