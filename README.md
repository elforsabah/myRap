@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Weighing Session'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true
define root view entity ZI_WR_WEIGHINGSESSION
  as select from zwr_weighsession
  association [0..*] to ZI_WR_SALESITEM_CONTRACTVH as _SalesItems
    on _SalesItems.SalesOrder = $projection.Vbeln
 
 
{
      key sessionid as Sessionid,
      vbeln as Vbeln,
      @UI.textArrangement: #TEXT_LAST
      loadtype as LoadType,
      @Semantics.quantity.unitOfMeasure: 'Grossweightunit'
      grossweight as Grossweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      grossweightunit as Grossweightunit,
      @Semantics.quantity.unitOfMeasure: 'Tareweightunit'
      tareweight as Tareweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      tareweightunit as Tareweightunit,
      @Semantics.quantity.unitOfMeasure: 'Netweightunit'
     
      netweight as Netweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      netweightunit as Netweightunit,
      step as Step,
      step1ok as Step1Ok,
      step2ok as Step2Ok,
      issummited as IsSummited,
     
      _SalesItems
}




managed implementation in class zbp_i_wr_weighingsession unique;
strict( 2 );

with draft;

define behavior for ZI_WR_WEIGHINGSESSION alias WeighingSession
persistent table zwr_weighsession
draft table ZWR_WEIGHB_DD
lock master
total etag Grossweight
authorization master ( instance )
etag master Grossweight

{
//  Keys must be readonly in strict draft
  field (readonly) Sessionid;  // <-- use your exact CDS element names
  field (numbering: managed) Sessionid;
create;
update;
delete;

draft action Edit;
draft action Activate;
draft action Discard;
draft action Resume;                 // <-- required
draft determine action Prepare;      //<-- required

action NextStep result [1] $self; // server validates & increments Step
action Submit result [1] $self; // final checks; printing trigger optional

action identifyCard parameter ZAE_WR_WEIGHINGSESSION result [1] $self;
action setloadType parameter ZAE_WR_WEIGHINGSESSION  result [1] $self;

//action identifyCard  result [1] $self;

validation validateStep1 on save { field Vbeln, Sessionid; } // Identification
validation validateStep2 on save { field Loadtype; } // Load type
validation ValidateLoadType on save { field LoadType; } // Calls a method to check against VH

determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math


}
