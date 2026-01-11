METHODS assign_start_to_end_date FOR DETERMINE ON MODIFY
  IMPORTING keys FOR Tour~assign_start_to_end_date
  CHANGING
    reported TYPE RESPONSE FOR REPORTED Tour
    failed   TYPE RESPONSE FOR FAILED   Tour.



METHOD assign_start_to_end_date.

  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
      FIELDS ( StartDate EndDate )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_tour).

  MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
      UPDATE FIELDS ( EndDate )
      WITH VALUE #(
        FOR ls IN lt_tour
        WHERE ( ls-StartDate IS NOT INITIAL AND ls-EndDate IS INITIAL )
        ( %tky    = ls-%tky
          EndDate = ls-StartDate )
      )
    FAILED   DATA(lt_fai)
    REPORTED DATA(lt_rep).

  "Forward framework responses
  APPEND LINES OF lt_fai-tour    TO failed-tour.
  APPEND LINES OF lt_rep-tour    TO reported-tour.
  APPEND LINES OF lt_rep-%other  TO reported-%other.

ENDMETHOD.
