" Use both POBJNR and the unique Service UUID to find the exact row
  if LINE_EXISTS( ORDERITEMRESULTS[ POBJNR = IO_BO_WDOI->DATAREF->POBJNR 
                                    /PLCP/PD_SERVICE_UUID = IO_BO_WDOI->DATAREF->/PLCP/PD_SERVICE_UUID ] ).
                                    
    LTOURID = ORDERITEMRESULTS[ POBJNR = IO_BO_WDOI->DATAREF->POBJNR 
                                /PLCP/PD_SERVICE_UUID = IO_BO_WDOI->DATAREF->/PLCP/PD_SERVICE_UUID ]-/PLCP/PD_TOUR_ID.
  endif.
