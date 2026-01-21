CLASS lhc_lanfroot DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    TYPES tt_lanf_position_input TYPE STANDARD TABLE OF zi_lanf_position_input WITH DEFAULT KEY.
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR lanfroot RESULT result.

    METHODS read FOR READ
      IMPORTING keys FOR READ lanfroot RESULT result.

    METHODS lock FOR LOCK
      IMPORTING keys FOR LOCK lanfroot.

    METHODS attachdocument FOR MODIFY
      IMPORTING keys FOR ACTION  lanfroot~attachdocument RESULT result.

    METHODS createlanf FOR MODIFY
      IMPORTING keys FOR ACTION lanfroot~createlanf RESULT result.

    METHODS map_positions_to_contract
      IMPORTING iv_contract_vbeln TYPE vbeln_va
                it_positions      TYPE tt_lanf_position_input
      CHANGING  ct_order_items    TYPE  bapisditm_tt
                ct_order_itemsx   TYPE  bapisditmx_tt
                ct_return         TYPE bapiret2_tab.


ENDCLASS.

CLASS lhc_lanfroot IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD read.
  ENDMETHOD.

  METHOD lock.
  ENDMETHOD.

  METHOD createlanf.

    DATA:
      ls_header_in    TYPE bapisdhd1,
      ls_header_inx   TYPE bapisdhd1x,
      lt_items_in     TYPE TABLE OF bapisditm,
      lt_items_inx    TYPE TABLE OF bapisditmx,
      lt_partners     TYPE TABLE OF bapiparnr,
      lt_return       TYPE TABLE OF bapiret2,
      lv_vbeln        TYPE vbeln_va,
      ls_contract_hdr TYPE vbak.

    READ TABLE keys INTO DATA(ls_key) INDEX 1.  "// Dummy key, ignore

    " Map input (deep param)
    DATA(ls_input) = ls_key-%param .

    " Fetch contract header to get sales org, etc.
    SELECT SINGLE * FROM vbak WHERE vbeln = @ls_input-contractvbeln INTO @ls_contract_hdr.
    IF sy-subrc <> 0.
      APPEND VALUE #(
               %msg = new_message(
                       id = '00' number = '001' v1 = |Contract { ls_input-contractvbeln } not found| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.

      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
    ENDIF.

    " Prepare BAPI header
    ls_header_in-doc_type   = 'DR'.  "// Debit Memo Request
    ls_header_in-sales_org  = ls_contract_hdr-vkorg.
    ls_header_in-distr_chan = ls_contract_hdr-vtweg.
    ls_header_in-division   = ls_contract_hdr-spart.
    ls_header_in-ref_doc    = ls_input-contractvbeln.
    ls_header_in-refdoc_cat = 'G'.  "// Contract
    ls_header_in-req_date_h = ls_input-deliverydate.
    ls_header_in-purch_no_c = ls_input-customerref.
    ls_header_in-name       = ls_input-description.  "// Map to text field

    ls_header_inx-updateflag = 'I'.
    ls_header_inx-doc_type   = 'X'.
    ls_header_inx-sales_org  = 'X'.
    ls_header_inx-distr_chan = 'X'.
    ls_header_inx-division   = 'X'.
    ls_header_inx-ref_doc    = 'X'.
    ls_header_inx-refdoc_cat = 'X'.
    ls_header_inx-req_date_h = 'X'.
    ls_header_inx-purch_no_c = 'X'.
    ls_header_inx-name       = 'X'.

    " Fetch partners from contract (VBPA)
    SELECT * FROM vbpa WHERE vbeln = @ls_input-contractvbeln INTO TABLE @DATA(lt_contract_partners).
    lt_partners = CORRESPONDING #( lt_contract_partners MAPPING partn_role = parvw partn_numb = kunnr ).

    " Map positions (filter Menge > 0, map to contract POSNR)

    " Map positions (filter Menge > 0, map to contract POSNR)
    map_positions_to_contract(
      EXPORTING
      iv_contract_vbeln = ls_input-contractvbeln
      it_positions      = CORRESPONDING #( ls_input-_positions )
      CHANGING ct_order_items  = lt_items_in
               ct_order_itemsx = lt_items_inx
               ct_return       = lt_return ).



    " Call BAPI
    CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
      EXPORTING
        order_header_in  = ls_header_in
        order_header_inx = ls_header_inx
      IMPORTING
        salesdocument    = lv_vbeln
      TABLES
        return           = lt_return
        order_items_in   = lt_items_in
        order_items_inx  = lt_items_inx
        order_partners   = lt_partners.

    " Check for errors
    READ TABLE lt_return TRANSPORTING NO FIELDS WITH KEY type = 'E'.
    IF sy-subrc = 0.
      CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.
      " Map errors to reported
      LOOP AT lt_return ASSIGNING FIELD-SYMBOL(<ret>) WHERE type = 'E'.
        APPEND VALUE #(
                %msg = new_message(
                id = <ret>-id
                number = <ret>-number
                v1 = <ret>-message_v1
                v2 = <ret>-message_v2
                v3 = <ret>-message_v3
                v4 = <ret>-message_v4
                severity = if_abap_behv_message=>severity-error

                ) ) TO reported-lanfroot.
      ENDLOOP.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    ELSE.
      CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = 'X'.
    ENDIF.

    result = VALUE #(
              (
                %param = VALUE #(
                  techkey   = 'X'
                  vbeln     = lv_vbeln
                  _messages = VALUE #(
                    FOR r IN lt_return
                    ( techkey = 'X'
                      msgid   = r-id
                      msgno   = r-number
                      msgty   = r-type
                      msgv1   = r-message_v1
                      msgv2   = r-message_v2
                      msgv3   = r-message_v3
                      msgv4   = r-message_v4 )
                  )
                )
              )
            ).

  ENDMETHOD.

  METHOD attachdocument.

    DATA: ls_input         TYPE zi_attach_input,
          ls_arc_doc       TYPE toav0,
          lt_return_attach TYPE TABLE OF bapiret2.

    READ TABLE keys INTO DATA(ls_key) INDEX 1.

    ls_input = CORRESPONDING #( ls_key-%param ).

    " Prepare archive link (adjust archiv_id and ar_object per your config)
    ls_arc_doc-sap_object  = 'BUS2032'.  "Sales document
    ls_arc_doc-object_id   = ls_input-vbeln.
    ls_arc_doc-ar_object   = 'SDOCMNT'.  " Archive object type
    ls_arc_doc-arc_doc_id  = ls_input-url. " // Or extract ID from URL if needed

    CALL FUNCTION 'ARCHIV_CONNECTION_INSERT'
      EXPORTING
        archiv_id  = '01' " // Your archive ID (e.g., from OAAD)
        arc_doc_id = ls_arc_doc-arc_doc_id
        ar_date    = sy-datum
        ar_object  = ls_arc_doc-ar_object
        sap_object = ls_arc_doc-sap_object
        doc_type   = 'PDF'
        descr      = ls_input-title
        object_id  = ls_arc_doc-object_id
      TABLES
        return     = lt_return_attach.

    " Check errors
    READ TABLE lt_return_attach TRANSPORTING NO FIELDS WITH KEY type = 'E'.
    IF sy-subrc = 0.
      LOOP AT lt_return_attach ASSIGNING FIELD-SYMBOL(<ret>) WHERE type = 'E'.
        APPEND VALUE #(
         %msg = new_message(
                  id = <ret>-id
                   number = <ret>-number
                   v1 = <ret>-message_v1
                   v2 = <ret>-message_v2
                   v3 = <ret>-message_v3
                   v4 = <ret>-message_v4
                   severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      ENDLOOP.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    ENDIF.

    " Set result (VBELN + messages)
        result = VALUE #(
          (
            %param = VALUE #(
              techkey   = 'X'
              vbeln     = ls_input-vbeln
              _messages = VALUE #(
                FOR r IN lt_return_attach
                ( TechKey = 'X'
                  Msgid   = r-id
                  Msgno   = r-number
                  Msgty   = r-type
                  Msgv1   = r-message_v1
                  Msgv2   = r-message_v2
                  Msgv3   = r-message_v3
                  Msgv4   = r-message_v4 )
              )
            )
          )
        ).

  ENDMETHOD.

  METHOD map_positions_to_contract.
    " Fetch contract positions
    SELECT posnr, matnr FROM vbap WHERE vbeln = @iv_contract_vbeln INTO TABLE @DATA(lt_contract_pos).

    LOOP AT it_positions ASSIGNING FIELD-SYMBOL(<pos>) WHERE menge > 0.
      READ TABLE lt_contract_pos ASSIGNING FIELD-SYMBOL(<contr_pos>) WITH KEY matnr = <pos>-matnr.
      IF sy-subrc = 0.
        APPEND VALUE #( itm_number = <contr_pos>-posnr
                        material   = <pos>-matnr
                        target_qty = <pos>-menge
                        target_qu  = <pos>-meins
                        ref_doc    = iv_contract_vbeln
                        ref_doc_it = <contr_pos>-posnr
                        ref_doc_ca = 'G' ) TO ct_order_items.

        APPEND VALUE #( itm_number = <contr_pos>-posnr
                        updateflag = 'I'
                        material   = 'X'
                        target_qty = 'X'
                        target_qu  = 'X'
                        ref_doc    = 'X'
                        ref_doc_it = 'X'
                        ref_doc_ca = 'X' ) TO ct_order_itemsx.
      ELSE.
        APPEND VALUE #( type = 'E' id = '00' number = '001' message = |No position for MATNR { <pos>-matnr } in contract { iv_contract_vbeln }| ) TO ct_return.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

ENDCLASS.

CLASS lsc_zi_lanf_root DEFINITION INHERITING FROM cl_abap_behavior_saver.
  PROTECTED SECTION.

    METHODS finalize REDEFINITION.

    METHODS check_before_save REDEFINITION.

    METHODS save REDEFINITION.

    METHODS cleanup REDEFINITION.

    METHODS cleanup_finalize REDEFINITION.

ENDCLASS.

CLASS lsc_zi_lanf_root IMPLEMENTATION.

  METHOD finalize.
  ENDMETHOD.

  METHOD check_before_save.
  ENDMETHOD.

  METHOD save.
  ENDMETHOD.

  METHOD cleanup.
  ENDMETHOD.

  METHOD cleanup_finalize.
  ENDMETHOD.

ENDCLASS.
