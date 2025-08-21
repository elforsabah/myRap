sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",
    "sap/m/MessageBox"
], function (PageController, MessageToast, Messaging, MessageBox) {
    "use strict";
    return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
        onInit: function () {
            var parentReturn = PageController.prototype.onInit.apply(this);

            // Messaging
            Messaging.registerObject(this.getView(), true);
            this.getView().setModel(Messaging.getMessageModel(), "message");

            // Wizard
            this.oWizard = this.byId("weighingWizard");

            // Router
            var oRouter = this.getAppComponent().getRouter();
            oRouter.getRoute("ZI_WR_WEIGHINGSESSIONMain").attachPatternMatched(this._onObjectMatched, this);

            this._enterWired = false; // guard so we wire Enter only once
            return parentReturn;
        },

        generateUUID: function () {
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
                var r = Math.random() * 16 | 0, v = c === 'x' ? r : (r & 0x3 | 0x8);
                return v.toString(16);
            });
        },

        _onObjectMatched: function () {
            var oModel = this.getView().getModel();
            var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION");
            var oNewContext = oListBinding.create({
                Sessionid: this.generateUUID()
            });
            this.getView().setBindingContext(oNewContext);
            oNewContext.requestProperty(["Vbeln", "Grossweight", "Grossweightunit", "Sessionid"]).catch(function () {});
        },

        // === ENTER wiring (ADDED) ===
        _setupEnterToNext: function () {
            if (this._enterWired) { return; }
            this._enterWired = true;

            // Step 1: Enter on Contract input -> reuse existing onNextStep
            var oIp = this.byId("ipContract");
            if (oIp) {
                oIp.addEventDelegate({
                    onsapenter: function (oEvent) {
                        oEvent.preventDefault();
                        oEvent.stopPropagation();
                        this.onNextStep();
                    }.bind(this)
                }, oIp);
            }

            // Step 3: Enter on Gross/Tare inputs -> compute & advance
            var aStep3Ids = ["ipGrossWeight", "ipTareWeight"];
            aStep3Ids.forEach(function (sId) {
                var oCtrl = this.byId(sId);
                if (oCtrl) {
                    oCtrl.addEventDelegate({
                        onsapenter: function (oEvent) {
                            oEvent.preventDefault();
                            oEvent.stopPropagation();
                            this._onStep3Enter();
                        }.bind(this)
                    }, oCtrl);
                }
            }.bind(this));
        },

        // Step 3 Enter handler (ADDED)
        _onStep3Enter: function () {
            var oGW = this.byId("ipGrossWeight");
            var oTW = this.byId("ipTareWeight");
            var oNW = this.byId("ipNetWeight");

            var gw = oGW ? parseFloat(oGW.getValue()) : NaN;
            var tw = oTW ? parseFloat(oTW.getValue()) : NaN;

            if (isNaN(gw) || isNaN(tw)) {
                MessageToast.show("Enter valid numbers for Gross and Tare.");
                return;
            }

            // persist to bound context so rebinds won't clear
            var oCtx = this.getView().getBindingContext();
            if (oCtx) {
                oCtx.setProperty("Grossweight", gw);
                oCtx.setProperty("TareWeight",  tw);
                oCtx.setProperty("NetWeight",   gw - tw);
            }
            if (oNW) { oNW.setValue(gw - tw); }

            this.oWizard.validateStep(this.byId("step3"));
            this.oWizard.nextStep();
        },

        onNextStep: function () {
            var oContext = this.getView().getBindingContext();
            var oCurrentStep = this.oWizard.getCurrentStep();
            var sStepId = oCurrentStep.split("--").pop();

            if (sStepId === "step1") {
                var sContractId = this.byId("ipContract").getValue();
                if (!sContractId) {
                    MessageToast.show("Please enter a Contract ID.");
                    return;
                }
                if (!oContext) {
                    MessageToast.show("No session context available.");
                    return;
                }

                // write user entry to model before action
                oContext.setProperty("Vbeln", sContractId);

                // FE EditFlow bound action call
                this.editFlow.invokeAction("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", {
                    model: this.getView().getModel(),
                    contexts: oContext,
                    parameterMap: { vbeln: sContractId }
                }).then(function (oResult) {
                    // optional: surface success messages
                    var aMsgs = Messaging.getMessageModel().getData() || [];
                    var aUnboundSuccess = aMsgs.filter(function (oMsg) {
                        return oMsg.getTarget && oMsg.getTarget() === "" &&
                               oMsg.getType && oMsg.getType() === "Success";
                    });
                    if (aUnboundSuccess.length > 0) {
                        MessageToast.show(aUnboundSuccess[0].getMessage());
                    }

                    // advance wizard
                    this.oWizard.validateStep(this.byId("step1"));
                    this.oWizard.nextStep();

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
                    MessageToast.show("Invalid Contract. Please try again.");
                });
            }
        },

        onAfterRendering: function(){
            var oIp = this.byId("ipContract");
            if (oIp) { oIp.focus(); }
            this._setupEnterToNext(); // wire Enter once
        },

        onChooseLoadType: function (oEvent) {
            const sLoadType   = oEvent.getSource().getBindingContext().getProperty("LoadType");
            const oSessionCtx = this.getView().getBindingContext();
            if (!oSessionCtx) { return; }

            oSessionCtx.setProperty("LoadType", sLoadType);
            this.oWizard.validateStep(this.byId("step2"));
            this.oWizard.nextStep();
        },

        onStepActivate: function () { /* optional per-step hooks */ },
        validateStep: function () { return true; },

        onCaptureWeight: function () {
            this.getView().getModel().setProperty("/Grossweight", 1000);
        },

        onCaptureFinalWeight: function () {
            var oModel = this.getView().getModel();
            var fGross = oModel.getProperty("/Grossweight");
            var fTare  = oModel.getProperty("/TareWeight");
            oModel.setProperty("/NetWeight", fGross - fTare);
        },

        onSubmit: function () {
            this.getView().getModel().submitChanges({
                success: function () { MessageToast.show("Weighing session submitted."); }
            });
        },

        onPrintSlip: function () {},
        onWizardComplete: function () {}
    });
});
