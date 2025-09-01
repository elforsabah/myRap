managed implementation in class zbp_i_wr_weighbridge unique;
strict( 2 );

with draft;

define behavior for ZI_WR_WEIGHBRIDGE  alias WeighingSession
persistent table vbak
draft table ZWR_WEIGHSESSI_D
lock master
total etag SalesDocument
authorization master ( instance )
etag master SalesDocument

{
//  Keys must be readonly in strict draft
  field (readonly) SalesDocument; // <-- use your exact CDS element names
  field (numbering: managed )  SESSIONID;
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
}



@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Interface View for weighbridge'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true
define root view entity ZI_WR_WEIGHBRIDGE
  as select from I_SalesDocument
{
  key SalesDocument               as SalesDocument,
      //These are calculated values and can only be seen at run time on the UI
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZWR_CL_HELPER'
      
      cast(' ' as abap.char(16) ) as SESSIONID,
      SDDocumentCategory          as SDDocumentCategory,
      SalesDocumentType           as SalesDocumentType,
      SalesDocumentProcessingType as SalesDocumentProcessingType,
      CreatedByUser               as CreatedByUser,
      LastChangedByUser           as LastChangedByUser,
      CreationDate                as CreationDate,
      CreationTime                as CreationTime,
      LastChangeDate              as LastChangeDate,
      LastChangeDateTime          as LastChangeDateTime,
      LastCustomerContactDate     as LastCustomerContactDate
}
where
      SalesDocumentType = 'ZKM'


Description	Resource	Path	Location	Type
The type of "SESSIONID" must be compatible with the type(s) of "ABP_BEHV_PID".	ZI_WR_WEIGHBRIDGE (Behavior Definition)	/S4D_100_prologaefo_en/.adt/wbobj2/bo/bdef/zi_wr_weighbridge	line 17	ABAP Syntax Check Problem


      
