" 6e — Single VBAP position for this specific service
"       FIX: use EWA_ORDER_OBJECT.SDAUFNR + SDPOSNR
"            NOT reference_id which is the whole SD contract
DATA lt_positions TYPE tt_positions.
CLEAR lt_positions.

IF ls_ewa-sdaufnr IS NOT INITIAL AND ls_ewa-sdposnr IS NOT INITIAL.
  SELECT SINGLE vbeln, posnr, matnr, arktx, kwmeng, meins, netpr
    FROM vbap
    WHERE vbeln = @ls_ewa-sdaufnr
      AND posnr = @ls_ewa-sdposnr
    INTO @DATA(ls_vbap).

  IF sy-subrc = 0.
    APPEND VALUE #(
      sort_number      = ls_vbap-posnr
      item_number      = ls_vbap-matnr
      item_description = ls_vbap-arktx
      quantity         = ls_vbap-kwmeng
      unit             = ls_vbap-meins
      item_price       = ls_vbap-netpr
      position_type    = 'P'
    ) TO lt_positions.
  ENDIF.

ELSEIF ls_svc-reference_id IS NOT INITIAL.
  " Fallback: if EWA has no SD position link, try reference_id
  " (this path should rarely trigger once SDAUFNR is populated)
  SELECT vbeln, posnr, matnr, arktx, kwmeng, meins, netpr
    FROM vbap
    WHERE vbeln = @ls_svc-reference_id
    INTO TABLE @DATA(lt_vbap_fb).

  lt_positions = VALUE #( FOR lp IN lt_vbap_fb
    ( sort_number      = lp-posnr
      item_number      = lp-matnr
      item_description = lp-arktx
      quantity         = lp-kwmeng
      unit             = lp-meins
      item_price       = lp-netpr
      position_type    = 'P' ) ).
ENDIF.
