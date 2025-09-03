METHOD get_instance_authorizations.
  READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
    ENTITY WeighingSession
    FIELDS ( sessionid ) WITH CORRESPONDING #( keys )
    RESULT DATA(lt_sessions)
    FAILED failed.

  result = VALUE #( FOR ls_session IN lt_sessions
    ( %tky   = ls_session-%tky
      %create              = if_abap_behv=>auth-allowed
      %update              = if_abap_behv=>auth-allowed
      %delete              = if_abap_behv=>auth-allowed
      %action-identifyCard = if_abap_behv=>auth-allowed
      %action-setloadType  = if_abap_behv=>auth-allowed
      %action-NextStep     = if_abap_behv=>auth-allowed
      %action-Submit       = if_abap_behv=>auth-allowed ) ).
ENDMETHOD.
