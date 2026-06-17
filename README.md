  method CONSTRUCTOR.
    if FO_LOG is not initial.
      raise exception type /PLCE/CX_BASEEXCEPTION
        message id /PLCE/CX_BASEEXCEPTION=>CS_APPL_LOG_ALREADY_CREATED-MSGID
        type IF_BALI_CONSTANTS=>C_SEVERITY_ERROR
        number /PLCE/CX_BASEEXCEPTION=>CS_APPL_LOG_ALREADY_CREATED-MSGNO.
    endif.
    if IV_LOG_HANDLE is initial.
      if IV_OBJECT is initial.
        raise exception type /PLCE/CX_BASEEXCEPTION
          message id /PLCE/CX_BASEEXCEPTION=>CS_NO_OBJECT_GIVEN-MSGID
          type IF_BALI_CONSTANTS=>C_SEVERITY_ERROR
          number /PLCE/CX_BASEEXCEPTION=>CS_NO_OBJECT_GIVEN-MSGNO.
      endif.
      if IV_SUBOBJECT is initial.
        raise exception type /PLCE/CX_BASEEXCEPTION
          message id /PLCE/CX_BASEEXCEPTION=>CS_NO_SUBOBJECT_GIVEN-MSGID
          type IF_BALI_CONSTANTS=>C_SEVERITY_ERROR
          number /PLCE/CX_BASEEXCEPTION=>CS_NO_SUBOBJECT_GIVEN-MSGNO.
      endif.
    endif.

*" check Log Object already exists
*    data: lv_log_handle type if_bali_log=>ty_handle,
*          lt_logs       type if_bali_log_db=>ty_log_table.
*
*    if iv_log_handle is not initial.
*        lv_log_handle = iv_log_handle.
*
*    elseif iv_log_handle is initial and iv_external_id is not initial.
*        data(lv_filter) = cl_bali_log_filter=>create( ).
*        try.
*            lv_filter = lv_filter->set_descriptor( object = iv_object subobject = iv_subobject external_id = iv_external_id ).
*            lt_logs = cl_bali_log_db=>get_instance( )->load_logs_via_filter( filter = lv_filter ).
*          catch cx_bali_runtime INTO DATA(l_exception).
*          CLEANUP.
*            clear lt_logs.
*        endtry.
*
*        if lines( lt_logs ) > 0.
*            lv_log_handle = lt_logs[ 1 ]->get_handle( ).
*        endif.
*
*    endif.
*
*    clear:
*      FO_LOG.

    try.
        if IV_LOG_HANDLE is initial.
          FO_LOG = CL_BALI_LOG=>CREATE_WITH_HEADER( HEADER = CL_BALI_HEADER_SETTER=>CREATE( OBJECT = IV_OBJECT
                                                                                            SUBOBJECT = IV_SUBOBJECT
                                                                                            EXTERNAL_ID = IV_EXTERNAL_ID ) ).
        else.
          FO_LOG_DB = CL_BALI_LOG_DB=>GET_INSTANCE( ).
          FO_LOG = FO_LOG_DB->LOAD_LOG( HANDLE = IV_LOG_HANDLE ).
        endif.
      catch CX_BALI_RUNTIME into data(LRO_BALI_EXCEPTION).
        raise exception type /PLCE/CX_BASEEXCEPTION
          exporting
            PREVIOUS = LRO_BALI_EXCEPTION.
    endtry.

    if FO_LOG is initial.
      raise exception type /PLCE/CX_BASEEXCEPTION
        message id /PLCE/CX_BASEEXCEPTION=>CS_NO_APPLICATION_LOG_CREATED-MSGID
        type IF_BALI_CONSTANTS=>C_SEVERITY_ERROR
        number /PLCE/CX_BASEEXCEPTION=>CS_NO_APPLICATION_LOG_CREATED-MSGNO.
    endif.
  endmethod.


<img width="818" height="423" alt="image" src="https://github.com/user-attachments/assets/232e3a6b-2cbe-4fc3-9559-b95e4907725f" />

  
