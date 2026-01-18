@EndUserText.label: 'LANF Inbound Request - Header'
@AbapCatalog.tableCategory: #TRANSPARENT
@AbapCatalog.deliveryClass: #A
@AbapCatalog.dataMaintenance: #ALLOWED
define table zlanf_req_h {

  key mandt               : abap.clnt not null;
  key request_uuid        : sysuuid_x16 not null;   // GUID (RAW16)

  external_request_id     : abap.char(70) not null; // idempotency key (must be UNIQUE via index)

  contract_vbeln          : abap.char(10);
  service_date            : abap.dats;
  bstkd                   : abap.char(35);
  ktext                   : abap.char(40);

  status                  : abap.char(12);          // NEW/PROCESSING/DONE/ERROR
  created_sd_vbeln        : abap.char(10);          // sales order number (when created)

  created_at              : timestampl;
  created_by              : syuname;
  changed_at              : timestampl;
  changed_by              : syuname;
}
