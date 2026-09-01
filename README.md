Auftrag &1: Behälternummer fehlt für Bewegungsart &2



      " --- 6d Containers ---------------------------------------------------
      " Spec + runtime rules:
      "   new     (Aufstellung) — containerNumberNew required, old empty
      "   collect (Einzug)      — containerNumberOld required, new empty
      "   change  (Wechsel)     — BOTH required
      " EWA CONTAINER / GERNR is the container standing at the customer,
      " so it is the OLD one — it only fills "new" for an Aufstellung.
      DATA(lv_cont_fallback) = COND string(
        WHEN ls_ewa-container IS NOT INITIAL
        THEN condense( |{ ls_ewa-container ALPHA = OUT }| )
        ELSE condense( |{ ls_ewa-gernr     ALPHA = OUT }| ) ).

      READ TABLE lt_srvwr INTO DATA(ls_wr)
        WITH KEY service_uuid = ls_svc-service_uuid.

      IF sy-subrc = 0.
        lv_cont_old = condense( ls_wr-container_atloc_tidnr ).
        lv_cont_new = condense( ls_wr-container_new_tidnr ).
      ENDIF.

      CASE lv_movement_type.
        WHEN 'new'.
          CLEAR lv_cont_old.
          IF lv_cont_new IS INITIAL.
            lv_cont_new = lv_cont_fallback.
          ENDIF.
        WHEN 'collect'.
          CLEAR lv_cont_new.
          IF lv_cont_old IS INITIAL.
            lv_cont_old = lv_cont_fallback.
          ENDIF.
        WHEN OTHERS.               " change
          IF lv_cont_old IS INITIAL.
            lv_cont_old = lv_cont_fallback.
          ENDIF.
      ENDCASE.

      " Fail here with a readable message rather than letting BMS
      " reject the request for a field we know is missing.
      DATA(lv_cont_missing) = xsdbool(
        ( lv_movement_type = 'new'     AND lv_cont_new IS INITIAL ) OR
        ( lv_movement_type = 'collect' AND lv_cont_old IS INITIAL ) OR
        ( lv_movement_type = 'change'  AND
          ( lv_cont_old IS INITIAL OR lv_cont_new IS INITIAL ) ) ).

      IF lv_cont_missing = abap_true.
        lv_count_error = lv_count_error + 1.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '034'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_svc_ref_out
                   v2       = lv_movement_type )
        ) TO reported-tour.
        CONTINUE.
      ENDIF.

      APPEND VALUE #(
        container_number_old         = lv_cont_old
        container_number_new         = lv_cont_new
        movement_type                = lv_movement_type
        container_type_name          = lv_ctype_name
        container_movement_type_info = lv_move_info
        internal_remark              = lv_internal_remark
        customer_owned               = abap_false
      ) TO lt_containers.
