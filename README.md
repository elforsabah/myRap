METHOD getdefaultsforterminateservice.

  DATA lv_text TYPE tewaconftypet-vtext.

  SELECT SINGLE vtext
    FROM tewaconftypet
    WHERE spras    = @sy-langu
      AND conftype = 'PLSTORNO'
    INTO @lv_text.

  result = VALUE #(
    FOR key IN keys
    (
      %tky                    = key-%tky
      %param-definiertegrund  = 'PLSTORNO'
      %param-grundtext        = lv_text
    )
  ).

ENDMETHOD.
