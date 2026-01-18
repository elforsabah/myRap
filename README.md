@EndUserText.label: 'LANF Inbound Request - Items'
@AbapCatalog.tableCategory: #TRANSPARENT
@AbapCatalog.deliveryClass: #A
@AbapCatalog.dataMaintenance: #ALLOWED
define table zlanf_req_i {

  key mandt            : abap.clnt not null;
  key request_uuid     : sysuuid_x16 not null;
  key item_no          : abap.numc(6) not null;     // 000010, 000020, ...

  matnr                : abap.char(18);
  kwmeng               : abap.dec(15,3);            // quantity
  vrkme                : abap.char(3);              // unit

  contract_posnr       : abap.numc(6);              // derived from contract
}
