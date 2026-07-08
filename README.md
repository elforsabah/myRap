<img width="975" height="604" alt="image" src="https://github.com/user-attachments/assets/600d818a-a532-4ff0-aa50-524274a9a117" />


@EndUserText.label: 'Position Input for LANF'
define abstract entity ZI_LANF_POSITION_INPUT
 
{
  key ContractVbeln     : vbeln_va;     // Contract number (VBELN)
  key Matnr             : matnr;        // Material number
@Semantics.quantity.unitOfMeasure: 'Meins'
Menge             : dzmeng;       // Quantity (ZMENG, assuming decimal quantity type)
@Semantics.unitOfMeasure: true
Meins             : meins;        // Unit of measure (ZIEME)

_Parent           : association to parent ZI_LANF_CREATE_INPUT  on $projection.ContractVbeln = _Parent.ContractVbeln;

}


@EndUserText.label: 'Input for Document Attachment'
define root abstract entity ZI_ATTACH_INPUT

{
//  key TechKey       : abap.char(1);
  key Vbeln             : vbeln_va;     // LANF number
  Title             : text100;      // Title/Description
  Url               : url;          // Document URL (for archive link)
    
}

@EndUserText.label: 'Input for LANF Creation'
define root abstract entity ZI_LANF_CREATE_INPUT
  
{
//  key TechKey     : abap.char(1); // Required for root abstract entity
  key ContractVbeln     : vbeln_va;     // Contract number (VBELN)
  DeliveryDate      : kbdat;        // Delivery/Service date (FBUDA/KETDAT)
  CustomerRef       : bstkd;        // Customer reference (BSTKD)
  Description       : abap.char(225);      // Description (VBAK-TEXT)
  _Positions        : composition [0..*] of ZI_LANF_POSITION_INPUT;
    
}


@EndUserText.label: 'Response Structure'
define root abstract entity ZI_LANF_RESPONSE
{
 key Vbeln             : vbeln_va;     // Created LANF number
}

@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Root for LANF'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZI_LANF_ROOT as select from ddddlsrc
{
    
    key cast('' as abap.char(30)) as Techkey
} where 1 = 2 // Ensures no data is returned

@EndUserText.label: 'Message Structure'
define abstract entity ZI_MESSAGE
  
{
   key TechKey       : abap.char(1);
  Msgid             : msgid;
  Msgno             : msgnr;
  Msgty             : msgty;
  Msgv1             : symsgv;
  Msgv2             : symsgv;
  Msgv3             : symsgv;
  Msgv4             : symsgv;
  
//  _Parent           : association to parent ZI_LANF_RESPONSE on $projection.TechKey = _Parent.TechKey;
}


abstract;
strict ( 2 );

define behavior for ZI_ATTACH_INPUT //alias <alias_name>
{

}

abstract;
//strict ( 2 );

with hierarchy;

define behavior for ZI_LANF_CREATE_INPUT alias createin
{
 association _Positions;
 field( readonly ) ContractVbeln;
}

define behavior for ZI_LANF_POSITION_INPUT alias posIn
{

association _Parent;
field( readonly ) ContractVbeln;
field( readonly ) Matnr;
}

abstract;
strict ( 2 );
with hierarchy;
define behavior for ZI_LANF_RESPONSE
{
}

unmanaged implementation in class zbp_i_lanf_root unique;

define behavior for ZI_LANF_ROOT alias lanfroot
//persistent table ddddlsrc
lock master
authorization master ( instance )
//etag master <field_name>
{

 // First endpoint: Create LANF
  static action CreateLanf deep parameter ZI_LANF_CREATE_INPUT  result [1] ZI_LANF_RESPONSE;

  // Second endpoint: Attach PDF/Document Link
  static action AttachDocument  parameter ZI_ATTACH_INPUT deep result [1] ZI_LANF_RESPONSE;


//  create;
//  update;
//  delete;
}



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

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA lv_contract TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  DATA lt_pos_rfc  TYPE zlanf_pos_in_tt.
  lt_pos_rfc = VALUE #(
    FOR p IN ls_input-_positions
    WHERE ( menge > 0 )
    ( matnr = p-matnr
      menge = p-menge
      meins = p-meins )
  ).

  DATA lt_return  TYPE bapiret2_tab.
  DATA lv_new_so  TYPE vbeln_va.
  DATA lv_sysmsg  TYPE char60.
  DATA lv_commmsg TYPE char60.

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


METHOD attachdocument.

  DATA: ls_input         TYPE zi_attach_input,
        lt_return_attach TYPE bapiret2_tab,
        lv_vbtyp         TYPE vbak-vbtyp,
        lv_busobj        TYPE swo_objtyp.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  ls_input = CORRESPONDING #( ls_key-%param ).

  DATA(lv_vbeln) = |{ ls_input-vbeln ALPHA = IN }|.

  " Determine SD doc category (VBTYP)
  SELECT SINGLE vbtyp FROM vbak
    WHERE vbeln = @lv_vbeln
    INTO @lv_vbtyp.

  IF sy-subrc <> 0.
    APPEND VALUE #( type = 'E' id = '00' number = '001'
      message = |Sales document { lv_vbeln } not found| ) TO lt_return_attach.
  ELSE.
    " Determine correct BOR object for that category (avoids BUS2032/L issue)
    CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
      EXPORTING
        i_document_type   = lv_vbtyp
      IMPORTING
        e_business_object = lv_busobj.


    " Attach URL as GOS/BDS link
    CALL FUNCTION 'BDS_BUSINESSDOCUMENT_CRE_O_URL'
      EXPORTING
        classname       = lv_busobj
        classtype       = 'BO'
        object_key      = lv_vbeln
        url             = ls_input-url
        url_description = ls_input-title
      EXCEPTIONS
        nothing_found   = 1
        parameter_error = 2
        not_allowed     = 3
        error_kpro      = 4
        internal_error  = 5
        OTHERS          = 6.

    IF sy-subrc <> 0.
      APPEND VALUE #( type = 'E' id = sy-msgid number = sy-msgno
        message_v1 = sy-msgv1 message_v2 = sy-msgv2 message_v3 = sy-msgv3 message_v4 = sy-msgv4
        message = |Attach URL failed (SY-SUBRC={ sy-subrc })| ) TO lt_return_attach.
    ELSE.
      APPEND VALUE #( type = 'S' id = '00' number = '000'
        message = |URL attached successfully| ) TO lt_return_attach.
    ENDIF.
  ENDIF.

  " Return VBELN + messages
  result = VALUE #(
    ( %param = VALUE #(
*        techkey   = 'X'
        vbeln     = lv_vbeln
*        _messages = VALUE #(
*          FOR r IN lt_return_attach (
*            techkey = 'X'
*            msgid   = r-id
*            msgno   = r-number
*            msgty   = r-type
*            msgv1   = r-message_v1
*            msgv2   = r-message_v2
*            msgv3   = r-message_v3
*            msgv4   = r-message_v4 ) )

            ) ) ).

ENDMETHOD.

METHOD map_positions_to_contract.

  SELECT posnr, matnr
    FROM vbap
    WHERE vbeln = @iv_contract_vbeln
    INTO TABLE @DATA(lt_contract_pos).

  LOOP AT it_positions ASSIGNING FIELD-SYMBOL(<pos>) WHERE menge > 0.

    DATA(lv_matnr) = <pos>-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    READ TABLE lt_contract_pos ASSIGNING FIELD-SYMBOL(<contr_pos>)
      WITH KEY matnr = lv_matnr.

    IF sy-subrc = 0.
      APPEND VALUE bapisditm(
        itm_number = <contr_pos>-posnr
        material   = lv_matnr
        target_qty = <pos>-menge
        target_qu  = <pos>-meins
        ref_doc    = iv_contract_vbeln
        ref_doc_it = <contr_pos>-posnr
        ref_doc_ca = 'G' ) TO ct_order_items.

      APPEND VALUE bapisditmx(
        itm_number = <contr_pos>-posnr
        updateflag = 'I'
        material   = 'X'
        target_qty = 'X'
        target_qu  = 'X'
        ref_doc    = 'X'
        ref_doc_it = 'X'
        ref_doc_ca = 'X' ) TO ct_order_itemsx.
    ELSE.
      APPEND VALUE bapiret2(
        type   = 'E'
        id     = '00'
        number = '001'
        message = |No position for MATNR { lv_matnr } in contract { iv_contract_vbeln }| ) TO ct_return.
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

FUNCTION z_lanf_create_dmreq_rfc
  IMPORTING
    VALUE(iv_contract) TYPE vbeln_va
    VALUE(iv_deliverydat) TYPE dats
    VALUE(iv_customerref) TYPE bstkd
    VALUE(iv_auart) TYPE auart DEFAULT 'ZLRA'
  EXPORTING
    VALUE(ev_vbeln) TYPE vbeln_va
  TABLES
    it_positions TYPE zlanf_pos_in_tt
    et_return TYPE bapiret2_tab.




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
*    ELSEIF lv_hits > 1.
*      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
*        message = |Material { <p>-matnr } occurs multiple times in contract { iv_contract } -> ambiguous| ) TO et_return.
*      CONTINUE.
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


