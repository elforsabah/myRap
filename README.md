REPLACE WITH:
  ELSE.
    " Success - add messages for each service
    LOOP AT lt_services ASSIGNING <ls_service>.
      DATA(ls_param_msg) = keys[ %tky = <ls_service>-%tky ]-%param.
      
      IF ls_param_msg-anlage IS NOT INITIAL.
        " Facility was assigned
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message( id = 'Z_MSG_SVR_TOUR_EXT'
                                            number = '015' ... )
                      ) TO reported-service.
      ELSE.
        " Facility was cleared
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message( id = 'Z_MSG_SVR_TOUR_EXT'
                                            number = '016' ... )
                      ) TO reported-service.
      ENDIF.
    ENDLOOP.
