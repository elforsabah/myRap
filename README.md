// + add to sap.ui.define deps:
"sap/ui/core/message/Message",
"sap/ui/core/MessageType"

// + add to factory function params:
Message,
MessageType


_setContractInlineError: function (sText) {
  // Inline on the input
  var oInput = this.byId("ipContract");
  if (oInput) {
    oInput.setValueState("Error");
    oInput.setValueStateText(sText);
    oInput.focus();
  }

  // Optional: also add a field-bound message (shows in FE message popover / keeps state on rebind)
  var oCtx = this.getView().getBindingContext();
  var oModel = this.getView().getModel();
  if (oCtx && oModel) {
    var sTarget = oCtx.getPath() + "/Vbeln"; // property bound to the input
    var aAll = Messaging.getMessageModel().getData() || [];
    var aOldForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
    if (aOldForField.length) { Messaging.removeMessages(aOldForField); }
    Messaging.addMessages(new Message({
      message: sText,
      type: MessageType.Error,
      target: sTarget,
      processor: oModel
    }));
  }
},

_clearContractInlineError: function () {
  var oInput = this.byId("ipContract");
  if (oInput) {
    oInput.setValueState("None");
    oInput.setValueStateText("");
  }
  var oCtx = this.getView().getBindingContext();
  if (oCtx) {
    var sTarget = oCtx.getPath() + "/Vbeln";
    var aAll = Messaging.getMessageModel().getData() || [];
    var aForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
    if (aForField.length) { Messaging.removeMessages(aForField); }
  }
}


 // ✅ inline on the field instead:
  this._setContractInlineError("Invalid Contract. Please try again.");
