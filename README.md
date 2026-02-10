METHOD adjusttime.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " 1. Read Current Duration using SQL (Bypasses BDEF restriction)
      "    Using the CDS View name for Statistic found in your View Definition
      SELECT SINGLE TotalDuration, TotalDurationUnit
        FROM /PLCE/R_PDServiceStatistic
        WHERE ServiceUUID = @<ls_key>-ServiceUUID
        INTO @DATA(ls_stat).

      " 2. Parse Input (Handle comma, negative numbers, etc.)
      "    Ensure the parameter name matches your Abstract Entity (ZeitInMin vs ZeitInMinute)
      DATA(lv_input_str) = CONV string( <ls_key>-%param-ZeitInMin ).
      REPLACE ALL OCCURRENCES OF ',' IN lv_input_str WITH '.'.
      CONDENSE lv_input_str NO-GAPS.

      DATA(lv_adjustment_min) TYPE decfloat34.
      TRY.
          lv_adjustment_min = lv_input_str.
        CATCH cx_sy_conversion_no_number.
          " Invalid number logic here if needed
      ENDTRY.

      " 3. Convert Current Duration to Minutes
      DATA(lv_current_min) TYPE decfloat34.
      
      IF ls_stat-TotalDurationUnit = 'H' OR ls_stat-TotalDurationUnit = 'HR'.
        lv_current_min = ls_stat-TotalDuration * 60.
      ELSEIF ls_stat-TotalDurationUnit = 'MIN'.
        lv_current_min = ls_stat-TotalDuration.
      ELSE.
        " Default to Hours if unit is missing
        lv_current_min = ls_stat-TotalDuration * 60.
      ENDIF.

      " 4. Calculate New Total (in Minutes)
      DATA(lv_new_total_min) = lv_current_min + lv_adjustment_min.
      IF lv_new_total_min < 0. 
        lv_new_total_min = 0. 
      ENDIF.

      " 5. Format for Display (e.g. "+ 45 MIN")
      "    We save this to the Custom Field because we cannot update Standard Statistic
      DATA(lv_display_val) TYPE string.
      
      " Logic: If you want to show the ADJUSTMENT (+45) or the NEW TOTAL (120)
      " Assuming you want to show the adjustment as per your screenshot:
      IF lv_adjustment_min > 0.
         lv_display_val = |+{ lv_adjustment_min } MIN|.
      ELSE.
         lv_display_val = |{ lv_adjustment_min } MIN|.
      ENDIF.
      
      " 6. Check/Create Extension Record
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom FIELDS ( ServiceUUID )
        WITH VALUE #( ( ServiceUUID = <ls_key>-ServiceUUID ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
         MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
           ENTITY Service CREATE BY \_ExtCustom
           AUTO FILL CID WITH VALUE #( ( ServiceUUID = <ls_key>-ServiceUUID ) )
           FAILED DATA(lt_failed_create).
      ENDIF.

      " 7. Update the Custom Field
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        UPDATE FIELDS ( zz_timeadjustment )
        WITH VALUE #( ( %key-ServiceUUID = <ls_key>-ServiceUUID
                        zz_timeadjustment = lv_display_val ) )
        FAILED DATA(lt_failed_upd)
        REPORTED DATA(lt_reported_upd).

      " 8. Refresh UI
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_result).

      result = VALUE #( FOR service IN lt_result ( %tky = service-%tky %param = service ) ).

    ENDLOOP.

  ENDMETHOD.
