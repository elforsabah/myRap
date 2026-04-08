
Punkt 1: Prozessintegration im SAP P&D (Das UI )

Der neue Knopf "Tour an BMS freigeben" wird auf System gebaut. 
  -  Wählt der Mitarbeiter (Disponent) eine Tour aus und drückt diesen neuen Knopf, sammelt das SAP-System automatisch alle einzelnen Aufträge zusammen, die auf dieser Tour gefahren werden sollen.
  - Das hinzufügen der Knopf ist wie bei andere Knöpfe auch möglich auf die Tourliste 

- Die Erfolgsmeldung (Status): Nach dem Klick muss der Mitarbeiter sofort sehen können, ob die Daten erfolgreich an das BMS gesendet wurden. Dafür gibt es die Spalte "BMS-Status". Dort erscheint dann beispielsweise ein grüner Haken, wenn alles gut ging, oder ein rotes Kreuz, wenn ein Fehler aufgetreten ist.

- Das Hinzufügen eine Spalte ist hier auch möglich wie bei

Punkt 2:  Daten-mapping: Von SA-Daten zu JSON-Payload
- Damit der Fahrer die SAP-Daten verstehen, müssen wir sie passend übersetzen. Dafür muss der TEC-Aufträge_v0.json Felder zu Felder aus die Aufträge richtig gemapped werden


Punkt 3: Der Datenversand (Die technische Kommunikation)
Hier geht es darum, wie die Daten vom SAP-System auf dem BMS System  kommen.
  - Sobald der Disponent auf "Freigeben" klickt, schnürt SAP ein elektronisches Paket mit all den oben genannten Daten und schickt es über das Netzwerk direkt an das System der Tablets (BMS).

-Wenn das Paket nicht ankommt (z. B. weil das Zielsystem gerade nicht erreichbar ist oder Pflichtdaten fehlen), bekommt der Disponent im SAP sofort eine verständliche Fehlermeldung. So weiß er direkt, was er korrigieren muss.

Punkt 4: Der Rückweg der Daten (Bidirektionale Schnittstelle)

Wenn der Fahrer draußen seinen Auftrag beendet, soll sein Tablet die gesammelten Echt-Daten (wann war er da, wie schwer war der Container?) automatisch zurück an das SAP-System schicken. Das SAP-System muss also darauf vorbereitet werden, diese Rückmeldungen elektronisch zu empfangen und direkt zu erfassen(verbuchen). 
Dafür werden wir voraussichtlich einen OData-Service oder REST-Endpunkt im SAP-System bereitstellen müssen, den das BMS aufruft.




CLASS lhc_Tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Tour RESULT result.

    METHODS createtour FOR MODIFY
      IMPORTING keys FOR ACTION Tour~createtour RESULT result.

    METHODS precheck_createtour FOR PRECHECK
      IMPORTING keys FOR ACTION Tour~createtour .

    METHODS assign_earliest_to_latest_date FOR DETERMINE ON MODIFY
    IMPORTING keys FOR Tour~assign_earliest_to_latest_date.
    METHODS touranBMSfreigeben FOR MODIFY
      IMPORTING keys FOR ACTION Tour~touranBMSfreigeben RESULT result.
ENDCLASS.

CLASS lhc_Tour IMPLEMENTATION.

  METHOD assign_earliest_to_latest_date.

    READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY Tour
        FIELDS ( StartDate EndDate )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_tour).

    MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY Tour
        UPDATE FIELDS ( EndDate )
        WITH VALUE #(
          FOR ls IN lt_tour
          WHERE ( StartDate IS NOT INITIAL AND EndDate IS INITIAL )
          ( %tky    = ls-%tky
            EndDate = ls-StartDate )
        )
      FAILED   DATA(lt_fai)
      REPORTED DATA(lt_rep).

  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

METHOD createtour.

    DATA lv_first_date   TYPE /plce/date.
    DATA lv_last_date    TYPE /plce/date.
    DATA lv_current_date TYPE /plce/date.
    DATA lv_exists       TYPE abap_bool. " Moved outside the loop

    " 1) Define a helper table to store the date alongside the message
    TYPES: BEGIN OF ty_msg_sort,
             date TYPE /plce/date,
             msg  TYPE REF TO if_abap_behv_message,
           END OF ty_msg_sort.
    DATA lt_msg_sort TYPE STANDARD TABLE OF ty_msg_sort.

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
        DATA(lv_tmp)  = lv_first_date.
        lv_first_date = lv_last_date.
        lv_last_date  = lv_tmp.
      ENDIF.

      lv_current_date = lv_first_date.

      " 2) Create one tour per day in the range
      WHILE lv_current_date <= lv_last_date.

        " Explicitly clear the variable on EVERY day's iteration
        CLEAR lv_exists.


        SELECT SINGLE @abap_true
          FROM /plce/r_pdtour
          WHERE TourTemplate = @<group>-template
            AND StartDate    = @lv_current_date
          INTO @lv_exists.

        IF lv_exists = abap_true.

          " 2) Append to the helper table instead of reported-%other directly
          APPEND VALUE #( date = lv_current_date
                          msg  = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT' " Your message class
                                   number   = '008' "
                                   severity = if_abap_behv_message=>severity-information " or warning
                                   v1       = |{ lv_current_date DATE = USER }|
                                   v2       = <group>-template
                                 )
                        ) TO lt_msg_sort.

        ELSE.

          DATA(lv_internal_cid) = cl_system_uuid=>create_uuid_x16_static( ).

          MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
            ENTITY Tour
              EXECUTE createTourWithTemplate
              FROM VALUE #(
                ( %cid                 = lv_internal_cid
                  %param-without_draft = 'X'
                  %param-tour_template = <group>-template
                  %param-start_date    = lv_current_date ) )
            MAPPED   DATA(mapped_tour)
            FAILED   DATA(failed_tour)
            REPORTED DATA(reported_tour).

            " Read created tour(s) to build result & message
            READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
              ENTITY Tour
                ALL FIELDS
                WITH CORRESPONDING #( mapped_tour-tour )
              RESULT DATA(tours).

            IF lines( tours ) > 0.
              " 3) Append to the helper table instead of reported-%other directly
              APPEND VALUE #( date = lv_current_date
                              msg  = new_message(
                                       id       = 'Z_MSG_SVR_TOUR_EXT'
                                       number   = '007' " Create a MSG: 'Success: Tour created for &1'
                                       severity = if_abap_behv_message=>severity-success
                                       v1       = |{ lv_current_date DATE = USER }|
                                       v2       = |{ tours[ 1 ]-TourId ALPHA = OUT }|
                                     )
                            ) TO lt_msg_sort.

              " Static action result: append all created tours using the ORIGINAL UI CID
              result = VALUE #( BASE result
                                FOR tour IN tours
                                (
                                 %cid   = lv_internal_cid
                                 %param = tour ) ).
            ENDIF.
        ENDIF.

        " next day
        lv_current_date = lv_current_date + 1.

      ENDWHILE.
    ENDLOOP.

    " 4) Sort the helper table by the date we saved
    SORT lt_msg_sort BY date DESCENDING.

    " 5) Move the sorted messages into reported-%other
    LOOP AT lt_msg_sort INTO DATA(ls_msg_sort).
      insert ls_msg_sort-msg into reported-%other INDEX 1.
    ENDLOOP.

  ENDMETHOD.



  METHOD precheck_createtour.
    " Today (system date)
    DATA(lv_today) = cl_abap_context_info=>get_system_date( ).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>).

      DATA lv_failed   TYPE abap_bool VALUE abap_false.
      DATA lv_start    TYPE /plce/date.
      DATA lv_end      TYPE /plce/date.
      DATA lv_template TYPE /plce/pdtour_template.

      lv_start    = <key>-%param-start_date.
      lv_end      = <key>-%param-end_date.
      lv_template = <key>-%param-tour_template.

      " unique CID for this inner action call
      DATA(lv_cid) = cl_system_uuid=>create_uuid_x16_static( ).

      " 1) Start date must not be empty
      IF lv_start IS INITIAL.
        APPEND VALUE #(
            %cid = <key>-%cid
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
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
                   id       = 'Z_MSG_SVR_TOUR_EXT'
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
      IF lv_template IS INITIAL.
        APPEND VALUE #(
             %cid = <key>-%cid
             %msg = new_message(
                      id       = 'Z_MSG_SVR_TOUR_EXT'
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
  ENDMETHOD.
  METHOD touranBMSfreigeben.



  ENDMETHOD.

ENDCLASS.


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Planning and Dispatching Tour'
@ObjectModel.semanticKey: ['TourId']
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define root view entity /PLCE/R_PDTour
  as select from /plce/tpdtour
  // compositions
  composition [0..*] of /PLCE/R_PDTourTaskAssignment as _TaskAssignments
  composition [0..*] of /PLCE/R_PDTourServiceAsgmt   as _ServiceAssignments
  composition [0..*] of /PLCE/R_PDTourResource       as _Resources
  composition [0..*] of /PLCE/R_PDTourAttachment     as _Attachments
  composition [0..*] of /PLCE/R_PDTourTask           as _TourTasks
  composition [0..*] of /PLCE/R_PDTourProductGroup   as _ProductGroups
  composition [0..*] of /PLCE/R_PDTourFuncLoc        as _FunctionalLocations
  composition [0..*] of /PLCE/R_PDTourPlant          as _Plants
  composition [0..*] of /PLCE/R_PDTourProfile        as _Profiles
  composition [0..1] of /PLCE/R_PDTourGeoRoute       as _GeoRoutes
  composition [0..*] of /PLCE/R_PDTourStatusHistory  as _StatusHistory
  //extensions  
  composition [0..1] of /PLCE/R_PDTourExtCustom      as _ExtCustom

  // associations
  //  association [1]    to /PLCE/R_PDProfile               as _Profile                on  $projection.PDProfile = _Profile.Profile
  association [1]    to /PLCE/R_PDTourTemplate       as _TourTemplate              on  $projection.TourTemplate = _TourTemplate.TourTemplate
  association [0..1] to /PLCE/R_PDFunctionalLocation as _PDStartFunctionalLocation on  $projection.StartFunctionalLocation = _PDStartFunctionalLocation.FunctionalLocation
  association [0..1] to /PLCE/R_PDFunctionalLocation as _PDEndFunctionalLocation   on  $projection.EndFunctionalLocation = _PDEndFunctionalLocation.FunctionalLocation
  association [0..*] to /PLCE/P_PDTourStatusT        as _PDTourStatusText          on  $projection.TourStatus = _PDTourStatusText.Value
  association [0..1] to /PLCE/R_PDTourResource       as _MainResource              on  $projection.MainResourceId = _MainResource.ResourceId
                                                                                   and $projection.TourUUID       = _MainResource.TourUUID
  association [1..1] to /PLCE/R_PDTour_L             as _TourLookup                on  $projection.TourUUID = _TourLookup.TourUUID
  //TODO anpassen nach AssignmentUmbau
  association [1]    to /PLCE/P_PDTourStatistic      as _Statistic                 on  _Statistic.TourUuid = $projection.TourUUID
  association [0..*] to /PLCE/P_PDWorkAreaTour       as _WorkAreaTours             on  _WorkAreaTours.TourUUID = $projection.TourUUID

  association [0..*] to /PLCE/R_PDMTourServiceResult as _TourResults               on  $projection.TourUUID = _TourResults.TourUUID

  association [0..*] to /PLCE/R_BGProcessingEntity   as _PendingProcessings        on  _PendingProcessings.EntityType = 'TOUR'
                                                                                   and _PendingProcessings.EntityUUID = $projection.TourUUID
{
      @Semantics.uuid: true
      @UI.hidden: true
  key tour_uuid                      as TourUUID,
      @EndUserText.label: 'Tour ID'
      tour_id                        as TourId,
      @ObjectModel.foreignKey.association: '_TourTemplate'
      tour_template                  as TourTemplate,
      //      tour_type                  as TourType,
//      @ObjectModel.text.association: '_PDTourStatusText'
      tour_status                    as TourStatus,

      @EndUserText.label: 'Scheduled Start'
      @Semantics.dateTime: true
      date_time_start                as ScheduledDateTimeStart, //TODO default calculation / later optimizer
      @EndUserText.label: 'Tour Start Date'
      date_start                     as StartDate,
      @EndUserText.label: 'Tour End Date'
      date_end                       as EndDate,
      earliest_start_time            as EarliestStartTime,
      latest_start_time              as LatestStartTime,
      start_functional_location      as StartFunctionalLocation,
      end_functional_location        as EndFunctionalLocation,
      @EndUserText.label: 'Maximum Tour Duration'
      @Semantics.quantity.unitOfMeasure: 'MaximumTourDurationUnit'
      maximum_tour_duration          as MaximumTourDuration,
      maximum_tour_duration_unit     as MaximumTourDurationUnit,

      @Semantics.quantity.unitOfMeasure: 'CalculatedTourDurationUnit'
      calculated_tour_duration       as CalculatedTourDuration,
      calculated_tour_duration_unit  as CalculatedTourDurationUnit,
      @EndUserText.label: 'Capacity used (in %)'
      calculated_tour_capacity       as CalculatedTourCapacity,

      external_tour_id               as ExternalTourID,
      created_for_simulation         as CreatedForSimulation,
      //      pd_profile                                           as PDProfile,

      @EndUserText.label: 'Main Resource'
      main_resource_id               as MainResourceId,

      @EndUserText.label: 'Additional Information'
      additional_text                as AdditionalText,

      @EndUserText.label: 'Resource Assignment Changed At'
      resource_assignment_changed_at as ResourceAssignmentChangedAt,
      // is updated when a /PLCE/R_PDTourTaskAssignment was changed 
      @EndUserText.label: 'Service Assignment Changed At'
      service_assignments_changed_at as ServiceAssignmentChangedAt,

      @EndUserText.label: 'Sequence Mode'
      sequence_mode                  as SequenceMode,


      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      created_by                     as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      created_at                     as CreatedAt,
      @Semantics.user.lastChangedBy: true
      @EndUserText.label: 'Last Changed By'
      last_changed_by                as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      @EndUserText.label: 'Last Changed At'
      last_changed_at                as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      @EndUserText.label: 'Local Last Changed At'
      local_last_changed_at          as LocalLastChangedAt,

      //only for controlling process flow / no persistence
      cast('' as /plce/char20 )      as SessionProcessFlag,

      //Compositions
      _Resources,
      _Attachments,

      //Association
      _Statistic,
      _MainResource,
      _TourTasks,
      _TourTemplate,
      _TourLookup,
      _ProductGroups,
      _FunctionalLocations,
      _Plants,
      _PDStartFunctionalLocation,
      _PDEndFunctionalLocation,
      _PDTourStatusText,
      //      _Profile,
      _TaskAssignments,
      _ServiceAssignments,
      _WorkAreaTours,
      _Profiles,
      _TourResults,
      _GeoRoutes,
      _StatusHistory,
      _PendingProcessings,
      
      //Extension
      _ExtCustom
}
