


Step 1 — Create the helper method (start here)
In SE24 → ZCL_WR_CCMG_MISC, add a public static method GET_CONTRACT_WASTE:

Importing: IV_VBELN TYPE VBELN, IV_POSNR TYPE POSNR_VA
Returning: RT_MATNR TYPE RSELOPTION (range of MATNR)

method get_contract_waste.
  constants lc_char_wastes type atnam value 'Z_CHAR_WASTELIST_WASTES'.

  select single cuobj from vbap
    where vbeln = @iv_vbeln and posnr = @iv_posnr
    into @data(lv_cuobj).
  if sy-subrc <> 0 or lv_cuobj is initial.
    return.
  endif.

  data lt_config type table of /* CONFIGURATION line type from SE37 */ cuxt_conf_line.
  call function 'VC_I_GET_CONFIGURATION'
    exporting instance      = lv_cuobj
             language       = sy-langu
    tables   configuration  = lt_config
    exceptions others       = 4.
  if sy-subrc <> 0.
    return.
  endif.

  loop at lt_config into data(ls) where atnam = lc_char_wastes.
    append value #( sign = 'I' option = 'EQ' low = ls-atwrt ) to rt_matnr.  " ATWRT already MATNR-18
  endloop.
endmethod.
Test it immediately with a small report or SE24 test (F8), passing VBELN='0040000319', POSNR='000010'. You should get back …3113 and …3115. ✅ Don't move on until this returns the right materials.

When you create the method, double‑click the CONFIGURATION parameter in SE37 for VC_I_GET_CONFIGURATION to get its exact line type, and use that for lt_config. You only rely on fields ATNAM and ATWRT.

Step 2 — Add the contract carriers to the search help
In SE11 → /WATP/HCMA_WASTE_AVVCODE → Definition, add two parameters:

Parameter	Data element	IMP	EXP	LPos	SPos
VBELN	VBELN_VA	✅	⬜	0	0
POSNR	POSNR_VA	✅	⬜	0	0
LPos=0/SPos=0 keep them invisible in the F4 dialog (no extra filter fields, no extra columns). Activate. (Access key needed — add‑on object.)

Step 3 — Pass the contract into the F4
In SE24/SE80 → ZL_CN_ZWRELOCSD_NEW → GET_V_ZZWASTE_DEF, add the two mappings (copy the pattern from GET_V_ZZWDF_DEF in the same class):


IV_INPUT_MAPPING = value #(
    ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' )
    ( CONTEXT_ATTR = 'STRUCT.VBELN'       F4_ATTR = 'VBELN' )   "<- add
    ( CONTEXT_ATTR = 'STRUCT.POSNR'       F4_ATTR = 'POSNR' ) ) "<- add
Leave IV_OUTPUT_MAPPING unchanged. Activate.

Step 4 — Inject the restriction in the exit
The exit at STEP = 'SELECT' must read VBELN/POSNR, call your helper, and add the result as MATNR selopts (the sub‑function already maps MATNR → V_EWAEL_WASTE~MATNR).

Preferred — via the BAdI (no add‑on modification). Both /WATP/CMA_WASTE_AVV_SHLP_EXIT and …/ARB… start with the macro SEARCHHELP_CALL_BADI. Expand that macro (double‑click it / where‑used) to get the BAdI definition name, then create a customer implementation that does:


read table SHLP-SELOPT into data(ls_v) with key shlpfield = 'VBELN'.
read table SHLP-SELOPT into data(ls_p) with key shlpfield = 'POSNR'.
delete SHLP-SELOPT where shlpfield = 'VBELN'.
delete SHLP-SELOPT where shlpfield = 'POSNR'.

if ls_v-low is not initial.
  data(lr) = zcl_wr_ccmg_misc=>get_contract_waste( iv_vbeln = ls_v-low iv_posnr = ls_p-low ).
  loop at lr into data(r).
    append value #( shlpfield = 'MATNR' sign = r-sign option = r-option low = r-low ) to SHLP-SELOPT.
  endloop.
  if lr is initial.
    append value #( shlpfield = 'MATNR' sign = 'I' option = 'EQ' low = '##########' ) to SHLP-SELOPT.
  endif.
endif.
This keeps everything in your namespace. ✅ Verify the BAdI runs at the SELECT step and gives a changeable SHLP.

Fallback — modify /WATP/ARB_WASTE_AVV_SHLP_EXIT directly (access key) — same code, placed at the start of the BEGIN_SEARCHHELP_SELECT … END_SEARCHHELP_SELECT block, before the existing SELECT. Also make sure VBELN/POSNR are deleted from SHLP-SELOPT so they don't reach F4_CONV_SELOPT_TO_WHERECLAUSE (they're not columns of the view).

Step 5 — End‑to‑end test in the IC
Open the IC → BP Carl's Jr. (or inka* / contract 40000275 from the spec) → Standard Container Information of a container that belongs to a contract.
Open the Waste F4. → It should now list only the contract's active fractions (e.g. 3113 / 3115), not all 26.
Check a container on a different contract → different fraction set.
Edge case: a brand‑new container line with no contract yet (STRUCT.VBELN empty) → helper returns empty → list empty. Decide if that's acceptable (matches the spec "a selection must be made") or if you want a fallback (see below).
Two decisions before you start
Modification vs BAdI for Steps 2 & 4. Step 2 (adding params) is unavoidably a modification of the add‑on search help. Step 4 can be a BAdI. If your team forbids add‑on modifications, tell me and I'll work out a fully KEY_REF‑resolver variant that adds no search‑help parameters (Channel 2 from the WDF exit) — more code, zero add‑on changes.
Empty‑VBELN behavior: restrict to nothing (strict, per the acceptance criterion) vs. fall back to the old full list during creation. I'd go strict; say the word if you want the fallback guard.
Want me to expand the BAdI implementation once you've pulled the BAdI name from SEARCHHELP_CALL_BADI, or produce the no‑modification KEY_REF variant?
