CLASS lhc_WeighingSession DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR WeighingSession RESULT result.

    METHODS identifyCard FOR Modify
      IMPORTING keys FOR ACTION WeighingSession~identifyCard RESULT result.

    METHODS NextStep FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~NextStep RESULT result.

    METHODS setloadType FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~setloadType RESULT result.

    METHODS Submit FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~Submit RESULT result.

    METHODS calcNet FOR DETERMINE ON MODIFY
      IMPORTING keys FOR WeighingSession~calcNet.

    METHODS ValidateLoadType FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~ValidateLoadType.

    METHODS validateStep1 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep1.

    METHODS validateStep2 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep2.

ENDCLASS.

CLASS lhc_WeighingSession IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD identifyCard.

  DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_weighingsession.
  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
    " Validate contract existence (adjust table name if not VBAK)
    SELECT SINGLE @abap_true FROM vbak
      WHERE vbeln = @<ls_key>-%param-vbeln
      INTO @DATA(lv_exists).
    IF sy-subrc = 0.
      " ✓ Valid: report success message only (no update)
      APPEND VALUE #(
        %tky = <ls_key>-%tky
        %msg = new_message(
                 id = 'ZWR_WEIGHBRIGE_MESS'
                 number = '000'
                 severity = if_abap_behv_message=>severity-success
                 v1 = 'Contract is valid' ) )
        TO reported-weighingsession.

      " Collect for result
      APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
    ELSE.
      " ✗ Invalid: report bound error and fail the action
      APPEND VALUE #(
        %tky = <ls_key>-%tky
        %msg = new_message(
                 id = 'ZWR_WEIGHBRIGE_MESS'
                 number = '001'
                 severity = if_abap_behv_message=>severity-error
                 v1 = <ls_key>-%param-vbeln )
        %element-vbeln = if_abap_behv=>mk-on  " Bind message to Vbeln field
      ) TO reported-weighingsession.

      " Fail the function (set failed to prevent result for this key)
      APPEND VALUE #(
        %tky = <ls_key>-%tky
        %fail-cause = if_abap_behv=>cause-unspecific
      ) TO failed-weighingsession.
    ENDIF.
  ENDLOOP.

  " Read and return full data for successful instances
  IF lt_success_keys IS NOT INITIAL.
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
      ENTITY WeighingSession
      ALL FIELDS WITH lt_success_keys
      RESULT DATA(lt_result)
      FAILED DATA(lt_read_failed).
    result = CORRESPONDING #( lt_result ).
  ENDIF.
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

  METHOD NextStep.
  ENDMETHOD.

  METHOD Submit.
  ENDMETHOD.

  METHOD calcNet.
  ENDMETHOD.

  METHOD ValidateLoadType.
  ENDMETHOD.

  METHOD validateStep1.
  ENDMETHOD.

  METHOD validateStep2.
  ENDMETHOD.

ENDCLASS.




managed implementation in class zbp_i_wr_weighingsession unique;
strict( 2 );

with draft;

define behavior for ZI_WR_WEIGHINGSESSION alias WeighingSession
persistent table zwr_weighsession
draft table ZWR_WEIGHB_DD
lock master
total etag Grossweight
authorization master ( instance )
etag master Grossweight

{
//  Keys must be readonly in strict draft
  field (readonly) Sessionid;  // <-- use your exact CDS element names
  field (numbering: managed) Sessionid;
create;
update;
delete;

draft action Edit;
draft action Activate;
draft action Discard;
draft action Resume;                 // <-- required
draft determine action Prepare;      //<-- required

action NextStep result [1] $self; // server validates & increments Step
action Submit result [1] $self; // final checks; printing trigger optional

action identifyCard parameter ZAE_WR_WEIGHINGSESSION result [1] $self ;
action setloadType parameter ZAE_WR_WEIGHINGSESSION  result [1] $self;

//action identifyCard  result [1] $self;

validation validateStep1 on save { field Vbeln, Sessionid; } // Identification
validation validateStep2 on save { field Loadtype; } // Load type
validation ValidateLoadType on save { field LoadType; } // Calls a method to check against VH

determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math


}


sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",
    "sap/m/MessageBox",
    "sap/ui/core/message/Message",
    "sap/ui/core/library",
    "sap/ui/model/Filter",
    "sap/ui/model/FilterOperator",
    "sap/m/Button"
],
    function (PageController, MessageToast, Messaging, MessageBox, Message, coreLibrary, Filter, FilterOperator, Button) {
        "use strict";
        return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
            formatter: {
                concatMaterialText: function (sMaterial, sMaterialText) {
                    return (sMaterial || '') + ' - ' + (sMaterialText || '');
                }
            },

            onInit: function () {
                PageController.prototype.onInit.apply(this);

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

            },

            _onObjectMatched: function () {
                var oModel = this.getView().getModel();
                var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION");
                var oNewContext = oListBinding.create({}); // No initial data; let backend manage Sessionid (UUID)
                this.getView().setBindingContext(oNewContext);
                oNewContext.requestProperty(["Vbeln", "Grossweight", "Grossweightunit", "Sessionid"]).catch(function () { });
            },

            // === ENTER wiring (ADDED) ===
            _setupEnterToNext: function () {
                if (this._enterWired) { return; }
                this._enterWired = true;

                // Step 1: Enter on Contract input -> reuse existing onNextStep
                var oIp = this.byId("step1InputContract");
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

            onScanCard: function () {
                var oInput = this.byId("step1InputContract");
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
                var oIp = this.byId("step1InputContract");
                if (oIp) { oIp.focus(); }
                this._setupEnterToNext(); // wire Enter once
            },

            onChooseLoadType: function (oEvent) {
                const sLoadType = oEvent.getSource().getBindingContext().getProperty("Material");
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
                var oInput = this.byId("step1InputContract");
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
                var oInput = this.byId("step1InputContract");
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


