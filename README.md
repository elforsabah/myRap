  method /plce/if_bgprocessing_unit~execute.
    data:
      lretry   type abap_boolean,
      lmessage type ref to if_abap_behv_message,
      lex_root type ref to cx_root,
      lothers  type /plce/cl_pd_messagehelper=>_msg_ref_table,
      llog     type ref to /plce/cl_appllog_helper.

    if lines( service_complete ) is not initial or lines( service_confirm ) is not initial.

      modify entities of /PLCE/R_PDService
        entity Service
          execute completeService from service_complete
          execute confirmService from service_confirm
      failed data(lfailed)
      reported data(lreported).
    endif.

    if lfailed is initial and lines( service_sequence ) is not initial.

      sequence_services(
        changing
          cv_reported = lreported
          cv_failed = lfailed ).

    endif.

    if lfailed is not initial.
      /plce/cl_base_misc=>check_response( exporting is_response = lreported changing ct_messages = lothers ).
      lmessage = /plce/cl_pd_messagehelper=>get_temporary_error( messages = lothers ).
      lretry = cond #( when i_retry_possible = abap_true and lmessage is not initial then abap_true else abap_false ).

      if lmessage is initial.
        lmessage = value #( lothers[ 1 ] default new /plce/cx_bgp( textid = /plce/cx_bgp=>unknown_reason ) ).
      endif.

      if lmessage is instance of cx_root.
        lex_root = cast cx_root( lmessage ).
      else.
        lex_root = new /plce/cx_bgp( textid = /plce/cx_bgp=>unknown_reason ).
      endif.

***   save log if more than one message exists.

      if lines( lothers ) > 1.

        try.
            llog = new /plce/cl_appllog_helper(
              iv_object      = c_appllog_object
              iv_subobject   = c_appllog_subobject
              iv_external_id = conv #( i_bgpunit )
            ).

            llog->add_abap_behavior_messages( it_message = lothers ).
            llog->save_log( iv_use_2nd_db_connection = abap_true ).

          catch /plce/cx_baseexception.
*          nop
        endtry.
      endif.


      raise exception new /plce/cx_bgp_unit( retry = lretry previous = lex_root ).

    endif.

  endmethod.
