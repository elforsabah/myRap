SM34 → view cluster BSPVC_DLC_FS → Pflegen

Select fact sheet /WATP/EWA_ACCOUNT_FS
Go to the view assignments and find the row for ZWR_IC_OV_ACCT/Qmel
Copy it (copy-as, rather than typing a fresh row — it carries the right usage fields)
Change:

Component Name  : ZWR_IC_OV_ACCT
Interface View  : ZWR_IC_OV_ACCT/Holiday
Inbound Plug    : FROM_IC
Maintain the title in the text table — at least German and English. This is where Qmel's stray "Services" comes from, so set yours properly: Urlaubstage / Holidays
Save to a customizing request
