abstract;

strict(2);
with hierarchy;

define behavior for ZI_LANF_RESPONSE alias response
{
 association _Messages;
}

define behavior for ZI_MESSAGE alias Msg
{
association _Parent;
}

abstract;
strict ( 2 );

with hierarchy;

define behavior for ZI_LANF_CREATE_INPUT alias createin
{
 association _Positions;
}

define behavior for ZI_LANF_POSITION_INPUT alias posIn
{

association _Parent;
}

abstract;
strict ( 2 );
with hierarchy;
define behavior for ZI_ATTACH_INPUT //alias <alias_name>
{

}



unmanaged implementation in class zbp_i_lanf_root unique;

define behavior for ZI_LANF_ROOT alias lanfroot
//persistent table ddddlsrc
lock master
authorization master ( instance )
//etag master <field_name>
{

 // First endpoint: Create LANF
  static action CreateLanf deep parameter ZI_LANF_CREATE_INPUT result [0..*] ZI_LANF_RESPONSE;

  // Second endpoint: Attach PDF/Document Link
  static action AttachDocument  deep parameter ZI_ATTACH_INPUT result [0..*] ZI_LANF_RESPONSE;


//  create;
//  update;
//  delete;
}


Der "result"-Typ von "ATTACHDOCUMENT" ist nicht als "deep" deklariert. Daher wird "ZI_LANF_RESPONSE" als flacher Typ verwendet und die BDEF-Definition ("with hierarchy") wird nicht verwendet.
Der "result"-Typ von "CREATELANF" ist nicht als "deep" deklariert. Daher wird "ZI_LANF_RESPONSE" als flacher Typ verwendet und die BDEF-Definition ("with hierarchy") wird nicht verwendet.

Klasse kann während der BDEF-Implementierungsprüfung nicht analysiert werden. Es sind Inkonsistenzen oder Fehler vorhanden: Das Datenobjekt "%PARAM" besitzt keine Komponente mit dem Namen "_MESSAGES".
