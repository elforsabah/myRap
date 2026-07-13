<img width="1792" height="419" alt="image" src="https://github.com/user-attachments/assets/f02490ca-cf9a-4f9d-bc46-e89a05510d21" />
<img width="1283" height="423" alt="image" src="https://github.com/user-attachments/assets/f0af567c-a103-46c1-a9f2-ae699a51b7b2" />
*&---------------------------------------------------------------------*
*& Report ZWR_WATP_CONTAINER_OVERVIEW
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zwr_watp_container_overview.
INCLUDE /watp/macro_range.

DEFINE __add.
  IF &3 IS INITIAL.
    CONCATENATE &3 &2 INTO &3 SEPARATED BY space.
  ELSE.
    CONCATENATE &3 &1 &2 INTO &3 SEPARATED BY space.
  ENDIF.
END-OF-DEFINITION.

TYPES:
  BEGIN OF _tbehtxt,
    matnr   TYPE matnr,
    bauform TYPE bauform,
    bautxt  TYPE bautxt,
  END OF _tbehtxt.


TABLES:
  /watp/vcaequz,
  ewaelocsd_new,
  eqbs.

DATA:
  gt_output      TYPE STANDARD TABLE OF zwr_s_cs_container_overview, "ZWRS_CONTAINER_OVERVIEW,
  gr_output      TYPE REF TO zwr_s_cs_container_overview, "ZWRS_CONTAINER_OVERVIEW,
  gt_address     TYPE /watp/paddress,
  gt_partnerinfo TYPE /watp/pbpartnerinfo,
  gt_behtxt      TYPE STANDARD TABLE OF _tbehtxt.


SELECTION-SCREEN BEGIN OF BLOCK b1 WITH FRAME TITLE TEXT-001.

  SELECT-OPTIONS:
    styp FOR /watp/vcaequz-matnr,
    sser FOR /watp/vcaequz-sernr,
    stid FOR /watp/vcaequz-tidnr.

SELECTION-SCREEN END OF BLOCK b1.

SELECTION-SCREEN BEGIN OF BLOCK b2 WITH FRAME TITLE TEXT-002.

  SELECT-OPTIONS:
    sgpl FOR /watp/vcaequz-tplnr,
    scon FOR ewaelocsd_new-vbeln MATCHCODE OBJECT vmva,
    sgp  FOR ewaelocsd_new-kunnr.

SELECTION-SCREEN END OF BLOCK b2.

SELECTION-SCREEN BEGIN OF BLOCK b3 WITH FRAME TITLE TEXT-003.

  SELECT-OPTIONS:
    slgort  FOR eqbs-b_lager.

SELECTION-SCREEN END OF BLOCK b3.


PARAMETERS:
  psel TYPE char1 AS LISTBOX VISIBLE LENGTH 20 OBLIGATORY DEFAULT 'A' USER-COMMAND sp1.


INITIALIZATION.

  DATA:
    lt_val   TYPE vrm_values,
    lv_name  TYPE vrm_id,
    lv_value TYPE vrm_value.

  CLEAR lt_val.
  lv_name = 'PSEL'.


  lv_value-key = 'A'.
  lv_value-text = 'Alle'(004).
  APPEND lv_value TO lt_val.

  lv_value-key = 'K'.
  lv_value-text = 'Kunden'(005).
  APPEND lv_value TO lt_val.

  lv_value-key = 'L'.
  lv_value-text = 'Lager'(006).
  APPEND lv_value TO lt_val.

  CALL FUNCTION 'VRM_SET_VALUES'
    EXPORTING
      id     = lv_name           " Name der Wertemenge
      values = lt_val.         " Wertetabelle für ID.

AT SELECTION-SCREEN OUTPUT.

  IF psel = 'K'.

    LOOP AT SCREEN.
      IF screen-name CS 'SLGORT'.
        screen-invisible = 1.
        screen-input = 0.
      ENDIF.
      MODIFY SCREEN.
    ENDLOOP.

  ELSEIF psel = 'L'.

    LOOP AT SCREEN.
      IF screen-name CS 'SGPL' OR screen-name CS 'SCON' OR screen-name CS 'SGP'.
        screen-invisible = 1.
        screen-input = 0.
      ENDIF.

      MODIFY SCREEN.
    ENDLOOP.

  ENDIF.

START-OF-SELECTION.

  IF psel = 'A'.

    PERFORM select_from_table.

    PERFORM  get_contract.

  ELSEIF psel = 'K'.

    PERFORM select_from_table.

    PERFORM get_contract.

  ELSEIF psel = 'L'.

    PERFORM select_from_table.

  ENDIF.

  PERFORM get_additional_data.

  PERFORM prepare_for_display.

  PERFORM display_alv.

  """"""""""""""""""""""""""""""""""""""""""""""""""""FORM DEFINITION"""""""""""""""""""""""""""""""""""""""""""""""""""""""

FORM get_contract.

  DATA:
    lcount        TYPE int8,
    lt_output_con TYPE STANDARD TABLE OF zwr_s_cs_container_overview, "ZWRS_CONTAINER_OVERVIEW,
    lr_output_con TYPE REF TO zwr_s_cs_container_overview, "ZWRS_CONTAINER_OVERVIEW,
    lt_output_tmp TYPE STANDARD TABLE OF zwr_s_cs_container_overview. "ZWRS_CONTAINER_OVERVIEW.

  IF lines( gt_output ) > 0.
    SELECT
      c~vbeln AS vbeln,
      c~equnr AS equnr,
      c~kunnr AS partner,
      v~ktext AS vbeln_desc
      FROM ewaelocsd_new AS c
      INNER JOIN vbak AS v ON v~vbeln = c~vbeln
      FOR ALL ENTRIES IN @gt_output
      WHERE c~equnr = @gt_output-equnr
      AND c~vbeln IN @scon
      AND c~kunnr IN @sgp
      AND c~bis >= @sy-datum
      AND c~ab  <= @sy-datum
      INTO CORRESPONDING FIELDS OF TABLE @lt_output_con.
  ENDIF.

  LOOP AT gt_output REFERENCE INTO gr_output.
    lcount = 0.
    LOOP AT lt_output_con REFERENCE INTO lr_output_con WHERE equnr = gr_output->equnr.
      IF lcount = 0.
        gr_output->vbeln = lr_output_con->vbeln.
        gr_output->partner = lr_output_con->partner.
        gr_output->vbeln_desc = lr_output_con->vbeln_desc.
        lcount = 1.
      ELSE.
        INSERT gr_output->* INTO TABLE lt_output_tmp.
        gr_output->vbeln = lr_output_con->vbeln.
        gr_output->partner = lr_output_con->partner.
        gr_output->vbeln_desc = lr_output_con->vbeln_desc.
      ENDIF.
    ENDLOOP.
  ENDLOOP.

  INSERT LINES OF lt_output_tmp INTO TABLE gt_output.

  IF scon IS NOT INITIAL OR sgp IS NOT INITIAL.
    DELETE gt_output WHERE vbeln NOT IN scon OR partner NOT IN sgp.
  ENDIF.

ENDFORM.

FORM select_from_table.

  DATA:
    lv_empty_logiknr TYPE logiknr,
    lv_where         TYPE char2048,
    lv_conttype      TYPE kombinat VALUE 'B'.

  PERFORM build_where USING lv_where.

  SELECT
         a~equnr      AS equnr,
         a~sernr      AS sernr,
         a~matnr      AS matnr,
         a~tplnr      AS devloc,
         s~b_werk     AS werk,
         s~b_lager    AS lgort,
         tx~pltxt     AS pltxt,
         a~tidnr      AS tidnr
    INTO CORRESPONDING FIELDS OF TABLE @gt_output
    FROM /watp/vcaequz AS a
    INNER JOIN etyp AS t ON a~matnr = t~matnr
    LEFT OUTER JOIN eqbs AS s ON a~equnr = s~equnr
    LEFT OUTER JOIN iflotx AS tx ON a~tplnr = tx~tplnr AND tx~spras = @sy-langu
      WHERE (lv_where).

ENDFORM.

FORM build_where USING par_where TYPE char2048.

  __add 'AND' 'S~B_LAGER    in @SLGORT' par_where.          "#EC NOTEXT
  __add 'AND' 'A~TPLNR      in @SGPL' par_where.            "#EC NOTEXT
  __add 'AND' 'A~MATNR      in @STYP' par_where.            "#EC NOTEXT
  __add 'AND' 'A~SERNR      in @SSER' par_where.            "#EC NOTEXT
  __add 'AND' 'A~TIDNR      in @STID' par_where.            "#EC NOTEXT
  __add 'AND' '( A~DATAB <= @SY-DATUM and A~DATBI >= @SY-DATUM )' par_where. "#EC NOTEXT
  __add 'AND' 'T~KOMBINAT     = @LV_CONTTYPE' par_where.

  IF psel = 'K'.
    __add 'AND' 'A~LOGIKNR    <> @LV_EMPTY_LOGIKNR' par_where. "#EC NOTEXT
  ELSEIF psel = 'L'.
    __add 'AND' 'A~LOGIKNR    = @LV_EMPTY_LOGIKNR' par_where. "#EC NOTEXT
  ENDIF.
ENDFORM.

FORM get_additional_data.

  DATA:
    lt_devloc  TYPE tplnr_tab,
    lt_partner TYPE /watp/ppartners,
    lt_behtyp  TYPE STANDARD TABLE OF matnr.

  LOOP AT gt_output REFERENCE INTO gr_output.
    IF gr_output->devloc IS NOT INITIAL.
      INSERT gr_output->devloc INTO TABLE lt_devloc.
    ENDIF.
    IF gr_output->partner IS NOT INITIAL.
      INSERT gr_output->partner INTO TABLE lt_partner.
    ENDIF.
    IF gr_output->matnr IS NOT INITIAL.
      INSERT gr_output->matnr INTO TABLE lt_behtyp.
    ENDIF.
  ENDLOOP.

  SORT lt_devloc.
  DELETE ADJACENT DUPLICATES FROM lt_devloc.
  SORT lt_partner.
  DELETE ADJACENT DUPLICATES FROM lt_partner.
  SORT lt_behtyp.
  DELETE ADJACENT DUPLICATES FROM lt_behtyp.

  IF lines( lt_devloc ) IS NOT INITIAL.

    /watp/cl_bo_devloc=>read_addresses(
      EXPORTING
        par_devlocs   = lt_devloc         " Tabelle über TechnischePlatznummern
      IMPORTING
        par_addresses =  gt_address       " Liste von Adressen
    ).
    SORT gt_address BY external_key.
  ENDIF.

  IF lines( lt_partner ) IS NOT INITIAL.
    gt_partnerinfo = /watp/cl_bo_partner=>read_partnerinfos_bulk(
       EXPORTING
         par_partners = lt_partner         " Liste von Geschäftspartnernummern
    ).
    SORT gt_partnerinfo BY partner.
  ENDIF.

  IF lines( lt_behtyp ) IS NOT INITIAL.
    SELECT * FROM etyp
      INTO CORRESPONDING FIELDS OF TABLE @gt_behtxt
      FOR ALL ENTRIES IN @lt_behtyp
      WHERE etyp~matnr = @lt_behtyp-table_line.
    SORT gt_behtxt BY matnr.
  ENDIF.

ENDFORM.

FORM prepare_for_display.

  DATA:
    lr_partnerinfo TYPE REF TO /watp/sbpartnerinfo,
    lr_address     TYPE REF TO /watp/saddress,
    lr_behtxt      TYPE REF TO _tbehtxt.

  LOOP AT gt_output REFERENCE INTO gr_output.
    gr_output->con_count = 1.
    IF gr_output->devloc IS NOT INITIAL.
      gr_output->status = 'K'.
    ELSE.
      gr_output->status = 'L'.
    ENDIF.
    IF gr_output->devloc IS NOT INITIAL.
      CLEAR: gr_output->lgort,
             gr_output->werk.
      READ TABLE gt_address WITH KEY external_key = gr_output->devloc REFERENCE INTO lr_address BINARY SEARCH.
      IF sy-subrc = 0.
        gr_output->city = lr_address->city1.
        gr_output->post_code = lr_address->post_code1.
        CONCATENATE lr_address->street lr_address->house_num1 INTO gr_output->street SEPARATED BY space.
        CONCATENATE gr_output->street lr_address->house_num2 INTO gr_output->street.
      ENDIF.
    ENDIF.
    IF gr_output->partner IS NOT INITIAL.
      READ TABLE gt_partnerinfo WITH KEY partner = gr_output->partner REFERENCE INTO lr_partnerinfo BINARY SEARCH.
      IF sy-subrc = 0.
        gr_output->partnername = lr_partnerinfo->name1.
      ENDIF.
    ENDIF.
    IF gr_output->matnr IS NOT INITIAL.
      READ TABLE gt_behtxt WITH KEY matnr = gr_output->matnr REFERENCE INTO lr_behtxt BINARY SEARCH.
      IF sy-subrc = 0.
        IF lr_behtxt->bautxt IS NOT INITIAL.
          gr_output->behtyptxt = lr_behtxt->bautxt.
        ELSE.
          gr_output->behtyptxt = lr_behtxt->bauform.
        ENDIF.
      ENDIF.
    ENDIF.
  ENDLOOP.

ENDFORM.

FORM display_alv.
  DATA:
    lv_key       TYPE salv_s_layout_key, "value SY-REPID,
    lr_table     TYPE REF TO cl_salv_table,
    lr_functions TYPE REF TO cl_salv_functions,
    lr_layout    TYPE REF TO cl_salv_layout,
    lr_display   TYPE REF TO cl_salv_display_settings.

  lv_key-report = sy-repid.

  TRY.
      cl_salv_table=>factory(                           "#EC CI_NOORDER
        IMPORTING
          r_salv_table = lr_table
        CHANGING
          t_table = gt_output ).
    CATCH cx_salv_msg. "
  ENDTRY.

  IF lr_table IS NOT INITIAL.
**"-----Optional
**---Setting the ALV toolbar
    lr_functions = lr_table->get_functions( ).
    lr_functions->set_all( abap_true ).
**---Setting the Layout Save property
    lr_layout = lr_table->get_layout( ).
    lr_layout->set_initial_layout( value = '/DEFAULT' ).
    lr_layout->set_key( lv_key ).
    lr_layout->set_save_restriction( cl_salv_layout=>restrict_none ).

    lr_table->display( ).
  ENDIF.
ENDFORM.
""""""""""""""""""""""""""""""""""""""""""""""""""""FORM DEFINITION"""""""""""""""""""""""""""""""""""""""""""""""""""""""



