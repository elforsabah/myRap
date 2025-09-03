
sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",
    "sap/m/MessageBox",
    "sap/ui/core/message/Message",
    "sap/ui/core/library",
    "sap/ui/model/Filter",
  "sap/ui/model/FilterOperator"


],
    function (PageController, MessageToast, Messaging, MessageBox, Message, coreLibrary, Filter, FilterOperator) {
        "use strict";
        return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
            onInit: function () {
               var superResult =  PageController.prototype.onInit.apply(this);

                // Messaging
                Messaging.registerObject(this.getView(), true);
                this.getView().setModel(Messaging.getMessageModel(), "message");

                // Wizard
                this.oWizard = this.byId("weighingWizard");

                // Router
                var oRouter = this.getAppComponent().getRouter();
                oRouter.getRoute("ZI_WR_WEIGHINGSESSIONMain").attachPatternMatched(this._onObjectMatched, this);

                this._enterWired = false; // guard so we wire Enter only once
                this._isFromScan = true;
                return superResult
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
                oNewContext.requestProperty(["Vbeln", "Grossweight", "Grossweightunit", "Sessionid"]).catch(function () { });
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
            _applyLtFilter: function () {
                var sVbeln = this.getView().getBindingContext().getProperty("Vbeln");
                var oVBox = this.byId("ltContainer");
                var oBinding = oVBox && oVBox.getBinding("items");
                if (!oBinding) { return; }

                if (sVbeln) {
                    var oFilter = new Filter("SalesOrder", FilterOperator.EQ, sVbeln);
                    oBinding.filter([oFilter]);          // only items of that contract
                } else {
                    oBinding.filter([]);                  // no contract -> clear filter
                }
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
                    oCtx.setProperty("TareWeight", tw);
                    oCtx.setProperty("NetWeight", gw - tw);
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
                        // advance wizard
                        this._clearContractInlineError();
                        //this._applyLtFilter();
                        this.oWizard.validateStep(this.byId("step1"));
                        
                        this.oWizard.nextStep();

                    }.bind(this)).catch(function (oError) {

                        // ✅ inline on the field instead:
                        this._setContractInlineError("Invalid Contract. Please try again.");
                        // MessageBox.error("Invalid Contract. Please try again.");
                    }.bind(this));
                }
            },
            onScanCard: function () {
                var oInput = this.byId("ipContract");
                if (oInput) {
                    oInput.focus();  // Focus the input so the scanner's "keystrokes" go here
                    //oInput.setValue("");  // Optional: Clear any existing value to avoid appending
                    this._isFromScan = true;
                    MessageToast.show("Please scan the RFID card now.");  // User prompt (shows a toast message)
                }
            },
            onContractChange: function (oEvent) {
                var oInput = oEvent.getSource();
                var sValue = oInput.getValue();
                if (sValue.length > 10) {
                    oInput.setValueState("Error");
                    oInput.setValueStateText("Contract ID cannot exceed 10 characters.");
                } else {
                    oInput.setValueState("None");
                }
            },
            onAfterRendering: function () {
                var oIp = this.byId("ipContract");
                if (oIp) { oIp.focus(); }
                this._setupEnterToNext(); // wire Enter once
            },

            onChooseLoadType: function (oEvent) {
                const sLoadType = oEvent.getSource().getBindingContext().getProperty("LoadType");
                const oSessionCtx = this.getView().getBindingContext();
                if (!oSessionCtx) { return; }

                oSessionCtx.setProperty("LoadType", sLoadType);
                this.oWizard.validateStep(this.byId("step2"));
                this.oWizard.nextStep();
            },

            onStepActivate: function () { /* optional per-step hooks */ },
            validateStep: function () { /* return true; */ },

            onCaptureWeight: function () {
                this.getView().getModel().setProperty("/Grossweight", 1000);
            },

            onCaptureFinalWeight: function () {
                var oModel = this.getView().getModel();
                var fGross = oModel.getProperty("/Grossweight");
                var fTare = oModel.getProperty("/TareWeight");
                oModel.setProperty("/NetWeight", fGross - fTare);
            },

            onSubmit: function () {
                this.getView().getModel().submitChanges({
                    success: function () { MessageToast.show("Weighing session submitted."); }
                });
            },

            _setContractInlineError: function (sText) {
                // Inline on the input
                var oInput = this.byId("ipContract");
                if (oInput) {
                    oInput.setValueState("Error");
                    oInput.setValueStateText(sText);
                    oInput.focus();
                }

                // Optional: also add a field-bound message (shows in FE message popover / keeps state on rebind)
                var oCtx = this.getView().getBindingContext();
                var oModel = this.getView().getModel();
                if (oCtx && oModel) {
                    var sTarget = oCtx.getPath() + "/Vbeln"; // property bound to the input
                    var aAll = Messaging.getMessageModel().getData() || [];
                    var aOldForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                    if (aOldForField.length) { Messaging.removeMessages(aOldForField); }
                    Messaging.addMessages(new Message({
                        message: sText,
                        type: coreLibrary.MessageType.Error,
                        target: sTarget,
                        processor: oModel
                    }));
                }
            },

            _clearContractInlineError: function () {
                var oInput = this.byId("ipContract");
                if (oInput) {
                    oInput.setValueState("None");
                    oInput.setValueStateText("");
                }
                var oCtx = this.getView().getBindingContext();
                if (oCtx) {
                    var sTarget = oCtx.getPath() + "/Vbeln";
                    var aAll = Messaging.getMessageModel().getData() || [];
                    var aForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                    if (aForField.length) { Messaging.removeMessages(aForField); }
                }
            },

            onPrintSlip: function () { },
            onWizardComplete: function () { }
        });
    });

**the View**


<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" >
 <!-- STEP 1: Identification -->
<WizardStep id="step1" title="Identification">
  <HBox id="step1HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step1Rail" width="36rem">
      <Panel id="step1Panel" class="stepPanel" expandable="false">
        <content>
          <f:Form id="step1Form" editable="true">
            <f:layout>
              <f:ResponsiveGridLayout
                id="step1FormLayout"
                labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
                adjustLabelSpan="false"/>
            </f:layout>
            <f:formContainers>
              <f:FormContainer id="step1FormContainer">
                <f:formElements>

                  <!-- Row 1 -->
                  <f:FormElement id="step1FormElementInput" label="Please Enter your Contract ID">
                    <f:fields>
                      <Input id="step1InputContract"
                             value="{Vbeln}"
                             width="100%"
                             maxLength="10"
                             required="true"
                             placeholder="Scan or enter Contract ID"
                             class="sapUiSizeCompact"
                             change=".onContractChange"/>
                    </f:fields>
                  </f:FormElement>

                  <!-- Row 2 -->
                  <f:FormElement id="step1FormElementScan" label="">
                    <f:fields>
                      <HBox id="step1ScanRow" width="100%" justifyContent="Center" class="sapUiMediumMarginTop">
                        <Button id="step1BtnScanCard"
                                width="12rem"
                                type="Emphasized"
                                icon="sap-icon://business-card"
                                text="Scan Card"
                                press="onScanCard"/>
                      </HBox>
                    </f:fields>
                  </f:FormElement>

                </f:formElements>
              </f:FormContainer>
            </f:formContainers>
          </f:Form>
        </content>
      </Panel>
    </VBox>
  </HBox>
</WizardStep>

<!-- STEP 2: Choose Load Type -->
<WizardStep id="step2" title="Choose Load Type">
  <HBox id="step2HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step2Rail" width="36rem">
      <Panel id="step2Panel" class="stepPanel" expandable="false">
        <content>
          <f:Form id="step2Form" editable="true">
            <f:layout>
              <f:ResponsiveGridLayout
                id="step2FormLayout"
                labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
                adjustLabelSpan="false"/>
            </f:layout>
            <f:formContainers>
              <f:FormContainer id="step2FormContainer">
                <f:formElements>

                  <!-- Row 1 -->
                  <f:FormElement id="step2FormElementLoadType" label="">
                    <f:fields>
                      <VBox id="step2LtContainer"
                            items="{
                              path: '/ZI_WR_SALESITEM_CONTRACTVH',
                              parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                            }">
                        <Button id="step2LtButton"
                                class="loadTypeBtn"
                                width="100%"
                                text="{= ${Material} + ' - ' + ${MaterialText} }"
                                press=".onChooseLoadType"/>
                      </VBox>
                    </f:fields>
                  </f:FormElement>

                  <!-- Row 2 -->
                  <f:FormElement id="step2FormElementSelection" label="">
                    <f:fields>
                      <Text id="step2SelectedText"
                            class="sapUiSmallMarginTop"
                            text="{= ${binding>SalesOrder} ? 'Selected: ' + ${binding>SalesOrder} : ''}"/>
                    </f:fields>
                  </f:FormElement>

                </f:formElements>
              </f:FormContainer>
            </f:formContainers>
          </f:Form>
        </content>
      </Panel>
    </VBox>
  </HBox>
</WizardStep>

<WizardStep id="step3" title="Weighing">
  <HBox id="step3HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step3Rail" width="36rem">
      <!-- TODO: weighing content -->
    </VBox>
  </HBox>
</WizardStep>

<WizardStep id="step4" title="Printing">
  <HBox id="step4HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step4Rail" width="36rem">
      <!-- TODO: weighing content -->
    </VBox>
  </HBox>
</WizardStep>


</Wizard>
</content>
</Page>
</mvc:View>

