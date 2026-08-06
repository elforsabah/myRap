Step 0 — Analyse the existing Qmel tab (do this first)
SE03 → Objekte in Aufträgen suchen → search ZWR_IC_OV_ACCT* and *QMEL*. The transport that delivered the Qualitätsmeldungen tab lists every object that was touched — repository, view classes, configuration, and the fact sheet customizing entry. That last one is what you need for step 6, and it's the only piece I can't determine from the screenshots.

Then SE80 → open ZL_*QMELDATA_IMPL and read DO_PREPARE_OUTPUT / DO_INIT_CONTEXT. You're looking for how it gets the current business partner — you'll reuse that exact code in step 3.

The enhancement set ZWR_ENHSET is already active and the component is already enhanced (ZWR_IC_OV_ACCT exists), so don't press "Enhance Component" again.

Step 1 — DDIC structure
SE11 → create structure ZWR_S_HDAY_LOG with PARTNER, OXYGENID, START_DATE, END_DATE, ERNAM, ERDAT, ADM_ERTIME, AENAM, AEDAT, ADM_AETIME, LOEVM — same field names and data elements as in ZWR_CTP_OP_HDAY, so a MOVE-CORRESPONDING suffices in step 3. Package ZWR_CLASSIC_CRM_CEWA, activate.

Step 2 — Create the view
BSP_WD_CMPWB → component EWA_IC_OV_ACCT, enhancement set ZWR_ENHSET → change mode → right-click Views → Anlegen. In the wizard:

View Name HolidayData (the system prefixes ZWR_IC_OV_ACCT/)
Select Model Nodes → leave empty. There's no BOL object for this table.
Add Value Nodes → HOLIDAYLOG, structure ZWR_S_HDAY_LOG, cardinality 0..n
View Type → Table View, table node HOLIDAYLOG
A value node rather than a GenIL/BOL object is the right call here — building a BOL object for a read-only log is disproportionate.

Step 3 — Fill the node with data
SE24 on the generated ZL_..._HOLIDAYDATA_IMPL → redefine DO_PREPARE_OUTPUT:


lv_partner = me->get_current_partner( ).     " <- Logik aus QmelData
IF lv_partner IS INITIAL. RETURN. ENDIF.

SELECT * FROM zwr_ctp_op_hday INTO TABLE lt_hday
  WHERE partner = lv_partner.                " AK 1 + 2

SORT lt_hday BY start_date DESCENDING
                end_date   DESCENDING.       " AK 3

CREATE OBJECT lr_col.
LOOP AT lt_hday INTO ls_hday.
  CREATE DATA lr_data TYPE zwr_s_hday_log.
  ASSIGN lr_data->* TO <ls_disp>.
  MOVE-CORRESPONDING ls_hday TO <ls_disp>.
  CREATE OBJECT lr_vnode EXPORTING iv_data_ref = lr_data.
  lr_col->add( iv_entity = lr_vnode ).
ENDLOOP.

me->typed_context->holidaylog->collection_wrapper->set_collection( lr_col ).
Add a private attribute GV_LAST_PARTNER and skip the re-read when the partner hasn't changed — DO_PREPARE_OUTPUT fires on every round trip. The full method including that guard is in the file.

GET_CURRENT_PARTNER is the one method you should copy rather than write: which context node holds the BP (BUILHEADER, ACCOUNT, …) and which attribute (BP_NUMBER, PARTNER) depends on how the fact sheet passes it through. That's why step 0.3 comes first.

Step 4 — Window in the Runtime Repository
BSP_WD_CMPWB → Runtime Repository Editor → change mode → right-click Windows → Add Window → Holiday → then add view ZWR_IC_OV_ACCT/HolidayData as default. Save. The editor writes the XML — don't hand-edit the BSP page.

What you leave alone: <Views> (design-time only), the OverviewPage ViewArea (Qmel/Services/SDInvoice aren't there either — it's legacy), <ComponentInterface> (Qmel has no entry and works), <ComponentUsages> (the fact sheet usage ZWR_IC_OV_ACCT0001 is generated at runtime).

Step 5 — Configuration
View → tab Configuration → New Configuration, role key <DEFAULT> (Qmel: sought /WATP/WAIC, found <DEFAULT>). Move fields to Displayed Fields in this order: Gültig ab, Gültig bis, Geändert von, Geändert am, Geändert um, Angelegt von, Angelegt am, Oxygen-ID. Table Navigation = Both.

Do not implement GET_BUTTONS — the Create button visible on Qualitätsmeldungen must not appear here.

Step 6 — Register the tab in the fact sheet
Without this the view exists but stays invisible. Take the customizing entry you found in step 0.2, create an analogous one for ZWR_IC_OV_ACCT/Holiday under object type /WATP/EWA_ACCOUNT_FS, with a sequence number higher than Qmel's so it lands to the right of Qualitätsmeldungen. This goes into a customizing request, not the workbench request.

If you didn't find it via SE03: SPRO → CRM → UI Framework → UI Framework Definition → Fact Sheet. Fallback for locating it: search table BSPC_DL_XMLSTRX2 for Qmel.

Step 7 — Tab label
Maintain where Qualitätsmeldungen is maintained — the F2 popup says Herkunft des Bezeichners: Kundenkonfiguration, so the configuration entry or an OTR text (SOTR_EDIT). Proposed title "Urlaubstage", still to be confirmed.

Step 8 — Test
BP 1000123 in S4D/110: tab appears right of Qualitätsmeldungen; contents match SE16 on ZWR_CTP_OP_HDAY WHERE PARTNER = 1000123 (AK 1); switch to another BP and confirm no foreign rows (AK 2); newest validity on top (AK 3); Geändert von / Geändert am populated; no Create button. Then retest in S4Q/110 with 0052240024 after transport.

Step 9 — Transport
Workbench: structure, BSP application, the three generated classes, configuration, OTR texts. Customizing: the fact sheet registration from step 6. Cross-check your object list against Qmel's from step 0.2 — if something's in theirs and not yours, you've missed a registration point.

Steps 3 and 4 are independent and can run in parallel. Everything before step 6 is invisible in the UI, so don't be alarmed when the tab doesn't show up after step 5.
