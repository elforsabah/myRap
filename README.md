// Extension Custom
define behavior for /PLCE/R_PDServiceExtCustom alias ExtCustom
persistent table /plce/tpdsrvcst
draft table /plce/dpdsrvcst
lock dependent by _Service
authorization dependent by _Service
extensible
{
  field ( readonly ) ServiceUUID;

  update;
  delete;

  association _Service { with draft; }

  mapping for /plce/tpdsrvcst corresponding extensible
  {
    ServiceUUID = service_uuid;
  }
}
//define behavior for /PLCE/R_PDServiceTaskExtCustom alias ExtTaskCustom
//persistent table /plce/tpdsrvtcst
//draft table /plce/dpdsrvtcst
//lock dependent by _Service
//authorization dependent by _Service
//extensible
//{
//  field ( readonly ) ServiceUUID, ServiceTaskUUID;
//


Description	Resource	Path	Location	Type
"/PLCE/DPDSRVCST" is not a suitable draft persistency for "/PLCE/R_PDSERVICEEXTCUSTOM" (there is no "POINT_OF_ORIGIN" field).	/PLCE/R_PDSERVICE (Behavior Definition)	.adt/wbobj2/bo/bdef/%2fplce%2fr_pdservice	line 312	ABAP Syntax Check Problem


