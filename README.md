CLASS lhc_attachment DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
        IMPORTING REQUEST requested_authorizations FOR attachment RESULT result.

    METHODS deleteattachment FOR MODIFY
      IMPORTING keys FOR ACTION attachment~deleteattachment RESULT result.

ENDCLASS.

CLASS lhc_attachment IMPLEMENTATION.

  METHOD deleteattachment.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      IF <ls_key>-uuid = <ls_key>-touruuid.

        MODIFY ENTITIES OF /plce/r_pdtour
           ENTITY attachments
             DELETE FROM VALUE #( ( %key-touruuid       = <ls_key>-touruuid
                                    %key-attachmentuuid = <ls_key>-attachmentuuid ) )
           MAPPED   DATA(tour_mapped)
           FAILED   DATA(tour_failed)
           REPORTED DATA(tour_reported).

        mapped   = VALUE #( attachment = CORRESPONDING #( BASE ( mapped-attachment )   tour_mapped-attachments ) ).
        failed   = VALUE #( attachment = CORRESPONDING #( BASE ( failed-attachment )   tour_failed-attachments ) ).
        reported = VALUE #( attachment = CORRESPONDING #( BASE ( reported-attachment ) tour_reported-attachments ) ).

      ELSE.

        MODIFY ENTITIES OF /plce/r_pdservice
          ENTITY attachments
            DELETE FROM VALUE #( ( %key-serviceuuid       = <ls_key>-uuid
                                   %key-attachmentuuid = <ls_key>-attachmentuuid ) )
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
      lt_empty         TYPE STANDARD TABLE OF /plce/r_pdserviceextwr WITH EMPTY KEY,
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
*        "Report success
*        APPEND VALUE #( %cid = <fs_key>-%cid
*                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                            number   = '000'
*                                            severity = if_abap_behv_message=>severity-success )
*                       ) TO reported-tour.
*      ELSE.
*        "Report error
*        APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-tour.
*        APPEND VALUE #( %cid = <fs_key>-%cid
*                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                            number   = '001'
*                                            severity = if_abap_behv_message=>severity-error )
*                       ) TO reported-tour.
*        RETURN.
*      ENDIF.
*
        APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> ) ) TO failed-tour.

        APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                            number   = '000'
                                            severity = if_abap_behv_message=>severity-error )
                     ) TO reported-tour.
      ELSE.

        IF sy-subrc <> 0.
          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                              number   = '001'
                                              severity = if_abap_behv_message=>severity-information )
                        ) TO reported-tour.
        ENDIF.
        RETURN.

      ENDIF.


      "Read tour data
      READ ENTITIES OF zr_pdtour IN LOCAL MODE
      ENTITY tour
      FIELDS ( touruuid tourid tourtemplate )
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
          MODIFY ENTITIES OF /plce/r_pdtour
           ENTITY tour
           EXECUTE addattachment FROM VALUE #( FOR attachment IN lt_tour_attm ( %is_draft = if_abap_behv=>mk-off
                                                                                %pky-touruuid = attachment-touruuid
                                                                                touruuid = attachment-touruuid
                                                                                %param = VALUE #( attachment = attachment-attachment
                                                                                                  mimetype   = attachment-mimetype
                                                                                                  filename   = attachment-filename
                                                                                                  comments   = attachment-comments ) ) )
           RESULT DATA(result_attm)
           FAILED DATA(failed_attm).

          LOOP AT failed_attm-tour ASSIGNING FIELD-SYMBOL(<fs_tour>).
*            APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-tour.
*            APPEND VALUE #( %cid = <fs_key>-%cid
*                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                                number   = '002'
*                                                severity = if_abap_behv_message=>severity-error )
*                       ) TO reported-tour.
*          ENDLOOP.
*
*          IF sy-subrc <> 0.
*            APPEND VALUE #( %cid = <fs_key>-%cid
*                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                                number   = '003'
*                                                severity = if_abap_behv_message=>severity-information )
*                          ) TO reported-tour.
*          ENDIF.

            APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> ) ) TO failed-tour.

            APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                                number   = '002'
                                                severity = if_abap_behv_message=>severity-error )
                         ) TO reported-tour.
          ENDLOOP.

          IF sy-subrc <> 0.
            APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                                number   = '003'
                                                severity = if_abap_behv_message=>severity-information )
                          ) TO reported-tour.
          ENDIF.


        ENDIF.

      ENDIF.

      IF mt_selected_services IS INITIAL. RETURN. ENDIF.

      "Fill in Material
      READ ENTITIES OF /plce/r_pdservice
        ENTITY service BY \_extwaste
         FIELDS ( serviceuuid material )
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
        <ls_service>-material = VALUE #( lt_srv_materials[ <ls_service>-serviceuuid ]-material OPTIONAL ).
      ENDLOOP.

      DATA(lt_service_attm) =
      lo_misc->create_manual_service_attm(
        it_selected_services = CORRESPONDING #( mt_selected_services
                                                MAPPING
                                                service_uuid = serviceuuid
                                                service_type = servicetype )
      ).

      IF lt_service_attm IS NOT INITIAL.

        "Add newly generated attachments
        MODIFY ENTITIES OF /plce/r_pdservice
         ENTITY service
         EXECUTE addattachment FROM VALUE #( FOR attachment IN lt_service_attm ( %is_draft = if_abap_behv=>mk-off
                                                                                 %pky-serviceuuid = attachment-serviceuuid
                                                                                 serviceuuid = attachment-serviceuuid
                                                                                 %param = VALUE #( attachment = attachment-attachment
                                                                                                   mimetype   = attachment-mimetype
                                                                                                   filename   = attachment-filename
                                                                                                   comments   = attachment-comments ) ) )
         RESULT DATA(result_srv)
         FAILED DATA(failed_srv).

*        LOOP AT failed_srv-service ASSIGNING FIELD-SYMBOL(<fs_service>).
*          APPEND VALUE #( %tky = CORRESPONDING( <fs_key> ) ) TO failed-tour.
*          APPEND VALUE #( %tky = <fs_key>-%tky
*                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                              number   = '002'
*                                              severity = if_abap_behv_message=>severity-error )
*                     ) TO reported-tour.
*        ENDLOOP.
*
*        IF sy-subrc <> 0.
*          APPEND VALUE #( %tky = <fs_key>-%tky
*                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                              number   = '003'
*                                              severity = if_abap_behv_message=>severity-information )
*                        ) TO reported-tour.
*        ENDIF.


        LOOP AT failed_srv-service ASSIGNING FIELD-SYMBOL(<fs_service>).
          " Use CORRESPONDING to map TourUUID and %is_draft automatically
          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> ) ) TO failed-tour.

          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                              number   = '002'
                                              severity = if_abap_behv_message=>severity-error )
                       ) TO reported-tour.
        ENDLOOP.

        IF sy-subrc <> 0.
          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
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




<img width="758" height="237" alt="image" src="https://github.com/user-attachments/assets/ad11276e-4e9f-43b7-8f55-48879539e185" />

<img width="831" height="725" alt="image" src="https://github.com/user-attachments/assets/d9c12f8c-b4c8-4bf8-b469-bf46586435df" />

<img width="1244" height="435" alt="image" src="https://github.com/user-attachments/assets/154b076c-21b6-4661-9935-ff390be132ce" />

@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Service Assignment',
    typeNamePlural: 'Service Assignments'
  },
  presentationVariant: [{
    sortOrder: [{
      by: 'TourSequence', direction:  #ASC
    }],
   visualizations: [{ type: #AS_LINEITEM }]
  }]
}

annotate entity ZC_PDTOURSERVICEASGMT with
{
  @UI.facet: [
    { id: 'F0_CoreDefault',
      type: #FIELDGROUP_REFERENCE,
      targetElement: '_Service._ServiceCore',
      targetQualifier: 'DefaultInformation' }
  //    { id: 'F1_WRDefault',
  //      type: #FIELDGROUP_REFERENCE,
  //      targetElement: '_Service',
  //      targetQualifier: 'WR_Default' }
      ]
  @UI.lineItem: [
    { position: 10, hidden: true }
  //    { type: #FOR_ACTION, dataAction: 'Unassign', label: 'Unassign', invocationGrouping: #CHANGE_SET },
  //    { type: #FOR_ACTION, dataAction: 'MoveService', label: 'Move', invocationGrouping: #CHANGE_SET }
    ]
  ServiceUuid;

  @UI.lineItem: [{ position: 20, importance: #HIGH }]
  TourSequence;

  @UI.lineItem: [{ position: 30, importance: #HIGH }]
  ReferenceId;

  @UI.lineItem: [{ position: 40, cssDefault.width: '5rem', importance: #HIGH }]
  WorkStatusIcon;
  @UI.lineItem: [{ position: 50, cssDefault.width: '5rem', importance: #HIGH }]
  ServiceStatusIcon;

  @UI.lineItem: [{ position: 60 , importance: #HIGH }]
  CustomerInfo;

  @UI.lineItem: [{ position: 70 , importance: #HIGH }]
  FullAddress;

  @UI.lineItem: [{ position: 80 }]
  @UI.textArrangement: #TEXT_ONLY
  ServiceType;

  @UI: { lineItem: [{ position: 90, type: #AS_FIELDGROUP, valueQualifier: 'lfg_containertypeat', label: 'ContType At Loc #'}] }
  @UI.fieldGroup: [ {qualifier: 'lfg_containertypeat', position: 10 }]
  ContainerTypeAtLocation;
  @UI.lineItem: [{ hidden: true }]
  //  @UI.fieldGroup: [ {position: 30, qualifier: 'WR_Default', type: #AS_CONNECTED_FIELDS, valueQualifier: 'cf_containertypeat' }]
  //  @UI.connectedFields: [{ qualifier: 'cf_containertypeat', name: 'containertype', groupLabel: 'Container Type At', template: '{containertype_count} {containertype}' }]
  ContainerTypeAtLocationWiText;
  @UI.lineItem: [{ hidden: true }]
  @UI.fieldGroup: [{ qualifier: 'lfg_containertypeat', position: 20 }]
  //  @UI.connectedFields: [{ qualifier: 'cf_containertypeat', name: 'containertype_count' }]
  ContainerAtLocationCount;

  @UI: { lineItem: [{ position: 100, type: #AS_FIELDGROUP, valueQualifier: 'lfg_containertypenew', label: 'Container Type New #' }] }
  @UI.fieldGroup: [{ qualifier: 'lfg_containertypenew', position: 10 }]
  ContainerTypeNew;
  @UI.lineItem: [{ hidden: true }]
  //  @UI.fieldGroup: [{ position: 40, qualifier: 'WR_Default', type: #AS_CONNECTED_FIELDS, valueQualifier: 'cf_containertypenew' }]
  //  @UI.connectedFields: [{ qualifier: 'cf_containertypenew', name: 'containertype', groupLabel: 'Container Type New',  template: '{containertype_count} {containertype}' }]
  ContainerTypeNewWithText;
  @UI.lineItem: [{ hidden: true }]
  @UI.fieldGroup: [{ qualifier: 'lfg_containertypenew', position: 20 }]
  //  @UI.connectedFields: [{ qualifier: 'cf_containertypenew', name: 'containertype_count' }]
  ContainerNewCount;

  @UI: { lineItem: [{ position: 110, type: #AS_FIELDGROUP, valueQualifier: 'lfg_material', label: 'Material #' }] }
  @UI.fieldGroup: [{ qualifier: 'lfg_material', position: 10 }]
  Material;
  @UI.lineItem: [{ hidden: true }]
  //  @UI.fieldGroup: [{ position: 50, qualifier: 'WR_Default', type: #AS_CONNECTED_FIELDS, valueQualifier: 'cf_material' }]
  //  @UI.connectedFields: [{ qualifier: 'cf_material', name: 'material', groupLabel: 'Material', template: '{material_weight} {material}' }]
  MaterialWithText;
  //  @UI.fieldGroup: [{ position: 51, qualifier: 'WR_Default'  }]
  //  MaterialText;
  @UI.lineItem: [{ hidden: true }]
  @UI.fieldGroup: [{ qualifier: 'lfg_material', position: 20 }]
  //  @UI.connectedFields: [{ qualifier: 'cf_material', name: 'material_weight' }]
  MaterialWeight;

  @UI.lineItem: [{ position: 120 }]
  MaterialGroup;

  @UI.lineItem: [{ position: 130 }]
  TotalDuration;

}



@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Tour',
    typeNamePlural: 'Tours',
    title: {
      type: #STANDARD, value: 'TourId'
    },
    description: {
      type: #STANDARD, value: 'TourTemplateName'
    }
  },
  lineItem: [{ criticality: 'TourCriticality' }],

  presentationVariant: [{
    sortOrder: [{
      by: 'TourId', direction:  #ASC
    }], visualizations: [{ type: #AS_LINEITEM }]
  },{
    sortOrder: [{
      by: 'TourId', direction:  #ASC
    }], visualizations: [{ type: #AS_LINEITEM, qualifier: 'DefaultMap' }], qualifier: 'DefaultMap'
  }]
}
annotate entity ZC_PDTOUR with
{
  @UI.facet: [{ label: 'Services',
                type: #LINEITEM_REFERENCE,
                id: 'SrvLineItemRef',
                purpose: #STANDARD,
                targetElement: '_ServiceAssignments' },
              { label: 'Documents',
                type: #LINEITEM_REFERENCE,
                id: 'DocLineItemRef',
                purpose: #STANDARD,
                targetElement: '_Attachments' }]

  @UI.lineItem: [{ position: 1 },
                 { hidden: true },
                 { type:#FOR_ACTION, dataAction:'RegenerateDocs', label:'Regenerate Documents', invocationGrouping: #CHANGE_SET, position: 10 }]
  TourUuid;

  @UI.lineItem: [{ position: 10, importance: #HIGH }]
  @UI.identification: [{ position: 10 }]
  TourId;

  @UI.lineItem: [{ position: 20, importance: #HIGH }]
  @UI.textArrangement: #TEXT_SEPARATE
  TourTemplate;

  @UI.lineItem: [{ position: 30, importance: #HIGH, cssDefault.width: '5rem' }]
  WorkStatusIcon;

  @UI.lineItem: [{ position: 40, criticality: 'TourStatusColorValue', criticalityRepresentation: #WITH_ICON, importance: #HIGH }]
  @UI.textArrangement: #TEXT_ONLY
  TourStatus;
  @UI.lineItem: [{ hidden: true }]
  TourStatusColorValue;
  @UI.lineItem: [{ position: 50,  importance: #HIGH, cssDefault.width: '5rem' }]
  TourStatusIcon;

  @UI.selectionField: [{ position: 10}]
  TourStartDate;
  @UI.lineItem: [{ position: 60 }]
  ScheduledDateTimeStart;

  @UI.lineItem: [{ position: 70 }]
  MainResourceId;

  @UI.hidden: true
  TourCriticality;

}


@Metadata.layer: #CORE
@UI:{ headerInfo: {
      typeName: 'Document',
      typeNamePlural: 'Documents',
      title: { type: #STANDARD, value: 'AttachmentUUID' }
               },
  presentationVariant: [{
    sortOrder: [{
      by: 'CreatedAt', direction:  #DESC
    }],
   visualizations: [{ type: #AS_LINEITEM }]
  }]
}
annotate entity ZC_PDATTACHMENT with
{
  @UI.lineItem: [
       { type: #FOR_ACTION, dataAction: 'deleteAttachment', label: 'Delete' }]
  @UI.hidden: true
  Uuid;
  @UI.hidden: true
  AttachmentUuid;
  @UI.hidden: true
  TourUUID;
  @UI.lineItem: [{ position: 10, label: 'Order' }]
  ReferenceId;
  @UI.lineItem: [{ position: 20, label: 'Tour Sequence' }]
  TourSequence;
  @UI.lineItem: [{ position: 30, label: 'Document' }]
  Attachment;
  @UI.lineItem: [{ position: 40 }]
  CreatedAt;
  @UI.lineItem: [{ position: 50 }]
  CreatedBy;


}

@EndUserText.label: 'Parameters for Attachment'
define abstract entity ZAE_ATTACHTMENT_PARAM
{
    AttachmentItemsjson : abap.string(0);
    ServiceWRItemsjson  : abap.string(0);
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour Service Attachments'
@Metadata.allowExtensions: true
define view entity ZC_PDATTACHMENT
  as projection on ZR_PDATTACHMENT
{
      @UI.hidden: true
  key Uuid,
      @UI.hidden: true
  key AttachmentUuid,
      @UI.hidden: true
  key TourUUID,
      _Tour.TourId,
      TourSequence,
      ReferenceId,
      Filename,
      @UI.hidden: true
      Mimetype,
      Attachment,
      CreatedBy,
      CreatedAt,
      /* Associations */
      _Tour : redirected to parent ZC_PDTOUR
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Consumption View (Tour WR)'
@Search.searchable: true
@Metadata.allowExtensions: true
define root view entity ZC_PDTOUR
  provider contract transactional_query
  as projection on ZR_PDTOUR

{
  key TourUuid,
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourWA_VH', element: 'TourId' } }]
      @Search: { defaultSearchElement: true, ranking: #HIGH }
      TourId,

      @EndUserText.label: 'Tour Template'
      @ObjectModel.text.element: ['TourTemplateName']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourTemplateWA_VH', element: 'TourTemplate' } }]
      @Search: { defaultSearchElement: true }
      TourTemplate,
      @EndUserText.label: 'Template Description'
      @Consumption.filter.hidden: true
      @Semantics.text: true
      _TourTemplate._Text.TourTemplateName         as TourTemplateName : localized,
      @UI.hidden: true
      _TourTemplate.ColorTour                      as ColorTour,

      @EndUserText.label: 'Tour Status'
      @ObjectModel.text.element: ['TourStatusText']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourStatus_VH', element: 'TourStatus' } }]
      @Consumption.filter: { multipleSelections: true, selectionType: #SINGLE }
      TourStatus,
      @Semantics.text: true
      @UI.hidden: true
      _PDTourStatusText.Description                as TourStatusText   : localized,
      @UI.hidden: true
      _TourLookup.TourStatusColorValue             as TourStatusColorValue,
      @Consumption.filter.hidden: true
      _PDTourStatusText.IconURL                    as TourStatusIcon   : localized,

      @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
      StartDate                                    as TourStartDate,
      @Consumption.filter.selectionType: #INTERVAL
      EndDate                                      as TourEndDate,
      @Consumption.filter.selectionType: #INTERVAL
      ScheduledDateTimeStart,

      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDResource_VH', element: 'ResourceId'} }]
      MainResourceId,

      @EndUserText.label: 'Work Status'
      @UI.textArrangement: #TEXT_ONLY
      @ObjectModel.text.element: ['WorkStatusText']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDWorkStatus_VH', element: 'Value'} }]
      _TourLookup.WorkStatus,
      @UI.hidden: true
      _TourLookup._WorkStatusText.Description      as WorkStatusText   : localized,
      @Consumption.filter.hidden: true
      _TourLookup._WorkStatusText.IconURL          as WorkStatusIcon   : localized,
      _TourLookup._TourCriticality.TourCriticality as TourCriticality,

      /* Associations */
      _Attachments        : redirected to composition child ZC_PDATTACHMENT,
      @Consumption.filter.hidden:false
      _ServiceAssignments : redirected to composition child ZC_PDTOURSERVICEASGMT
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour Service Assignments'
@Search.searchable: true
@Metadata.allowExtensions: true
define view entity ZC_PDTOURSERVICEASGMT
  as projection on ZR_PDTOURSERVICEASGMT
{
      @UI.hidden: true
  key TourUuid,
      @UI.hidden: true
  key ServiceUuid,
      _Tour.TourId                                                                           as TourId,
      _Service.ServiceId                                                                     as ServiceId,

      @ObjectModel.text.element: ['ServiceTypeText']
      _Service.ServiceType                                                                   as ServiceType,
      _Service._ServiceType._ServiceTypeText.ServiceTypeDescription                          as ServiceTypeText             : localized,

      @EndUserText.label: 'Service Status Description'
      @ObjectModel.text.element: ['ServiceStatusText']
      @UI.textArrangement: #TEXT_ONLY
      _Service.ServiceStatus,
      @UI.hidden: true
      _Service._ServiceStatusText[1:Language = $session.system_language].Description         as ServiceStatusText,
      _Service._ServiceStatusText[1:Language = $session.system_language].IconURL             as ServiceStatusIcon,

      @UI.hidden: true
      Removed,
      TourSequence,

      @Search.defaultSearchElement: true
      _Service.ReferenceId                                                                   as ReferenceId,

      @Search.defaultSearchElement: true
      _Service.CustomerInfo                                                                  as CustomerInfo,
      _Service.AdditionalText                                                                as AdditionalText,

      _Service.FunctionalLocation                                                            as FunctionalLocation,
      _Service._FunctionalLocation.FullAddress                                               as FullAddress,

      //Extension WR
      @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
      _Service._ExtWaste.Material                                                            as Material,
      @ObjectModel.text.element: ['MaterialText']
      @UI.textArrangement: #TEXT_LAST
      _Service._ExtWaste.Material                                                            as MaterialWithText,

      @EndUserText.label: 'Material Description'
      _Service._ExtWaste._Material._Text.MaterialName                                        as MaterialText                : localized,
      @Semantics.quantity.unitOfMeasure:'MaterialWeightUnit'
      _Service._ExtWaste.MaterialWeight                                                      as MaterialWeight,
      @UI.hidden: true
      cast( _Service._ExtWaste.MaterialWeightUnit as meins preserving type )                 as MaterialWeightUnit, //show localized key/allow fiori app value check
      @ObjectModel.text.element: [ 'MaterialGroupText' ]
      @EndUserText.label: 'Material Group'
      @UI.textArrangement: #TEXT_LAST
      _Service._ExtWaste._Material.MaterialProductGroup                                      as MaterialGroup,
      @EndUserText.label: 'Material Group Description'
      _Service._ExtWaste._Material._ProductGroup._ProductGroupText.ProductGroupName          as MaterialGroupText           : localized,

      @ObjectModel.text.element: ['PlantLocationText']
      _Service._ExtWaste.PlantLocation                                                       as PlantLocation,
      @UI.hidden: true
      _Service._ExtWaste._PlantLocation._FuctionalLocationText.FunctionalLocationDescription as PlantLocationText           : localized,

      _Service._ExtWaste.ContainerSourceLocation                                             as ContainerSourceLocation,
      @EndUserText.label: 'Address - Container Source'
      _Service._ExtWaste._ContainerSourceLocation.FullAddress                                as ContainerSourceAddress,
      _Service._ExtWaste.ContainerFinalLocation                                              as ContainerFinalLocation,
      @EndUserText.label: 'Address - Container Final'
      _Service._ExtWaste._ContainerFinalLocation.FullAddress                                 as ContainerFinalAddress,

      @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
      _Service._ExtWaste.ContainerTypeAtLocation                                             as ContainerTypeAtLocation,
      @ObjectModel.text.element: ['ContainerTypeAtLocationText']
      @UI.textArrangement: #TEXT_LAST
      @Consumption.filter.hidden: true
      _Service._ExtWaste.ContainerTypeAtLocation                                             as ContainerTypeAtLocationWiText,

      @UI.hidden: true
      _Service._ExtWaste._ContainerTypeAtLocation._Text.TransportPackagingTypeName           as ContainerTypeAtLocationText : localized,

      _Service._ExtWaste.ContainerAtLocationCount                                            as ContainerAtLocationCount,
      _Service._ExtWaste.ContainerAtLocationTidnr                                            as ContainerAtLocationTidnr,

      @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
      _Service._ExtWaste.ContainerTypeNew                                                    as ContainerTypeNew,
      @ObjectModel.text.element: ['ContainerTypeNewText']
      @UI.textArrangement:  #TEXT_LAST

      @Consumption.filter.hidden: true
      _Service._ExtWaste.ContainerTypeNew                                                    as ContainerTypeNewWithText,
      @UI.hidden: true
      _Service._ExtWaste._ContainerTypeNew._Text.TransportPackagingTypeName                  as ContainerTypeNewText        : localized,
      _Service._ExtWaste.ContainerNewCount                                                   as ContainerNewCount,
      _Service._ExtWaste.ContainerNewTidnr                                                   as ContainerNewTidnr,
      @UI.hidden: true
      _Service._ExtWaste.ServiceFrequency                                                    as ServiceFrequency,
      @EndUserText.label: 'Service Frequency'
      _Service._ExtWaste._ServiceFrequencyText.ServiceFrequencyText                          as ServiceFrequencyText        : localized,
      //Extension WR


      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      _Service._Statistic.TotalDuration,
      @UI.hidden: true
      cast( _Service._Statistic.TotalDurationUnit as meins preserving type )                 as TotalDurationUnit, //show localized key/allow fiori app value check

      @EndUserText.label: 'Work Status Description'
      @ObjectModel.text.element: ['WorkStatusText']
      @UI.textArrangement: #TEXT_ONLY
      _Lookup.WorkStatus,
      @UI.hidden: true
      _Lookup._WorkStatusText.Description                                                    as WorkStatusText              : localized,
      _Lookup._WorkStatusText.IconURL                                                        as WorkStatusIcon              : localized,

      _Service.PlanningStatus,

      /* Associations */
      _Service : redirected to /PLCE/C_PDMNLServiceWR,
      _Tour    : redirected to parent ZC_PDTOUR
}






