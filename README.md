CLASS lhc_zi_lanf_root DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    TYPES tt_lanf_position_input TYPE STANDARD TABLE OF zi_lanf_position_input WITH DEFAULT KEY.

    METHODS createlanf FOR FUNCTION
      IMPORTING keys RESULT TABLE FOR FUNCTION zi_lanf_root~createlanf result.  // Added 'TABLE'

    METHODS attachdocument FOR FUNCTION
      IMPORTING keys RESULT TABLE FOR FUNCTION zi_lanf_root~attachdocument result.  // Added 'TABLE'

    METHODS map_positions_to_contract
      IMPORTING iv_contract_vbeln TYPE vbeln_va
                it_positions TYPE tt_lanf_position_input
      CHANGING ct_order_items TYPE bapisditm_tt
               ct_order_itemsx TYPE bapisditmx_tt
               ct_return TYPE bapiret2_tab.
ENDCLASS.
