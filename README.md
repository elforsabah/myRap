@EndUserText.label: 'Action Result'
define abstract entity Z_A_SO_ACTION_RESULT
{
  key ExternalRequestId : abap.char(40);

  SalesOrder            : vbeln_va;
  MsgType               : abap.char(1);     // S/W/E
  MsgText               : abap.char(255);
}
