 " 6e Positions from VBAP
      DATA lt_positions TYPE tt_positions.
      CLEAR lt_positions.
      IF ls_svc-reference_id IS NOT INITIAL.
        SELECT vbeln, posnr, matnr, arktx, kwmeng, meins, netpr
          FROM vbap
          WHERE vbeln = @ls_svc-reference_id
          INTO TABLE @DATA(lt_vbap).

        lt_positions = VALUE #( FOR ls_p IN lt_vbap
          ( sort_number      = ls_p-posnr
            item_number      = ls_p-matnr
            item_description = ls_p-arktx
            quantity         = ls_p-kwmeng
            unit             = ls_p-meins
            item_price       = ls_p-netpr
            position_type    = 'P' ) ).
      ENDIF.
