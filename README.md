abstract;
strict ( 2 );
with hierarchy;

define behavior for Z_A_SO_CREATE_REF alias Payload
{
  association to_Item;
}

define behavior for Z_A_SO_CREATE_REF_ITEM alias PayloadItem
{
  association _Parent;
}
