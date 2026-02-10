CLASS lhc_service IMPLEMENTATION.

  " ... (Existing Setfachbereich method) ...

  METHOD adjusttime.
    " 1. Loop through keys to process the action
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " 2. Check if ExtCustom exists for this ServiceUUID
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        FIELDS ( ServiceUUID )
        WITH VALUE #( ( ServiceUUID = <ls_key>-ServiceUUID ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
        " 3. Create Extension record if it does not exist
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

        " Add reported/failed from creation to method output if necessary
        APPEND LINES OF lfailed_create-service TO failed-service.
        APPEND LINES OF lreported_create-service TO reported-service.
      ENDIF.

      " 4. Prepare Value (Normalize 15,5 to 15.5 for DB storage if DB field is numeric)
      " Assuming the parameter is CHAR/STRING based on your precheck requirement
      DATA(lv_time_val) = <ls_key>-%param-zz_timeadjustment.
      REPLACE ALL OCCURRENCES OF ',' IN lv_time_val WITH '.'.
      CONDENSE lv_time_val NO-GAPS.

      " 5. Update the field in ExtCustom
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        UPDATE FIELDS ( zz_timeadjustment )
        WITH VALUE #( ( %key-ServiceUUID = <ls_key>-ServiceUUID
                        zz_timeadjustment = lv_time_val ) )
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).

      " Aggregate errors
      APPEND LINES OF lt_failed_update-extcustom TO failed-extcustom.
      APPEND LINES OF lt_reported_update-extcustom TO reported-extcustom.

      " 6. Read Result ($self = Service) to refresh UI
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
        ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).

      " 7. Map result
      LOOP AT lt_service_result ASSIGNING FIELD-SYMBOL(<ls_result>).
        INSERT VALUE #( %tky   = <ls_result>-%tky
                        %param = <ls_result> ) INTO TABLE result.
      ENDLOOP.

      CLEAR: lt_ext_create, lt_failed_update, lt_reported_update.
    ENDLOOP.
  ENDMETHOD.

  METHOD precheck_adjusttime.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      
      DATA(lv_input) = <ls_key>-%param-zz_timeadjustment.

      " Requirement: Allow Integers or Decimals with . or , (e.g. 15 or 15.5 or 15,5)
      " Regex: ^ = start, \d+ = numbers, ([.,]\d+)? = optional group of dot/comma and more numbers, $ = end
      FIND REGEX '^\d+([.,]\d+)?$' IN lv_input.

      IF sy-subrc <> 0.
        " Create Failed Key
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) 
               TO failed-service.

        " Create Error Message
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text( 
                                 severity = if_abap_behv_message=>severity-error
                                 text     = 'Invalid format. Use format 15, 15.5 or 15,5' ) 
                        %element = VALUE #( zz_timeadjustment = if_abap_behv=>mk-on ) 
                      ) TO reported-service.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.

ENDCLASS.
