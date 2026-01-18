@EndUserText.label: 'LANF Inbound Request - Document Links'
@AbapCatalog.tableCategory: #TRANSPARENT
@AbapCatalog.deliveryClass: #A
@AbapCatalog.dataMaintenance: #ALLOWED
define table zlanf_doclink {

  key mandt            : abap.clnt not null;
  key doclink_uuid     : sysuuid_x16 not null;

  request_uuid         : sysuuid_x16 not null;
  sd_vbeln             : abap.char(10);             // filled after SD created

  title                : abap.char(120);
  url                  : abap.char(255);

  status               : abap.char(12);             // NEW/ATTACHED/ERROR
  message              : abap.char(255);

  created_at           : timestampl;
  changed_at           : timestampl;
  attached_at          : timestampl;
}
