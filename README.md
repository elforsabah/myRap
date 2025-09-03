METHOD identifyCard.
  READ ENTITIES OF ZI_WR_WEIGHINGSESSION IN LOCAL MODE
    ENTITY WeighingSession
    FIELDS ( Vbeln ) WITH CORRESPONDING #( keys )
    RESULT DATA(lt_sessions).

  " Validate contract existence
  SELECT SINGLE * FROM vbak WHERE vbeln = @parameters-vbeln INTO @DATA(ls_contract).
  IF sy-subrc <> 0.
    FAILED-weighingsession = CORRESPONDING #( keys ).
    REPORTED-weighingsession = VALUE #( %msg = new_message( id = 'Z_WR_MSG' number = '001' severity = ms-error v1 = parameters-vbeln ) ).
    RETURN.
  ENDIF.

  " Proceed with identification logic if valid
  " ...
ENDMETHOD.
