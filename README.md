CLASS zcl_wr_weighbridge_helper DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    CLASS-METHODS cl_get_open_weighproc
      IMPORTING
        iv_contract       TYPE vbeln_va
      RETURNING
        VALUE(rv_weighingnr) TYPE ewawa_weighingnr.

  PROTECTED SECTION.

  PRIVATE SECTION.

ENDCLASS.

CLASS zcl_wr_weighbridge_helper IMPLEMENTATION.

  METHOD cl_get_open_weighproc.
    TYPES:
      BEGIN OF ty_weighinglead,
        weighingnr_lead TYPE ewa_wa_weighproc-weighingnr_lead,
      END OF ty_weighinglead.

    DATA:
      lr_ranges     TYPE REF TO ewapranges,
      lt_weighingnr TYPE STANDARD TABLE OF ty_weighinglead,
      lv_max        TYPE int4 VALUE 1,
      lv_grossid    TYPE ewa_wa_weighproc-gross_id,
      lv_tareid     TYPE ewa_wa_weighproc-tare_id,
      lv_weighinglead TYPE ewa_wa_weighproc-weighingnr_lead.

    IF iv_contract IS INITIAL.
      RETURN.
    ENDIF.

    CREATE DATA lr_ranges.
    FIELD-SYMBOLS: <lt_ranges> TYPE STANDARD TABLE OF ewasrange.
    ASSIGN lr_ranges->* TO <lt_ranges>.

    " For CONTRACT_VBELN
    TYPES tt_vbeln_range TYPE RANGE OF vbeln_va.
    DATA(lt_contract_range) = VALUE tt_vbeln_range( ( sign = 'I' option = 'EQ' low = iv_contract ) ).
    DATA(lr_contract_ref) = REF #( lt_contract_range ).
    APPEND VALUE #( name = 'CONTRACT_VBELN' range = lr_contract_ref ) TO <lt_ranges>.

    " For GROSS_ID
    TYPES tt_gross_range TYPE RANGE OF ewa_wa_weighproc-gross_id.
    DATA(lt_gross_range) = VALUE tt_gross_range( ( sign = 'I' option = 'NE' low = lv_grossid ) ).
    DATA(lr_gross_ref) = REF #( lt_gross_range ).
    APPEND VALUE #( name = 'GROSS_ID' range = lr_gross_ref ) TO <lt_ranges>.

    " For TAREID
    TYPES tt_tare_range TYPE RANGE OF ewa_wa_weighproc-tare_id.
    DATA(lt_tare_range) = VALUE tt_tare_range( ( sign = 'I' option = 'EQ' low = lv_tareid ) ).
    DATA(lr_tare_ref) = REF #( lt_tare_range ).
    APPEND VALUE #( name = 'TAREID' range = lr_tare_ref ) TO <lt_ranges>.

    " For WEIGHINGNR_LEAD
    TYPES tt_lead_range TYPE RANGE OF ewa_wa_weighproc-weighingnr_lead.
    DATA(lt_lead_range) = VALUE tt_lead_range( ( sign = 'I' option = 'NE' low = lv_weighinglead ) ).
    DATA(lr_lead_ref) = REF #( lt_lead_range ).
    APPEND VALUE #( name = 'WEIGHINGNR_LEAD' range = lr_lead_ref ) TO <lt_ranges>.

    cl_eewa_or_weighingprocess=>cl_read(
      EXPORTING
        par_objtype = cl_eewa_bo_weighingprocess=>cot_weighingprocess
        par_domain  = 'OPEN'
        par_ranges  = lr_ranges
        par_maxrows = lv_max
      IMPORTING
        par_table   = lt_weighingnr
    ).

    IF line_exists( lt_weighingnr[ 1 ] ).
      rv_weighingnr = lt_weighingnr[ 1 ]-weighingnr_lead.
    ENDIF.
  ENDMETHOD.

ENDCLASS.
