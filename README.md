onNextStep: function () {
  var oContext = this.getView().getBindingContext();
  var oCurrentStep = this.oWizard.getCurrentStep();
  var sStepId = oCurrentStep.split("--").pop();

  if (sStepId === "step1") {
    var sContractId = this.byId("step1InputContract").getValue();
    if (!sContractId) { MessageToast.show("Please enter a Contract ID."); return; }
    if (!oContext)     { MessageToast.show("No session context available."); return; }

    // 1) Ensure the transient draft is persisted
    oContext.created()
      // 2) Acquire lock only if we’re still on ACTIVE instance
      .then(function () {
        var bIsActive = oContext.getProperty("IsActiveEntity");
        return bIsActive ? this.editFlow.editDocument(oContext) : Promise.resolve();
      }.bind(this))
      // 3) Call bound action
      .then(function () {
        sContractId = sContractId.padStart(10, '0');

        return this.editFlow.invokeAction(
          "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard",
          {
            model: this.getView().getModel(),
            contexts: [oContext], // must be an array
            parameterValues: [{ name: "vbeln", value: sContractId }],
            skipParameterDialog: true
          }
        );
      }.bind(this))
      // 4) Success (your original logic)
      .then(function () {
        var aMsgs = Messaging.getMessageModel().getData() || [];
        var aUnboundSuccess = aMsgs.filter(function (oMsg) {
          return oMsg.getTarget && oMsg.getTarget() === "" &&
                 oMsg.getType && oMsg.getType() === "Success";
        });
        if (aUnboundSuccess.length > 0) {
          MessageBox.show(aUnboundSuccess[0].getMessage(), { title: "Success" });
        }

        var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')";
        var oVBox = this.byId("step2LtContainer");
        var oTemplate = new Button({
          text: {
            parts: [
              { path: 'Material', targetType: 'any' },
              { path: 'MaterialText', targetType: 'any' }
            ],
            formatter: '.formatter.concatMaterialText'
          },
          press: [this.onChooseLoadType, this],
          width: "100%"
        });
        oTemplate.addStyleClass("loadTypeBtn");
        oVBox.bindItems({
          path: sPath,
          parameters: {
            $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Language",
            $orderby: "SalesOrder,SalesOrderitem"
          },
          template: oTemplate,
          templateShareable: false
        });

        this._clearContractInlineError();
        this.oWizard.validateStep(this.byId("step1"));
        this.oWizard.nextStep();
      }.bind(this))
      // 5) Error (keep inline message)
      .catch(function (oError) {
        this._setContractInlineError("Invalid Contract or draft lock issue. Please try again.");
        // Optionally inspect oError.message for 423, etc.
      }.bind(this));
  }
},
