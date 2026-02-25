" ====================================================================
      " NEW: Forcing the Fiori UI to refresh the Tour (Cross-BO Hack)
      " ====================================================================
      " A. Find the Tour(s) associated with this Service
      SELECT TourUUID
        FROM /PLCE/R_PDTourServiceAsgmt 
        WHERE ServiceUUID = @<ls_key>-ServiceUUID
        INTO TABLE @DATA(lt_tours).

      IF sy-subrc = 0.
        " B. Read the current status of the Tour (NO 'IN LOCAL MODE' ALLOWED HERE)
        READ ENTITIES OF /PLCE/R_PDTour
          ENTITY Tour
          FIELDS ( TourStatus )
          WITH VALUE #( FOR ls_tour IN lt_tours ( TourUUID = ls_tour-TourUUID ) )
          RESULT DATA(lt_tour_data).

        " C. Write the EXACT SAME status back to the Tour. 
        " This changes no data, but registers the Tour as 'modified' in the framework!
        IF lt_tour_data IS NOT INITIAL.
          MODIFY ENTITIES OF /PLCE/R_PDTour
            ENTITY Tour
            UPDATE FIELDS ( TourStatus )
            WITH VALUE #( FOR ls_data IN lt_tour_data ( 
                            TourUUID   = ls_data-TourUUID 
                            TourStatus = ls_data-TourStatus ) ).
        ENDIF.
      ENDIF.
      " ====================================================================
