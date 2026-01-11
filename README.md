extension implementation in class zbp_e_bp_r_pdtour unique;

extend behavior for Tour
{

 static action  ( precheck )  createtour parameter ZAE_D_TOURTOURTEMPLATE_AB result [0..*] $self;

 validation assign_earliest_to_latest_date on save { create; update; }

}

extend behavior for ExtCustom
{
}


CLASS lhc_Tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Tour RESULT result.

    METHODS createtour FOR MODIFY
      IMPORTING keys FOR ACTION Tour~createtour RESULT result.

    METHODS precheck_createtour FOR PRECHECK
      IMPORTING keys FOR ACTION Tour~createtour .


    METHODS assign_earliest_to_latest_date FOR VALIDATE ON SAVE
    IMPORTING keys FOR Tour~assign_earliest_to_latest_date.

ENDCLASS.

CLASS lhc_Tour IMPLEMENTATION.


 METHOD assign_earliest_to_latest_date.

  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD createtour.

    DATA lv_cid          TYPE abp_behv_cid.
    DATA lv_first_date   TYPE /plce/date.
    DATA lv_last_date    TYPE /plce/date.
    DATA lv_current_date TYPE /plce/date.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>)
         GROUP BY ( template   = <key>-%param-tour_template
                    first_date = <key>-%param-start_date
                    last_date  = <key>-%param-end_date )
         ASSIGNING FIELD-SYMBOL(<group>).

      " 1) Determine date range for this request
      lv_first_date = <group>-first_date.
      " If no last_date was entered, use first_date (create just one tour)
      lv_last_date  = COND /plce/date(
                         WHEN <group>-last_date IS INITIAL
                         THEN lv_first_date
                         ELSE <group>-last_date ).

      " Simple safety: if user swapped dates, flip them
      IF lv_last_date < lv_first_date.
        DATA(lv_tmp) = lv_first_date.
        lv_first_date = lv_last_date.
        lv_last_date  = lv_tmp.
      ENDIF.

      lv_current_date = lv_first_date.

      " 2) Create one tour per day in the range
      WHILE lv_current_date <= lv_last_date.

        " unique CID for this inner action call
        lv_cid = cl_system_uuid=>create_uuid_x16_static( ).

        MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY Tour
            EXECUTE createTourWithTemplate
            FROM VALUE #(
              ( %cid                 = lv_cid
                %param-without_draft = 'X'
                %param-tour_template = <group>-template
                %param-start_date    = lv_current_date ) )
          MAPPED   DATA(mapped_tour)
          FAILED   DATA(failed_tour)
          REPORTED DATA(reported_tour).

        " If inner action failed for this day, you could:
        " - collect messages
        " - optionally set failed-createtour for the outer action
        IF failed_tour IS INITIAL.

          " Read created tour(s) to build result & message
          READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
            ENTITY Tour
              ALL FIELDS
              WITH CORRESPONDING #( mapped_tour-tour )
            RESULT DATA(tours).

          IF lines( tours ) > 0.
            " Success message per created tour (optional)
            INSERT NEW /plce/cx_pd_exception(
                     textid   = /plce/cx_pd_exception=>tour_confirmed
                     severity = if_abap_behv_message=>severity-success
                     tour     = tours[ 1 ]-TourId )
              INTO TABLE reported-%other.

            " Static action result: append all created tours
            result = VALUE #( BASE result
                              FOR tour IN tours
                              (
                               %cid = lv_cid
                               %param = tour ) ).
          ENDIF.

        ENDIF.

        " next day
        lv_current_date = lv_current_date + 1.

      ENDWHILE.
    ENDLOOP.
  ENDMETHOD.

  METHOD precheck_createtour.

    " Today (system date)
    DATA(lv_today) = cl_abap_context_info=>get_system_date( ).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>).

      DATA lv_failed TYPE abap_bool VALUE abap_false.
      DATA lv_start  TYPE /plce/date.
      DATA lv_end    TYPE /plce/date.
      DATA lv_template TYPE /plce/pdtour_template.

      lv_start = <key>-%param-start_date.
      lv_end   = <key>-%param-end_date.
      lv_template = <key>-%param-tour_template.

      " unique CID for this inner action call
      DATA(lv_cid) = cl_system_uuid=>create_uuid_x16_static( ).

      " 1) Start date must not be empty
      IF lv_start IS INITIAL.
        APPEND VALUE #(
            %cid = <key>-%cid
            %msg = new_message(
                     id       = 'Z_MSG_CL_SERVICE_EXT'
                     number   = '003'
                     severity = if_abap_behv_message=>severity-error
                     v1       = |{ lv_start DATE = USER }|
                   )
          )
     TO reported-tour.
        lv_failed = abap_true.
      ENDIF.

      " 2) Start date must be <= end date (if end date is given)
      IF lv_end IS NOT INITIAL AND lv_end < lv_start.


        APPEND VALUE #(
      %cid = <key>-%cid
      %msg = new_message(
               id       = 'Z_MSG_CL_SERVICE_EXT'
               number   = '002'
               severity = if_abap_behv_message=>severity-error
               v1       = |{ lv_start DATE = USER }|
               v2       = |{ lv_end DATE = USER }|
             )
    )
TO reported-tour.

        lv_failed = abap_true.
      ENDIF.

      " 3) Tour template must not be empty

      IF lv_template IS INITIAL..

        APPEND VALUE #(
             %cid = <key>-%cid
             %msg = new_message(
                      id       = 'Z_MSG_CL_SERVICE_EXT'
                      number   = '001'
                      severity = if_abap_behv_message=>severity-error
                    )
            )
            TO reported-tour.
                    lv_failed = abap_true.
                  ENDIF.

      " If any check failed, block this action call
      IF lv_failed = abap_true.
        APPEND VALUE #( %cid = <key>-%cid ) TO failed-tour.
      ENDIF.

    ENDLOOP.

@EndUserText.label: 'Service WR'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
@Search.searchable: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define root view entity /PLCE/C_PDMNLServiceWR
  provider contract transactional_query
  as projection on /PLCE/R_PDService
  association [1] to /PLCE/C_PDMNLService as _ServiceCore on _ServiceCore.ServiceUUID = $projection.ServiceUUID
  //association [1] to /PLCE/C_PDServiceC as _ServiceCore on _ServiceCore.ServiceUUID = $projection.ServiceUUID
{
          @UI.hidden: true
  key     ServiceUUID,

          @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.7 }
          //          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDService_VH', element: 'ServiceId' } }]
          ServiceId,
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDProfileWA_VH', element: 'Profile' } }]
          Profile,

          @ObjectModel.text.element: ['ServiceTypeDescription']
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDServiceTypeWA_VH', element: 'ServiceType'  } }]
          ServiceType,

          @UI.hidden: true
          _ServiceType._ServiceTypeText.ServiceTypeDescription                          as ServiceTypeDescription      : localized,


          @EndUserText.label: 'Service Status'
          @ObjectModel.text.element: ['ServiceStatusText']
          @UI.textArrangement: #TEXT_ONLY
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDServiceStatus_VH', element: 'ServiceStatus' } }]
          ServiceStatus,
          @UI.hidden: true
          _ServiceStatusText.Description                                                as ServiceStatusText           : localized,
          @Consumption.filter.hidden: true
          _ServiceStatusText.IconURL                                                    as ServiceStatusIcon           : localized,

          @Search:{ defaultSearchElement: true  }
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDOrderWAWR_VH', element: 'ReferenceId' } }]
          ReferenceId,
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDServicePriority_VH', element: 'ServicePriority' } }]
          ServicePriority,

          @UI.hidden: true
          _PlanningStatusText.Description                                               as PlanningStatusText          : localized,

          @ObjectModel.text.element: ['PlanningStatusText']
          @UI.textArrangement: #TEXT_ONLY
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDPlanningStatus_VH', element : 'PlanningStatus'} }]
          @Consumption.filter: { defaultValue: '', multipleSelections: true, selectionType: #SINGLE }
          PlanningStatus,

          @UI.hidden: true
          _ServiceLookup.PlanningStatusCriticality                                      as PlanningStatusCriticality,

          @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
          RequestedDate,
          @Consumption.filter.selectionType: #INTERVAL
          EarliestDate,
          @Consumption.filter.selectionType: #INTERVAL
          LatestDate,

          @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.7 }
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDCustomerInfoWA_VH', element: 'CustomerInfo'} }]
          CustomerInfo                                                                  as CustomerInfo,

          AdditionalText,

          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocServiceWA_VH', element: 'FunctionalLocation' } }]
          @EndUserText.label: 'Customer Location'
          FunctionalLocation,
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocServiceWA_VH', element: 'FullAddress' } }]
          _FunctionalLocation.FullAddress                                               as FullAddress,

          @UI.hidden: true
          @Semantics.unitOfMeasure: true
          cast( _Statistic.TotalDurationUnit as meins preserving type )                 as TotalDurationUnit, //show localized key/allow fiori app value check
          @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
          @Consumption.filter.hidden: true
          _Statistic.TotalDuration,

          @EndUserText.label: 'Work Status'
          @ObjectModel.text.element: ['WorkStatusText']
          @UI.textArrangement: #TEXT_ONLY
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDWorkStatus_VH', element: 'Value' } }]
          _ServiceLookup.WorkStatus,
          @UI.hidden: true
          _ServiceLookup._WorkStatusText.Description                                    as WorkStatusText              : localized,
          @Consumption.filter.hidden: true
          _ServiceLookup._WorkStatusText.IconURL                                        as WorkStatusIcon              : localized,

          @ObjectModel.text.element: ['ServiceWindowText']
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDServiceWindow_VH', element : 'ServiceWindow'} }]
          @UI.textArrangement: #TEXT_ONLY
          ServiceWindow,
          @UI.hidden: true
          _ServiceWindowText[1:Language = $session.system_language].Description         as ServiceWindowText,
          @Consumption.filter.selectionType: #SINGLE
          ServiceWindowStartTime,
          @Consumption.filter.selectionType: #SINGLE
          ServiceWindowEndTime,

          @Consumption.filter.hidden: true
          _TourAssignments[1:ServiceUUID = $projection.serviceuuid].TourId              as TourId,

          //Extension Waste
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_MDMATERIAL_VH', element: 'ResourceKey'} }]
          @Search:{ defaultSearchElement: true } // fuzzinessThreshold: 0.8, ranking: #LOW
          _ExtWaste.Material                                                            as Material,
          @ObjectModel.text.element: ['MaterialText']
          @UI.textArrangement: #TEXT_LAST
          @Consumption.filter.hidden: true
          _ExtWaste.Material                                                            as MaterialWithText,

          @EndUserText.label: 'Material Description'
          @Consumption.filter.hidden: true
          _ExtWaste._Material._Text.MaterialName                                        as MaterialText                : localized,
          @Semantics.quantity.unitOfMeasure:'MaterialWeightUnit'
          @Consumption.filter.hidden: true
          _ExtWaste.MaterialWeight                                                      as MaterialWeight,
          @UI.hidden: true
          cast( _ExtWaste.MaterialWeightUnit as meins preserving type )                 as MaterialWeightUnit, //show localized key/allow fiori app value check
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_MaterialGroup_VH', element: 'MaterialGroup'} }]
          @ObjectModel.text.element: [ 'MaterialGroupText' ]
          @EndUserText.label: 'Material Group'
          @UI.textArrangement: #TEXT_LAST
          _ExtWaste._Material.MaterialProductGroup                                      as MaterialGroup,
          @EndUserText.label: 'Material Group Description'
          @UI.hidden: true
          _ExtWaste._Material._ProductGroup._ProductGroupText.ProductGroupName          as MaterialGroupText           : localized,

          @ObjectModel.text.element: ['PlantLocationText']
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDNoCustomerLocWA_VH', element: 'FunctionalLocation' } }]
          @EndUserText.label: 'Plant Location'
          _ExtWaste.PlantLocation                                                       as PlantLocation,
          @UI.hidden: true
          _ExtWaste._PlantLocation._FuctionalLocationText.FunctionalLocationDescription as PlantLocationText           : localized,

          @ObjectModel.text.element: ['ContainerSourceAddress']
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocWA_VH', element: 'FunctionalLocation'} }]
          @EndUserText.label: 'Address - Container Source'
          _ExtWaste.ContainerSourceLocation                                             as ContainerSourceLocation,
          @UI.hidden: true
          _ExtWaste._ContainerSourceLocation.FullAddress                                as ContainerSourceAddress,
          @ObjectModel.text.element: ['ContainerFinalAddress']
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocWA_VH', element: 'FunctionalLocation'} }]
          @EndUserText.label: 'Address - Container Final'
          _ExtWaste.ContainerFinalLocation                                              as ContainerFinalLocation,
          @UI.hidden: true
          _ExtWaste._ContainerFinalLocation.FullAddress                                 as ContainerFinalAddress,

          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_MDTransptPackaggT_VH', element: 'ResourceKey' } }]
          //          @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
          _ExtWaste.ContainerTypeAtLocation                                             as ContainerTypeAtLocation,
          @ObjectModel.text.element: ['ContainerTypeAtLocationText']
          @UI.textArrangement: #TEXT_LAST
          @Consumption.filter.hidden: true
          _ExtWaste.ContainerTypeAtLocation                                             as ContainerTypeAtLocationWiText,

          @UI.hidden: true
          _ExtWaste._ContainerTypeAtLocation._Text.TransportPackagingTypeName           as ContainerTypeAtLocationText : localized,

          _ExtWaste.ContainerAtLocationCount                                            as ContainerAtLocationCount,
          _ExtWaste.ContainerAtLocationTidnr                                            as ContainerAtLocationTidnr,

          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_MDTransptPackaggT_VH', element: 'ResourceKey' } }]
          //          @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
          @ObjectModel.text.element: ['ContainerTypeNewText']
          _ExtWaste.ContainerTypeNew                                                    as ContainerTypeNew,
          @ObjectModel.text.element: ['ContainerTypeNewText']
          @UI.textArrangement:  #TEXT_LAST

          @Consumption.filter.hidden: true
          _ExtWaste.ContainerTypeNew                                                    as ContainerTypeNewWithText,
          @UI.hidden: true
          _ExtWaste._ContainerTypeNew._Text.TransportPackagingTypeName                  as ContainerTypeNewText        : localized,
          _ExtWaste.ContainerNewCount                                                   as ContainerNewCount,
          _ExtWaste.ContainerNewTidnr                                                   as ContainerNewTidnr,
          @UI.hidden: true
          _ExtWaste.ServiceFrequency                                                    as ServiceFrequency,
          @EndUserText.label: 'Service Frequency'
          _ExtWaste._ServiceFrequencyText.ServiceFrequencyText                          as ServiceFrequencyText        : localized,

         //map
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapTitle  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapPopup  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapColor  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapHighlight  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapSymbol  : abap.string,

          /* Associations */
          _ServiceTask        : redirected to composition child /PLCE/C_PDMNLServiceTaskWR,
          //_Attachments        : redirected to composition child /PLCE/C_PDMNLSERVICEATTACHMENT,
          _FunctionalLocation : redirected to /PLCE/C_PDMNLFuncLoc,
          _WorkAreaServices,
          _TourAssignments, //    : redirected to /PLCE/C_PDMNLTourServiceAsgWR,
          //_PlanningStatusText,
          _ServiceCore

}








*class LHC_SERVICETASK definition inheriting from CL_ABAP_BEHAVIOR_HANDLER.
*  private section.
*
*    methods calculateServiceTask for determine on modify
*      importing KEYS for ServiceTask~calculateServiceTask.
*
*endclass.
*
*class LHC_SERVICETASK implementation.
*
*  method calculateServiceTask.
*
*  endmethod.
*
*endclass.

CLASS lhc_Service DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_features FOR INSTANCE FEATURES
      IMPORTING keys REQUEST requested_features FOR Service RESULT result.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Service RESULT result.

    METHODS changeServiceType FOR MODIFY
      IMPORTING keys FOR ACTION Service~changeServiceType RESULT result.

    METHODS precheck_changeServiceType FOR PRECHECK
      IMPORTING keys FOR ACTION Service~changeServiceType.
    METHODS addAttachment FOR MODIFY
      IMPORTING keys FOR ACTION Service~addAttachment RESULT result.

*    methods testAction1 for modify
*      importing KEYS for action Service~testAction1.

*    methods precheck_testAction1 for precheck
*      importing KEYS for action Service~testAction1.

ENDCLASS.

CLASS lhc_Service IMPLEMENTATION.

  METHOD get_instance_features.
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
        FIELDS ( servicestatus ) WITH CORRESPONDING #( keys )
        RESULT DATA(services).
    result =
     VALUE #( FOR service IN services
              LET enable_by_status = COND #(
                                             WHEN service-servicestatus     = /plce/if_pd_constants=>c_pd_service_status-confirmed
                                             OR   service-servicestatus     = /plce/if_pd_constants=>c_pd_service_status-completed
                                             THEN if_abap_behv=>fc-o-disabled
                                             ELSE if_abap_behv=>fc-o-enabled )
              IN (                                %tky                      = service-%tky
                                                  %action-changeservicetype = enable_by_status )
            ).
  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD changeServiceType.
    DATA:
      ldata TYPE zcl_pd_proc_waste_orders=>zz_ty_data.

    READ TABLE keys INTO DATA(lkey) INDEX 1. "In-line action, only single entry!

    TRY.
        ldata = CORRESPONDING #(
        zcl_pd_proc_waste_orders=>zz_fill_service_task( par_key    = lkey-%key
                                                        par_svtpto = lkey-%param-ServiceTypeTo ) ).

        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY service
            UPDATE FROM CORRESPONDING #( ldata-srv_upd )
            CREATE BY \_servicetask SET FIELDS WITH ldata-tsks_crea
          ENTITY servicetask
            DELETE FROM CORRESPONDING #( ldata-tsks_dele )
            CREATE BY \_extwaste SET FIELDS WITH ldata-tsks_crea_wa
          ENTITY exttaskuniversal
            DELETE FROM CORRESPONDING #( ldata-tsks_dele_un )
          ENTITY exttaskwaste
            DELETE FROM CORRESPONDING #( ldata-tsks_dele_wa )
          MAPPED   DATA(lt_srv_map)
          REPORTED DATA(lt_srv_rep)
          FAILED   DATA(lt_srv_fai) ##SETFIELDS_OK.
        Failed   = CORRESPONDING #( DEEP lt_srv_fai ).
        Reported = CORRESPONDING #( DEEP lt_srv_rep ).

      CATCH /plce/cx_baseexception INTO DATA(lex).
        INSERT lex INTO TABLE reported-%other.
    ENDTRY.

  ENDMETHOD.

  METHOD precheck_changeServiceType.

    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
        FIELDS ( referenceid serviceid servicestatus servicetype ) WITH CORRESPONDING #( keys )
        RESULT DATA(services).

    IF lines( services ) > 1. "Now in-line action, only single line!
      INSERT new_message_with_text(
        text = 'Multiple services selected!' severity = if_abap_behv_message=>severity-error )
        INTO TABLE reported-%other.
      failed-service = VALUE #( FOR k IN keys ( %tky = k-%tky ) ).
*      REPORTED-SERVICE = value #( for K in KEYS where ( %TKY is not initial )
*      (
*        %TKY                      = K-%TKY
*        %ACTION-CHANGESERVICETYPE = IF_ABAP_BEHV=>MK-ON
*        %MSG                      = new /PLCE/CX_PD_EXCEPTION(
*                                SEVERITY  = IF_ABAP_BEHV_MESSAGE=>SEVERITY-ERROR
*                                TEXTID    = /PLCE/CX_PD_EXCEPTION=>CS_PDXXXX
*                                SERVICEID = SERVICES[ key ID %TKY = K-%TKY ]-SERVICEID )
*    ) ).
    ENDIF.
  ENDMETHOD.

*  method testAction1.
*  endmethod.
*
*  method precheck_testAction1.
*  endmethod.

  METHOD addAttachment.

    MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
       ENTITY Service
         CREATE BY \_Attachments
         AUTO FILL CID
         SET FIELDS WITH VALUE #( FOR k IN keys ( %pky = k-%pky
                                                  %target = VALUE #( ( Attachment = k-%param-attachment
                                                                       MimeType   = k-%param-mimetype
                                                                       Filename   = k-%param-filename
                                                                       Comments   = k-%param-comments ) )
                                                 ) )
       FAILED failed
       REPORTED reported
       MAPPED mapped.

    result = CORRESPONDING #( keys ).

  ENDMETHOD.

ENDCLASS.
    

  ENDMETHOD.
ENDCLASS.
