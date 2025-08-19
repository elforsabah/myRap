***********************************************************************************************************************
* Identification
***********************************************************************************************************************
  METHOD identifycard.
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
     ENTITY WeighingSession
     ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_sessions).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_session>).
      " TODO: look up driver/contract by CardId (from keys[ 1 ]-%param-CardId)
      SELECT SINGLE * FROM zwr_weighsession WHERE vbeln = @<ls_session>-%param-Vbeln INTO @DATA(ls_weighsessions).


      IF sy-subrc = 0.
*        <ls_session>-step1ok = abap_true.

        APPEND VALUE #(
         %msg = new_message(
           id       = 'ZWR_WEIGHBRIGE_MESS'
           number   = '000'
           severity = if_abap_behv_message=>severity-success

           v1       = 'Contract is Valid'

         )
       ) TO reported-weighingsession.

      ENDIF.

      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession UPDATE FIELDS ( step1ok ) WITH VALUE #( ( %tky = <ls_session>-%tky step1ok = abap_true ) ).
      result = VALUE #( ( %tky = <ls_session>-%tky ) ).
    ENDLOOP.
  ENDMETHOD.


              // Invoke the action
                oActionBinding.invoke()
                    .then(function (oResult) {
                        // On success: oResult may contain the returned entity (your action returns [1] $self)
                        // Clear previous messages (replaces oMessageManager.removeAllMessages())
                        Messaging.removeAllMessages();

                        // Get messages from Messaging (replaces direct SAP__Messages or oMessageManager)
                        var aMessages = Messaging.getMessageModel().getData();
                        console.log("All Messages:", aMessages);

                        if (aMessages.length > 0) {
                            aMessages.forEach(function (oMsg) {
                                console.log("Message Type:", oMsg.getType(), "Text:", oMsg.getMessage(), "Target:", oMsg.getTarget());
                            });
                            var oErrorMsg = aMessages.find(function (oMsg) { return oMsg.getType() === "Error"; });
                            if (oErrorMsg) {
                                MessageBox.error(oErrorMsg.getMessage());
                                return;
                            }
                        } else {
                            console.log("No messages returned.");
                        }

                        this.oWizard.validateStep(this.byId(sStepId));
                       // this.oWizard.nextStep();

                        // Optional: Refresh bindings if side effects occurred
                        oContext.refresh();
                    }.bind(this))
                    .catch(function (oError) {
                        MessageToast.show("Invalid Contract. Please try again.");
                        console.error(oError);
                    }.bind(this));
            } else {
                //this.oWizard.nextStep();
            }
