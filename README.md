extend view entity /PLCE/R_PDServiceExtCustom with
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on   _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
association [1] to /plce/tpdsrv as _tpdsrv  on  _tpdsrv.service_uuid  = $projection.ServiceUUID
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _tpdsrv,
  _ZZOrderObject
}

extend view entity /PLCE/C_PDMNLServiceWR with 
{
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'  // Reference your new class
   virtual service_criticality : abap.int1,
   /PLCE/R_PDService._ExtCustom._tpdsrv.zzpoint_origin_wdoi as point_of_origin
   // Value of this field(flag) will come from the table EWA_ORDER_OBJECT(field ZZPOINT_ORIGIN_WDOI ). 
                                                   // It defines if a service has been replanned
  
//  Similarly to how tours in the Planning Cockpit get a red bar at the start of the line to
//   indicate that a deviation occurred in the tour, we will implement a bar at the start of the
//    line of the service to indicate that this service has been replanned. This means that we 
//    extend the services to include a flag whether the service was created from a copied WDOI. 
//    A filter option on this flag will be added to the Planning Cockpit.



}

METHOD if_sadl_exit_calc_element_read~calculate.
  FIELD-SYMBOLS: <ls_original>             TYPE any,
                 <ls_calculated>           TYPE any,
                 <lv_RefId>                TYPE any,
                 <lv_service_criticality>  TYPE int1,
                 <lv_point_of_origin_wdoi> TYPE zwr_point_origin_wdoi.

  CLEAR ct_calculated_data.
  DATA lv_test TYPE String.
  LOOP AT it_original_data ASSIGNING <ls_original>.
    APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
    MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

    ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_RefId>.
    IF sy-subrc = 0.
      DATA lv_zzpoint_origin_wdoi TYPE zwr_point_origin_wdoi.

      " Fetch via compliant CDS (not direct table)
      SELECT SINGLE point_of_origin  " aliased field for ZZPOINT_ORIGIN_WDOI
        FROM zi_wr_ewa_order_object
        WHERE referenceid = @<lv_RefId>
        INTO @lv_zzpoint_origin_wdoi.
      IF sy-subrc = 0.
        " Map to 1 (red/critical if replanned) or 0 (neutral) — fixed reversed logic
        ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
        IF sy-subrc = 0.
          <lv_service_criticality> = COND #( WHEN lv_zzpoint_origin_wdoi IS NOT INITIAL THEN 1 ELSE 0 ).
        ENDIF.

        ASSIGN COMPONENT 'POINT_OF_ORIGIN' OF STRUCTURE <ls_calculated> TO <lv_point_of_origin_wdoi>.
        IF sy-subrc = 0.
          <lv_point_of_origin_wdoi> = lv_zzpoint_origin_wdoi.
        ENDIF.
      ELSE.
        " Fallback: Neutral if no data
        IF <lv_service_criticality> IS ASSIGNED.
          <lv_service_criticality> = 0.
        ENDIF.
      ENDIF.

ZCL_WR_SERVICE_EXTEND_CALC
    ENDIF.
  ENDLOOP.
ENDMETHOD.
