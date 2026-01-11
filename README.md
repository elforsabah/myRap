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
      ).

ENDMETHOD.
