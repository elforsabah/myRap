@EndUserText.label: 'Tour WR'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Search.searchable: true
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define root view entity /PLCE/C_PDMNLTourWR
  provider contract transactional_query
  as projection on /PLCE/R_PDTour
  association [1] to /PLCE/C_PDMNLTour as _TourCore on _TourCore.TourUUID = $projection.TourUUID
{
  key    TourUUID,
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
         _TourTemplate._Text.TourTemplateName                                            as TourTemplateName : localized,

         @UI.hidden: true
         _TourTemplate.ColorTour                                                         as ColorTour,

         @EndUserText.label: 'Tour Status'
         @ObjectModel.text.element: ['TourStatusText']
         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourStatus_VH', element: 'TourStatus' } }]
         @Consumption.filter: { multipleSelections: true, selectionType: #SINGLE }
         TourStatus,
         @Semantics.text: true
         @UI.hidden: true
         _PDTourStatusText.Description                                                   as TourStatusText   : localized,
         @UI.hidden: true
         _TourLookup.TourStatusColorValue                                                as TourStatusColorValue,
         @Consumption.filter.hidden: true
         _PDTourStatusText.IconURL                                                       as TourStatusIcon   : localized,

         @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
         StartDate                                                                       as TourStartDate,
         @Consumption.filter.selectionType: #INTERVAL
         EndDate                                                                         as TourEndDate,
         @Consumption.filter.selectionType: #INTERVAL
         ScheduledDateTimeStart                                                          as ScheduledDateTimeStart,

         @Semantics.quantity.unitOfMeasure: 'CalculatedTourDurationUnit'
         @Consumption.filter.hidden: true
         CalculatedTourDuration                                                          as TourDuration,
         @UI.hidden: true
         cast( CalculatedTourDurationUnit as meins preserving type )                     as CalculatedTourDurationUnit, //show localized key/allow fiori app value check
         @Consumption.filter.hidden: true
         CalculatedTourCapacity                                                          as TourCapacity,
//         @Consumption.filter.hidden: true
         @UI.hidden: true
         _TourLookup.TourCapacityColorValue                                              as TourCapacityColorValue,

         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDResource_VH', element: 'ResourceId'} }]
         MainResourceId                                                                  as MainResource,

         @Consumption.filter.selectionType: #SINGLE
         EarliestStartTime,
         //LatestStartTime,

         @Semantics.quantity.unitOfMeasure: 'MaximumTourDurationUnit'
         @Consumption.filter.hidden: true
         MaximumTourDuration,
         @UI.hidden: true
         cast( MaximumTourDurationUnit as meins preserving type )                        as MaximumTourDurationUnit, //show localized key/allow fiori app value check

         @EndUserText.label: 'Start Address'
         @Semantics.text: true
         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDNoCustomerLocWA_VH', element: 'FullAddress' },
                                              label            : 'Start Functional Location'
                                              }]
         _PDStartFunctionalLocation.FullAddress                                          as StartFLocFullAddress,
         @UI.hidden: true
         _PDStartFunctionalLocation._FuctionalLocationText.FunctionalLocationDescription as StartFLocName    : localized,
         @UI.hidden: true //@Consumption.filter.hidden: true
         _PDStartFunctionalLocation.Latitude                                             as StartLatitude,
         @UI.hidden: true //@Consumption.filter.hidden: true
         _PDStartFunctionalLocation.Longitude                                            as StartLongitude,

         @EndUserText.label: 'End Address'
         @Semantics.text: true
         @ObjectModel.text.element: [ 'EndFLocFullAddress' ]
         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDNoCustomerLocWA_VH', element: 'FullAddress' },
                                              label            : 'End Functional Location'
                                              }]
         _PDEndFunctionalLocation.FullAddress                                            as EndFLocFullAddress,
         @UI.hidden: true
         _PDEndFunctionalLocation._FuctionalLocationText.FunctionalLocationDescription   as EndFLocName      : localized,
         @UI.hidden: true //@Consumption.filter.hidden: true
         _PDEndFunctionalLocation.Latitude                                               as EndLatitude,
         @UI.hidden: true //@Consumption.filter.hidden: true
         _PDEndFunctionalLocation.Longitude                                              as EndLongitude,

          //map
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapTitle     : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapPopup     : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapColor     : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapHighlight : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapSymbol    : abap.string,

          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapTitle       : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapPopup       : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapColor       : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapHighlight   : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapSymbol      : abap.string,

         ExternalTourID,

         @UI.hidden: true
         CreatedForSimulation,

         //         PDProfile,
         @EndUserText.label: 'Route available'
         _TourLookup.IsGeoRouteAvailable,
         @EndUserText.label: 'Work Status'
         @UI.textArrangement: #TEXT_ONLY
         @ObjectModel.text.element: ['WorkStatusText']
         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDWorkStatus_VH', element: 'Value'} }]
         _TourLookup.WorkStatus,
         @UI.hidden: true
         _TourLookup._WorkStatusText.Description                                         as WorkStatusText   : localized,
         @Consumption.filter.hidden: true
         _TourLookup._WorkStatusText.IconURL                                             as WorkStatusIcon   : localized,

         @EndUserText.label: 'Driving Time'
         @Consumption.filter.hidden: true
         _TourLookup.DrivingTime                                                         as DrivingTime,
         @UI.hidden: true
         _TourLookup.DrivingTimeUnit                                                     as DrivingTimeUnit,

         @UI.hidden: true
         _TourLookup._TourCriticality.TourCriticality                                    as TourCriticality,
         @EndUserText.label: 'Deviations on Tour'
         _TourLookup._TourCriticality.DeviationsOnTour                                   as DeviationsOnTour,

         @UI.hidden: true
         _TourLookup._TourLookupWR.WeightUnit                                            as WeightUnit,
         @Consumption.filter.hidden: true
         _TourLookup._TourLookupWR.MaterialWeightInTOSum                                 as MaterialWeightInTOSum,
         @UI.hidden: true
         @Semantics.quantity.unitOfMeasure: 'WeightUnit'
         _TourLookup._TourLookupWR.VehicleCapacityInTOSum                                as VehicleCapacityInTOSum,
         @Consumption.filter.hidden: true
         @UI.hidden: true
         _TourLookup._TourLookupWR.CriticalTonnage                                       as CriticalityTonnage,

         @UI.hidden: true
         _TourLookup._TourLookupWR.CountServices                                         as CountServices,
         @UI.hidden: true
         _TourLookup._TourLookupWR.CountServicesCompleted                                as CountServicesCompleted,

         /*Associations*/
         _TourCore,
         _ServiceAssignments  : redirected to composition child /PLCE/C_PDMNLTourServiceAsgWR,
         _TaskAssignments     : redirected to composition child /PLCE/C_PDMNLTourTaskAsgmtWR,
         _Resources           : redirected to /PLCE/C_PDMNLTourResource,
         _Attachments         : redirected to /PLCE/C_PDMNLTourAttachment,
         _FunctionalLocations : redirected to /PLCE/C_PDMNLTourFuncLoc,
         _GeoRoutes           : redirected to /PLCE/C_PDMNLTourGeoRoute,
         _WorkAreaTours
         //         /PLCE/R_PDTour._Profiles as _Profiles

}



extend view entity /PLCE/C_PDMNLTourWR with {
//   /PLCE/R_PDTour._ExtCustom.zz_remaining_duration, // Restdauer
   
 
   // 1. Used Capacity (MUST BE DEC FOR PROGRESS BAR TO WORK)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
//   @EndUserText.label: 'Kapazitätsauslastung'
 
//   @UI.hidden: true
   virtual tour_capacity_new : abap.dec( 10, 2 ), 

   // 2. Total Duration (TARGET - Link Unit HERE)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit' 
//   @UI.hidden: true
   virtual tour_duration_min : abap.quan( 10, 2 ),      

   // 3. Unit Field (Holds 'MIN')
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.unitOfMeasure: true
   @UI.hidden: true
   virtual zz_duration_unit : abap.unit( 3 ),

   // 4. Remaining Duration (Standard display)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
//   @EndUserText.label: 'Restdauer'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
//   @UI.hidden: true
   virtual zz_remaining_duration : abap.quan( 10, 2 ),
   
   
   // 4. MaximumTourDuration converted (Standard display)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
//   @EndUserText.label: 'Converted MaximumTourDuration '
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
   @UI.hidden: true
   virtual MaximumTourDuration_Converted : abap.quan( 10, 2 ),
   
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @UI.hidden: true
   virtual zz_capacity_criticality : abap.int1
   
   
}
 
    
CLASS zcl_wr_tour_extend_calc DEFINITION
  PUBLIC
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES if_sadl_exit .
    INTERFACES if_sadl_exit_calc_element_read .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_wr_tour_extend_calc IMPLEMENTATION.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    " Request the base Tour fields needed for math and matching
    APPEND 'DRIVINGTIME'             TO et_requested_orig_elements.
     " --- NEW: Request the MaximumTourDuration and its Unit ---
    APPEND 'MAXIMUMTOURDURATION'     TO et_requested_orig_elements.
    APPEND 'MAXIMUMTOURDURATIONUNIT' TO et_requested_orig_elements.
    APPEND 'TOURCAPACITY'            TO et_requested_orig_elements.
    APPEND 'TOURDURATION'            TO et_requested_orig_elements.
    APPEND 'TOURUUID'                TO et_requested_orig_elements.


  ENDMETHOD.

   METHOD if_sadl_exit_calc_element_read~calculate.
    " -------------------------------------------------------------------------
    " 1. Data Declarations
    " -------------------------------------------------------------------------
    TYPES: BEGIN OF ty_tour_key,
             touruuid TYPE sysuuid_x16,
           END OF ty_tour_key.

    TYPES: BEGIN OF ty_service_data,
             touruuid             TYPE sysuuid_x16,
             totalserviceduration TYPE p LENGTH 10 DECIMALS 2,
             totaltimeadjustment  TYPE p LENGTH 10 DECIMALS 2,
             TotalTaskDuration  TYPE p LENGTH 10 DECIMALS 2,
           END OF ty_service_data.

    DATA: lt_tour_keys   TYPE STANDARD TABLE OF ty_tour_key,
          ls_tour_key    TYPE ty_tour_key,
          lt_service_agg TYPE STANDARD TABLE OF ty_service_data.

    DATA: lv_tour_uuid    TYPE sysuuid_x16,
          lv_tour_dur_h   TYPE p LENGTH 10 DECIMALS 2,
          lv_driving_h    TYPE p LENGTH 10 DECIMALS 2,

          lv_tour_dur_min TYPE p LENGTH 10 DECIMALS 2,
          lv_driving_min  TYPE p LENGTH 10 DECIMALS 2,

          lv_svc_dur_h    TYPE p LENGTH 10 DECIMALS 2,
          lv_svc_dur_min  TYPE p LENGTH 10 DECIMALS 2,
          lv_svc_adj_min  TYPE p LENGTH 10 DECIMALS 2,
          lv_svc_task_min  TYPE p LENGTH 10 DECIMALS 2,

          lv_new_dur_min  TYPE p LENGTH 10 DECIMALS 2,
          lv_rem_min      TYPE p LENGTH 10 DECIMALS 2,

          lv_max_dur      TYPE p LENGTH 10 DECIMALS 3,
          lv_max_dur_min  TYPE p LENGTH 10 DECIMALS 2. " Maximum capacity in minutes

    FIELD-SYMBOLS: <ls_orig>             TYPE any,
                   <ls_calc>             TYPE any,
                   <lv_val_uuid>         TYPE any,
                   <lv_val_dur>          TYPE any,
                   <lv_val_cap>          TYPE any,
                   <lv_val_drive>        TYPE any,

                   <lv_val_max_dur>      TYPE any,
                   <lv_val_max_dur_unit> TYPE any,

                   <lv_set_cap>          TYPE any,
                   <lv_set_rem>          TYPE any,
                   <lv_set_tot>          TYPE any,
                   <lv_set_unit>         TYPE any,
                   <lv_set_max_conv>     TYPE any.

    " -------------------------------------------------------------------------
    " 2. Collect all Tour UUIDs for our fast DB query
    " -------------------------------------------------------------------------
    LOOP AT it_original_data ASSIGNING <ls_orig>.
      ASSIGN COMPONENT 'TOURUUID' OF STRUCTURE <ls_orig> TO <lv_val_uuid>.
      IF sy-subrc = 0 AND <lv_val_uuid> IS NOT INITIAL.
        ls_tour_key-touruuid = <lv_val_uuid>.
        APPEND ls_tour_key TO lt_tour_keys.
      ENDIF.
    ENDLOOP.

    " Remove duplicates to make the query even faster
    SORT lt_tour_keys BY touruuid.
    DELETE ADJACENT DUPLICATES FROM lt_tour_keys COMPARING touruuid.

    " -------------------------------------------------------------------------
    " 3. Fetch Aggregated Service Data from our new CDS View
    " -------------------------------------------------------------------------
    IF lt_tour_keys IS NOT INITIAL.
      SELECT touruuid,
             totalserviceduration,
             totaltimeadjustment,
             TotalTaskDuration
        FROM zi_tour_svc_sum_wr   " <--- Must match your new CDS view name
        FOR ALL ENTRIES IN @lt_tour_keys
        WHERE touruuid = @lt_tour_keys-touruuid
        INTO TABLE @lt_service_agg.
    ENDIF.

    " -------------------------------------------------------------------------
    " 4. Perform math and assign to virtual fields row-by-row
    " -------------------------------------------------------------------------
    LOOP AT it_original_data ASSIGNING <ls_orig>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calc> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " --- A. Get Source Tour Fields ---
      ASSIGN COMPONENT 'DRIVINGTIME'             OF STRUCTURE <ls_orig> TO <lv_val_drive>.
      ASSIGN COMPONENT 'TOURDURATION'            OF STRUCTURE <ls_orig> TO <lv_val_dur>.
      ASSIGN COMPONENT 'TOURCAPACITY'            OF STRUCTURE <ls_orig> TO <lv_val_cap>.
      ASSIGN COMPONENT 'TOURUUID'                OF STRUCTURE <ls_orig> TO <lv_val_uuid>.

      ASSIGN COMPONENT 'MAXIMUMTOURDURATION'     OF STRUCTURE <ls_orig> TO <lv_val_max_dur>.
      ASSIGN COMPONENT 'MAXIMUMTOURDURATIONUNIT' OF STRUCTURE <ls_orig> TO <lv_val_max_dur_unit>.

      IF sy-subrc <> 0. CONTINUE. ENDIF.

      lv_tour_uuid  = <lv_val_uuid>.
      lv_tour_dur_h = COND #( WHEN <lv_val_dur> IS ASSIGNED THEN <lv_val_dur> ELSE 0 ).
      lv_driving_h  = COND #( WHEN <lv_val_drive> IS ASSIGNED THEN <lv_val_drive> ELSE 0 ).

      " --- B. Convert Maximum Tour Duration to Minutes FIRST ---
      IF <lv_val_max_dur> IS ASSIGNED AND <lv_val_max_dur_unit> IS ASSIGNED.
        lv_max_dur = <lv_val_max_dur>.
        IF <lv_val_max_dur_unit> = 'H'.
          lv_max_dur_min = lv_max_dur * 60.
        ELSE.
          lv_max_dur_min = lv_max_dur. " Assume already MIN if not H
        ENDIF.
      ELSE.
        lv_max_dur_min = 0.
      ENDIF.

      " Convert standard Tour fields to Minutes
      lv_tour_dur_min = lv_tour_dur_h * 60.
      lv_driving_min  = lv_driving_h * 60.

      " --- C. Read pre-aggregated Service Data from our DB result ---
      READ TABLE lt_service_agg INTO DATA(ls_agg) WITH KEY touruuid = lv_tour_uuid.
      IF sy-subrc = 0.
        lv_svc_dur_h   = ls_agg-totalserviceduration.
        lv_svc_adj_min = ls_agg-totaltimeadjustment. " Already in minutes
        lv_svc_task_min = ls_agg-totaltaskduration.  " Task
      ELSE.
        CLEAR: lv_svc_dur_h, lv_svc_adj_min, lv_svc_task_min.
      ENDIF.

      " Convert Service TotalDuration to Minutes
      lv_svc_dur_min = lv_svc_dur_h * 60.

      " --- D. THE MATH ---
      IF lv_max_dur_min = 0.
        " If Maximum Duration is 0, force everything to 0 for a clean '0 von 0' UI
        lv_new_dur_min  = 0.
        lv_rem_min      = 0.
        lv_tour_dur_min = 0. " Overwrites target to 0
      ELSE.
        " 1. NewDuration = Service Duration(Min) + DrivingTime(Min) + Adjustment(Min)
        lv_new_dur_min = lv_svc_dur_min + lv_driving_min + lv_svc_adj_min + lv_svc_task_min.

        " 2. Remaining = Maximum Duration(Min) - NewDuration
        " (This correctly caps the available time to the Maximum Duration)
        lv_rem_min = lv_max_dur_min - lv_new_dur_min.
      ENDIF.

        " --- NEW: Determine Color (Criticality) ---
      DATA lv_crit TYPE int1.
      IF lv_max_dur_min = 0.
        lv_crit = 0. " Neutral/Grey
      ELSEIF lv_new_dur_min > lv_max_dur_min.
        lv_crit = 1. " 1 = RED (Over capacity)
      ELSEIF lv_new_dur_min >= ( lv_max_dur_min * '0.75' ).
        lv_crit = 2. " 2 = YELLOW (Warning: 90% or more full)
      ELSE.
        lv_crit = 3. " 3 = GREEN (Good)
      ENDIF.

      " Map the color to the UI
      ASSIGN COMPONENT 'ZZ_CAPACITY_CRITICALITY' OF STRUCTURE <ls_calc> TO FIELD-SYMBOL(<lv_set_crit>).
      IF sy-subrc = 0. <lv_set_crit> = lv_crit. ENDIF.

      " Map 1: tour_capacity_new = NewDuration (Used Capacity)
      ASSIGN COMPONENT 'TOUR_CAPACITY_NEW' OF STRUCTURE <ls_calc> TO <lv_set_cap>.
      IF sy-subrc = 0. <lv_set_cap> = lv_new_dur_min. ENDIF.

      " Map 2: tour_duration_min = TourDuration (Target for Progress Bar)
      ASSIGN COMPONENT 'TOUR_DURATION_MIN' OF STRUCTURE <ls_calc> TO <lv_set_tot>.
      IF sy-subrc = 0. <lv_set_tot> = lv_tour_dur_min. ENDIF.

      " Map 3: zz_remaining_duration = Remaining Time
      ASSIGN COMPONENT 'ZZ_REMAINING_DURATION' OF STRUCTURE <ls_calc> TO <lv_set_rem>.
      IF sy-subrc = 0. <lv_set_rem> = lv_rem_min. ENDIF.

      " Map 4: Force Unit to 'MIN' for the UI
      ASSIGN COMPONENT 'ZZ_DURATION_UNIT' OF STRUCTURE <ls_calc> TO <lv_set_unit>.
      IF sy-subrc = 0. <lv_set_unit> = 'MIN'. ENDIF.

      " Map 5: MaximumTourDuration_Converted
      ASSIGN COMPONENT 'MAXIMUMTOURDURATION_CONVERTED' OF STRUCTURE <ls_calc> TO <lv_set_max_conv>.
      IF sy-subrc = 0. <lv_set_max_conv> = lv_max_dur_min. ENDIF.
    ENDLOOP.
  ENDMETHOD.
ENDCLASS.


