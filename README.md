METHOD adjusttime.

    " 1. Loop through the keys (Service Instances)
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " 2. Check if Extension Record exists
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        FIELDS ( ServiceUUID )
        WITH VALUE #( ( ServiceUUID = <ls_key>-ServiceUUID ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
        " 3. Create Extension if missing
        DATA: lv_cid        TYPE string VALUE '$abap_cid_adj_',
              lt_ext_create TYPE TABLE FOR CREATE /PLCE/R_PDService\_ExtCustom.

        APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
        <ls_ext_create>-ServiceUUID = <ls_key>-ServiceUUID.
        <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY Service
          CREATE BY \_ExtCustom
          AUTO FILL CID SET FIELDS WITH lt_ext_create
          MAPPED DATA(lmapped_create)
          FAILED DATA(lfailed_create)
          REPORTED DATA(lreported_create).
        
        " Optional: Handle creation errors here if needed
      ENDIF.

      " 4. Prepare and Format the Value
      DATA(lv_raw_input) = <ls_key>-%param-ZeitInMin.
      DATA(lv_formatted_result) TYPE string.
      
      " Normalize input (replace comma with dot for calculation check)
      DATA(lv_calc_val) TYPE string.
      lv_calc_val = lv_raw_input.
      REPLACE ALL OCCURRENCES OF ',' IN lv_calc_val WITH '.'.
      CONDENSE lv_calc_val NO-GAPS.
      
      " Convert to number to check sign (handling potential errors if input is somehow bad)
      TRY.
          DATA(lv_number) = CONV decfloat34( lv_calc_val ).
          
          IF lv_number > 0.
             " Positive: Add '+' sign
             lv_formatted_result = |+{ lv_number } MIN|.
             
          ELSEIF lv_number < 0.
             " Negative: Minus sign is automatic
             lv_formatted_result = |{ lv_number } MIN|.
             
          ELSE.
             " Zero
             lv_formatted_result = '0 MIN'.
          ENDIF.

      CATCH cx_sy_conversion_no_number.
          " Fallback if conversion fails (should be caught by precheck, but safe to handle)
          lv_formatted_result = |{ lv_raw_input } MIN|. 
      ENDTRY.

      " 5. Update the field with the formatted string
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        UPDATE FIELDS ( zz_timeadjustment )
        WITH VALUE #( ( %key-ServiceUUID = <ls_key>-ServiceUUID
                        zz_timeadjustment = lv_formatted_result ) )
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).

      " 6. Aggregate errors
      APPEND LINES OF lt_failed_update-extcustom TO failed-extcustom.
      APPEND LINES OF lt_reported_update-extcustom TO reported-extcustom.

      " 7. Read Result to refresh UI
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
        ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).

      LOOP AT lt_service_result ASSIGNING FIELD-SYMBOL(<ls_result>).
        INSERT VALUE #( %tky   = <ls_result>-%tky
                        %param = <ls_result> ) INTO TABLE result.
      ENDLOOP.

    ENDLOOP.
    
  ENDMETHOD.




METHOD precheck_adjusttime.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

    " 1. Get Input
    DATA(lv_input_str) TYPE string.
    lv_input_str = <ls_key>-%param-ZeitInMin. 

    " 2. Remove spaces (Crucial for regex)
    CONDENSE lv_input_str NO-GAPS.

    " 3. Validate Format 
    " Regex: ^ = Start, -? = Optional Minus, [0-9]+ = Digits, ([.,]...)? = Optional decimals
    IF NOT matches( val = lv_input_str regex = '^-?[0-9]+([.,][0-9]+)?$' ).

      APPEND VALUE #( %tky = <ls_key>-%tky
                      %fail-cause = if_abap_behv=>cause-unspecific )
             TO failed-service.

      APPEND VALUE #( %tky = <ls_key>-%tky
                      %msg = new_message_with_text(
                               severity = if_abap_behv_message=>severity-error
                               text     = 'Invalid format. Use: 45, -45, 15.5' )
                    ) TO reported-service.
    ENDIF.
  ENDLOOP.

ENDMETHOD.
