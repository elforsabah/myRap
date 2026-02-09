CLASS lhc_attachment DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
        IMPORTING REQUEST requested_authorizations FOR Attachment RESULT result.

    METHODS deleteAttachment FOR MODIFY
      IMPORTING keys FOR ACTION Attachment~deleteAttachment RESULT result.

ENDCLASS.

CLASS lhc_attachment IMPLEMENTATION.

  METHOD deleteAttachment.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      IF <ls_key>-Uuid = <ls_key>-TourUUID.

        MODIFY ENTITIES OF /PLCE/R_PDTour
           ENTITY Attachments
             DELETE FROM VALUE #( ( %key-TourUUID       = <ls_key>-TourUUID
                                    %key-AttachmentUUID = <ls_key>-AttachmentUuid ) )
           MAPPED   DATA(tour_mapped)
           FAILED   DATA(tour_failed)
           REPORTED DATA(tour_reported).

        mapped   = VALUE #( attachment = CORRESPONDING #( BASE ( mapped-attachment )   tour_mapped-attachments ) ).
        failed   = VALUE #( attachment = CORRESPONDING #( BASE ( failed-attachment )   tour_failed-attachments ) ).
        reported = VALUE #( attachment = CORRESPONDING #( BASE ( reported-attachment ) tour_reported-attachments ) ).

      ELSE.

        MODIFY ENTITIES OF /PLCE/R_PDService
          ENTITY Attachments
            DELETE FROM VALUE #( ( %key-ServiceUUID       = <ls_key>-Uuid
                                   %key-AttachmentUUID = <ls_key>-AttachmentUuid ) )
          MAPPED   DATA(service_mapped)
          FAILED   DATA(service_failed)
          REPORTED DATA(service_reported).

        mapped   = VALUE #( attachment = CORRESPONDING #( BASE ( mapped-attachment )   service_mapped-attachments ) ).
        failed   = VALUE #( attachment = CORRESPONDING #( BASE ( failed-attachment )   tour_failed-attachments ) ).
        reported = VALUE #( attachment = CORRESPONDING #( BASE ( reported-attachment ) tour_reported-attachments ) ).

      ENDIF.
    ENDLOOP.

  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

ENDCLASS.

CLASS lhc_tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PUBLIC SECTION.

    TYPES: BEGIN OF ty_tour_with_attachments,
             tour_uuid     TYPE /plce/pdtour_uuid,
             tour_id       TYPE /plce/pdtour_id,
             tour_template TYPE /plce/pdtour_template,
             attachments   TYPE STANDARD TABLE OF zc_printconfig WITH EMPTY KEY,
           END OF ty_tour_with_attachments.

    DATA:
      BEGIN OF ms_selected_tour,
        touruuid TYPE /plce/pdtour_uuid,
      END OF ms_selected_tour,
      mt_selected_attachments_config TYPE STANDARD TABLE OF zc_printconfig WITH EMPTY KEY,
      mt_selected_services           TYPE STANDARD TABLE OF /plce/c_pdmnlservicewr   WITH EMPTY KEY,
      mt_selected_tours              LIKE STANDARD TABLE OF ms_selected_tour.

  PRIVATE SECTION.

    METHODS get_instance_features FOR INSTANCE FEATURES
      IMPORTING keys REQUEST requested_features FOR tour RESULT result.

    METHODS regeneratedocs FOR MODIFY
      IMPORTING keys FOR ACTION tour~regeneratedocs.

    METHODS generatedocuments  FOR MODIFY
      IMPORTING keys FOR ACTION tour~generatedocuments  RESULT result.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR tour RESULT result.

ENDCLASS.

CLASS lhc_tour IMPLEMENTATION.

  METHOD get_instance_features.

    READ ENTITIES OF zr_pdtour IN LOCAL MODE
      ENTITY tour
      FIELDS ( tourid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(tours).

    result = VALUE #( FOR tour IN tours ( %key = tour-%key
                                          %features-%action-regeneratedocs = if_abap_behv=>fc-o-enabled ) ).

  ENDMETHOD.

  METHOD regeneratedocs.

    MODIFY ENTITIES OF /plce/r_pdtour
        ENTITY tour
        EXECUTE generatedocuments
        FROM CORRESPONDING #( keys )
        FAILED DATA(ls_failed).

    IF ls_failed-tour IS INITIAL.
      reported-tour = VALUE #( FOR key IN keys (  %tky = key-%tky
                                                  %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                                                      number   = '000'
                                                                      severity = if_abap_behv_message=>severity-success ) ) ).
    ENDIF.

  ENDMETHOD.


  METHOD generatedocuments.

    DATA:
      lt_attachments   TYPE STANDARD TABLE OF zc_printconfig WITH EMPTY KEY,
      lt_valid_forms_h TYPE HASHED TABLE OF zwr_pdforms WITH UNIQUE KEY formtype printform,
      lt_empty         TYPE STANDARD TABLE OF /PLCE/R_PDServiceExtWR WITH EMPTY KEY,
      lt_success_keys  TYPE TABLE FOR READ IMPORT zr_pdtour.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<fs_key>).

      " Step 1: Get the JSON files
      DATA(lv_attachments_tab) = <fs_key>-%param-attachmentitemsjson.
      DATA(lv_services_tab)    = <fs_key>-%param-servicewritemsjson.

      "Step 2: De-serialize JSON files to internal tables
      /ui2/cl_json=>deserialize(
        EXPORTING
          json        = lv_attachments_tab
          pretty_name = /ui2/cl_json=>pretty_mode-camel_case
        CHANGING
          data        = mt_selected_attachments_config ).

      /ui2/cl_json=>deserialize(
         EXPORTING
           json        = lv_services_tab
           pretty_name = /ui2/cl_json=>pretty_mode-camel_case
         CHANGING
           data        = mt_selected_services ).

      /ui2/cl_json=>deserialize(
       EXPORTING
         json        = lv_services_tab
         pretty_name = /ui2/cl_json=>pretty_mode-camel_case
       CHANGING
         data        = mt_selected_tours ).
      DELETE ADJACENT DUPLICATES FROM mt_selected_tours.


      IF ( mt_selected_attachments_config IS NOT INITIAL ).
        "Report success
        APPEND VALUE #( %cid = <fs_key>-%cid
                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                            number   = '000'
                                            severity = if_abap_behv_message=>severity-success )
                       ) TO reported-tour.
      ELSE.
        "Report error
        APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-tour.
        APPEND VALUE #( %cid = <fs_key>-%cid
                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                            number   = '001'
                                            severity = if_abap_behv_message=>severity-error )
                       ) TO reported-tour.
        RETURN.
      ENDIF.

      "Read tour data
      READ ENTITIES OF zr_pdtour IN LOCAL MODE
      ENTITY Tour
      FIELDS ( TourUuid TourId TourTemplate )
      WITH CORRESPONDING #( mt_selected_tours )
      RESULT DATA(lt_tours).

      DATA(ls_tour) = VALUE #( lt_tours[ 1 ] OPTIONAL ).

      "Set up helper class
      DATA(lo_misc) = zcl_wr_misc_pd=>get_instance( ).

      "Build table for tour print configuration
      DATA(ls_tour_conf) =
        lo_misc->build_for_tour(
          is_tour       = CORRESPONDING #( ls_tour )
          it_sel_config = mt_selected_attachments_config
        ).

      "Create selected tour attachments
      IF ls_tour_conf IS NOT INITIAL.

        DATA(lt_tour_attm) = lo_misc->create_manual_tour_attm( is_tour_with_conf = ls_tour_conf ).
        IF lt_tour_attm IS NOT INITIAL.
          "Add newly generated Attachments on the selected Tour
          MODIFY ENTITIES OF /PLCE/R_PDTour
           ENTITY Tour
           EXECUTE AddAttachment FROM VALUE #( FOR attachment IN lt_tour_attm ( %is_draft = if_abap_behv=>mk-off
                                                                                %pky-TourUUID = attachment-touruuid
                                                                                TourUUID = attachment-touruuid
                                                                                %param = VALUE #( attachment = attachment-attachment
                                                                                                  mimetype   = attachment-mimetype
                                                                                                  filename   = attachment-filename
                                                                                                  comments   = attachment-comments ) ) )
           RESULT DATA(result_attm)
           FAILED DATA(failed_attm).

          LOOP AT failed_attm-tour ASSIGNING FIELD-SYMBOL(<fs_tour>).
            APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-tour.
            APPEND VALUE #( %cid = <fs_key>-%cid
                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                                number   = '002'
                                                severity = if_abap_behv_message=>severity-error )
                       ) TO reported-tour.
          ENDLOOP.

          IF sy-subrc <> 0.
            APPEND VALUE #( %cid = <fs_key>-%cid
                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                                number   = '003'
                                                severity = if_abap_behv_message=>severity-information )
                          ) TO reported-tour.
          ENDIF.

        ENDIF.

      ENDIF.

      IF mt_selected_services IS INITIAL. RETURN. ENDIF.

      "Fill in Material
      READ ENTITIES OF /PLCE/R_PDService
        ENTITY Service BY \_ExtWaste
         FIELDS ( ServiceUUID Material )
         WITH CORRESPONDING #( mt_selected_services )
         RESULT DATA(lt_srv_materials).

*      LOOP AT lt_srv_materials ASSIGNING FIELD-SYMBOL(<ls_material>) WHERE Material IS INITIAL.
*        APPEND INITIAL LINE TO lt_empty ASSIGNING FIELD-SYMBOL(<ls_empty>).
*        <ls_empty>-ServiceUUID = <ls_material>-ServiceUUID.
*      ENDLOOP.
*
*      LOOP AT lt_empty ASSIGNING <ls_empty>.
*        DELETE lt_srv_materials WHERE ServiceUUID = <ls_empty>-ServiceUUID.
*      ENDLOOP.
*
*      IF lines( lt_srv_materials ) = 0.
*        APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-serviceassignment.
*        APPEND VALUE #( %cid = <fs_key>-%cid
*                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                            number   = '004'
*                                            severity = if_abap_behv_message=>severity-error )
*                       ) TO reported-serviceassignment.
*        RETURN.
*      ENDIF.

      LOOP AT mt_selected_services ASSIGNING FIELD-SYMBOL(<ls_service>).
        <ls_service>-Material = VALUE #( lt_srv_materials[ <ls_service>-ServiceUUID ]-Material OPTIONAL ).
      ENDLOOP.

      DATA(lt_service_attm) =
      lo_misc->create_manual_service_attm(
        it_selected_services = CORRESPONDING #( mt_selected_services
                                                MAPPING
                                                service_uuid = ServiceUUID
                                                service_type = ServiceType )
      ).

      IF lt_service_attm IS NOT INITIAL.

        "Add newly generated attachments
        MODIFY ENTITIES OF /PLCE/R_PDService
         ENTITY Service
         EXECUTE AddAttachment FROM VALUE #( FOR attachment IN lt_service_attm ( %is_draft = if_abap_behv=>mk-off
                                                                                 %pky-ServiceUUID = attachment-serviceuuid
                                                                                 ServiceUUID = attachment-serviceuuid
                                                                                 %param = VALUE #( attachment = attachment-attachment
                                                                                                   mimetype   = attachment-mimetype
                                                                                                   filename   = attachment-filename
                                                                                                   comments   = attachment-comments ) ) )
         RESULT DATA(result_srv)
         FAILED DATA(failed_srv).

        LOOP AT failed_srv-service ASSIGNING FIELD-SYMBOL(<fs_service>).
          APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-tour.
          APPEND VALUE #( %cid = <fs_key>-%cid
                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                              number   = '002'
                                              severity = if_abap_behv_message=>severity-error )
                     ) TO reported-tour.
        ENDLOOP.

        IF sy-subrc <> 0.
          APPEND VALUE #( %cid = <fs_key>-%cid
                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                              number   = '003'
                                              severity = if_abap_behv_message=>severity-information )
                        ) TO reported-tour.
        ENDIF.

      ENDIF.

    ENDLOOP.

    result = VALUE #( FOR key IN keys ( %cid = key-%cid
                                        %param = ls_tour ) ).

  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

ENDCLASS.
