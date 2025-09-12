class ZCL_WR_SERVICE_EXTEND_CALC definition
  public
  final
  create public .

public section.

  interfaces IF_SADL_EXIT .
  interfaces IF_SADL_EXIT_CALC_ELEMENT_READ .
  protected section.
  private section.
ENDCLASS.



CLASS ZCL_WR_SERVICE_EXTEND_CALC IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_WR_SERVICE_EXTEND_CALC->IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE
* +-------------------------------------------------------------------------------------------------+
* | [--->] IT_ORIGINAL_DATA               TYPE        STANDARD TABLE
* | [--->] IT_REQUESTED_CALC_ELEMENTS     TYPE        TT_ELEMENTS
* | [<-->] CT_CALCULATED_DATA             TYPE        STANDARD TABLE
* | [!CX!] CX_SADL_EXIT
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE.
    data: lt_original_data type standard table of /PLCE/C_PDMNLServiceWR with default key,
          lbadi            type ref to /plce/pd_ui_manual,
          lt_service_map   type /plce/ppdmnlservicemap.

      ct_calculated_data = corresponding #( lt_original_data ).

  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_WR_SERVICE_EXTEND_CALC->IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO
* +-------------------------------------------------------------------------------------------------+
* | [--->] IT_REQUESTED_CALC_ELEMENTS     TYPE        TT_ELEMENTS
* | [--->] IV_ENTITY                      TYPE        STRING
* | [<---] ET_REQUESTED_ORIG_ELEMENTS     TYPE        TT_ELEMENTS
* | [!CX!] CX_SADL_EXIT
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
  endmethod.
ENDCLASS.
