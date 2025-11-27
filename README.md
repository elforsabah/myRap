onDialogCancel: function (oEvent) {
    // oEvent is the press event of the Cancel button
    var oSource = oEvent.getSource();        // the Button
    var oDialog = oSource.getParent();       // for beginButton/endButton this is the Dialog

    if (oDialog && oDialog.close) {
        oDialog.close();
    }
}
