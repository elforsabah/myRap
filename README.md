Check this before anything else: SE24 → /WATP/CL_BO_CSCABNAVV_BASE → look for AVV_CHANGING (or AVV_GET) in the method list, by analogy with AVVS_CHANGING. If it exists, that's the correct writable path:


        loop at <TABLE_Z> assigning <ITEM_Z>.
          check <ITEM_Z>-WASTEMODE is initial.
          AVV_CHANGING( exporting PAR_DETAILINDEX = <ITEM_Z>-DETAIL_INDEX
                        importing PAR_AVV_ITEM    = data(LITEM_Z) ).
          if LITEM_Z is bound.
            LITEM_Z->WASTEMODE = LDEFMODE_Z.
            AVV_CHANGED( PAR_DETAILINDEX = LITEM_Z->DETAIL_INDEX ).
          endif.
        endloop.
