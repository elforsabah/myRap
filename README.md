onScanCard: function () {
    var oInput = this.byId("ipContract");
    if (oInput) {
        oInput.focus();  // Focus the input so the scanner's "keystrokes" go here
        oInput.setValue("");  // Optional: Clear any existing value to avoid appending
        MessageToast.show("Please scan the RFID card now.");  // User prompt (shows a toast message)
    }
},
