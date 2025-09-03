METHOD identifyCard.
  " Your logic here
  IF contract_is_valid(vbeln).
    " Set entity properties, e.g., entity-vbeln = vbeln
    reported-weighingsession = VALUE #( %msg = new_message( id = 'ZMSG' number = '001' severity = ms-success v1 = 'Valid' ) ).
  ELSE.
    failed-weighingsession = VALUE #( %fail = VALUE #( cause = invalid_entity ) ).
    reported-weighingsession = VALUE #( %msg = new_message( id = 'ZMSG' number = '002' severity = ms-error v1 = 'Invalid Contract' ) ).
  ENDIF.
ENDMETHOD.
