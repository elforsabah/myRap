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

      " ====================================================================
      " 4. Prepare the Numeric Value (String formatting removed!)
      " ====================================================================
      DATA(lv_raw_input) = <ls_key>-%param-ZeitInMin.
      
      " Assuming your DB field is now an INT4. If it's a DEC, change TYPE i to TYPE p.
      DATA lv_numeric_result TYPE i. 

      " Normalize input (replace comma with dot if user typed "37,5")
      DATA lv_calc_val TYPE string.
      lv_calc_val = lv_raw_input.
      REPLACE ALL OCCURRENCES OF ',' IN lv_calc_val WITH '.'.
      CONDENSE lv_calc_val NO-GAPS.

      " Safely convert the input to a pure number
      TRY.
          lv_numeric_result = CONV i( lv_calc_val ).
      CATCH cx_sy_conversion_no_number.
          " Fallback if the user typed garbage text instead of a number
          lv_numeric_result = 0. 
      ENDTRY.

      " ====================================================================
      " 5. Update the field with the PURE NUMBER
      " ====================================================================
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        UPDATE FIELDS ( zz_timeadjustment )
        WITH VALUE #( ( %key-ServiceUUID = <ls_key>-ServiceUUID
                        zz_timeadjustment = lv_numeric_result ) ) " <--- Pure Number
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
