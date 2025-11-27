sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    return {
        // Dialog instance
        _oDialog: null,

        manuallyGenerateDocs: function (oEvent, oData) {
            var oView = this.getView();

            // Lazy-create the dialog
            if (!this._oDialog) {
                this._oDialog = sap.ui.xmlfragment(
                    oView.getId(),
                    "fragment.Popup"
                );
                oView.addDependent(this._oDialog);
            }

            this._oDialog.open();
        },

        onCloseDialog: function () {
            if (this._oDialog) {
                this._oDialog.close();
            }
        }
    };
});
