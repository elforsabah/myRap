CLASS zcl_wr_service_extend_calc DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES if_sadl_exit_calc_element_read.

  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_wr_service_extend_calc IMPLEMENTATION.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    " If your calculation for point_of_origin depends on other CDS fields, list them here.
    " Example: If it depends on ServiceType, add:
    " INSERT 'SERVICETYPE' INTO TABLE et_requested_orig_elements.
    " For no dependencies, leave empty.
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~calculate.
    " Type the input/output to match your CDS projection view structure
    DATA: lt_original_data TYPE STANDARD TABLE OF /plce/c_pdmnlservicewr,
          lt_calculated_data TYPE STANDARD TABLE OF /plce/c_pdmnlservicewr.

    lt_original_data = it_original_data.  " Input: Original data from CDS

    LOOP AT lt_original_data ASSIGNING FIELD-SYMBOL(<ls_original>).
      APPEND INITIAL LINE TO lt_calculated_data ASSIGNING FIELD-SYMBOL(<ls_calculated>).
      MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

      " Add your custom logic for point_of_origin here
      " Example: Simple static value
      <ls_calculated>-point_of_origin = 1.

      " Or based on other fields, e.g.:
      " IF <ls_original>-service_type = 'ABC'.
      "   <ls_calculated>-point_of_origin = 1.
      " ELSE.
      "   <ls_calculated>-point_of_origin = 0.
      " ENDIF.
    ENDLOOP.

    ct_calculated_data = lt_calculated_data.  " Output: Return calculated data
  ENDMETHOD.

ENDCLASS.
