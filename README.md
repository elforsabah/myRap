1. SE16 → /WATP/TARBNW → Execute with no selection.

Record three things:

Are there any entries at all?
Are AVVCODE and WDPLANT filled, and do they match your popup (150101 → 7, 200301 → 67)?
Is there more than one row per AVVCODE? ← this is the one that matters most, see below.
2. SE11 → /WATP/TARBNW → Fields → confirm WASTEMODE is a column.

It must be, because AVV_CHECK_SPEC compares PAR_AVV_ITEM->WASTEMODE <> LARB_NW-WASTEMODE. The reader just doesn't offer it as a range. Confirm it rather than assume — if it sits on a detail table instead, the SELECT below changes.

3. SE11 → /WATP/SCS_CABAVVD_DP → Where-Used List → Programs + Classes.

You sent me the structure's 62 components, but not the where-used. That list is what identifies the ALV builder, and without it Step 6 (making the columns non-input) has no target.
