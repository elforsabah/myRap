@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZAE_D_TERMINATE_SERVICE
{
   @EndUserText.label: 'Grund: STORNO DURCH DISPO Bemerkung'
//  @UI.multiLineText: true
  @ObjectModel.mandatory: true
    @Consumption.valueHelpDefinition: [
    {
      entity            : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Bemerkung' },
//      additionalBinding : [
//        { localElement: 'stornogrund', element: 'Bemerkung', usage: #RESULT }
//      ],
      useForValidation  : true
    }
  ]
  stornogrund : abap.char( 255 );
}


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

        @ObjectModel.text.element: [ 'Bemerkung' ]
        @UI.textArrangement: #TEXT_ONLY  
        @UI.hidden: true
        key conftype as Conftype,
        bemerkung as Bemerkung   
}


<img width="466" height="294" alt="image" src="https://github.com/user-attachments/assets/e7cf86b2-05a9-4c87-9d98-b77aecfa5dda" />
