METHOD precheck_adjusttime.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      
      " ---------------------------------------------------------------------
      " 1. Parse Input & Format Validation
      " ---------------------------------------------------------------------
      DATA(lv_input_str) = CONV string( <ls_key>-%param-ZeitInMin ).
      REPLACE ALL OCCURRENCES OF ',' IN lv_input_str WITH '.'.
      CONDENSE lv_input_str NO-GAPS.

      " Regex: Allow integers or decimals, optional minus sign
      IF NOT matches( val = lv_input_str regex = '^-?[0-9]+([.,][0-9]+)?$' ).
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text( severity = if_abap_behv_message=>severity-error
                                                      text     = 'Invalid format. Use: 45, -45, 15.5' )
                      ) TO reported-service.
        CONTINUE. 
      ENDIF.

      " Convert to Number for calculations
      DATA(lv_adj_min) TYPE decfloat34.
      TRY.
          lv_adj_min = lv_input_str.
        CATCH cx_sy_conversion_no_number.
          CONTINUE.
      ENDTRY.

      " ---------------------------------------------------------------------
      " 2. Limit Check: Max +/- 600 Minutes (10 Hours)
      " ---------------------------------------------------------------------
      IF lv_adj_min > 600 OR lv_adj_min < -600.
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text( severity = if_abap_behv_message=>severity-error
                                                      text     = 'Input limited to +/- 600 Minutes (10 Hours).' )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      " ---------------------------------------------------------------------
      " 3. Logical Check: Resulting Duration cannot be < 0
      " ---------------------------------------------------------------------
      
      " Read current duration from Statistics View (Bypass BDEF)
      SELECT SINGLE TotalDuration, TotalDurationUnit
        FROM /PLCE/R_PDServiceStatistic
        WHERE ServiceUUID = @<ls_key>-ServiceUUID
        INTO @DATA(ls_stat).

      IF sy-subrc = 0.
        " Convert Current Duration to Minutes
        DATA(lv_current_min) TYPE decfloat34.
        
        CASE ls_stat-TotalDurationUnit.
          WHEN 'H' OR 'HR'.
            lv_current_min = ls_stat-TotalDuration * 60.
          WHEN 'MIN' OR 'M'.
            lv_current_min = ls_stat-TotalDuration.
          WHEN OTHERS.
            lv_current_min = ls_stat-TotalDuration * 60. " Default assumption
        ENDCASE.

        " Check if subtraction is too large
        IF ( lv_current_min + lv_adj_min ) < 0.
           APPEND VALUE #( %tky = <ls_key>-%tky
                           %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
           
           " Create a helpful message showing the max possible reduction
           DATA(lv_msg_text) = |Adjustment too large. Max reduction allowed: -{ lv_current_min } MIN|.
           
           APPEND VALUE #( %tky = <ls_key>-%tky
                           %msg = new_message_with_text( severity = if_abap_behv_message=>severity-error
                                                         text     = lv_msg_text )
                         ) TO reported-service.
        ENDIF.
      ENDIF.

    ENDLOOP.

  ENDMETHOD.
