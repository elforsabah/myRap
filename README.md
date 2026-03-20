@AbapCatalog.viewEnhancementCategory: [#NONE]
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



@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZAE_D_TERMINATE_SERVICE
{

  @EndUserText.label: 'Grund'
  @ObjectModel.text.element: ['GrundText']
  @UI.textArrangement: #TEXT_ONLY
  @Consumption.valueHelpDefinition: [
    {
      entity          : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Conftype' }, /* <-- MUST BE THE KEY */
      additionalBinding: [{ localElement: 'GrundText', element: 'Vtext', usage: #RESULT }], /* Fetches text for display */
      useForValidation: true
    }
  ]
  definiertegrund: abap.char( 10 ); /* It is safer to match the actual length of CONFTYPE (usually 4 or 10) */

  @UI.hidden: true
  GrundText : abap.char( 255 ); /* Silently holds the text for the UI to display */

  @EndUserText.label: 'Grund für Storno (Bemerkung)'
  @UI.multiLineText: true
  @ObjectModel.mandatory: true
  stornogrund : abap.string( 0 ); 
}
