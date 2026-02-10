METHOD adjusttime.

    " 1. Read TotalDuration via the Association '_Statistic'
    "    (The field is not in 'Service', so we must follow the association)
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service BY \_Statistic
      FIELDS ( TotalDuration TotalDurationUnit )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_statistics)
      FAILED DATA(lt_failed_read).

    " 2. Loop through the INPUT keys (the user's request)
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " 3. Find the corresponding Statistic record
      READ TABLE lt_statistics ASSIGNING FIELD-SYMBOL(<ls_stat>)
           WITH KEY ServiceUUID = <ls_key>-ServiceUUID.
      
      IF sy-subrc <> 0.
        " Handle error: Statistic record not found
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text( severity = if_abap_behv_message=>severity-error 
                                                      text = 'Statistic record not found.' ) 
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      " 4. Parse the Input 
      "    FIX: Ensure the parameter name matches your Abstract Entity (likely ZeitInMin)
      DATA(lv_input_str) = CONV string( <ls_key>-%param-ZeitInMin ). 
      REPLACE ALL OCCURRENCES OF ',' IN lv_input_str WITH '.'.
      CONDENSE lv_input_str NO-GAPS.

      DATA(lv_adjustment_min) TYPE decfloat34.
      TRY.
          lv_adjustment_min = lv_input_str.
        CATCH cx_sy_conversion_no_number.
          CONTINUE. 
      ENDTRY.

      " 5. Convert Current Duration to Minutes
      DATA(lv_current_min) TYPE decfloat34.

      IF <ls_stat>-TotalDurationUnit = 'H' OR <ls_stat>-TotalDurationUnit = 'HR'.
        lv_current_min = <ls_stat>-TotalDuration * 60.
      ELSEIF <ls_stat>-TotalDurationUnit = 'MIN'.
        lv_current_min = <ls_stat>-TotalDuration.
      ELSE.
        " Default to Hours if unit is empty
        lv_current_min = <ls_stat>-TotalDuration * 60.
      ENDIF.

      " 6. Calculate New Total
      DATA(lv_new_total_min) = lv_current_min + lv_adjustment_min.
      IF lv_new_total_min < 0. 
        lv_new_total_min = 0. 
      ENDIF.

      " 7. Convert back to Hours (assuming DB stores Hours)
      DATA(lv_new_total_h) TYPE p LENGTH 16 DECIMALS 3.
      lv_new_total_h = lv_new_total_min / 60.

      " 8. Update the Statistic Entity
      "    IMPORTANT: You must modify the entity that actually holds the field.
      "    Check your BDEF to confirm the entity alias (e.g., ServiceStatistic or Statistic).
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ServiceStatistic 
        UPDATE FIELDS ( TotalDuration )
        WITH VALUE #( ( %tky = <ls_stat>-%tky  " Use the key from the statistic record
                        TotalDuration = lv_new_total_h ) )
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).

      " Aggregate errors
      APPEND LINES OF lt_failed_update-servicestatistic TO failed-service. 
      APPEND LINES OF lt_reported_update-servicestatistic TO reported-service.

    ENDLOOP.

    " 9. Read Result ($self) to refresh UI
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_result).

    result = VALUE #( FOR service IN lt_result ( %tky = service-%tky %param = service ) ).

  ENDMETHOD.
