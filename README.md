
  METHOD if_sadl_exit_calc_element_read~calculate.
    FIELD-SYMBOLS: <ls_original> TYPE any,
                   <ls_calculated> TYPE any,
                   <lv_ServiceUUID> TYPE sysuuid_x16,
                   <lv_point_of_origin> TYPE abap_char1.

    CLEAR ct_calculated_data.

    LOOP AT it_original_data ASSIGNING <ls_original>.
      APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
      MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

      ASSIGN COMPONENT 'SERVICEUUID' OF STRUCTURE <ls_original> TO <lv_ServiceUUID>.
      IF sy-subrc = 0.
        DATA(lv_zzpoint_origin_wdoi) = VALUE abap_char1( ).
        " Fetch via compliant CDS (not direct table)
        SELECT SINGLE point_of_origin  " aliased field for ZZPOINT_ORIGIN_WDOI
          FROM zi_wr_ewa_order_object
          WHERE PdServiceUuid = @<lv_ServiceUUID>
          INTO @lv_zzpoint_origin_wdoi.

        IF sy-subrc = 0.
          " Map to '1' (red/critical if replanned) or '0' (neutral)
          ASSIGN COMPONENT 'POINT_OF_ORIGIN' OF STRUCTURE <ls_calculated> TO <lv_point_of_origin>.
          IF sy-subrc = 0.
            <lv_point_of_origin> = COND #( WHEN lv_zzpoint_origin_wdoi = 'X' THEN 0 ELSE 1 ).
          ENDIF.
        ELSE.
          " Fallback: Neutral if no data
          IF <lv_point_of_origin> IS ASSIGNED.
            <lv_point_of_origin> = '0'.
          ENDIF.
        ENDIF.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.


This the extension with the virtual element field

extend view entity /PLCE/C_PDMNLServiceWR with 
{
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'  // Reference your new class
//   @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'  // Reference your new class
   virtual point_of_origin : abap.int1
   
    // Value of this field(flag) will come from the table EWA_ORDER_OBJECT(field ZZPOINT_ORIGIN_WDOI ). 
                                                   // It defines if a service has been replanned
  
//  Similarly to how tours in the Planning Cockpit get a red bar at the start of the line to
//   indicate that a deviation occurred in the tour, we will implement a bar at the start of the
//    line of the service to indicate that this service has been replanned. This means that we 
//    extend the services to include a flag whether the service was created from a copied WDOI. 
//    A filter option on this flag will be added to the Planning Cockpit
}



getting this error

ABAP Runtime error 'ASSIGN_TYPE_CONFLICT'

