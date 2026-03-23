@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Value Help für Storno'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@ObjectModel.resultSet.sizeCategory: #XS
define view entity ZAE_D_TERMINATE_SERVICE_VH as select from tewaconftypet
{
  @ObjectModel.text.element: [ 'Vtext' ]
  @UI.textArrangement: #TEXT_ONLY  /* <-- Moves the text arrangement to the key */
  key conftype as Conftype,

  @UI.hidden: true                 /* <-- UNCOMMENT THIS: Hide the 'DE' column! */
  key spras as Spras,

  @Semantics.text: true
  vtext as Vtext
} where spras = $session.system_language 
