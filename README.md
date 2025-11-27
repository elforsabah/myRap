onDialogChoose: function () {
    var oTopTable    = oExtAPI.byId("TopTable");
    var oBottomTable = oExtAPI.byId("BottomTable");

    function getSelectedObjects(oInnerTable) {
        if (!oInnerTable) { return []; }
        var aContexts = oInnerTable.getSelectedContexts();
        return aContexts.map(function (oCtx) { return oCtx.getObject(); });
    }

    var aTopSelected    = getSelectedObjects(oTopTable);
    var aBottomSelected = getSelectedObjects(oBottomTable);

    if (!aTopSelected.length && !aBottomSelected.length) {
        MessageToast.show("Please select at least one row in one of the tables.");
        return;
    }

    // Here you call your backend logic with aTopSelected & aBottomSelected
    // ...
    oActionHandlers.onDialogCancel();
}
