  method ZZGET_ORDERITEMS.
    data:
      LRANGES type ref to /WATP/PRANGES.

    clear CT_ORDERITEMS.

    LRANGES = ZZGET_ORDERITEM_RANGES( EV_SCOPE = EV_SCOPE ).

    /WATP/CL_OR_EWAORDERPOS=>CL_READ(
      exporting
        PAR_OBJTYPE   = /WATP/CL_BO_EWAORDERPOS=>COT_ORDERPOS
        PAR_OBJREADER = /WATP/CL_BO_EWAORDERPOS=>COT_ORDERPOS
        PAR_RANGES    = LRANGES
      importing
        PAR_TABLE     = CT_ORDERITEMS
    ).

**  INFO FLAG ZZWR_AUTO_PROCESS:
**      Standard (automatisch Zurückmelden)
**    X Automatisch Zurückmelden
**    F Fehler beim automatischen Zurückmelden
**    Z automatisches Zurückmelden erfolgt
    delete CT_ORDERITEMS where ZZWR_AUTO_PROCESS = 'F'.  "type ZWR_DEEWA_WDOC_AUTO_PROCESS.
    delete CT_ORDERITEMS where ZZWR_AUTO_PROCESS = 'Z'.

    sort CT_ORDERITEMS by ORDER_DATE ORDERNR ORDER_LAUFNR.

  endmethod.



    method ZZGET_ORDERITEMS.
    data:
      LRANGES type ref to /WATP/PRANGES.

    clear CT_ORDERITEMS.

    LRANGES = ZZGET_ORDERITEM_RANGES( EV_SCOPE = EV_SCOPE ).

    /WATP/CL_OR_EWAORDERPOS=>CL_READ(
      exporting
        PAR_OBJTYPE   = /WATP/CL_BO_EWAORDERPOS=>COT_ORDERPOS
        PAR_OBJREADER = /WATP/CL_BO_EWAORDERPOS=>COT_ORDERPOS
        PAR_RANGES    = LRANGES
      importing
        PAR_TABLE     = CT_ORDERITEMS
    ).

**  INFO FLAG ZZWR_AUTO_PROCESS:
**      Standard (automatisch Zurückmelden)
**    X Automatisch Zurückmelden
**    F Fehler beim automatischen Zurückmelden
**    Z automatisches Zurückmelden erfolgt
    delete CT_ORDERITEMS where ZZWR_AUTO_PROCESS = 'F'.  "type ZWR_DEEWA_WDOC_AUTO_PROCESS.
    delete CT_ORDERITEMS where ZZWR_AUTO_PROCESS = 'Z'.

    sort CT_ORDERITEMS by ORDER_DATE ORDERNR ORDER_LAUFNR.

  endmethod.
