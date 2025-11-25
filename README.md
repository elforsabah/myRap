@EndUserText.label: 'Abstract View für Tour Create'

define abstract entity ZAE_D_TOURTOURTEMPLATE_AB

{
  @EndUserText.label: 'Tour Template'
  @Consumption.valueHelpDefinition: [
     {
       entity   : { name: '/PLCE/C_PDTourTemplate_VH', element: 'TourTemplate' },
       label    : 'TourTemplate',
       useForValidation: true
     }
   ]
  tour_template : /plce/pdtour_template;

  @EndUserText.label: 'First Tour Date'
  start_date    : /plce/date;

  @EndUserText.label: 'Last Tour Date'
  end_date      : /plce/date;
}
