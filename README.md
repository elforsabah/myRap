@AbapCatalog.extensibility: {extensible: true }
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour assigned Service'
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define view entity /PLCE/C_PDMTourService
  as select from /PLCE/R_PDTourServiceAsgmt as ServiceAssignment
    inner join   /PLCE/R_PDMTour            as _Tour on  _Tour.TourUUID          = ServiceAssignment.TourUUID
                                                     and _Tour.CommunicationUser = $session.user
  association [0..*] to /PLCE/C_PDMTourTask           as _ServiceTasks on  _ServiceTasks.TourUUID    = $projection.TourUUID
                                                                       and _ServiceTasks.ResourceId  = $projection.ResourceId
                                                                       and _ServiceTasks.ServiceUUID = $projection.ServiceUUID
  association [0..1] to /PLCE/C_PDMFunctionalLocation as _Location     on  _Location.FunctionalLocation = $projection.FunctionalLocation
  association [0..1] to /PLCE/C_PDMTourServiceUNI     as _ExtUNI       on  _ExtUNI.ServiceUUID = $projection.ServiceUUID
  association [0..1] to /PLCE/C_PDMTourServiceWR      as _ExtWR        on  _ExtWR.ServiceUUID = $projection.ServiceUUID
  association [0..*] to /PLCE/C_PDMServiceAttachment  as _Attachments  on  _Attachments.ServiceUUID = $projection.ServiceUUID
                                                                       and _Attachments.TourUUID    = $projection.TourUUID
                                                                       and _Attachments.ResourceId  = $projection.ResourceId
  association [0..*] to /PLCE/C_PDMServiceNote        as _Notes        on  _Notes.ServiceUUID = $projection.ServiceUUID
{
  key ServiceAssignment.TourUUID                                                                                              as TourUUID,
  key _Tour.ResourceId                                                                                                        as ResourceId,
  key ServiceAssignment._Service.ServiceUUID                                                                                  as ServiceUUID,

      ServiceAssignment.TourSequence                                                                                          as TourSequence,

      ServiceAssignment._Service.Profile                                                                                      as Profile,

      //Type
      ServiceAssignment._Service.ServiceType                                                                                  as ServiceType,
      ServiceAssignment._Service._ServiceType.ServiceCategory                                                                 as ServiceCategory,
      ServiceAssignment._Service._ServiceType._ServiceTypeText[1: Language = $session.system_language].ServiceTypeDescription as ServiceTypeDescription,

      //Date
      ServiceAssignment._Service.RequestedDate                                                                                as ServiceDate,


      //Admin data
      ServiceAssignment._Service.LastChangedAt,

      //External References
      ServiceAssignment._Service.ReferenceId                                                                                  as OrderNr,


      //Customer
      ServiceAssignment._Service.CustomerInfo                                                                                 as CustomerInfo,
      ServiceAssignment._Service.AdditionalText                                                                               as AdditionalText,

      //Location Address data
      @Consumption.hidden: true
      ServiceAssignment._Service.FunctionalLocation                                                                           as FunctionalLocation,

      ServiceAssignment._Service.ServiceWindow                                                                                as ServiceWindow,
      ServiceAssignment._Service.ServiceWindowStartTime                                                                       as ServiceWindowStartTime,
      ServiceAssignment._Service.ServiceWindowEndTime                                                                         as ServiceWindowEndTime,
      //Associations
      _Location,
      _ServiceTasks,
      _Attachments,
      //Extensions
      _ExtUNI,
      _ExtWR,
      _Notes

}
where
  ServiceAssignment.Removed is initial
