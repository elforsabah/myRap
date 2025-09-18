Description	Resource	Path	Location	Type
No component exists with the name "ZZPOINT_ORIGIN_WDOI".	ZBP_ZPD_BP_R_PDSERVICE (Local Types)	/S4D_100_prologaefo_en/.adt/classlib/classes/zbp_zpd_bp_r_pdservice	line 33	ABAP Syntax Check Problem


extension implementation in class zbp_zpd_bp_r_pdservice unique;

extend behavior for Service
{
  determination ZZServiceCreateDetermination on modify { create; }

  determination SetPointOfOrigin on modify { create; update; }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
 field ( suppress ) zzpobjnr;

// field ( suppress  ) point_of_origin;

}

class lhc_Service definition inheriting from CL_ABAP_BEHAVIOR_HANDLER.
  private section.

    methods ZZServiceCreateDetermination for determine on modify
      importing KEYS for Service~ZZServiceCreateDetermination.

    methods SetPointOfOrigin for determine on modify
      importing KEYS for Service~SetPointOfOrigin.


endclass.

class lhc_Service implementation.

 METHOD setpointoforigin.  
 
 READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
    FIELDS ( referenceid )
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services).

  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
    DATA lv_zzpoint_origin_wdoi TYPE zwr_point_origin_wdoi.
    SELECT SINGLE point_of_origin
      FROM zi_wr_ewa_order_object
      WHERE referenceid = @<ls_service>-ReferenceInternalId
      INTO @lv_zzpoint_origin_wdoi.

    IF sy-subrc = 0 AND lv_zzpoint_origin_wdoi IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
        UPDATE FIELDS ( ZZPOINT_ORIGIN_WDOI )
        WITH VALUE #( ( %key-ServiceUUID = <ls_service>-ServiceUUID
                        ZZPOINT_ORIGIN_WDOI = lv_zzpoint_origin_wdoi ) ).
    ENDIF.
  ENDLOOP.
  
 ENDMETHOD.

  method ZZServiceCreateDetermination.
    data:
      LV_CID            type STRING value '$abap_cid1_',
      LT_SERVICE_CREATE type table for create /PLCE/R_PDService\_ExtCustom.

    loop at KEYS reference into data(LKEY).
      insert initial line into table LT_SERVICE_CREATE assigning field-symbol(<LS_SERVICE_CREATE>) else unassign.
      if <LS_SERVICE_CREATE> is assigned.
        <LS_SERVICE_CREATE>-%TKY = LKEY->%TKY.
        <LS_SERVICE_CREATE>-%TARGET = value #( ( %CID = LV_CID && SY-TABIX ) ).
      endif.
    endloop.


    modify entities of /PLCE/R_PDService in local mode
      entity Service
       create by \_ExtCustom
         auto fill cid set fields with LT_SERVICE_CREATE
       mapped data(LMAPPED)
       failed data(LFAILED)
       reported data(LREPORTED).

    /PLCE/CL_BASE_MISC=>CHECK_RESPONSE( exporting IS_RESPONSE = LREPORTED-%OTHER changing CT_MESSAGES = REPORTED-%OTHER ).
  endmethod.

endclass.


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

