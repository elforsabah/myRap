METHOD createlanf.

  "Incoming call line (static action => usually exactly one)
  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  "Use incoming TechKey for response key (important!)
  DATA(lv_tk) TYPE abap_char1.
  lv_tk = ls_input-techkey.
  IF lv_tk IS INITIAL.
    lv_tk = '1'.
  ENDIF.

  DATA(lv_contract) TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  "Map deep positions (CDS) -> DDIC table type used by RFC FM
  DATA(lt_pos_rfc) TYPE zlanf_pos_in_tt.
  lt_pos_rfc = VALUE #(
    FOR p IN ls_input-_positions
    WHERE ( menge > 0 )
    ( techkey = lv_tk
      matnr   = p-matnr
      menge   = p-menge
      meins   = p-meins )
  ).

  IF lt_pos_rfc IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |No positions with Menge > 0|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  DATA(lt_return) TYPE bapiret2_tab.
  DATA(lv_new_so) TYPE vbeln_va.
  DATA(lv_sysmsg)  TYPE char60.
  DATA(lv_commmsg) TYPE char60.

  "Loopback RFC => separate LUW => avoids RAP update-task dump
  CALL FUNCTION 'Z_LANF_CREATE_DMREQ_RFC'
    DESTINATION 'NONE'
    EXPORTING
      iv_contract     = lv_contract
      iv_deliverydat  = ls_input-deliverydate     "<<< match your FM name (IV_DELIVERYDAT)
      iv_customerref  = ls_input-customerref
      iv_auart        = 'ZLRA'
    IMPORTING
      ev_vbeln        = lv_new_so
    TABLES
      it_positions    = lt_pos_rfc                "<<< IMPORTANT: mapped DDIC table
      et_return       = lt_return
    EXCEPTIONS
      system_failure        = 1 MESSAGE lv_sysmsg
      communication_failure = 2 MESSAGE lv_commmsg
      OTHERS                = 3.

  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |RFC call failed: { COND string(
                   WHEN sy-subrc = 1 THEN lv_sysmsg
                   WHEN sy-subrc = 2 THEN lv_commmsg
                   ELSE |SUBRC { sy-subrc }| ) }|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "BAPI errors?
  IF lv_new_so IS INITIAL
     OR line_exists( lt_return[ type = 'E' ] )
     OR line_exists( lt_return[ type = 'A' ] ).

    LOOP AT lt_return INTO DATA(ls_r) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message(
        id       = ls_r-id
        number   = ls_r-number
        v1       = ls_r-message_v1
        v2       = ls_r-message_v2
        v3       = ls_r-message_v3
        v4       = ls_r-message_v4
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "Return value in response (THIS is what fixes value:[])
  CLEAR result.

  APPEND VALUE #(
    %cid   = ls_key-%cid
    %param = VALUE #(
      techkey = lv_tk
      vbeln   = |{ lv_new_so ALPHA = OUT }|
      _messages = VALUE #(
        "Return BAPI messages (optional)
        FOR r IN lt_return
        ( techkey = lv_tk
          msgid   = r-id
          msgno   = r-number
          msgty   = r-type
          msgv1   = r-message_v1
          msgv2   = r-message_v2
          msgv3   = r-message_v3
          msgv4   = r-message_v4 )
      )
    )
  ) TO result.

ENDMETHOD.
