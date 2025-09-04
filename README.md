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
        console.log("Waiting for entity creation...");  // Debug: Start of create chain
        oContext.created().then(function () {
            console.log("Entity created successfully. Invoking identifyCard...");  // Debug: Create succeeded
            var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", oContext);
            oAction.setParameter("Vbeln", sContractId);
            return oAction.invoke();
        }.bind(this)).then(function (oResult) {
            console.log("identifyCard invoked successfully.");  // Debug: Action succeeded
            // Messages are handled in _onMessageChange, so no need for additional checks here
            // Refresh context to pull server-set Vbeln and any other updates
            oContext.refresh();
            // Bind step 2 dynamically
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
            // advance wizard
            this._clearContractInlineError();
            this.oWizard.validateStep(this.byId("step1"));
            this.oWizard.nextStep();
        }.bind(this)).catch(function (oError) {
            console.error("Error in create or identifyCard: ", oError);  // Debug: Unified catch for full error details
            // Handle failure (original logic consolidated into one catch for simplicity)
            var sErrorMsg = oError.message || "Unknown error";
            if (oError.status === 404 || oError.status === 400) {  // Example: Customize based on common HTTP errors
                this._setContractInlineError("Invalid Contract. Please try again.");
            } else {
                this._setContractInlineError("Failed to create weighing session or identify contract: " + sErrorMsg);
            }
        }.bind(this));
    }
},
