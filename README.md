    onNextStep: function () {
      var oContext = this.getView().getBindingContext();
      var oCurrentStep = this.oWizard.getCurrentStep();
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

        Log.info("[NextStep] starting, ctx path=" + (oContext.getPath ? oContext.getPath() : "<no path>"));

        // 1) Ensure the transient draft is persisted
        return oContext.created()
          .then(function () {
            Log.info("[NextStep] context.created() resolved");
            var bIsActive = oContext.getProperty && oContext.getProperty("IsActiveEntity");
            Log.info("[NextStep] IsActiveEntity=" + bIsActive);
            return bIsActive ? this.editFlow.editDocument(oContext) : Promise.resolve();
          }.bind(this))
          .then(function () {
            Log.info("[NextStep] editDocument (if needed) done, preparing invokeAction");

            // Pad with leading zeros
            sContractId = sContractId.padStart(10, '0');

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
          .then(function () {
            Log.info("[NextStep] invokeAction resolved");

            // Surface success messages if any
            var aMsgs = Messaging.getMessageModel().getData() || [];
            var aUnboundSuccess = aMsgs.filter(function (oMsg) {
              return oMsg.getTarget && oMsg.getTarget() === "" &&
                     oMsg.getType && oMsg.getType() === "Success";
            });
            if (aUnboundSuccess.length > 0) {
              MessageBox.show(aUnboundSuccess[0].getMessage(), { title: "Success" });
            }

            // Bind step 2 dynamically
            var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')";
            var oVBox = this.byId("step2LtContainer");
            var oTemplate = new sap.m.Button({
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
            var sMsg = (oError && (oError.message || oError.toString())) || "Unknown error";
            Log.error("[NextStep] chain failed: " + sMsg, oError && oError.stack);
            this._setContractInlineError("Invalid Contract or draft/lock issue. Please try again.");
            // MessageBox.error(sMsg); // optional
          }.bind(this));
      }
    }
