@EndUserText.label: 'LANF DocLink (Interface)'
define view entity Z_I_LanfDocLink
  as select from zlanf_doclink
{
  key doclink_uuid,
      request_uuid,
      sd_vbeln,
      title,
      url,
      status,
      message,
      created_at,
      changed_at,
      attached_at,

      _Parent : association to parent Z_I_LanfReq
                on $projection.request_uuid = _Parent.request_uuid
}
