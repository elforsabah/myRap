sap.ui.define([
    "sap/m/MessageToast"
], function(MessageToast) {
    'use strict';

    return {
        /**
         * Generated event handler.
         *
         * @param oContext the context of the page on which the event was fired. `undefined` for list report page.
         * @param aSelectedContexts the selected contexts of the table rows.
         */
        manualattachments: function(oContext, aSelectedContexts) {
            MessageToast.show("Custom handler invoked.");
        },

        manualattachments2: function(oContext, aSelectedContexts) {
            MessageToast.show("Custom handler invoked 2.");
        }
    };
});
