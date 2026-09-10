REPORT z_test_tour_gendoc.

PARAMETERS p_uuid TYPE /plce/pdtour_uuid.

START-OF-SELECTION.

  MODIFY ENTITIES OF /plce/r_pdtour
    ENTITY tour EXECUTE tourgenerateDocument
    FROM VALUE #( ( %tky-touruuid = p_uuid ) )
    FAILED DATA(ls_f) REPORTED DATA(ls_r).

  IF ls_f IS INITIAL.
    COMMIT ENTITIES RESPONSE OF /plce/r_pdtour
      FAILED DATA(ls_sf) REPORTED DATA(ls_sr).
  ENDIF.

  LOOP AT ls_r-tour INTO DATA(ls_m).
    WRITE: / ls_m-%msg->if_message~get_text( ).
  ENDLOOP.
