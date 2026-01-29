FUNCTION Z_LANF_CREATE_DMREQ_RFC.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(IV_CONTRACT) TYPE  VBELN_VA
*"     VALUE(IV_DELIVERYDAT) TYPE  DATS
*"     VALUE(IV_CUSTOMERREF) TYPE  BSTKD
*"     VALUE(IV_AUART) TYPE  AUART DEFAULT 'ZLRA'
*"  EXPORTING
*"     VALUE(EV_VBELN) TYPE  VBELN_VA
*"  TABLES
*"      IT_POSITIONS TYPE  ZLANF_POS_IN_TT
*"      ET_RETURN TYPE  BAPIRET2_TAB
*"----------------------------------------------------------------------

CLEAR: ev_vbeln, et_return[].

  "Run SD in German (your system has TSPAT only DE)
  DATA(lv_old_langu) = sy-langu.
  SET LOCALE LANGUAGE 'D'.

  "1) Contract header
  SELECT SINGLE * FROM vbak
    WHERE vbeln = @iv_contract
    INTO @DATA(ls_vbak).

  IF sy-subrc <> 0.
    APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
      message = |Contract { iv_contract } not found| ) TO et_return.
    SET LOCALE LANGUAGE lv_old_langu.
    RETURN.
  ENDIF.

  "2) Contract items for mapping (POSNR/MATNR/UoM)
  TYPES: BEGIN OF ty_ctr,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
           vrkme TYPE vrkme,
         END OF ty_ctr.
  DATA lt_ctr TYPE STANDARD TABLE OF ty_ctr WITH EMPTY KEY.

  SELECT posnr, matnr, vrkme
    FROM vbap
    WHERE vbeln = @iv_contract
    INTO TABLE @lt_ctr.

  IF lt_ctr IS INITIAL.
    APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
      message = |No items in contract { iv_contract }| ) TO et_return.
    SET LOCALE LANGUAGE lv_old_langu.
    RETURN.
  ENDIF.

  "3) Aggregate positions by contract POSNR (prevents duplicates / M_/011)
  TYPES: BEGIN OF ty_agg,
           posnr TYPE posnr_va,
           qty   TYPE kwmeng,
           meins TYPE meins,
           matnr TYPE matnr,
         END OF ty_agg.
  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
  FIELD-SYMBOLS <agg> TYPE ty_agg.

  LOOP AT it_positions ASSIGNING FIELD-SYMBOL(<p>) WHERE menge > 0.

    DATA lv_matnr TYPE matnr.
    lv_matnr = <p>-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    "Detect ambiguity: same material multiple times in contract
    DATA lv_hits TYPE i VALUE 0.
    DATA ls_ctr  TYPE ty_ctr.
    LOOP AT lt_ctr INTO ls_ctr WHERE matnr = lv_matnr.
      lv_hits += 1.
      IF lv_hits = 1.
        "keep first hit in ls_ctr
      ENDIF.
    ENDLOOP.

    IF lv_hits = 0.
      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
        message = |Material { <p>-matnr } not found in contract { iv_contract }| ) TO et_return.
      CONTINUE.
    ELSEIF lv_hits > 1.
      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
        message = |Material { <p>-matnr } occurs multiple times in contract { iv_contract } -> ambiguous| ) TO et_return.
      CONTINUE.
    ENDIF.

    "UoM check
    IF <p>-meins IS NOT INITIAL AND ls_ctr-vrkme IS NOT INITIAL AND <p>-meins <> ls_ctr-vrkme.
      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
        message = |UoM mismatch for { <p>-matnr }: { <p>-meins } <> contract { ls_ctr-vrkme }| ) TO et_return.
      CONTINUE.
    ENDIF.

    ASSIGN lt_agg[ posnr = ls_ctr-posnr ] TO <agg>.
    IF sy-subrc = 0.
      <agg>-qty = <agg>-qty + <p>-menge.
    ELSE.
      INSERT VALUE ty_agg(
        posnr = ls_ctr-posnr
        qty   = <p>-menge
        meins = ls_ctr-vrkme
        matnr = lv_matnr ) INTO TABLE lt_agg.
    ENDIF.

  ENDLOOP.

  IF line_exists( et_return[ type = 'E' ] ) OR lt_agg IS INITIAL.
    SET LOCALE LANGUAGE lv_old_langu.
    RETURN.
  ENDIF.

  "4) Build BAPI (CREATEFROMDATA1) structures
  DATA ls_head   TYPE bapisdhead1.
  DATA ls_headx  TYPE bapisdhead1x.
  DATA lt_items  TYPE STANDARD TABLE OF bapisditem WITH DEFAULT KEY.
  DATA lt_itemsx TYPE STANDARD TABLE OF bapisditemx WITH DEFAULT KEY.
  DATA lt_sched  TYPE STANDARD TABLE OF bapischedule WITH DEFAULT KEY.
  DATA lt_schedx TYPE STANDARD TABLE OF bapischedulex WITH DEFAULT KEY.
  DATA lt_part   TYPE STANDARD TABLE OF bapipartnr WITH DEFAULT KEY.
  DATA lt_cond   TYPE STANDARD TABLE OF bapicondition WITH DEFAULT KEY.

  CLEAR: ls_head, ls_headx.

  ls_head-doc_type   = iv_auart.
  ls_head-sales_org  = ls_vbak-vkorg.
  ls_head-distr_chan = ls_vbak-vtweg.
  ls_head-division   = ls_vbak-spart.
  ls_head-req_date_h = iv_deliverydat.
  ls_head-purch_no_c = iv_customerref.
  ls_head-ref_doc    = iv_contract.
  ls_head-ref_doc_cat = 'G'.

  ls_headx-updateflag = 'I'.
  ls_headx-doc_type   = 'X'.
  ls_headx-sales_org  = 'X'.
  ls_headx-distr_chan = 'X'.
  ls_headx-division   = 'X'.
  ls_headx-req_date_h = 'X'.
  ls_headx-purch_no_c = 'X'.
  ls_headx-ref_doc    = 'X'.
  ls_headx-ref_doc_cat = 'X'.

  "Partners from contract
  SELECT parvw, kunnr
    FROM vbpa
    WHERE vbeln = @iv_contract AND kunnr <> ''
    INTO TABLE @DATA(lt_vbpa).

  lt_part = VALUE #( FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).
  IF NOT line_exists( lt_part[ partn_role = 'AG' ] ) AND ls_vbak-kunnr IS NOT INITIAL.
    APPEND VALUE bapipartnr( partn_role = 'AG' partn_numb = ls_vbak-kunnr ) TO lt_part.
  ENDIF.

  "Conditions: pick PR00 from contract pricing if possible; else fallback to VBAP-NETPR
  DATA lv_knumv TYPE knumv.
  lv_knumv = ls_vbak-knumv.

  LOOP AT lt_agg INTO DATA(ls_agg).

    APPEND VALUE bapisditem(
      itm_number = ls_agg-posnr
      material   = ls_agg-matnr
      target_qty = ls_agg-qty
      target_qu  = ls_agg-meins
      ref_doc    = iv_contract
      ref_doc_it = ls_agg-posnr
      ref_doc_ca = 'G'
    ) TO lt_items.

    APPEND VALUE bapisditemx(
      itm_number = ls_agg-posnr
      updateflag = 'I'
      material   = 'X'
      target_qty = 'X'
      target_qu  = 'X'
      ref_doc    = 'X'
      ref_doc_it = 'X'
      ref_doc_ca = 'X'
    ) TO lt_itemsx.

    APPEND VALUE bapischedule(
      itm_number = ls_agg-posnr
      sched_line = '0001'
      req_qty    = ls_agg-qty
      req_date   = iv_deliverydat
    ) TO lt_sched.

    APPEND VALUE bapischedulex(
      itm_number = ls_agg-posnr
      sched_line = '0001'
      updateflag = 'I'
      req_qty    = 'X'
      req_date   = 'X'
    ) TO lt_schedx.

    "Try get PR00 (or first active price condition) from PRCD_ELEMENTS
    DATA: lv_kschl TYPE kschl VALUE 'PR00',
          lv_kbetr TYPE kbetr_kond,
          lv_waers TYPE waers,
          lv_kpein TYPE kpein,
          lv_kmein TYPE kmein.

    CLEAR: lv_kbetr, lv_waers, lv_kpein, lv_kmein.

    IF lv_knumv IS NOT INITIAL.
      SELECT SINGLE kschl, kbetr, waers, kpein, kmein
        FROM prcd_elements
        WHERE knumv = @lv_knumv
          AND kposn = @ls_agg-posnr
          AND kinak = ''
          AND kstat = ''
          AND kschl = 'PR00'
        INTO @DATA(ls_pr00).

      IF sy-subrc = 0.
        lv_kschl = ls_pr00-kschl.
        lv_kbetr = ls_pr00-kbetr.
        lv_waers = ls_pr00-waers.
        lv_kpein = ls_pr00-kpein.
        lv_kmein = ls_pr00-kmein.
      ENDIF.
    ENDIF.

    "Fallback: VBAP-NETPR if PRCD_ELEMENTS not found
    IF lv_kbetr IS INITIAL.
      SELECT SINGLE netpr, kpein, kmein
        FROM vbap
        WHERE vbeln = @iv_contract AND posnr = @ls_agg-posnr
        INTO @DATA(ls_vbap_price).
      lv_kbetr = ls_vbap_price-netpr.
      lv_kpein = COND #( WHEN ls_vbap_price-kpein IS INITIAL THEN 1 ELSE ls_vbap_price-kpein ).
      lv_kmein = COND #( WHEN ls_vbap_price-kmein IS INITIAL THEN ls_agg-meins ELSE ls_vbap_price-kmein ).
      lv_waers = ls_vbak-waerk.
    ENDIF.

    IF lv_kbetr IS INITIAL.
      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
        message = |No price found for item { ls_agg-posnr } in contract -> cannot satisfy Nettopreis/Nettowert| ) TO et_return.
      CONTINUE.
    ENDIF.

    APPEND VALUE bapicondition(
      itm_number = ls_agg-posnr
      cond_st_no = '010'
      cond_count = '01'
      cond_type  = lv_kschl
      cond_value = lv_kbetr
      currency   = lv_waers
      cond_p_unt = COND #( WHEN lv_kpein IS INITIAL THEN 1 ELSE lv_kpein )
      cond_unit  = COND #( WHEN lv_kmein IS INITIAL THEN ls_agg-meins ELSE lv_kmein )
    ) TO lt_cond.

  ENDLOOP.

  IF line_exists( et_return[ type = 'E' ] ).
    SET LOCALE LANGUAGE lv_old_langu.
    RETURN.
  ENDIF.

  "5) Call BAPI in this RFC session (allowed to use update-task)
  CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA1'
    EXPORTING
      sales_header_in   = ls_head
      sales_header_inx  = ls_headx
      int_number_assignment = space
    IMPORTING
      salesdocument_ex  = ev_vbeln
    TABLES
      return              = et_return
      sales_items_in      = lt_items
      sales_items_inx     = lt_itemsx
      sales_partners      = lt_part
      sales_schedules_in  = lt_sched
      sales_schedules_inx = lt_schedx
      sales_conditions_in = lt_cond.

  IF ev_vbeln IS INITIAL OR line_exists( et_return[ type = 'E' ] ) OR line_exists( et_return[ type = 'A' ] ).
    CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.
  ELSE.
    CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
      EXPORTING wait = abap_true.
  ENDIF.

  SET LOCALE LANGUAGE lv_old_langu.

ENDFUNCTION.
