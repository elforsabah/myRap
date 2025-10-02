@EndUserText.label: 'Abstract View für Arbeitbereich'

define abstract entity ZAE_D_WORKAREA_AB
 
{
  @EndUserText.label: 'Neue Arbeitsbereich'
//  @UI.defaultValue: #( 'ELEMENT_OF_REFERENCED_ENTITY: ServiceType' )
 @Consumption.valueHelpDefinition: [
    {
      entity         : { name: 'ZZPD_WORKREA_VH', element: 'WorkArea' },
      label          : 'Workarea'
     
    }
  ]
  
   WorkArea : /plce/pdwork_area;
    
}
