 <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_WEIGHBRIDGE_HELPER=>CL_GET_OPEN_WEIGHPROC
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_CONTRACT                    TYPE        VBELN_VA
* | [<-()] RV_WEIGHINGNR                  TYPE        EWAWA_WEIGHINGNR
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method CL_GET_OPEN_WEIGHPROC.
    types:
      begin of TY_WEIGHINGLEAD,
        WEIGHINGNR_LEAD type EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD,
      end of TY_WEIGHINGLEAD.
    data:
      LV_CONTRACT     type VBELN_VA,
      LV_GROSSID      type EWA_WA_WEIGHPROC-GROSS_ID,
      LV_TAREID       type EWA_WA_WEIGHPROC-TARE_ID,
      LV_WEIGHINGLEAD type EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD,
      LV_MAX          type INT4 value 1,
      LT_WEIGHINGNR   type standard table of TY_WEIGHINGLEAD.

    clear RV_WEIGHINGNR.
    if IV_CONTRACT is not initial.
      DATA_BO_RANGE X.
      SET_BO_RANGE X CONTRACT_VBELN IV_CONTRACT VBELN_VA.
      SET_BO_RANGE_CMPLT X GROSS_ID LV_GROSSID EWA_WA_WEIGHPROC-GROSS_ID I NE.
      SET_BO_RANGE_CMPLT X TAREID LV_TAREID EWA_WA_WEIGHPROC-TARE_ID I EQ.
      SET_BO_RANGE_CMPLT X WEIGHINGNR_LEAD LV_WEIGHINGLEAD EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD I NE.

      CL_EEWA_OR_WEIGHINGPROCESS=>CL_READ(
        exporting
          PAR_OBJTYPE = CL_EEWA_BO_WEIGHINGPROCESS=>COT_WEIGHINGPROCESS
          PAR_DOMAIN  = 'OPEN'
          PAR_RANGES  = LRANGEXS
          PAR_MAXROWS = LV_MAX
        importing
          PAR_TABLE   = LT_WEIGHINGNR
      ).
      if LINES( LT_WEIGHINGNR ) > 0.
        RV_WEIGHINGNR = LT_WEIGHINGNR[ 1 ]-WEIGHINGNR_LEAD.
      endif.
    endif.
  endmethod.
ENDCLASS.
