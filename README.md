@ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'  // Add for the flag
   virtual point_of_origin : zwr_point_origin_wdoi  // Use your type


<lv_point_of_origin> TYPE zwr_point_origin_wdoi.

ASSIGN COMPONENT 'POINT_OF_ORIGIN' OF STRUCTURE <ls_calculated> TO <lv_point_of_origin>.
        IF sy-subrc = 0.
          <lv_point_of_origin> = lv_zzpoint_origin_wdoi.  // Set dynamically on read
        ENDIF.
