"Detect ambiguity: same material multiple times in contract
    DATA(lv_hits) = 0.
    DATA(ls_ctr)  = VALUE ty_ctr( ).
    LOOP AT lt_ctr INTO ls_ctr WHERE matnr = lv_matnr.
      lv_hits += 1.
    ENDLOOP.
