manualattachments: function (oContext, aSelectedContexts) {
    oExtAPI = this; // ExtensionAPI

    const oTourCtx = aSelectedContexts && aSelectedContexts[0];
    if (!oTourCtx) {
        MessageToast.show("Please select a tour first.");
        return;
    }

    const sTourId = oTourCtx.getProperty("TourId");

    const openWithTour = () => {
        oDialog._currentTourId = sTourId;   // <- store current selection
        oDialog.open();                    // <- afterOpen will read _currentTourId
    };

    // Re-use existing dialog
    if (oDialog) {
        openWithTour();
        return;
    }

    // First-time load
    oExtAPI.loadFragment({
        name: "zpdattachment.ext.fragments.GenerateDocDialog",
        controller: oActionHandlers
    }).then(function (oLoadedDialog) {

        oDialog = oLoadedDialog;
        oExtAPI.addDependent(oDialog);

        // Attach ONCE. Important: read oDialog._currentTourId at runtime (no stale closure)
        oDialog.attachAfterOpen(function () {
            // give it a bit more time than 20x50ms if your system is slow
            waitAndApplyFilters(oDialog._currentTourId, 80, 100);
        });

        openWithTour();
    });
},
