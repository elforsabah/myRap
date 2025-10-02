" Step 4: Set action result ($self = Service instances)
  READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_service_result).

  result = VALUE #( FOR ls_service IN lt_service_result ( %tky   = ls_service-%tky
                                                         %param = ls_service ) ).
