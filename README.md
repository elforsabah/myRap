onNextStep: function () {
  var oContext = this.getView().getBindingContext();
  var oCurrentStep = this.oWizard.getCurrentStep();
  // FIX: getId() instead of split on the object
  var sStepId = (oCurrentStep && oCurrentStep.getId && oCurrentStep.getId().split("--").pop()) || "";

  if (sStepId === "step1") {
    var sContractId = this.byId("step1InputContract").getValue();
    if (!sContractId) {
      MessageToast.show("Please enter a Contract ID.");
      return;
    }
    if (!oContext) {
      MessageToast.show("No session context available.");
      return;
    }

    // Helpful tracing
    jQuery.sap.log.info("[NextStep] starting, ctx path=" + (oContext.getPath ? oContext.getPath() : "<no path>"));

    // 1) Ensure the transient draft is persisted
    return oContext.created()
      .then(function () {
        jQuery.sap.log.info("[NextStep] context.created() resolved");
        // 2) Acquire RAP draft lock only if context is ACTIVE
        var bIsActive = oContext.getProperty && oContext.getProperty("IsActiveEntity");
        jQuery.sap.log.info("[NextStep] IsActiveEntity=" + bIsActive);
        return bIsActive ? this.editFlow.editDocument(oContext) : Promise.resolve();
      }.bind(this))
      .then(function () {
        jQuery.sap.log.info("[NextStep] editDocument (if needed) done, preparing invokeAction");

        // Pad with leading zeros for internal format
        sContractId = sContractId.padStart(10, '0');

        // 3) FE bound action (contexts must be an array; name w/o "(...)")
        return this.editFlow.invokeAction(
          "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard",
          {
            model: this.getView().getModel(),
            contexts: [oContext],
            parameterValues: [{ name: "vbeln", value: sContractId }],
            skipParameterDialog: true
          }
        );
      }.bind(this))
      .then(function (oResult) {
        jQuery.sap.log.info("[NextStep] invokeAction resolved");

        // optional: surface success messages
        var aMsgs = Messaging.getMessageModel().getData() || [];
        var aUnboundSuccess = aMsgs.filter(function (oMsg) {
          return oMsg.getTarget && oMsg.getTarget() === "" &&
                 oMsg.getType && oMsg.getType() === "Success";
        });
        if (aUnboundSuccess.length > 0) {
          MessageBox.show(aUnboundSuccess[0].getMessage(), { title: "Success" });
        }

        // Bind step 2 dynamically (unchanged)
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
      .catch(function (oError) {
        // Stronger diagnostics so we see *why* invokeAction didn't run
        var sMsg = (oError && (oError.message || oError.toString())) || "Unknown error";
        jQuery.sap.log.error("[NextStep] chain failed: " + sMsg, oError && oError.stack);
        this._setContractInlineError("Invalid Contract or draft/lock issue. Please try again.");
        // MessageBox.error(sMsg); // optional
      }.bind(this));
  }
},
