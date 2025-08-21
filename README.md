  METHOD identifycard.
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
     ENTITY WeighingSession
     ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_sessions).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_session>).
      " TODO: look up driver/contract by vbeln (from keys[ 1 ]-%param-vbeln)
      SELECT SINGLE * FROM vbak WHERE vbeln = @<ls_session>-Vbeln INTO @DATA(ls_weighsessions).

      IF sy-subrc = 0.
*        <ls_session>-step1ok = abap_true.
        APPEND VALUE #(
         %msg = new_message(
           id       = 'ZWR_WEIGHBRIGE_MESS'
           number   = '000'
           severity = if_abap_behv_message=>severity-success

           v1       = 'Contract is valid'

         )
       ) TO reported-weighingsession.

      ELSE.

*        <ls_session>-step1ok = abap_true.
          APPEND VALUE #(
           %msg = new_message(
             id       = 'ZWR_WEIGHBRIGE_MESS'
             number   = '000'
             severity = if_abap_behv_message=>severity-success

             v1       = 'Contract is not valid'

           )
         ) TO reported-weighingsession.

        ENDIF.
*
        MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
          ENTITY WeighingSession UPDATE FIELDS ( step1ok ) WITH VALUE #( ( %tky = <ls_session>-%tky step1ok = abap_true ) ).
        result = VALUE #( ( %tky = <ls_session>-%tky ) ).
      ENDLOOP.
    ENDMETHOD.

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
