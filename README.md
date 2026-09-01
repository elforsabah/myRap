<img width="1442" height="990" alt="image" src="https://github.com/user-attachments/assets/fd0237c0-4171-49f1-9771-587efe4e1fc7" />


@EndUserText.label: 'Generate Attachments for P&D objects'
define service ZSD_PDATTACMENTS {
  expose ZC_PDTOUR                   as Tour;
  expose ZC_PDTOURSERVICEASGMT       as ServiceAssignment;
  expose ZC_PDATTACHMENT             as Attachment;
  expose ZC_PRINTCONFIG              as PrintConfiguration;
  expose /PLCE/C_PDMNLServiceWR      as ServiceWR;
  expose /PLCE/C_PDTourTemplateWA_VH as TourTemplateVH;
  expose /PLCE/C_PDTourWA_VH         as TourIdVH;
  expose /PLCE/C_PDTourStatus_VH     as TourStatusVH;
  expose /PLCE/C_PDResource_VH       as ResourceVH;
  expose /PLCE/C_PDWorkStatus_VH     as WorkStatusVH;
}

projection;
strict ( 2 );

define behavior for ZC_PDTOUR //alias <alias_name>
{

  use action RegenerateDocs;
  use action generatedocuments;

  use association _Attachments;
  use association _ServiceAssignments;
}

define behavior for ZC_PDATTACHMENT //alias <alias_name>
{
  use delete;

  use action deleteAttachment;

  use association _Tour;
}

define behavior for ZC_PDTOURSERVICEASGMT //alias <alias_name>
{

  use association _Tour;
}


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

*    MODIFY ENTITIES OF /plce/r_pdtour
*        ENTITY tour
*        EXECUTE generatedocuments
*        FROM CORRESPONDING #( keys )
*        FAILED DATA(ls_failed).
*
*    IF ls_failed-tour IS INITIAL.
*      reported-tour = VALUE #( FOR key IN keys (  %tky = key-%tky
*                                                  %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                                                      number   = '000'
*                                                                      severity = if_abap_behv_message=>severity-success ) ) ).
*    ENDIF.

  ENDMETHOD.


  METHOD generatedocuments.

*    DATA:
*      lt_attachments   TYPE STANDARD TABLE OF zc_printconfig WITH EMPTY KEY,
**      lt_valid_forms_h TYPE HASHED TABLE OF zwr_pdforms WITH UNIQUE KEY formtype printform,
*      lt_empty         TYPE STANDARD TABLE OF /plce/r_pdserviceextwr WITH EMPTY KEY,
*      lt_success_keys  TYPE TABLE FOR READ IMPORT zr_pdtour.
*
*    LOOP AT keys ASSIGNING FIELD-SYMBOL(<fs_key>).
*
*      " Step 1: Get the JSON files
*      DATA(lv_attachments_tab) = <fs_key>-%param-attachmentitemsjson.
*      DATA(lv_services_tab)    = <fs_key>-%param-servicewritemsjson.
*
*      "Step 2: De-serialize JSON files to internal tables
*      /ui2/cl_json=>deserialize(
*        EXPORTING
*          json        = lv_attachments_tab
*          pretty_name = /ui2/cl_json=>pretty_mode-camel_case
*        CHANGING
*          data        = mt_selected_attachments_config ).
*
*      /ui2/cl_json=>deserialize(
*         EXPORTING
*           json        = lv_services_tab
*           pretty_name = /ui2/cl_json=>pretty_mode-camel_case
*         CHANGING
*           data        = mt_selected_services ).
*
*      /ui2/cl_json=>deserialize(
*       EXPORTING
*         json        = lv_services_tab
*         pretty_name = /ui2/cl_json=>pretty_mode-camel_case
*       CHANGING
*         data        = mt_selected_tours ).
*      DELETE ADJACENT DUPLICATES FROM mt_selected_tours.
*
*
*      IF ( mt_selected_attachments_config IS NOT INITIAL ).
**        "Report success
**        APPEND VALUE #( %cid = <fs_key>-%cid
**                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
**                                            number   = '000'
**                                            severity = if_abap_behv_message=>severity-success )
**                       ) TO reported-tour.
**      ELSE.
**        "Report error
**        APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-tour.
**        APPEND VALUE #( %cid = <fs_key>-%cid
**                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
**                                            number   = '001'
**                                            severity = if_abap_behv_message=>severity-error )
**                       ) TO reported-tour.
**        RETURN.
**      ENDIF.
**
*        APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> ) ) TO failed-tour.
*
*        APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
*                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                            number   = '000'
*                                            severity = if_abap_behv_message=>severity-error )
*                     ) TO reported-tour.
*      ELSE.
*
*        IF sy-subrc <> 0.
*          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
*                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                              number   = '001'
*                                              severity = if_abap_behv_message=>severity-information )
*                        ) TO reported-tour.
*        ENDIF.
*        RETURN.
*
*      ENDIF.
*
*
*      "Read tour data
*      READ ENTITIES OF zr_pdtour IN LOCAL MODE
*      ENTITY tour
*      FIELDS ( touruuid tourid tourtemplate )
*      WITH CORRESPONDING #( mt_selected_tours )
*      RESULT DATA(lt_tours).
*
*      DATA(ls_tour) = VALUE #( lt_tours[ 1 ] OPTIONAL ).
*
*      "Set up helper class
**      DATA(lo_misc) = zcl_wr_misc_pd=>get_instance( ).
*
*      "Build table for tour print configuration
*      DATA(ls_tour_conf) =
*        lo_misc->build_for_tour(
*          is_tour       = CORRESPONDING #( ls_tour )
*          it_sel_config = mt_selected_attachments_config
*        ).
*
*      "Create selected tour attachments
*      IF ls_tour_conf IS NOT INITIAL.
*
*        DATA(lt_tour_attm) = lo_misc->create_manual_tour_attm( is_tour_with_conf = ls_tour_conf ).
*        IF lt_tour_attm IS NOT INITIAL.
*          "Add newly generated Attachments on the selected Tour
*          MODIFY ENTITIES OF /plce/r_pdtour
*           ENTITY tour
*           EXECUTE addattachment FROM VALUE #( FOR attachment IN lt_tour_attm ( %is_draft = if_abap_behv=>mk-off
*                                                                                %pky-touruuid = attachment-touruuid
*                                                                                touruuid = attachment-touruuid
*                                                                                %param = VALUE #( attachment = attachment-attachment
*                                                                                                  mimetype   = attachment-mimetype
*                                                                                                  filename   = attachment-filename
*                                                                                                  comments   = attachment-comments ) ) )
*           RESULT DATA(result_attm)
*           FAILED DATA(failed_attm).
*
*          LOOP AT failed_attm-tour ASSIGNING FIELD-SYMBOL(<fs_tour>).
**            APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-tour.
**            APPEND VALUE #( %cid = <fs_key>-%cid
**                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
**                                                number   = '002'
**                                                severity = if_abap_behv_message=>severity-error )
**                       ) TO reported-tour.
**          ENDLOOP.
**
**          IF sy-subrc <> 0.
**            APPEND VALUE #( %cid = <fs_key>-%cid
**                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
**                                                number   = '003'
**                                                severity = if_abap_behv_message=>severity-information )
**                          ) TO reported-tour.
**          ENDIF.
*
*            APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> ) ) TO failed-tour.
*
*            APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
*                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                                number   = '002'
*                                                severity = if_abap_behv_message=>severity-error )
*                         ) TO reported-tour.
*          ENDLOOP.
*
*          IF sy-subrc <> 0.
*            APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
*                            %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                                number   = '003'
*                                                severity = if_abap_behv_message=>severity-information )
*                          ) TO reported-tour.
*          ENDIF.
*
*
*        ENDIF.
*
*      ENDIF.
*
*      IF mt_selected_services IS INITIAL. RETURN. ENDIF.
*
*      "Fill in Material
*      READ ENTITIES OF /plce/r_pdservice
*        ENTITY service BY \_extwaste
*         FIELDS ( serviceuuid material )
*         WITH CORRESPONDING #( mt_selected_services )
*         RESULT DATA(lt_srv_materials).
*
**      LOOP AT lt_srv_materials ASSIGNING FIELD-SYMBOL(<ls_material>) WHERE Material IS INITIAL.
**        APPEND INITIAL LINE TO lt_empty ASSIGNING FIELD-SYMBOL(<ls_empty>).
**        <ls_empty>-ServiceUUID = <ls_material>-ServiceUUID.
**      ENDLOOP.
**
**      LOOP AT lt_empty ASSIGNING <ls_empty>.
**        DELETE lt_srv_materials WHERE ServiceUUID = <ls_empty>-ServiceUUID.
**      ENDLOOP.
**
**      IF lines( lt_srv_materials ) = 0.
**        APPEND VALUE #( %cid = <fs_key>-%cid ) TO failed-serviceassignment.
**        APPEND VALUE #( %cid = <fs_key>-%cid
**                        %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
**                                            number   = '004'
**                                            severity = if_abap_behv_message=>severity-error )
**                       ) TO reported-serviceassignment.
**        RETURN.
**      ENDIF.
*
*      LOOP AT mt_selected_services ASSIGNING FIELD-SYMBOL(<ls_service>).
*        <ls_service>-material = VALUE #( lt_srv_materials[ <ls_service>-serviceuuid ]-material OPTIONAL ).
*      ENDLOOP.
*
*      DATA(lt_service_attm) =
*      lo_misc->create_manual_service_attm(
*        it_selected_services = CORRESPONDING #( mt_selected_services
*                                                MAPPING
*                                                service_uuid = serviceuuid
*                                                service_type = servicetype )
*      ).
*
*      IF lt_service_attm IS NOT INITIAL.
*
*        "Add newly generated attachments
*        MODIFY ENTITIES OF /plce/r_pdservice
*         ENTITY service
*         EXECUTE addattachment FROM VALUE #( FOR attachment IN lt_service_attm ( %is_draft = if_abap_behv=>mk-off
*                                                                                 %pky-serviceuuid = attachment-serviceuuid
*                                                                                 serviceuuid = attachment-serviceuuid
*                                                                                 %param = VALUE #( attachment = attachment-attachment
*                                                                                                   mimetype   = attachment-mimetype
*                                                                                                   filename   = attachment-filename
*                                                                                                   comments   = attachment-comments ) ) )
*         RESULT DATA(result_srv)
*         FAILED DATA(failed_srv).
*
**        LOOP AT failed_srv-service ASSIGNING FIELD-SYMBOL(<fs_service>).
**          APPEND VALUE #( %tky = CORRESPONDING( <fs_key> ) ) TO failed-tour.
**          APPEND VALUE #( %tky = <fs_key>-%tky
**                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
**                                              number   = '002'
**                                              severity = if_abap_behv_message=>severity-error )
**                     ) TO reported-tour.
**        ENDLOOP.
**
**        IF sy-subrc <> 0.
**          APPEND VALUE #( %tky = <fs_key>-%tky
**                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
**                                              number   = '003'
**                                              severity = if_abap_behv_message=>severity-information )
**                        ) TO reported-tour.
**        ENDIF.
*
*
*        LOOP AT failed_srv-service ASSIGNING FIELD-SYMBOL(<fs_service>).
*          " Use CORRESPONDING to map TourUUID and %is_draft automatically
*          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> ) ) TO failed-tour.
*
*          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
*                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                              number   = '002'
*                                              severity = if_abap_behv_message=>severity-error )
*                       ) TO reported-tour.
*        ENDLOOP.
*
*        IF sy-subrc <> 0.
*          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
*                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
*                                              number   = '003'
*                                              severity = if_abap_behv_message=>severity-information )
*                        ) TO reported-tour.
*        ENDIF.
*
*
*      ENDIF.
*
*    ENDLOOP.
*
*    result = VALUE #( FOR key IN keys ( %cid = key-%cid
*                                        %param = ls_tour ) ).

  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

ENDCLASS.


projection;
strict ( 2 );

define behavior for ZC_PDTOUR //alias <alias_name>
{

  use action RegenerateDocs;
  use action generatedocuments;

  use association _Attachments;
  use association _ServiceAssignments;
}

define behavior for ZC_PDATTACHMENT //alias <alias_name>
{
  use delete;

  use action deleteAttachment;

  use association _Tour;
}

define behavior for ZC_PDTOURSERVICEASGMT //alias <alias_name>
{

  use association _Tour;
}

managed implementation in class zbp_r_pdtour unique;
strict ( 2 );

define behavior for ZR_PDTOUR alias Tour
persistent table /plce/tpdtour
lock master
authorization master ( global )
{
  field ( readonly ) TourUuid;

  association _Attachments;
  association _ServiceAssignments;

  action ( features : instance ) RegenerateDocs;
  static action generatedocuments parameter ZAE_ATTACHTMENT_PARAM result [1] $self;
  side effects
  {
    action RegenerateDocs affects entity _Attachments;
  }

  mapping for /plce/tpdtour corresponding
    {
      TourUuid               = tour_uuid;
      TourId                 = tour_id;
      TourTemplate           = tour_template;
      TourStatus             = tour_status;
      ScheduledDateTimeStart = date_time_start;
      MainResourceId         = main_resource_id;
    }
}

define behavior for ZR_PDATTACHMENT alias Attachment
persistent table /plce/tpdtrattm
lock dependent by _Tour
authorization dependent by _Tour
{

  field ( readonly ) Uuid, AttachmentUuid, TourUUID, CreatedBy, CreatedAt;

  action deleteAttachment result [0..*] $self;
  delete;
  association _Tour;

  mapping for /plce/tpdtrattm corresponding
    {
      TourUUID       = tour_uuid;
      AttachmentUuid = attachment_uuid;
      CreatedAt      = created_at;
      CreatedBy      = created_by;
    }
}

define behavior for ZR_PDTOURSERVICEASGMT alias ServiceAssignment
persistent table /plce/tpdtassrvc
lock dependent by _Tour
authorization dependent by _Tour
{
  field ( readonly ) ServiceUuid, TourUuid;
  association _Tour;

  mapping for /plce/tpdtassrvc corresponding
    {
      TourUuid            = tour_uuid;
      ServiceUuid         = service_uuid;
      LeadServiceTaskUuid = lead_service_task_uuid;
    }
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

//      /* Associations */
      _Attachments        : redirected to composition child ZC_PDATTACHMENT,
//      @Consumption.filter.hidden:false
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


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Projection View of ZI_PRINTCONFIG'
@Metadata.ignorePropagatedAnnotations: true

@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}

define root view entity ZC_PRINTCONFIG 
provider contract transactional_query
as projection on ZI_PRINTCONFIG

{
   @UI.selectionField: [{ position: 10 }]
   @UI.lineItem: [{ position: 10 }]
    key Field,
    @UI.lineItem: [{ position: 20 }]
    key Printform,
    @UI.lineItem: [{ position: 30 }]
    key Formtype,
    @UI.lineItem: [{ position: 40 }]
    ParameterIn,
    @UI.lineItem: [{ position: 50 }]
    Filenameteemplate
}


//@AbapCatalog.viewEnhancementCategory: [#NONE]
//@AccessControl.authorizationCheck: #NOT_REQUIRED
//@EndUserText.label: 'P&D Attachments'
//@Metadata.ignorePropagatedAnnotations: true
//@ObjectModel.usageType:{
//    serviceQuality: #X,
//    sizeCategory: #S,
//    dataClass: #MIXED
//}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'P&D Attachments'
@Metadata.ignorePropagatedAnnotations: true
define view entity ZI_PDATTACHMENT
  as select from /plce/tpdtrattm
{

  key tour_uuid       as Uuid,
  key attachment_uuid as AttachmentUuid,
      filename        as Filename,
      mimetype        as Mimetype,
      attachment      as Attachment,
      created_by      as CreatedBy,
      created_at      as CreatedAt
}
union all select from /plce/tpdsrvattm
{
  key service_uuid    as Uuid,
  key attachment_uuid as AttachmentUuid,
      filename        as Filename,
      mimetype        as Mimetype,
      attachment      as Attachment,
      created_by      as CreatedBy,
      created_at      as CreatedAt

}


@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Interface View for ZWR_CWAPRTCONF'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define root view entity ZI_PRINTCONFIG
  as select from zwr_cwaprtconf
{

      @UI.selectionField: [{ position: 10 }]
      @UI.lineItem: [{ position: 10 }]
  key field             as Field,
      @UI.lineItem: [{ position: 20 }]
  key printform         as Printform,
      @UI.lineItem: [{ position: 30 }]
  key formtype          as Formtype,
      @UI.lineItem: [{ position: 40 }]
      parameter_in      as ParameterIn,
      @UI.lineItem: [{ position: 500 }]
      filenameteemplate as Filenameteemplate
}

@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'R View on ZI_PDATTACHMENT'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZR_PDATTACHMENT
  as select from ZI_PDATTACHMENT

  association     to parent ZR_PDTOUR           as _Tour         on $projection.TourUUID = _Tour.TourUuid

  association [1] to /PLCE/R_PDTourServiceAsgmt as _ServiceAsgmt on _ServiceAsgmt.ServiceUUID = $projection.Uuid

{
  key Uuid,
      @Semantics.uuid: true
  key AttachmentUuid,
      @ObjectModel.foreignKey.association: '_Tour'
  key case when _ServiceAsgmt.TourUUID is null then Uuid else _ServiceAsgmt.TourUUID end as TourUUID,
      _ServiceAsgmt.TourSequence,
      _ServiceAsgmt._Service.ReferenceId,
      Filename,
      @Semantics.mimeType: true
      Mimetype,
      @Semantics.largeObject: { mimeType: 'Mimetype',
                                fileName: 'Filename',
                                contentDispositionPreference: #INLINE }
      Attachment,
      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      CreatedAt,

      _Tour
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'R View on /PLCE/TPDTOUR'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZR_PDTOUR
  as select from /plce/tpdtour
  // compositions
   composition [0..*] of ZR_PDTOURSERVICEASGMT  as _ServiceAssignments
   composition [0..*] of ZR_PDATTACHMENT        as _Attachments

  // associations
  association [1..1] to /PLCE/R_PDTour_L       as _TourLookup       on $projection.TourUuid = _TourLookup.TourUUID
  association [0..1] to /PLCE/R_PDTourTemplate as _TourTemplate     on $projection.TourTemplate = _TourTemplate.TourTemplate
  association [0..*] to /PLCE/P_PDTourStatusT  as _PDTourStatusText on $projection.TourStatus = _PDTourStatusText.Value

{
      @Semantics.uuid: true
      @UI.hidden: true
  key tour_uuid        as TourUuid,
      @EndUserText.label: 'Tour ID'
      tour_id          as TourId,
      tour_template    as TourTemplate,
      tour_status      as TourStatus,
      @EndUserText.label: 'Scheduled Start'
      @Semantics.dateTime: true
      date_time_start  as ScheduledDateTimeStart,
      @EndUserText.label: 'Tour Start Date'
      date_start       as StartDate,
      @EndUserText.label: 'Tour End Date'
      date_end         as EndDate,
      @EndUserText.label: 'Main Resource'
      main_resource_id as MainResourceId,

      _TourLookup,
      _TourTemplate,
      _PDTourStatusText,
      _ServiceAssignments,
       _Attachments
}



@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'R View on /PLCE/TPDTASSRVC'
@Metadata.ignorePropagatedAnnotations: true
define view entity ZR_PDTOURSERVICEASGMT
  as select from /plce/tpdtassrvc
  association        to parent ZR_PDTOUR             as _Tour                      on  $projection.TourUuid = _Tour.TourUuid
  association [0..1] to /PLCE/R_PDService            as _Service                   on  _Service.ServiceUUID = $projection.ServiceUuid
  association [0..1] to /PLCE/R_PDTourTaskAssignment as _LeadServiceTaskAssignment on  _LeadServiceTaskAssignment.TourUUID        = $projection.TourUuid
                                                                                   and _LeadServiceTaskAssignment.TaskScope       = 'S'
                                                                                   and _LeadServiceTaskAssignment.ServiceUUID     = $projection.ServiceUuid
                                                                                   and _LeadServiceTaskAssignment.ServiceTaskUUID = $projection.LeadServiceTaskUuid
  association [1..1] to /PLCE/R_PDTourServiceAsgmt_L as _Lookup                    on  _Lookup.TourUUID    = $projection.TourUuid
                                                                                   and _Lookup.ServiceUUID = $projection.ServiceUuid

{
  key tour_uuid                               as TourUuid,
  key service_uuid                            as ServiceUuid,
      lead_service_task_uuid                  as LeadServiceTaskUuid,
      _LeadServiceTaskAssignment.TourSequence as TourSequence,
      @EndUserText.label: 'Removed'
      removed                                 as Removed,

      _Tour,
      _Service,
      _Lookup,
      _LeadServiceTaskAssignment
} where removed <> 'X'


