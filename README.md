METHOD createlanf.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA(lv_contract) TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  DATA(lt_pos_rfc) TYPE zlanf_pos_in_tt.
  lt_pos_rfc = VALUE #(
    FOR p IN ls_input-_positions
    WHERE ( menge > 0 )
    ( matnr = p-matnr
      menge = p-menge
      meins = p-meins )
  ).

  DATA(lt_return) TYPE bapiret2_tab.
  DATA(lv_new_so) TYPE vbeln_va.
  DATA(lv_sysmsg)  TYPE char60.
  DATA(lv_commmsg) TYPE char60.

  CALL FUNCTION 'Z_LANF_CREATE_DMREQ_RFC'
    DESTINATION 'NONE'
    EXPORTING
      iv_contract     = lv_contract
      iv_deliverydat  = ls_input-deliverydate
      iv_customerref  = ls_input-customerref
      iv_auart        = 'ZLRA'
    IMPORTING
      ev_vbeln        = lv_new_so
    TABLES
      it_positions    = lt_pos_rfc
      et_return       = lt_return
    EXCEPTIONS
      system_failure        = 1 MESSAGE lv_sysmsg
      communication_failure = 2 MESSAGE lv_commmsg
      OTHERS                = 3.

  IF sy-subrc <> 0 OR lv_new_so IS INITIAL
     OR line_exists( lt_return[ type = 'E' ] )
     OR line_exists( lt_return[ type = 'A' ] ).

    "you can still report errors via reported-... even if response is VBELN-only
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

    IF sy-subrc <> 0.
      APPEND VALUE #( %msg = new_message(
        id       = '00' number = '001'
        v1       = |RFC failed: { COND string(
                   WHEN sy-subrc = 1 THEN lv_sysmsg
                   WHEN sy-subrc = 2 THEN lv_commmsg
                   ELSE |SUBRC { sy-subrc }| ) }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDIF.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "Return only VBELN
  result = VALUE #(
    ( %cid   = ls_key-%cid
      %param = VALUE #( vbeln = |{ lv_new_so ALPHA = OUT }| ) )
  ).

ENDMETHOD.
