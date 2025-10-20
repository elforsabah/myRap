*&1 Name
define DATA_BO_RANGE.
  set extended check off.
  data:
    LRANGE&1 type ref to EWASRANGE,
    LRANGE&1S type ref to EWAPRANGES.

  field-symbols:
    <LVALUE&1> type any,
    <LRANGE&1> type DATA.
  create data LRANGE&1S.
  set extended check on.
end-of-definition.
