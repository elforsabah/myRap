" Attempt to match using the Service UUID first (if the BO item already has it)
" If not, fall back to matching by the specific item Sequence Number (LAUFNR)
loop at ORDERITEMRESULTS reference into data(LRESULTREF) 
  where POBJNR = LITEMKEYREF->POBJNR
    and ( /PLCP/PD_SERVICE_UUID = LITEMREF->/PLCP/PD_SERVICE_UUID " Match by UUID if available
          or ( LITEMREF->/PLCP/PD_SERVICE_UUID is initial         " OR if UUID is blank...
               and LAUFNR = LITEMREF->LAUFNR ) ).                 " Match by standard SAP Sequence Number
