CLASS ZCL_WR_SERVICE_EXTEND_CALC DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    INTERFACES IF_SADL_EXIT_CALC_ELEMENT_READ.

  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS ZCL_WR_SERVICE_EXTEND_CALC IMPLEMENTATION.

  METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE.
    DATA: lt_original_data TYPE STANDARD TABLE OF /PLCE/C_PDMNLServiceWR WITH DEFAULT KEY,
          lbadi TYPE REF TO /plce/pd_ui_manual,
          lt_service_map TYPE /plce/ppdmnlservicemap.

    lt_original_data = CORRESPONDING #( it_original_data ).  " Assign input data here

    " Add your custom calculation logic for point_of_origin2
    " Example: Loop over lt_original_data, compute the virtual field based on ZI_WR_EWA_ORDER_OBJECT or other sources
    LOOP AT lt_original_data ASSIGNING FIELD-SYMBOL(<fs_data>).
      " Your logic, e.g.:
      " SELECT SINGLE point_of_origin FROM ZI_WR_EWA_ORDER_OBJECT WHERE key_field = <fs_data>-ServiceUUID INTO @<fs_data>-point_of_origin2.
    ENDLOOP.

    ct_calculated_data = CORRESPONDING #( lt_original_data ).
  ENDMETHOD.

  METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
    " Specify original fields needed for calculation (optimize by listing dependencies)
    " Example: If calculation depends on ServiceUUID
    APPEND 'SERVICEUUID' TO et_requested_orig_elements.

    " Validate requested elements
    LOOP AT it_requested_calc_elements ASSIGNING FIELD-SYMBOL(<fs_element>).
      CASE <fs_element>.
        WHEN 'POINT_OF_ORIGIN2'.
          " Supported
        WHEN OTHERS.
          RAISE EXCEPTION TYPE cx_sadl_exit_calc_invalid_param
            EXPORTING iv_reason = 'Unsupported element: ' && <fs_element>.
      ENDCASE.
    ENDLOOP.
  ENDMETHOD.

ENDCLASS.
