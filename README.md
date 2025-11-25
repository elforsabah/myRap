onNextStep: function () {
    var oCurrentStep = this.oWizard.getCurrentStep();
    var sStepId = oCurrentStep.split("--").pop();

    if (sStepId === "step1") {
        // 1) Read ID card number from local model
        var oLocalModel = this.getView().getModel("local");
        var sIdCardNr = oLocalModel.getProperty("/contractId"); // here: contractId is the ID card input

        if (!sIdCardNr) {
            MessageToast.show(this.getResourceBundle().getText("enterContractId"));
            return;
        }

        // IDCardNr is 9 characters
        sIdCardNr = sIdCardNr.padStart(9, "0");

        var oModel = this.getView().getModel();

        // 2) Find the entity by IDCardNr
        var oListBinding = oModel.bindList(
            "/ZI_WR_R_WEIGHBRIDGE",
            undefined,
            undefined,
            new Filter("IDCardNr", FilterOperator.EQ, sIdCardNr),
            { $$ownRequest: true }
        );

        oListBinding.requestContexts(0, 1).then(function (aContexts) {
            if (aContexts.length > 0) {

                // 3) We have a match → get the SalesDocument key
                var oFoundCtx      = aContexts[0];
                var sSalesDocument = oFoundCtx.getProperty("SalesDocument"); // THIS is what you wanted

                // Optional: make sure it’s 10 characters (usually it already is)
                sSalesDocument = sSalesDocument.padStart(10, "0");

                // Store SalesDocument in local model for all subsequent steps
                oLocalModel.setProperty("/contractId", sSalesDocument);

                console.log("Invoking identifyCard with IDCardNr", sIdCardNr, "and SalesDocument", sSalesDocument);

                // 4) Build action path using the *SalesDocument* key of the entity
                var sEntityPath = oFoundCtx.getPath(); // e.g. "/ZI_WR_R_WEIGHBRIDGE('0000123456')"
                var sActionPath = sEntityPath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(...)";
                var oAction     = oModel.bindContext(sActionPath);

                // Parameter Vbeln is the ID-card (9 chars)
                oAction.setParameter("Vbeln", sIdCardNr);

                oAction.execute().then(function () {
                    console.log("identifyCard invoked successfully.");

                    // 5) Use the returned context from action as your session context
                    var oContext = oAction.getBoundContext();
                    this.getView().setBindingContext(oContext);
                    oContext.refresh();

                    // 6) Load types: use the SalesDocument (contract) here
                    var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sSalesDocument + "')/Set";
                    var oVBox = this.byId("step2LtContainer");

                    var oTemplate = new Button({
                        text: {
                            parts: [
                                { path: 'Avvcode',       targetType: 'any' },
                                { path: 'Material',      targetType: 'any' },
                                { path: 'MaterialText',  targetType: 'any' }
                            ],
                            formatter: '.formatter.concatMaterialText'
                        },
                        press: [this.onChooseLoadType, this],
                        width: "100%"
                    });
                    oTemplate.addStyleClass("loadTypeBtn");

                    if (oVBox) {
                        try {
                            oVBox.bindAggregation("items", {
                                path: sPath,
                                parameters: {
                                    $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Avvcode,Language,Korselsnrindicator,Korselsnr",
                                    $orderby: "SalesOrder,SalesOrderitem"
                                },
                                template: oTemplate,
                                templateShareable: false,
                                events: {
                                    change: function (oEvent) {
                                        if (oEvent.getParameter("reason") === "Rejected") {
                                            MessageToast.show(this.getResourceBundle().getText("failedLoadTypes"));
                                            this._skipToWeigh();
                                        }
                                    }.bind(this)
                                }
                            });

                            var oBinding = oVBox.getBinding("items");
                            oBinding.attachEventOnce("dataReceived", function () {
                                if (oBinding.getLength() === 0) {
                                    this._skipToWeigh();
                                }
                            }.bind(this));

                        } catch (oError) {
                            console.error("Binding error: ", oError);
                            this._setContractInlineError(this.getResourceBundle().getText("failedMaterials"));
                            return;
                        }
                    } else {
                        console.error("step2LtContainer not found");
                    }

                    // Messages: same as before
                    var aMessages = Messaging.getMessageModel().getData() || [];
                    var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                        return oMsg.getType() === "Success" && oMsg.getCode() === "ZWR_WEIGHBRIGE_MESS/007";
                    });
                    if (aSpecificSuccesses.length > 0) {
                        oLocalModel.setProperty("/transaction_state", "2. Weighing");
                    } else {
                        oLocalModel.setProperty("/transaction_state", "1. Weighing");
                    }

                    this._clearContractInlineError();
                    this.oWizard.validateStep(this.byId("step1"));
                    MessageToast.show(this.getResourceBundle().getText("contractValid"));
                    this.oWizard.nextStep();

                }.bind(this)).catch(function (oError) {
                    console.error("Error in identifyCard: ", oError);
                    var sErrorMsg = oError.message || "Unknown error";
                    this._setContractInlineError(sErrorMsg);
                }.bind(this));

            } else {
                // No entity found for this ID card
                this._setContractInlineError(this.getResourceBundle().getText("contrnotfd"));
            }
        }.bind(this)).catch(function (oError) {
            console.error("Entity check error: ", oError);
            var sErrorMsg = oError.message || "Unknown error";
            this._setContractInlineError(sErrorMsg);
        }.bind(this));
    }
}
