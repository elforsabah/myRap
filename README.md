<img width="438" height="317" alt="image" src="https://github.com/user-attachments/assets/130fd556-7e10-4976-a080-6ba1a4fb745d" />

@EndUserText.label: 'Abstract View für Arbeitbereich'

define abstract entity ZAE_D_TERMINATE_SERVICE
{

 @EndUserText.label: 'Grund'
   @Consumption.valueHelpDefinition: [
    {
      entity         : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Stornogrund' },
      label          : 'Storno',
      useForValidation: true
    }
  ]
  definiertegrund: abap.char( 255 );
  @EndUserText.label: 'Grund für Storno'
  @UI.multiLineText: true
  @ObjectModel.mandatory: true
   stornogrund : abap.string( 0 ); 
}


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
define view entity ZAE_D_TERMINATE_SERVICE_VH as select from zwr_pd_storno_gr
{
    @UI.hidden: true
    key stornokey as Stornokey,
    @Semantics.text: true
    @UI.textArrangement: #TEXT_ONLY
    stornogrund as Stornogrund
}
