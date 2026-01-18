@EndUserText.label: 'LANF Inbound Request - Messages'
@AbapCatalog.tableCategory: #TRANSPARENT
@AbapCatalog.deliveryClass: #A
@AbapCatalog.dataMaintenance: #ALLOWED
define table zlanf_msg {

  key mandt            : abap.clnt not null;
  key msg_uuid         : sysuuid_x16 not null;

  request_uuid         : sysuuid_x16 not null;
  msg_ts               : timestampl not null;

  msg_type             : abap.char(1);              // S/W/E
  msg_text             : abap.char(255);
  msg_context          : abap.char(60);             // field/action/technical
}
