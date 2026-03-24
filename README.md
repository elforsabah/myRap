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


  " 1. Do the Waste Order Storno/Delete in the Backend
      CALL FUNCTION 'Z_WR_CANCEL_WASTE_ORDER_RFC' DESTINATION 'NONE'
        EXPORTING
          iv_pobjnr            = CONV char30( <ls_service>-referenceinternalid )
          iv_reason_predefined = 'PLSTORNO'
          iv_reason_text       = CONV string( ls_param-stornogrund )
          iv_serviceuuid       = <ls_service>-serviceuuid
        IMPORTING
          et_return            = lt_return.

<img width="681" height="281" alt="image" src="https://github.com/user-attachments/assets/8722ee98-1dfc-4784-ac7c-edaa1051f28a" />

