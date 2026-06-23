" Map 7: BMS criticality from ZZ_BMS_STATUS
      ASSIGN COMPONENT 'ZZBMSSTATUS' OF STRUCTURE <ls_orig>
        TO FIELD-SYMBOL(<lv_bms_status>).

      IF sy-subrc = 0.
        DATA lv_bms_crit TYPE int1.
        CASE <lv_bms_status>.
          WHEN 'SENT'        OR 'FREIGEGEBEN' OR 'BEENDET'.
            lv_bms_crit = 3.    " green
          WHEN 'PARTIAL'     OR 'BEGONNEN'.
            lv_bms_crit = 2.    " orange
          WHEN 'ERROR'.
            lv_bms_crit = 1.    " red
          WHEN OTHERS.
            lv_bms_crit = 0.    " grey — initial / STORNIERT
        ENDCASE.

        ASSIGN COMPONENT 'ZZBMSCRITICALITY' OF STRUCTURE <ls_calc>
          TO FIELD-SYMBOL(<lv_set_bms_crit>).
        IF sy-subrc = 0.
          <lv_set_bms_crit> = lv_bms_crit.
        ENDIF.
      ENDIF.


@ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
  @EndUserText.label: 'BMS Criticality'
  @UI.hidden: true
  virtual ZzBmsCriticality : abap.int1,
