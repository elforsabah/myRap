                // write user entry to model before action
                oContext.setProperty("Vbeln", sContractId);

                // FE EditFlow bound action call
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
                        MessageBox.Success(aUnboundSuccess[0].getMessage());
                    }
                    // advance wizard
                    this.oWizard.validateStep(this.byId("step1"));
                    this.oWizard.nextStep()

                    // IMPORTANT: do NOT call oContext.refresh() (would clear inputs)
                    // Pull only what backend changed (safe if the paths exist)
                    oContext.requestSideEffects([
                        { $PropertyPath: "Vbeln" },
                        { $PropertyPath: "CustomerName" }, // adjust to your real fields
                        { $PropertyPath: "Step1Ok" }       // optional boolean flag if you have it
                    ]).catch(function () { /* ignore */ });

                }.bind(this)).catch(function (oError) {
                    // keep user input; just show message
                    console.error("Action Error:", oError);
                    MessageBox.error("Invalid Contract. Please try again.");
                }); 
            }
        },
