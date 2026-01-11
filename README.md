METHOD assign_earliest_to_latest_date.

  "Read current transactional buffer values
  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
      FIELDS ( EarliestDate LatestDate )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_tour).

  "Only update when LatestDate is initial (or adapt if you want to always overwrite)
  DATA(lt_upd) =
    VALUE TABLE FOR UPDATE /PLCE/R_PDTour\Tour(
      FOR ls IN lt_tour
      WHERE ( ls-EarliestDate IS NOT INITIAL AND ls-LatestDate IS INITIAL )
      ( %tky       = ls-%tky
        LatestDate = ls-EarliestDate )
    ).

  IF lt_upd IS INITIAL.
    RETURN.
  ENDIF.

  MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
      UPDATE FIELDS ( LatestDate )
      WITH lt_upd
    REPORTED DATA(lt_rep)
    FAILED   DATA(lt_fai).

  "If you want, forward reported/failed (not mandatory)
  reported = CORRESPONDING #( DEEP lt_rep ).
  failed   = CORRESPONDING #( DEEP lt_fai ).

ENDMETHOD.
