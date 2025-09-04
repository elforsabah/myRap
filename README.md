onWeighStep3: function () {
    var oContext = this.getView().getBindingContext();
    var oModel = this.getView().getModel();
    var oLocalModel = this.getView().getModel("local");

    if (!oContext) {
        MessageToast.show("No session context available.");
        return;
    }

    // Ensure the entity is persisted before calling the action
    oModel.submitBatch("weighingGroup").then(function () {
        console.log("Entity persisted. Invoking determineWeight...");
        // Call the determineWeight action
        var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.determineWeight(...)", oContext);
        return oAction.invoke();
    }.bind(this)).then(function (oResult) {
        console.log("determineWeight invoked successfully.");
        // Assuming the action returns the weight in a field (e.g., Grossweight)
        // Adjust the field name based on your action's result structure
        var oData = oResult.getObject();
        var fWeight = oData.Grossweight || 0; // Adjust field name as per your action
        // Update the local model
        oLocalModel.setProperty("/mainWeight", fWeight.toString());
        // Optionally refresh the context to sync other fields
        oContext.refresh();
        MessageToast.show("Weight determined: " + fWeight);
    }.bind(this)).catch(function (oError) {
        console.error("Error in determineWeight: ", oError);
        var sErrorMsg = oError.message || "Failed to determine weight.";
        MessageToast.show(sErrorMsg);
    });
}

METHOD determineWeight.
  DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_weighingsession.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
    " Read the current session data
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
      ENTITY WeighingSession
      ALL FIELDS WITH VALUE #( ( %tky = <ls_key>-%tky ) )
      RESULT DATA(lt_sessions)
      FAILED DATA(lt_failed).

    IF lt_sessions IS NOT INITIAL.
      DATA(ls_session) = lt_sessions[ 1 ].
      " Example: Assume weight is calculated or fetched (e.g., from a device or table)
      DATA(lv_weight) = ls_session-Grossweight. " Adjust logic as needed
      IF lv_weight IS INITIAL.
        " Simulate fetching weight (replace with actual logic, e.g., from a device or table)
        lv_weight = 1000.00. " Example value in kg
        " Update the entity with the weight
        MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
          ENTITY WeighingSession
          UPDATE FIELDS ( Grossweight )
          WITH VALUE #( ( %tky = <ls_key>-%tky
                          Grossweight = lv_weight ) )
          FAILED DATA(lt_update_failed)
          REPORTED DATA(lt_reported).
      ENDIF.

      " Report success
      APPEND VALUE #(
        %tky = <ls_key>-%tky
        %msg = new_message(
                 id = 'ZWR_WEIGHBRIGE_MESS'
                 number = '002'
                 severity = if_abap_behv_message=>severity-success
                 v1 = |Weight { lv_weight } determined| )
      ) TO reported-weighingsession.

      " Collect for result
      APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
    ELSE.
      " Report error if session not found
      APPEND VALUE #(
        %tky = <ls_key>-%tky
        %msg = new_message(
                 id = 'ZWR_WEIGHBRIGE_MESS'
                 number = '003'
                 severity = if_abap_behv_message=>severity-error
                 v1 = 'Session not found' )
      ) TO reported-weighingsession.
      APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-not_found ) TO failed-weighingsession.
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
