extend view entity /PLCE/C_PDMNLServiceWR with
association [0..1] to /PLCE/R_PDServiceExtCustom as _ExtCustom on $projection.ServiceUUID = _ExtCustom.ServiceUUID
{
  _ExtCustom.ZZWDIO as ZZWDIO,
  case when _ExtCustom.ZZWDIO = '4' then 3 else 0 end as RowCriticality
}
