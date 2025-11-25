METHOD precheck_createtour.

  " Today (system date)
  DATA(lv_today) = cl_abap_context_info=>get_system_date( ).

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>).

    DATA(lv_failed)   TYPE abap_bool VALUE abap_false.
    DATA(lv_start)    TYPE /plce/date.
    DATA(lv_end)      TYPE /plce/date.

    lv_start = <key>-%param-start_date.
    lv_end   = <key>-%param-end_date.

    " --- 1) Start date must not be in the past ---------------------------
    IF lv_start < lv_today.
      APPEND VALUE #(
        %cid = <key>-%cid
        %msg = new_message_with_text(
                 severity = if_abap_behv_message=>severity-error
                 text     = |First Tour Date { lv_start } must not be in the past.| )
      ) TO reported-%other.
      lv_failed = abap_true.
    ENDIF.

    " --- 2) Start date must be <= end date (if end date is given) -------
    IF lv_end IS NOT INITIAL AND lv_end < lv_start.
      APPEND VALUE #(
        %cid = <key>-%cid
        %msg = new_message_with_text(
                 severity = if_abap_behv_message=>severity-error
                 text     = |First Tour Date { lv_start } must be before Last Tour Date { lv_end }.| )
      ) TO reported-%other.
      lv_failed = abap_true.
    ENDIF.

    " Mark this action call as failed so createtour is not executed
    IF lv_failed = abap_true.
      APPEND VALUE #( %cid = <key>-%cid ) TO failed-createtour.
    ENDIF.

  ENDLOOP.

ENDMETHOD.
