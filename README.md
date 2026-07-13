CLASS zcl_container_stock_calc DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    INTERFACES if_sadl_exit_calc_element_read.

  PRIVATE SECTION.
    CONSTANTS:
      BEGIN OF gc_loc,
        l1000 TYPE lgort_d VALUE '1000',
        l1010 TYPE lgort_d VALUE '1010',
        l1020 TYPE lgort_d VALUE '1020',
        l2000 TYPE lgort_d VALUE '2000',
        l2010 TYPE lgort_d VALUE '2010',
        l3000 TYPE lgort_d VALUE '3000',
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

    SELECT a~matnr   AS behtyp,
           s~b_lager AS lgort,
           e~herst   AS herst
      FROM /watp/vcaequz AS a
      INNER JOIN etyp AS t      ON a~matnr = t~matnr
      INNER JOIN equi AS e      ON e~equnr = a~equnr
      LEFT OUTER JOIN eqbs AS s ON s~equnr = a~equnr
      FOR ALL ENTRIES IN @lt_result
      WHERE a~matnr    =  @lt_result-containertype
        AND a~tplnr    =  @space
        AND a~datab    <= @sy-datum
        AND a~datbi    >= @sy-datum
        AND t~kombinat =  'B'
      INTO TABLE @DATA(lt_equip).

    DELETE lt_equip WHERE herst = 'DUMMY'.

    SELECT matnr, bauform, bautxt FROM etyp
      FOR ALL ENTRIES IN @lt_result
      WHERE matnr = @lt_result-containertype
      INTO TABLE @DATA(lt_behtxt).
    SORT lt_behtxt BY matnr.

    LOOP AT lt_result ASSIGNING FIELD-SYMBOL(<row>).

      CLEAR: <row>-loc1000, <row>-loc1010, <row>-loc1020,
             <row>-loc2000, <row>-loc2010, <row>-loc3000,
             <row>-withoutlocation, <row>-otherlocations, <row>-totalonstock.

      LOOP AT lt_equip ASSIGNING FIELD-SYMBOL(<eq>) WHERE behtyp = <row>-containertype.
        <row>-totalonstock += 1.
        IF <eq>-lgort IS INITIAL.
          <row>-withoutlocation += 1.
        ELSE.
          CASE <eq>-lgort.
            WHEN gc_loc-l1000. <row>-loc1000 += 1.
            WHEN gc_loc-l1010. <row>-loc1010 += 1.
            WHEN gc_loc-l1020. <row>-loc1020 += 1.
            WHEN gc_loc-l2000. <row>-loc2000 += 1.
            WHEN gc_loc-l2010. <row>-loc2010 += 1.
            WHEN gc_loc-l3000. <row>-loc3000 += 1.
            WHEN OTHERS.       <row>-otherlocations += 1.
          ENDCASE.
        ENDIF.
      ENDLOOP.

      <row>-difference = <row>-totalonstock - <row>-safetystock.
      <row>-differencecriticality =
        COND #( WHEN <row>-totalonstock <  <row>-safetystock THEN 1
                WHEN <row>-totalonstock =  <row>-safetystock THEN 2
                ELSE 3 ).

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
