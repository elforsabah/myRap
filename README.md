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

                    // Pad with leading zeros for internal format
                    sContractId = sContractId.padStart(10, '0');

                    // FE EditFlow bound action call (no client-side setProperty to avoid PATCH/lock error)
                    this.editFlow.invokeAction("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", {
                        model: this.getView().getModel(),
                        contexts: oContext,
                        parameterValues: [{ name: "vbeln", value: sContractId }],
                        skipParameterDialog: true
                    }).then(function (oResult) {
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

                        // ✅ inline on the field instead:
                        this._setContractInlineError("Invalid Contract. Please try again.");
                        // MessageBox.error("Invalid Contract. Please try again.");
                    }.bind(this));
                }
            },
