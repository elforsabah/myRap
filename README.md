onNextStep: function () {
  var oContext = this.getView().getBindingContext();
  var oCurrentStep = this.oWizard.getCurrentStep();
  var sStepId = oCurrentStep.split("--").pop();

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

    // 1) Ensure the transient draft is persisted
    oContext.created()
      // 2) Acquire RAP draft lock (enter FE edit mode)
      .then(function () {
        return this.editFlow.editDocument(oContext);
      }.bind(this))
      // 3) Now safely call the bound action
      .then(function () {
        // Pad with leading zeros for internal format
        sContractId = sContractId.padStart(10, '0');

        return this.editFlow.invokeAction(
          // ✅ action name WITHOUT "(...)"
          "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard",
          {
            model: this.getView().getModel(),
            // ✅ contexts must be an ARRAY
            contexts: [oContext],
            parameterValues: [{ name: "vbeln", value: sContractId }],
            skipParameterDialog: true
          }
        );
      }.bind(this))
      // 4) Success handling (unchanged)
      .then(function (oResult) {
        // optional: surface success messages
        var aMsgs = Messaging.getMessageModel().getData() || [];
        var aUnboundSuccess = aMsgs.filter(function (oMsg) {
          return oMsg.getTarget && oMsg.getTarget() === "" &&
                 oMsg.getType && oMsg.getType() === "Success";
        });
        if (aUnboundSuccess.length > 0) {
          MessageBox.show(aUnboundSuccess[0].getMessage(), {
            title: "Success"
          });
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

        // advance wizard (unchanged)
        this._clearContractInlineError();
        this.oWizard.validateStep(this.byId("step1"));
        this.oWizard.nextStep();
      }.bind(this))
      // 5) Error handling (unchanged UX; clearer message)
      .catch(function (oError) {
        // Inline on the field
        this._setContractInlineError("Invalid Contract or lock not acquired. Please try again.");
        // Optional: show technical message too
        // MessageBox.error((oError && oError.message) || "Action failed");
      }.bind(this));
  }
},
