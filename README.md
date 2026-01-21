abstract;

with hierarchy ZI_LANF_CREATE_INPUT {
  field ( mandatory ) ContractVbeln, DeliveryDate;
  field ( optional ) CustomerRef, Description;
}
abstract;

with hierarchy ZI_LANF_RESPONSE {
  field ( readonly ) Vbeln;
}
