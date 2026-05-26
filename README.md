" Get order fields using referenceid instead of pobjnr
SELECT SINGLE zz_tech_fachbe, wdplantnr, zz_pobjnr_main
  FROM zi_wr_ewa_order_object
  WHERE referenceid = @<ls_service>-referenceid  " ✅ Use ReferenceId
  INTO @DATA(ls_order_fields).
