function /PLCP/BGPROCESSUNIT.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(I_BGPUNIT) TYPE  /PLCE/UUID
*"----------------------------------------------------------------------
  read entities of /PLCE/R_BGProcessingUnit entity ProcessingUnit
       all fields with value #( ( ProcessingUnitUUID = i_bgpunit ) )
     result data(units).

  loop at units assigning field-symbol(<unit>).

    " call of /plce/cl_bgprocessing_helper=>deserializeprocessingunit would only allow classes in /PLCE/ namespace or C1 released objects
    "data(ref) = /plce/cl_bgprocessing_helper=>deserializeprocessingunit( exporting i_value = <unit>-Content ).
    data: ref type ref to /plce/if_bgprocessing_unit.
    call transformation id
             source xml <unit>-Content
             result model = ref.

    if ref is not initial.
      try.

          data(retry_possible) = /plcp/cl_bgprocessing=>is_retry_possible( i_bgpunit = <unit>-ProcessingUnitUUID ).

          ref->execute( i_retry_possible = retry_possible i_bgpunit = <unit>-ProcessingUnitUUID ).
          modify entities of /PLCE/R_BGProcessingUnit entity ProcessingUnit
            delete from value #( ( corresponding #( <unit> ) ) ).

        catch /plce/cx_bgp_unit into data(lex_bgp).

          rollback entities. "#EC CI_ROLLBACK

          data: units_update type table for update /PLCE/R_BGProcessingUnit,
                lex_org type ref to CX_ROOT.

          append value #( %tky = <unit>-%tky Status = <unit>-Status  ) to units_update assigning field-symbol(<unit_update>).

          "collect message
          try.
            lex_org = cond #( when lex_bgp->previous is not initial then lex_bgp->previous else lex_bgp ).
            data(bapiret) = /plce/cl_base_misc=>convert_to_plce_exception( lex_org )->get_as_bapiret( ).
          catch /plce/cx_baseexception.
          endtry.

          <unit_update>-SystemMessageClass = bapiret-id.
          <unit_update>-SystemMessageNumber = bapiret-number.
          <unit_update>-SystemMessageVariable1 = bapiret-message_v1.
          <unit_update>-SystemMessageVariable2 = bapiret-message_v2.
          <unit_update>-SystemMessageVariable3 = bapiret-message_v3.
          <unit_update>-SystemMessageVariable4 = bapiret-message_v4.

          <unit_update>-%control-SystemMessageClass = if_abap_behv=>mk-on.
          <unit_update>-%control-SystemMessageNumber = if_abap_behv=>mk-on.
          <unit_update>-%control-SystemMessageVariable1 = if_abap_behv=>mk-on.
          <unit_update>-%control-SystemMessageVariable2 = if_abap_behv=>mk-on.
          <unit_update>-%control-SystemMessageVariable3 = if_abap_behv=>mk-on.
          <unit_update>-%control-SystemMessageVariable4 = if_abap_behv=>mk-on.

          if lex_bgp->retry = 'X'.
            data(retry_success) = /plcp/cl_bgprocessing=>retry_unit( i_bgpunit = <unit>-ProcessingUnitUUID ).
          elseif lex_bgp->postpone = 'X'.
             <unit_update>-Status = /plce/if_constants=>c_bgp_unit_status-postponed.
             <unit_update>-%control-Status = if_abap_behv=>mk-on.
             clear: <unit_update>-BGRFCUnitId, <unit_update>-BGRFCLockId.
             <unit_update>-%control-BGRFCUnitId = if_abap_behv=>mk-on.
             <unit_update>-%control-BGRFCLockId = if_abap_behv=>mk-on.
          endif.

          modify entities of /PLCE/R_BGProcessingUnit entity ProcessingUnit
            update from units_update.

          commit entities.

          if not (
          ( lex_bgp->retry = abap_true and retry_success = abap_true )
           or ( lex_bgp->postpone = 'X' )
          ).
            message lex_org type 'E'.
          endif.

        catch cx_root into data(lex).
          rollback entities. "#EC CI_ROLLBACK
          message lex type 'E'.
      endtry.
    endif.

  endloop.

endfunction.
