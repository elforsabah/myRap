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

    TYPES:
      BEGIN OF ty_bo_range,
        fieldname TYPE char30,  " Assumed type for field name
        sign      TYPE ddsign,
        option    TYPE ddoption,
        low       TYPE char50,  " Assumed sufficient length
        high      TYPE char50,
      END OF ty_bo_range.

    TYPES tt_bo_range TYPE STANDARD TABLE OF ty_bo_range WITH DEFAULT KEY.

    DATA:
      lt_ranges     TYPE tt_bo_range,
      lt_weighingnr TYPE STANDARD TABLE OF ty_weighinglead,
      lv_max        TYPE int4 VALUE 1.

    IF iv_contract IS INITIAL.
      RETURN.
    ENDIF.

    lt_ranges = VALUE #(
      ( fieldname = 'CONTRACT_VBELN' sign = 'I' option = 'EQ' low = iv_contract )
      ( fieldname = 'GROSS_ID'       sign = 'I' option = 'NE' low = space )
      ( fieldname = 'TAREID'         sign = 'I' option = 'EQ' low = space )
      ( fieldname = 'WEIGHINGNR_LEAD' sign = 'I' option = 'NE' low = space )
    ).

    cl_eewa_or_weighingprocess=>cl_read(
      EXPORTING
        par_objtype = cl_eewa_bo_weighingprocess=>cot_weighingprocess
        par_domain  = 'OPEN'
        par_ranges  = lt_ranges
        par_maxrows = lv_max
      IMPORTING
        par_table   = lt_weighingnr
    ).

    IF line_exists( lt_weighingnr[ 1 ] ).
      rv_weighingnr = lt_weighingnr[ 1 ]-weighingnr_lead.
    ENDIF.
  ENDMETHOD.

ENDCLASS.
