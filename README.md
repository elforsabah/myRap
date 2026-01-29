METHOD createlanf.

  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.

  "1) Input
  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA(lv_contract) TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  "2) Contract header
  SELECT SINGLE * FROM vbak
    WHERE vbeln = @lv_contract
    INTO @DATA(ls_vbak).

  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message(
      id       = '00' number = '001'
      v1       = |Contract { lv_contract } not found|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "3) Contract items
  TYPES: BEGIN OF ty_ctr_item,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
           werks TYPE werks_d,
           vrkme TYPE vrkme,
           pstyv TYPE pstyv,
           netpr TYPE netpr,
           kpein TYPE kpein,
           kmein TYPE kmein,
         END OF ty_ctr_item.

  DATA lt_ctr_items TYPE STANDARD TABLE OF ty_ctr_item WITH EMPTY KEY.

  SELECT posnr, matnr, werks, vrkme, pstyv, netpr, kpein, kmein
    FROM vbap
    WHERE vbeln = @lv_contract
    INTO TABLE @lt_ctr_items.

  IF lt_ctr_items IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00' number = '001'
      v1       = |No items found in contract { lv_contract }|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "4) Aggregate input positions by POSNR (we derive POSNR by MATNR mapping)
  TYPES: BEGIN OF ty_agg,
           posnr TYPE posnr_va,
           qty   TYPE kwmeng,
         END OF ty_agg.

  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
  FIELD-SYMBOLS <agg> TYPE ty_agg.

  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.

    DATA(lv_matnr) TYPE matnr.
    lv_matnr = ls_pos-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    "Map MA
