@EndUserText.label: 'LANF Message (Interface)'
define view entity Z_I_LanfMsg
  as select from zlanf_msg
{
  key msg_uuid,
      request_uuid,
      msg_ts,
      msg_type,
      msg_text,
      msg_context
}
