sap.ui.define([
    "sap/m/MessageToast",
    "sap/m/MessageBox"
], function (MessageToast, MessageBox) {
    "use strict";

    var oExtAPI;              // sap.fe.core.ExtensionAPI
    var oDialog;              // Dialog instance
    var sSelectedTourId;      // TourId from ZC_PDTOUR
    var oSelectedTourDates = {
        from: null,
        to:   null
    };

    // --- helper: get inner MDC FilterBar created by macros:FilterBar ---
    function getInnerFilterBar(sMacroId) {
        // macros:FilterBar -> inner FilterBar has suffix "::FilterBar"
        return oExtAPI && oExtAPI.byId(sMacroId + "::FilterBar");
    }

    // --- apply filters and trigger GO on both tables ---
    function applyTourFilters() {
        // ===== SERVICE WR =====
        var oServiceFB = getInnerFilterBar("ServiceWRFilterBar");
        if (oServiceFB && oServiceFB.setFilterConditions) {
            var mCond = oServiceFB.getFilterConditions() || {};

            // 1) mandatory RequestedDate on ServiceWR
            //    -> use tour start / end date as interval
            if (oSelectedTourDates.from && oSelectedTourDates.to) {
                mCond.RequestedDate = [{
                    operator: "BT",
                    values: [oSelectedTourDates.from, oSelectedTourDates.to],
                    isEmpty: false
                }];
            }

            // 2) our hidden TourId filter so we only see services of this tour
            if (sSelectedTourId) {
                mCond.TourId = [{
                    operator: "EQ",
                    values: [sSelectedTourId],
                    isEmpty: false
                }];
            }

            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();               // behaves like pressing GO
        }

        // ===== ATTACHMENTS (PrintConfiguration) =====
        // Here you just want data → trigger GO once.
        var oAttachFB = getInnerFilterBar("AttachmentFilterBar");
        if (oAttachFB && oAttachFB.search) {
            var mCondAttach = oAttachFB.getFilterConditions() || {};
            // (optional) if you also want to restrict attachments by date,
            // copy the same RequestedDate logic here.
            oAttachFB.setFilterConditions(mCondAttach);
            oAttachFB.search();
        }
    }

    var oActionHandlers = {

        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;  // ExtensionAPI in FE V4

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }

            var oTour = oTourCtx.getObject();
            sSelectedTourId = oTour.TourId;

            // use tour start / end date for mandatory RequestedDate filter
            oSelectedTourDates.from = oTour.TourStartDate;
            oSelectedTourDates.to   = oTour.TourEndDate || oTour.TourStartDate;

            // dialog already created
            if (oDialog) {
                oDialog.open();
                // ensure new tour’s filters are applied as well
                setTimeout(applyTourFilters, 0);
                return;
            }

            // first time: load fragment
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                oDialog.open();
                // wait until controls are created, then apply filters
                setTimeout(applyTourFilters, 0);
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
                MessageBox.error(oError.message || "Error while generating documents.");
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



@Consumption.filter: { mandatory: false, hidden: false, selectionType: #INTERVAL }
RequestedDate,
