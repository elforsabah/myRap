sap.ui.define([
    "sap/m/MessageToast",
    "sap/ui/core/Fragment",
    "sap/m/MessageBox"
], function (MessageToast, Fragment, MessageBox) {
    "use strict";

    var oExtAPI;   // sap.fe.core.ExtensionAPI
    var oDialog;   // Dialog instance

    // ================================================================
    // Helper: resolve a control by its *local* ID
    // (works even with FE/macros prefixes and in fragments)
    // ================================================================
    function resolveControl(sLocalId) {
        var oCtrl = oExtAPI && oExtAPI.byId && oExtAPI.byId(sLocalId);
        if (oCtrl) {
            return oCtrl;
        }

        var mElements = sap.ui.getCore().mElements || {};
        for (var sId in mElements) {
            if (!Object.prototype.hasOwnProperty.call(mElements, sId)) {
                continue;
            }

            // match plain, with "--", and with "::"
            if (
                sId === sLocalId ||
                sId.endsWith("--" + sLocalId) ||
                sId.indexOf(sLocalId) >= 0
            ) {
                return mElements[sId];
            }
        }

        return null;
    }

    // ================================================================
    // Helper: wait until inner sap.ui.mdc.FilterBar is initialized
    // ================================================================
    function waitForFilterBarAndRun(oFilterBar, fnApply) {
        if (!oFilterBar) {
            return;
        }

        var bIsMdc = oFilterBar.isA && oFilterBar.isA("sap.ui.mdc.FilterBar");

        if (bIsMdc && oFilterBar.isInitialised && oFilterBar.isInitialised()) {
            fnApply();
            return;
        }

        var fnHandler = function () {
            if (oFilterBar.detachInitialized) {
                oFilterBar.detachInitialized(fnHandler);
            }
            if (oFilterBar.detachInitialise) {
                oFilterBar.detachInitialise(fnHandler);
            }
            fnApply();
        };

        if (oFilterBar.attachInitialized) {
            oFilterBar.attachInitialized(fnHandler);
        }
        if (oFilterBar.attachInitialise) {
            oFilterBar.attachInitialise(fnHandler);
        }
    }

    // ================================================================
    // Apply filters + trigger GO on both tables
    // ================================================================
    function applyFiltersAndSearch(sTourId) {
        // We want the *inner* MDC FilterBars behind the macros:FilterBar
        // → ID pattern: <macroId>::FilterBar
        var oServiceFB = resolveControl("ServiceWRFilterBar::FilterBar");
        var oAttachFB  = resolveControl("AttachmentFilterBar::FilterBar");

        // --- SERVICE WR: TourId = selected Tour ---
        waitForFilterBarAndRun(oServiceFB, function () {
            var mCond = oServiceFB.getFilterConditions() || {};

            mCond.TourId = [{
                operator : "EQ",
                values   : [sTourId],
                isEmpty  : false
            }];

            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();
        });

        // --- ATTACHMENTS: just trigger GO ---
        waitForFilterBarAndRun(oAttachFB, function () {
            oAttachFB.search();
        });
    }

    // ================================================================
    // Helper: get selected row objects from a macros:Table
    // (the macros table itself already supports getSelectedContexts)
    // ================================================================
    function getSelectedObjectsFromTableId(sTableId) {
        var oTable = oExtAPI && oExtAPI.byId && oExtAPI.byId(sTableId);
        if (!oTable || !oTable.getSelectedContexts) {
            return [];
        }

        var aContexts = oTable.getSelectedContexts() || [];
        return aContexts.map(function (oCtx) {
            return oCtx.getObject();
        });
    }

    // ================================================================
    // Action handlers
    // ================================================================
    var oActionHandlers = {
        // Called from FE action "manualattachments"
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // ExtensionAPI

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }

            var sTourId = oTourCtx.getProperty("TourId");

            // --- Dialog already created → just reopen and re-apply filters ---
            if (oDialog) {
                oDialog._currentTourId = sTourId;

                if (oDialog._afterOpenHandler) {
                    oDialog.
