METHOD adjusttime.

    " 1. Read the current TotalDuration from the Service entity
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      FIELDS ( TotalDuration TotalDurationUnit )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

      " 2. Get the input parameter for this specific key
      READ TABLE keys WITH KEY %tky = <ls_service>-%tky ASSIGNING FIELD-SYMBOL(<ls_key>).
      CHECK sy-subrc = 0.

      " 3. Parse the Input (ZeitInMinute)
      " We handle the string conversion here to support inputs like "-15" or "15,5"
      DATA(lv_input_str) = CONV string( <ls_key>-%param-ZeitInMinute ).
      REPLACE ALL OCCURRENCES OF ',' IN lv_input_str WITH '.'.
      CONDENSE lv_input_str NO-GAPS.

      DATA(lv_adjustment_min) TYPE decfloat34.
      TRY.
          lv_adjustment_min = lv_input_str.
        CATCH cx_sy_conversion_no_number.
          " Skip invalid numbers (should be caught by precheck, but safe to ignore here)
          CONTINUE.
      ENDTRY.

      " 4. Convert Current Duration (Hours) to Minutes
      DATA(lv_current_min) TYPE decfloat34.

      " Check Unit: If 'H' or 'HR', multiply by 60. If 'MIN', take as is.
      IF <ls_service>-TotalDurationUnit = 'H' OR <ls_service>-TotalDurationUnit = 'HR'.
        lv_current_min = <ls_service>-TotalDuration * 60.
      ELSEIF <ls_service>-TotalDurationUnit = 'MIN'.
        lv_current_min = <ls_service>-TotalDuration.
      ELSE.
        " Default Fallback: Assume Hours (based on your screenshot 0,283 H)
        lv_current_min = <ls_service>-TotalDuration * 60.
      ENDIF.

      " 5. Calculate New Total in Minutes
      DATA(lv_new_total_min) = lv_current_min + lv_adjustment_min.

      " Optional: Prevent negative duration
      IF lv_new_total_min < 0.
        lv_new_total_min = 0.
      ENDIF.

      " 6. Convert back to Hours for storage
      " (Assuming the field TotalDuration is stored in Hours)
      DATA(lv_new_total_h) TYPE p LENGTH 16 DECIMALS 3. " Adjust decimals to match your DB field
      lv_new_total_h = lv_new_total_min / 60.

      " 7. Update TotalDuration in Service Entity
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
        UPDATE FIELDS ( TotalDuration )
        WITH VALUE #( ( %tky = <ls_service>-%tky
                        TotalDuration = lv_new_total_h ) )
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).

      " Aggregate errors
      APPEND LINES OF lt_failed_update-service TO failed-service.
      APPEND LINES OF lt_reported_update-service TO reported-service.

    ENDLOOP.

    " 8. Read Result ($self) to refresh UI
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_result).

    result = VALUE #( FOR service IN lt_result ( %tky = service-%tky %param = service ) ).

  ENDMETHOD.
