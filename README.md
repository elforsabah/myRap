@EndUserText.label: 'LANF Request Item (Interface)'
define view entity Z_I_LanfReqItem
  as select from zlanf_req_i
{
  key request_uuid,
  key item_no,

      matnr,
      kwmeng,
      vrkme,
      contract_posnr,

      _Parent : association to parent Z_I_LanfReq
                on $projection.request_uuid = _Parent.request_uuid
}
