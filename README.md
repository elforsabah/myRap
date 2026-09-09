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
