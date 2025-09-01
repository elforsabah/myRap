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
  key cast( '' as zwr_sessionid ) as SESSIONID,
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
