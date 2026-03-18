CLASS /plce/cl_waste_order_api DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    " Reusable method to cancel/negatively confirm the waste order item
    CLASS-METHODS cancel_waste_order_item
      IMPORTING
        iv_order_reference TYPE string " Assuming this is ReferenceId
        iv_reason_code     TYPE string " definiertegrund (Customizing Code)
        iv_reason_text     TYPE string " stornogrund (Free text)
      EXPORTING
        et_messages        TYPE bapirettab
      RETURNING
        VALUE(rv_success)  TYPE abap_bool.
ENDCLASS.

CLASS /plce/cl_waste_order_api IMPLEMENTATION.
  METHOD cancel_waste_order_item.
    " ----------------------------------------------------------------------
    " TODO: Implement IS-U Waste specific BAPI / Function Module call here.
    " 1. Map iv_reason_text to EWA_ORDER_OBJECT-TEXT
    " 2. Set EWA_ORDER_OBJECT-CONFTYPE1 = 'Storno durch Dispo' (or customizing code via iv_reason_code)
    " 3. Perform negative confirmation
    " ----------------------------------------------------------------------
    rv_success = abap_true. " Set to false if BAPI fails
  ENDMETHOD.
ENDCLASS.
