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
            this._isFromScan = true;
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

            /*             // In onInit, after getting oIp:
                        oIp.attachChange(function (oEvent) {
                            var sValue = oEvent.getParameter("value");
                            if (sValue && sValue.length >= 10) {  // Vbeln is at least 10 chars;
                                this.onNextStep();
                            }
                        }.bind(this)); */

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
                       // { $PropertyPath: "Vbeln" },
                        //{ $PropertyPath: "CustomerName" }, // adjust to your real fields
                        { $PropertyPath: "Step1Ok" }       // optional boolean flag if you have it
                    ]).catch(function () { /* ignore */ });

                }.bind(this)).catch(function (oError) {
                    // keep user input; just show message
                   // console.error("Action Error:", oError);
                    /* var sMsg = this._isFromScan ? "Invalid Card Scan. Please try again." : "Invalid Contract. Please try again";
                    MessageBox.error(sMsg);
                    this._isFromScan = false; */
                  //  MessageBox.error("Invalid Contract. Please try again.");
                });
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
            if (sValue.length < 10) {
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


**My Backend**

CLASS lhc_WeighingSession DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR WeighingSession RESULT result.

    METHODS NextStep FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~NextStep RESULT result.

    METHODS Submit FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~Submit RESULT result.

    METHODS calcNet FOR DETERMINE ON MODIFY
      IMPORTING keys FOR WeighingSession~calcNet.

    METHODS validateStep1 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep1.

    METHODS validateStep2 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep2.
    METHODS identifycard FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~identifycard RESULT result.

    METHODS setloadType FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~setloadType RESULT result.

    METHODS ValidateLoadType FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~ValidateLoadType.

ENDCLASS.

CLASS lhc_WeighingSession IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD NextStep.
  ENDMETHOD.

  METHOD Submit.
  ENDMETHOD.

  METHOD calcNet.
  ENDMETHOD.

  METHOD validateStep1.
  ENDMETHOD.

  METHOD validateStep2.
  ENDMETHOD.

  METHOD ValidateLoadType.
  ENDMETHOD.

***********************************************************************************************************************
* Identification
***********************************************************************************************************************
  METHOD identifycard.
  READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
    ENTITY WeighingSession
    ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_sessions).

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_session>).

    SELECT SINGLE vbeln FROM zi_wr_weighingsession
      WHERE vbeln = @<ls_session>-%param-vbeln
      INTO @DATA(lv_vbeln).

    IF sy-subrc = 0.
      " ✓ Valid: mark step ok
      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        UPDATE FIELDS ( step1ok )
        WITH VALUE #( ( %tky = <ls_session>-%tky  step1ok = abap_true ) ).

      APPEND VALUE #(
        %tky = <ls_session>-%tky
        %msg = new_message(
                 id       = 'ZWR_WEIGHBRIGE_MESS'
                 number   = '000'
                 severity = if_abap_behv_message=>severity-success
                 v1       = 'Contract is valid' ) )
        TO reported-weighingsession.

      result = VALUE #( ( %tky = <ls_session>-%tky ) ).

    ELSE.
      " ✗ Invalid: keep step NOT ok and fail the action
      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        UPDATE FIELDS ( step1ok )
        WITH VALUE #( ( %tky = <ls_session>-%tky  step1ok = abap_false ) ).

          APPEND VALUE #(
          %tky       = <ls_session>-%tky
          %msg       = new_message(
                         id       = 'ZWR_WEIGHBRIGE_MESS'
                         number   = '001'
                         severity = if_abap_behv_message=>severity-error
                         v1       = <ls_session>-vbeln )
              %element-vbeln = if_abap_behv=>mk-on                                 " <-- bind message to Vbeln
            ) TO reported-weighingsession.

   ENDIF.
  ENDLOOP.
ENDMETHOD.


***********************************************************************************************************************
* Selection of Load type
***********************************************************************************************************************
    METHOD setloadType.

*   READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
*      ENTITY WeighingSession FIELDS ( loadtype ) WITH CORRESPONDING #( keys )
*      RESULT DATA(lt_sessions).
*    LOOP AT lt_sessions ASSIGNING FIELD-SYMBOL(<ls_session>).
*      <ls_session>-loadtype = keys[ sy-tabix ]-%param-LoadType.
*      <ls_session>-step2ok  = abap_true.
*      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
*        ENTITY WeighingSession UPDATE FIELDS ( loadtype step2ok )
*        WITH VALUE #( ( %tky = <ls_session>-%tky loadtype = <ls_session>-loadtype step2ok = abap_true ) ).
*      result = VALUE #( ( %tky = <ls_session>-%tky ) ).
*    ENDLOOP.

    ENDMETHOD.
ENDCLASS.









