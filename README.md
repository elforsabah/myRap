CLASS zcl_container_stock_calc DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    INTERFACES if_sadl_exit_calc_element_read.

  PRIVATE SECTION.
    " Lagerorte (T001L, Werk 4410). Lager A/B/C bestätigt; Werkstätten offen.
    CONSTANTS:
      BEGIN OF gc_loc,
        lager_a     TYPE lgort_d VALUE '3002',   " 02 Lager A
        lager_b     TYPE lgort_d VALUE '3003',   " 03 Lager B
        lager_c     TYPE lgort_d VALUE '3004',   " 04 Lager C
        werkstatt_w TYPE lgort_d VALUE 'WKW',    " TODO: echten LGORT eintragen
        werkstatt_x TYPE lgort_d VALUE 'WKX',    " TODO: echten LGORT eintragen
      END OF gc_loc.
ENDCLASS.

CLASS zcl_container_stock_calc IMPLEMENTATION.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    et_requested_orig_elements = VALUE #( ( `CONTAINERTYPE` )
                                          ( `SAFETYSTOCK` ) ).
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~calculate.

    DATA lt_result TYPE STANDARD TABLE OF zi_containerstock WITH DEFAULT KEY.
    lt_result = CORRESPONDING #( it_original_data ).

    IF lt_result IS INITIAL.
      RETURN.
    ENDIF.

    " "Lager"-Selektion 1:1 aus ZWR_WATP_CONTAINER_OVERVIEW (psel = 'L'),
    " ergänzt um Dummies-Ausschluss (EQUI~HERST) + Sicherheitsbestand (Tabelle).
    DATA lv_empty_logiknr TYPE logiknr.       " initial = auf Lager

    SELECT a~matnr   AS behtyp,
           s~b_lager AS lgort,
           e~herst   AS herst
      FROM /watp/vcaequz AS a
      INNER JOIN etyp AS t      ON a~matnr = t~matnr
      INNER JOIN equi AS e      ON e~equnr = a~equnr
      LEFT OUTER JOIN eqbs AS s ON a~equnr = s~equnr
      FOR ALL ENTRIES IN @lt_result
      WHERE a~matnr    =  @lt_result-containertype
        AND a~logiknr  =  @lv_empty_logiknr
        AND a~datab    <= @sy-datum
        AND a~datbi    >= @sy-datum
        AND t~kombinat =  'B'
      INTO TABLE @DATA(lt_equip).

    DELETE lt_equip WHERE herst = 'DUMMY'.

    " Behältertyp-Bezeichnung (etyp: bautxt/bauform – wie im Report)
    SELECT matnr, bauform, bautxt FROM etyp
      FOR ALL ENTRIES IN @lt_result
      WHERE matnr = @lt_result-containertype
      INTO TABLE @DATA(lt_behtxt).
    SORT lt_behtxt BY matnr.

    LOOP AT lt_result ASSIGNING FIELD-SYMBOL(<row>).

      CLEAR: <row>-lagera, <row>-lagerb, <row>-lagerc,
             <row>-werkstattw, <row>-werkstattx,
             <row>-withoutlocation, <row>-otherlocations, <row>-totalonstock.

      LOOP AT lt_equip ASSIGNING FIELD-SYMBOL(<eq>) WHERE behtyp = <row>-containertype.
        <row>-totalonstock += 1.
        IF <eq>-lgort IS INITIAL.
          <row>-withoutlocation += 1.
        ELSE.
          CASE <eq>-lgort.
            WHEN gc_loc-lager_a.     <row>-lagera += 1.
            WHEN gc_loc-lager_b.     <row>-lagerb += 1.
            WHEN gc_loc-lager_c.     <row>-lagerc += 1.
            WHEN gc_loc-werkstatt_w. <row>-werkstattw += 1.
            WHEN gc_loc-werkstatt_x. <row>-werkstattx += 1.
            WHEN OTHERS.             <row>-otherlocations += 1.
          ENDCASE.
        ENDIF.
      ENDLOOP.

      <row>-difference = <row>-totalonstock - <row>-safetystock.
      <row>-differencecriticality =
        COND #( WHEN <row>-totalonstock <  <row>-safetystock THEN 1
                WHEN <row>-totalonstock =  <row>-safetystock THEN 2
                ELSE 3 ).
      <row>-safetystockcriticality =
        COND #( WHEN <row>-safetystock = 0 THEN 1 ELSE 3 ).

      READ TABLE lt_behtxt INTO DATA(ls_txt)
           WITH KEY matnr = <row>-containertype BINARY SEARCH.
      IF sy-subrc = 0.
        <row>-containertypename =
          COND #( WHEN ls_txt-bautxt IS NOT INITIAL THEN ls_txt-bautxt
                  ELSE ls_txt-bauform ).
      ENDIF.

    ENDLOOP.

    ct_calculated_data = CORRESPONDING #( lt_result ).

  ENDMETHOD.

ENDCLASS.
