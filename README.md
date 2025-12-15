function applyFiltersAndSearch(sTourId) {
    var oServiceFB = resolveFilterBar("ServiceWRFilterBar");
    var oAttachFB  = resolveFilterBar("AttachmentFilterBar");

    // Attachments: if it should also depend on TourId, set it here too
    if (oAttachFB && oAttachFB.getFilterConditions && oAttachFB.setFilterConditions && oAttachFB.search) {
        var mA = oAttachFB.getFilterConditions() || {};
        // only if Attachment entity really has TourId as a filter field
        // mA.TourId = [{ operator: "EQ", values: [sTourId] }];
        oAttachFB.setFilterConditions(mA);
        oAttachFB.search();
    }

    // Service WR: TourId = selected Tour and trigger search
    if (oServiceFB && oServiceFB.getFilterConditions && oServiceFB.setFilterConditions && oServiceFB.search) {
        var mCond = oServiceFB.getFilterConditions() || {};
        mCond.TourId = [{
            operator: "EQ",
            values: [sTourId],
            isEmpty: false
        }];
        oServiceFB.setFilterConditions(mCond);
        oServiceFB.search();
    }
}
