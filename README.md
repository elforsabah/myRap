@EndUserText.label: 'Dummy Root for LANF Behaviors'
define root abstract entity ZI_LANF_ABSTRACT_ROOT {
  key TechKey : abap.char(1);
}


unmanaged;

define behavior for ZI_LANF_ABSTRACT_ROOT // Dummy root abstract entity (define it as empty if needed)
{
  // First endpoint: Create LANF
  static function CreateLanf deep parameter ZI_LANF_CREATE_INPUT result [1] ZI_LANF_RESPONSE;

  // Second endpoint: Attach PDF/Document Link
  static function AttachDocument deep parameter ZI_ATTACH_INPUT result [1] ZI_LANF_RESPONSE;
}
<img width="1623" height="754" alt="image" src="https://github.com/user-attachments/assets/382dbe5f-6004-4fa5-9eaf-754a9c029e09" />
