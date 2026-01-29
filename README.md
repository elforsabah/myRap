METHOD createlanf.

  "=============================================================
  " OPTION B: BAPI_SALESDOCUMENT_COPY + BAPI_SALESORDER_CHANGE
  "=============================================================

  CONSTANTS gc_rej TYPE abgru VALUE 'Z0'.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA(lv_contract) = CONV vbeln_va( |{ ls_input-contractvbeln ALPHA = IN }| ).

  "---- Validate contract exists
  SELECT SINGLE vbeln
    FROM vbak
    WHERE vbeln = @lv_contract
    INTO @DATA(lv_dummy).

  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message(
      id='00' number='001'
      v1 = |Contract { lv_contract } not found|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "---- Copy contract to new sales order
  DATA: lt_return TYPE bapiret2_tab,
        lv_new_so TYPE vbeln_va.

  CALL FUNCTION 'BAPI_SALESDOCUMENT_COPY'
    EXPORTING
      salesdocument    = lv_contract
      documenttype     = 'ZLRA'
      testrun          = space
    IMPORTING
      salesdocument_ex = lv_new_so
    TABLES
      return           = lt_return.

  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return INTO DATA(ls_e1) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message_with_text(
        text = ls_e1-message
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "---- Contract positions: posnr/matnr
  SELECT posnr, matnr
    FROM vbap
    WHERE vbeln = @lv_contract
    INTO TABLE @DATA(lt_ctr_pos).

  "---- Build wanted quantities
  TYPES: BEGIN OF ty_want,
           posnr TYPE posnr_va,
           qty   TYPE kwmeng,
           unit  TYPE vrkme,
         END OF ty_want.
  DATA lt_want TYPE HASHED TABLE OF ty_want WITH UNIQUE KEY posnr.

  " FIX: Declare lv_matnr OUTSIDE the loop to avoid 'is unknown' or redeclaration errors
  DATA lv_matnr TYPE matnr. 

  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.

    lv_matnr = ls_pos-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING
        input  = lv_matnr
      IMPORTING
        output = lv_matnr.

    " FIX: Check the table you actually selected (lt_ctr_pos)
    READ TABLE lt_ctr_pos INTO DATA(ls_ctr) WITH KEY matnr = lv_matnr.
    IF sy-subrc <> 0.
       " Error handling...
       CONTINUE.
    ENDIF.

    INSERT VALUE #(
      posnr = ls_ctr-posnr
      qty   = ls_pos-menge
      unit  = ls_pos-meins ) INTO TABLE lt_want.

  ENDLOOP.

  " ... Rest of the BAPI_SALESORDER_CHANGE logic ...

ENDMETHOD.
