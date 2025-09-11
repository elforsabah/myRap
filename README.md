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

