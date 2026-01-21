unmanaged implementation in class zcl_lanf_behavior unique;

define behavior for ZI_LANF_ROOT alias LanfRoot
{
  // First endpoint: Create LANF
  static function CreateLanf deep parameter ZI_LANF_CREATE_INPUT result [1] ZI_LANF_RESPONSE;

  // Second endpoint: Attach PDF/Document Link
  static function AttachDocument deep parameter ZI_ATTACH_INPUT result [1] ZI_LANF_RESPONSE;
}
