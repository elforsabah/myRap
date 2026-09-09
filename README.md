@EndUserText.label: 'Tour Attachments'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #L,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDTourAttachment 
  as select from /plce/tpdtrattm 
    association to parent /PLCE/R_PDTour as _Tour on $projection.TourUUID = _Tour.TourUUID
{
      @ObjectModel.foreignKey.association: '_Tour'
  key tour_uuid         as TourUUID,
      @Semantics.uuid: true
  key attachment_uuid   as AttachmentUUID,
      @EndUserText.label: 'Comments'
      comments          as Comments,
      @Semantics.largeObject: { mimeType: 'MimeType',
                                fileName: 'Filename',
                                contentDispositionPreference: #INLINE }
      attachment        as Attachment,
      @Semantics.mimeType: true
      mimetype          as MimeType,
      filename          as Filename,
      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      created_by        as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      created_at        as CreatedAt,
      @Semantics.user.lastChangedBy: true
      @EndUserText.label: 'Last Changed By'
      last_changed_by   as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      @EndUserText.label: 'Last Changed At'
      last_changed_at   as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      @EndUserText.label: 'Local Last Changed At'
      local_last_changed_at as LocalLastChangedAt,
      
      _Tour
}
