CLASS lhc_tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PUBLIC SECTION.

    DATA gt_selected_attachments TYPE STANDARD TABLE OF  zc_pdattachment.
    DATA gt_selected_services TYPE STANDARD TABLE OF  /PLCE/C_PDMNLServiceWR.

  PRIVATE SECTION.

    METHODS get_instance_features FOR INSTANCE FEATURES
      IMPORTING keys REQUEST requested_features FOR tour RESULT result.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR tour RESULT result.

    METHODS regeneratedocs FOR MODIFY
      IMPORTING keys FOR ACTION tour~regeneratedocs.

    METHODS generatedocuments  FOR MODIFY
      IMPORTING keys FOR ACTION tour~generatedocuments  RESULT result.

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

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD regeneratedocs.

    MODIFY ENTITIES OF /plce/r_pdtour
        ENTITY tour
        EXECUTE generatedocuments
        FROM CORRESPONDING #( keys )
        FAILED DATA(lt_failed).

  ENDMETHOD.


  METHOD generatedocuments.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<fs_key>).

      " Step 1: get the json file
      DATA(lt_attachmenitems) = <fs_key>-%param-AttachmentItemsjson.
      DATA(lt_services)       = <fs_key>-%param-ServiceWRItemsjson.
      DATA lt_success_keys TYPE TABLE FOR READ IMPORT ZR_PDTOUR.

      "Step 2: deserialize " Deserialize JSON to internal table

      /ui2/cl_json=>deserialize(
        EXPORTING
          json = lt_attachmenitems
          pretty_name = /ui2/cl_json=>pretty_mode-camel_case
        CHANGING
          data = gt_selected_attachments
      ).

      /ui2/cl_json=>deserialize(
         EXPORTING
           json = lt_services
           pretty_name = /ui2/cl_json=>pretty_mode-camel_case
         CHANGING
           data = gt_selected_services
       ).
      " Report success of Gross Weight
      APPEND VALUE #(
        %cid = <fs_key>-%cid
        %msg = new_message(
                 id = 'ZWR_TOURDOC_MESSAGES'
                 number = '000'
                 severity = if_abap_behv_message=>severity-success
                  )
      ) TO reported-tour.


    ENDLOOP.
  ENDMETHOD.

ENDCLASS.
