extend view entity /PLCE/R_PDService with
{
  association [0..*] to /PLCE/P_PDWorkAreaServiceExt as _WorkAreaServicesExt
    on $projection.ServiceUUID = _WorkAreaServicesExt.ServiceUUID
}
