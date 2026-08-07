Locating it — in order of certainty
1. SE03 → Requests/Tasks → Find Requests

Owner: same user as S4DK902986
Date: same range
Include Customizing requests
A customizing entry can never sit in the Development/Correction task you already found, so if it was transported it's in a separate request — and that request names the table outright. This is the one route that can't go wrong.

2. SE11 table search

F4 on the table name and search these patterns:


CRMC*FS*
/WATP/*FS*
Also try searching table descriptions for Fact Sheet. You want a table whose fields are component name / interface view / inbound plug / title — the exact columns from the available list. Then SE16 on it with INTERFACE_VIEW = ZWR_IC_OV_ACCT/Qmel to confirm, and copy that row.

Worth trying the /WATP/ pattern seriously: the fact sheet ID is in the /WATP/ namespace, so the definition may well live in an add-on table rather than a CRMC_ one.

3. SPRO → 
