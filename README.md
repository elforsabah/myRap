@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Value Help für Storno'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZAE_D_TERMINATE_SERVICE_VH as select from zwr_pd_storno_gr
{

        @ObjectModel.text.element: [ 'Bemerkung' ]
        @UI.textArrangement: #TEXT_ONLY  
        @UI.hidden: true
        key conftype as Conftype,
        bemerkung as Bemerkung   
}
@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZAE_D_TERMINATE_SERVICE
{

   @EndUserText.label: 'Grund: STORNO DURCH DISPO (Bemerkung)'
//  @UI.multiLineText: true
  @ObjectModel.mandatory: true
    @Consumption.valueHelpDefinition: [
    {
      entity            : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Bemerkung' }

    }
  ]
  stornogrund : abap.char( 255 );
}

extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend  behavior for Service
{

action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
action ( precheck, features : global ) adjusttime parameter ZAE_D_adjusttime_AB result [0..*] $self;

 action ( precheck, features : global ) terminateservice
  parameter ZAE_D_TERMINATE_SERVICE
  result [0..*] $self;


 determination Setfachbereich on modify { create; update; }


 side effects {

 action adjusttime affects $self, entity _ExtCustom;
 action assignworkarea affects $self;
 action terminateservice affects $self;

 }


}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
    zz_vehicleinfo = zz_vehicleinfo;
    zz_discrepancy = zz_discrepancy;
    zz_reactiontime =  zz_reactiontime;
    zz_order_date = zz_order_date;
    zz_timeadjustment = zz_timeadjustment;
 }

field ( readonly : update ) zz_tech_fachbe;
field ( readonly : update ) zz_vehicleinfo;
field ( readonly : update ) zz_discrepancy;
field ( readonly : update ) zz_reactiontime;
field ( readonly : update ) zz_order_date;
field ( readonly : update ) zz_timeadjustment;
}
