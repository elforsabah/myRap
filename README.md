extend view entity /PLCE/C_PDMNLServiceWR with 
{
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'  // Reference your new class
//   @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'  // Reference your new class
   virtual point_of_origin : abap.char(1)
   
   
    // Value of this field(flag) will come from the table EWA_ORDER_OBJECT(field ZZPOINT_ORIGIN_WDOI ). 
                                                   // It defines if a service has been replanned
  
//  Similarly to how tours in the Planning Cockpit get a red bar at the start of the line to
//   indicate that a deviation occurred in the tour, we will implement a bar at the start of the
//    line of the service to indicate that this service has been replanned. This means that we 
//    extend the services to include a flag whether the service was created from a copied WDOI. 
//    A filter option on this flag will be added to the Planning Cockpit.

}
