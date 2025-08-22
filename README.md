o auto-detect scan completion, you could add a change handler on the input:


// In onInit, after getting oIp:
oIp.attachChange(function (oEvent) {
    var sValue = oEvent.getParameter("value");
    if (sValue && sValue.length >= 10) {  // Assume Vbeln is at least 10 chars; adjust based on your format
        this.onNextStep();
    }
}.bind(this));
