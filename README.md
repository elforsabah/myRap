<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" >

      <!-- STEP 1: Identification -->
      <WizardStep id="step1" title="Identification" validated="{= ${viewModel>/step1_ok} ? true : false }">
        <HBox id="step1HBox" width="80%" justifyContent="Center">
          <Panel id="step1Panel" class="stepPanel" expandable="false"> <!-- headerText="1. Identification"> -->
            <content>
              <f:Form id="step1Form" editable="true">
                <f:layout><f:ResponsiveGridLayout id="tgrid"/></f:layout>
                <f:formContainers>
                  <f:FormContainer id="step1FormContainer">
                    <f:formElements>
                      <f:FormElement id="step1FormElementContract" label="Please Enter your Contract ID">
                        <f:fields>
                          <Input id="ipContract" value="{Vbeln}" width="100%" maxLength="15"/>  
                          <!-- <macros:Field id="ipContract" contextPath="/ZI_WR_WEIGHINGSESSION" metaPath="Vbeln" readOnly="false"  /> -->
                          <HBox id="step1ScanRow" width="100%" justifyContent="SpaceBetween" class="sapUiMediumMarginTop">
                            <Button id="btnScanCard"
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
              <HBox id="step1Buttons" justifyContent="Start" width="80%" class="sapUiMediumMarginTop">
                <Button id="btnStep1Next" text="Next" type="Emphasized" press="onNextStep" visible="false"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 2: Choose Load Type -->
      <WizardStep id="step2" title="Choose Load Type" validated="{= ${viewModel>/step2_ok} ? true : false }">
        <HBox id="step2HBox" width="80%" justifyContent="Center">
          <Panel id="step2Panel" class="stepPanel" expandable="false"> <!-- headerText="2. Choose Load Type"> -->
            <content>
              <VBox id="step2VBox">
                <!-- <Text id="step2Instruction" text="Position vehicle on weighbridge" class="sapUiSmallMarginBottom"/> -->

                <!-- Big tappable buttons from VH entity -->
                <VBox id="ltContainer"
                      items="{
                        path: '/ZI_WR_LOADTYPEVH',
                        parameters: { $select: 'LoadType,LoadTypeText', $orderby: 'LoadType' }
                      }">
                  <Button
                    id="ltButton"
                    class="loadTypeBtn"
                    width="100%"
                    text="{= ${LoadType} + ' - ' + ${LoadTypeText} }"
                    press=".onChooseLoadType"/>
                </VBox>

                <!-- Show selection -->
                <Text id="step2SelectedText" class="sapUiSmallMarginTop"
                      text="{= ${binding>LoadType} ? 'Selected: ' + ${binding>LoadType} : ''}"/>

                <HBox id="step2Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiMediumMarginTop">
                  <Button id="btnStep2Back" text="Back" press=".onBackToStep1"/>
                  <Button id="btnStep2Next" text="Next" type="Emphasized" press="onNextStep"/>
                </HBox>
              </VBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 3: Weighing -->
      <WizardStep id="step3" title="Weighing" validated="{= ${viewModel>/step3_ok} ? true : false }">
        <HBox id="step3HBox" width="80%" justifyContent="Center">
          <Panel id="step3Panel" class="stepPanel" expandable="false"> <!-- headerText="3. Weighing"> -->
            <content>
              <f:Form id="step3Form" editable="true">
                <f:layout><f:ResponsiveGridLayout id="tgrid1"/></f:layout>
                <f:formContainers>
                  <f:FormContainer id="step3FormContainer">
                    <f:formElements>
                      <f:FormElement id="step3FormElementGross" label="Gross Weight">
                        <f:fields>
                          <Input id="ipGrossWeight" value="{binding>GrossWeight}" width="100%" type="Number"/>
                        </f:fields>
                      </f:FormElement>
                      <f:FormElement id="step3FormElementTare" label="Tare Weight">
                        <f:fields>
                          <Input id="ipTareWeight" value="{binding>TareWeight}" width="100%" type="Number"/>
                        </f:fields>
                      </f:FormElement>
                      <f:FormElement id="step3FormElementNet" label="Net Weight">
                        <f:fields>
                          <Input id="ipNetWeight" value="{binding>NetWeight}" width="100%" type="Number" editable="false"/>
                        </f:fields>
                      </f:FormElement>
                    </f:formElements>
                  </f:FormContainer>
                </f:formContainers>
              </f:Form>

              <HBox id="step3Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiMediumMarginTop">
                <Button id="btnStep3Back" text="Back" press=".onBackToStep2"/>
                <Button id="btnFinish" text="Finish" type="Accept" press="onNextStep"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>
 <!-- STEP 4: Complete and Print -->
      <WizardStep id="step4" title="Complete and Print" validated="false">
        <HBox id="step4HBox" width="80%" justifyContent="Center">
          <Panel id="step4Panel" class="stepPanel" expandable="false"> <!-- headerText="4. Complete and Print"> -->
            <content>
              <VBox id="step4VBox" class="sapUiSmallMargin">
                <!-- Session Summary -->
                <Text id="step4SummaryText"
                      text="Session Summary: {path: 'Sessionid', targetType: 'any'}"
                      class="sapUiSmallMarginBottom"/>

                <!-- Action buttons -->
                <HBox id="step4Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiMediumMarginTop">
                  <Button id="btnStep4Print" text="Print Slip" type="Emphasized" press=".onPrintSlip"/>
                  <Button id="btnStep4Submit" text="Submit" type="Accept" press="onNextStep"/>
                </HBox>
              </VBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>
</Wizard>
</content>
</Page>
</mvc:View>




**The controller**

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
                            title: "Sucess"
                        });
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
        validateStep: function () { return true; },

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

        onPrintSlip: function () { },
        onWizardComplete: function () { }
    });
});





