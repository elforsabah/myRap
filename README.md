onContractChange: function (oEvent) {
    var oInput = oEvent.getSource();
    var sValue = oInput.getValue();
    if (sValue.length > 10) {
        oInput.setValueState("Error");
        oInput.setValueStateText("Contract ID cannot exceed 10 characters.");
    } else {
        oInput.setValueState("None");
    }
},
