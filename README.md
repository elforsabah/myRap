onNextStep: function () {
    var oContext = this.getView().getBindingContext();
    var oCurrentStep = this.oWizard.getCurrentStep();
    var sStepId = oCurrentStep.split("--").pop();
    if (sStepId === "step1") {
        var sContractId = this.getView().getModel("local").getProperty("/contractId");
        if (!sContractId) {
            MessageToast.show("Please enter a Contract ID.");
            return;
        }
        if (!oContext) {
            MessageToast.show("No session context available.");
            return;
        }
        // Pad with leading zeros for internal format
        sContractId = sContractId.padStart(10, '0');
        // Manual bound action call to avoid automatic popups from editFlow
        var oModel = this.getView().getModel();
        // Submit batch to persist the transient entity (create request)
        oModel.submitBatch("weighingGroup");
        // Wait for persistence confirmation
        oContext.created().then(function () {
            var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", oContext);
            oAction.setParameter("Vbeln", sContractId);
            oAction.invoke().then(function (oResult) {
                // Messages are handled in _onMessageChange, so no need for additional checks here
                // Refresh context to pull server-set Vbeln and any other updates
                oContext.refresh();
                // Bind step 2 dynamically
                var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')";
                var oVBox = this.byId("step2LtContainer");
                var oTemplate = new Button({
                    text: {
                        parts: [
                            {path: 'Material', targetType: 'any'},
                            {path: 'MaterialText', targetType: 'any'}
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
                // advance wizard
                this._clearContractInlineError();
                this.oWizard.validateStep(this.byId("step1"));
                this.oWizard.nextStep();
            }.bind(this)).catch(function (oError) {
                // Technical errors or action failures (messages handled in _onMessageChange)
                this._setContractInlineError("Invalid Contract. Please try again.");
            }.bind(this));
        }.bind(this)).catch(function (oError) {
            // Handle failure to create the entity
            this._setContractInlineError("Failed to create weighing session: " + (oError.message || "Unknown error"));
        }.bind(this));
    }
},
