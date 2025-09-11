**The /PLCE/CL_PDMNL_TOUR_LOC_S_CALC**
  
  method if_sadl_exit_calc_element_read~calculate.
    data: lt_original_data type standard table of /PLCE/C_PDMNLTourWR with default key,
          lbadi            type ref to /plce/pd_ui_manual,
          lt_tour_map   type /plce/ppdmnltourmap.

    lt_original_data = corresponding #( it_original_data ).
    lt_tour_map = corresponding #( it_original_data ).

    if line_exists( it_requested_calc_elements[ table_line =  'STARTMAPTITLE' ] )
      or line_exists( it_requested_calc_elements[ table_line =  'STARTMAPPOPUP' ] )
      or line_exists( it_requested_calc_elements[ table_line =  'STARTMAPCOLOR' ] )
      or line_exists( it_requested_calc_elements[ table_line =  'STARTMAPHIGHLIGHT' ] )
      or line_exists( it_requested_calc_elements[ table_line =  'STARTMAPSYMBOL' ] ).

      get badi lbadi.
      call badi lbadi->get_tour_start_location_map changing ct_tour_map = lt_tour_map.

      loop at lt_original_data assigning field-symbol(<data>).
        data(ltour) = lt_tour_map[ TourUUID = <data>-TourUUID ].
        if ltour is not initial.
          if line_exists( it_requested_calc_elements[ table_line =  'STARTMAPTITLE' ] ).
            <data>-StartMapTitle = ltour-maptitle.
          endif.
          if line_exists( it_requested_calc_elements[ table_line =  'STARTMAPPOPUP' ] ).
            <data>-StartMapPopup = ltour-mappopup.
          endif.
          if line_exists( it_requested_calc_elements[ table_line =  'STARTMAPCOLOR' ] ).
            <data>-StartMapColor = ltour-mapcolor.
          endif.
          if line_exists( it_requested_calc_elements[ table_line =  'STARTMAPHIGHLIGHT' ] ).
            <data>-StartMapHighlight = ltour-maphighlight.
          endif.
          if line_exists( it_requested_calc_elements[ table_line =  'STARTMAPSYMBOL' ] ).
            <data>-StartMapSymbol = ltour-mapsymbol.
          endif.
        endif.
        clear ltour.
      endloop.

    endif.
    ct_calculated_data = corresponding #( lt_original_data ).
  endmethod.


  method if_sadl_exit_calc_element_read~get_calculation_info.
    if line_exists( it_requested_calc_elements[ table_line =  'STARTMAPTITLE' ] )
      or line_exists( it_requested_calc_elements[ table_line =  'STARTMAPPOPUP' ] )
      or line_exists( it_requested_calc_elements[ table_line =  'STARTMAPCOLOR' ] )
      or line_exists( it_requested_calc_elements[ table_line =  'STARTMAPHIGHLIGHT' ] )
      or line_exists( it_requested_calc_elements[ table_line =  'STARTMAPSYMBOL' ] ).
      append 'TOURUUID' to et_requested_orig_elements.
    endif.
  endmethod.

**/PLCE/R_PDTour_L**
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'UI Lookup CDS for PDTour'
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define view entity /PLCE/R_PDTour_L
  as select from /PLCE/R_PDTour
  association [0..1] to /PLCE/P_PDWorkStatus      as _WorkStatus      on  _WorkStatus.EntityUUID = $projection.TourUUID
                                                                      and _WorkStatus.TourUUID   = $projection.TourUUID
  association [0..*] to /PLCE/P_PDWorkStatusT     as _WorkStatusText  on  _WorkStatusText.Value = $projection.WorkStatus

  association [0..1] to /PLCE/P_PDTourWR_L        as _TourLookupWR    on  _TourLookupWR.TourUUID = $projection.TourUUID
  association [1]    to /PLCE/P_PDTourCriticality as _TourCriticality on  _TourCriticality.TourUUID = $projection.TourUUID

{
      @Semantics.uuid: true
      @UI.hidden: true
  key TourUUID,

      cast (
      case TourStatus
          when '01' // /PLCE/IF_PD_CONSTANTS=>C_PD_TOUR_STATUS-IN_PLANNING
            then 0
          when '02' // /PLCE/IF_PD_CONSTANTS=>C_PD_TOUR_STATUS-Released
            then 2
          when '03' // /PLCE/IF_PD_CONSTANTS=>C_PD_TOUR_STATUS-Completed
            then 1
          when '04' // /PLCE/IF_PD_CONSTANTS=>C_PD_TOUR_STATUS-Confirmed
            then 3
          else 0
       end as /plce/criticality_color_value preserving type )                                                                                                                as TourStatusColorValue,
      cast(
      case
        when CalculatedTourCapacity = 0 then 0
        when CalculatedTourCapacity <= 75 then 3
        when CalculatedTourCapacity <= 100 then 2
        else  1
        end as /plce/criticality_color_value preserving type )                                                                                                               as TourCapacityColorValue,

      cast (
        case when _GeoRoutes[1:TourUUID = TourUUID].TourUUID is null
            then ''
          else 'X'
        end as abap_boolean preserving type )                                                                                                                                as IsGeoRouteAvailable,

      @UI.hidden: true
      cast('H' as abap.unit( 3 ))                                                                                                                                            as DrivingTimeUnit,
      @Semantics.quantity.unitOfMeasure : 'DrivingTimeUnit'
      unit_conversion(quantity => _GeoRoutes.Duration, source_unit => _GeoRoutes.DurationUnit, target_unit => $projection.DrivingTimeUnit, error_handling => 'SET_TO_NULL' ) as DrivingTime,

      coalesce(_WorkStatus.WorkStatus, case TourStatus when '02' then '1' else '0' end)                                                                                      as WorkStatus,

      //Association
      _WorkStatusText,
      _TourLookupWR,
      _TourCriticality
}
