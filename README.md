@EndUserText.label: 'LANF Request (Interface)'
define root view entity Z_I_LanfReq
  as select from zlanf_req_h
{
  key request_uuid,
      external_request_id,
      contract_vbeln,
      service_date,
      bstkd,
      ktext,
      status,
      created_sd_vbeln,
      created_at,
      created_by,
      changed_at,
      changed_by,

      _Items    : composition [0..*] of Z_I_LanfReqItem
                  on _Items.request_uuid = $projection.request_uuid,

      _DocLinks : composition [0..*] of Z_I_LanfDocLink
                  on _DocLinks.request_uuid = $projection.request_uuid,

      _Messages : association [0..*] to Z_I_LanfMsg
                  on _Messages.request_uuid = $projection.request_uuid
}
