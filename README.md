<img width="1826" height="443" alt="image" src="https://github.com/user-attachments/assets/0d247201-b361-4db6-8dee-b6c8ddb963c2" />


FUNCTION z_xft_create_event .
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*"  IMPORTING
*"     VALUE(EVENT) LIKE  SWETYPECOU-EVENT
*"     VALUE(RECTYPE) LIKE  SWETYPECOU-RECTYPE
*"     VALUE(OBJTYPE) LIKE  SWETYPECOU-OBJTYPE
*"     VALUE(OBJKEY) LIKE  SWEINSTCOU-OBJKEY
*"  TABLES
*"      EVENT_CONTAINER STRUCTURE  SWCONT
*"----------------------------------------------------------------------

  DATA:
* Simples
    lv_vbeln    TYPE vbeln,
    lv_objtyp   TYPE swo_objtyp,
* Structures
    ls_bapiret2 TYPE bapiret2,

    lt_vbfa     TYPE STANDARD TABLE OF vbfa WITH DEFAULT KEY,
    ls_message  TYPE /xft/message.

**********************************************************************

  IF event = zxft_if_base_constant=>co_create_event.

    "Create queue process for record creation
    CASE objtype.
      WHEN zxft_if_base_constant=>co_bor_type_quot.

        lv_objtyp = objtype.
        lv_vbeln  = objkey.
        CALL FUNCTION '/XFT/SAM_QM_CREATE_ENTRY_100'
          EXPORTING
            im_bor_object_type = lv_objtyp
            im_doc_number      = lv_vbeln
            im_appl_layer      = 'ZXFT_SAM' "zxft_if_base_constant=>co_appllayer_sauber_sam " ZXFT_SAM
*           IM_EXE_EARLIEST    =
          IMPORTING
            ex_return          = ls_bapiret2
*           EX_DIO_ID          =
          .
        CASE sy-subrc.
          WHEN 1 OR 2 OR 3.
            RETURN.
        ENDCASE.

        "Check errors
        IF ls_bapiret2-type = 'A' OR
           ls_bapiret2-type = 'E'.
          RETURN.
        ENDIF.
        COMMIT WORK AND WAIT.



      WHEN zxft_if_base_constant=>co_bor_type_sd_contract.      " BUS2034 - SD-Kontrakt erweitert

        DATA(ls_contract_data) = VALUE vbco6( vbeln = objkey ).

        CALL FUNCTION 'RV_ORDER_FLOW_INFORMATION'
          EXPORTING
            belegtyp      = 'B' " Angebot
            comwa         = ls_contract_data
            vorgaenger    = 'X'
          TABLES
            vbfa_tab      = lt_vbfa
          EXCEPTIONS
            no_vbfa       = 1
            no_vbuk_found = 2
            OTHERS        = 3.
        IF sy-subrc <> 0.
          " no previous order found
          RETURN.
        ENDIF.

        DELETE lt_vbfa WHERE posnv <> '000010' AND posnn <> '000010'.

        READ TABLE lt_vbfa WITH KEY vbeln = objkey INTO DATA(ls_vbfa).

        IF sy-subrc <> 0.
          " @todo: error handling

        ENDIF.

        DATA(lv_rec_id) = zxft_cl_sam_pro_public=>get_rec_by_vbeln( iv_vbeln = CONV vbeln( ls_vbfa-vbelv ) ).


        IF lv_rec_id IS NOT INITIAL.


          " Queue-Prozess zum Ändern der Akte erzeugen

          DATA(lo_dio_pub) = NEW /xft/cl_dio_public( ).

          TRY.

              DATA(lo_req) = lo_dio_pub->/xft/if_dio_public_input~create_request_s(
                               im_pro_type_id        = zxft_if_base_constant=>co_update_status_spva                                        " Prozesstyp ID
*                          im_pro_id             =                                         " Prozess ID für externe Vergabe (->Kommentar in Methode)
*                          im_pa_pro_id          =                                         " Vater Prozess ID
*                          im_description        =                                         " Beschreibung
*                          im_exe_earliest       =                                         " Früheste Ausführung (INITIAL = sofort)
                               im_properties_use     =  VALUE /xft/tt_string_nv( ( name = 'REC_ID' value = lv_rec_id )
                                                                                 ( name = 'NEW_APPL_STATUS' value = zxft_if_base_constant=>co_head_state_accepted )
                                                                                 ( name = 'NEW_SYS_STATUS' value = /xft/if_srm_dyn_rec_constant=>system_state_ready ) )                                  " String: Name - Wert
*                          im_properties_control =                                         " String: Name - Wert
*                          im_cms_documents      =                                         " /XFT/DIO_IFIN_DOC_CMS_S
*                          im_message            =                                         " Message
*                          im_queue_state        = /xft/if_dio_constant=>queue_state_ready " Queue state
                             ).

              " Request erzeugen
              lo_dio_pub->/xft/if_dio_public_input~execute_request( lo_req ).

              " Commit
              /xft/cl_dio_public=>transaction_commit( /xft/if_constant=>true ).

            CATCH /xft/cx_base.

              RAISE EXCEPTION TYPE /xft/cx_base
                EXPORTING
                  message_id     = 'ZXFT_NCL_BASE'
                  message_type   = 'E'
                  message_number = '026'. " Es konnte kein Statusupdate angestoßen werden.

          ENDTRY.


        ELSE.

          RAISE EXCEPTION TYPE /xft/cx_base
            EXPORTING
              message_id     = 'ZXFT_NCL_BASE'
              message_type   = 'E'
              message_number = '025' " Es konnte keine Akten-ID zur Angebots-ID & ermittelt werden
              message_v1     = CONV #( ls_vbfa-vbelv ).

        ENDIF.

      WHEN OTHERS.
        RETURN.
    ENDCASE.

  ENDIF.

ENDFUNCTION.
