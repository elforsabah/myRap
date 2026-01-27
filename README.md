  METHOD prepare_result_data_head.
    par_headref->order_date = par_dataref->tourdate.
    data:
      LDATE type EORDER_DATE,
      LTIME type TIMS.

    insert initial line into table ORDERRESULTS reference into PAR_HEADREF.
    PAR_HEADREF->/PLCP/PD_TOUR_ID = PAR_DATAREF->TOURID.
*    LORDERRESULTREF->ROUTE = PAR_DATAREF->TOURTEMPLATE.
    PAR_HEADREF->ROUTE = PAR_DATAREF->EXTERNALTOURTEMPLATEID.
    PAR_HEADREF->ORDERTXT = PAR_DATAREF->ADDITIONALTEXT.

    convert time stamp PAR_DATAREF->TOURDATE time zone SY-ZONLO
        into date LDATE time LTIME.

    PAR_HEADREF->ORDER_DATE = LDATE.


  ENDMETHOD.


    METHOD prepare_result_data_head.
    par_headref->order_date = par_dataref->tourdate.



  ENDMETHOD.
