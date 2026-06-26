
" 6m OrderNumber — smaufnr with ALPHA conversion and trailing space trim
        "    Falls back to lv_order_ref when smaufnr is initial to avoid
        "    sending a blank string that BMS rejects as required field
        DATA(lv_smaufnr_condensed) = condense( ls_ewa-smaufnr ).
        DATA(lv_order_number) = COND string(
          WHEN lv_smaufnr_condensed IS NOT INITIAL
          THEN |{ ls_ewa-smaufnr ALPHA = OUT }|
          ELSE lv_order_ref ).
