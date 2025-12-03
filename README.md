function applyFiltersAndSearch(sTourId) {
    if (!oExtAPI) {
        return;
    }

    // Resolve controls via FE ExtensionAPI
    var oServiceFB = oExtAPI.byId("ServiceWRFilterBar");
    var oAttachFB  = oExtAPI.byId("AttachmentFilterBar");

    // SERVICE WR – filter by TourId
    waitForFilterBarAndSearch(oServiceFB, function () {
        var mCond = oServiceFB.getFilterConditions() || {};
        mCond.TourId = [{
            operator: "EQ",
            values: [sTourId],
            isEmpty: false
        }];
        oServiceFB.setFilterConditions(mCond);
        oServiceFB.search();
    });

    // ATTACHMENTS – just trigger GO
    waitForFilterBarAndSearch(oAttachFB, function () {
        oAttachFB.search();
    });
}
