" Directly add to result (combine %tky with result entity fields)
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %param = VALUE #( pdfbase64 = lv_base64 ) ) TO result.
