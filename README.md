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
} where spras = $session.system_language and conftype = 'PLSTORNO'



@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZAE_D_TERMINATE_SERVICE
{

  @EndUserText.label: 'Grund'
  @ObjectModel.text.element: ['GrundText']
  @UI.textArrangement: #TEXT_ONLY
  @Consumption.valueHelpDefinition: [
    {
      entity          : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Conftype' },
      additionalBinding: [{ localElement: 'GrundText', element: 'Vtext', usage: #RESULT }], 
      useForValidation: true
    }
  ]
  definiertegrund: abap.char( 8 );

  @EndUserText.label: 'Grund für Storno (Bemerkung)'
  @UI.multiLineText: true
  @ObjectModel.mandatory: true
  stornogrund : abap.string( 0 ); 
}


<img width="218" height="88" alt="image" src="https://github.com/user-attachments/assets/8e5ff8b9-e7e0-4080-9174-cfd9748628ee" />




