@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Planning and Dispatching Action'
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #S,
  dataClass: #CUSTOMIZING
}
define view entity /PLCE/R_PDAction 
  as select from /plce/cpdaction
  association to parent /PLCE/R_PDACTION_S   as _Action_S on $projection.SingletonID = _Action_S.SingletonID
  composition [0..*] of /PLCE/R_PDActionText as _ActionText 
{
//  @ObjectModel.text.association: '_ActionText'
  key action            as Action,
    
      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      created_by                     as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      created_at                     as CreatedAt,
      @Semantics.user.lastChangedBy: true
      @EndUserText.label: 'Last Changed By'
      last_changed_by                as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      @EndUserText.label: 'Last Changed At'
      last_changed_at                as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      @EndUserText.label: 'Local Last Changed At'
      local_last_changed_at          as LocalLastChangedAt,
      1 as SingletonID,
  
  //Association
  _Action_S,
  _ActionText
}


@EndUserText.label: 'P&D Action Singleton'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #S,
  dataClass: #CUSTOMIZING
}
define root view entity /PLCE/R_PDAction_S
  as select from I_Language
    left outer join /plce/cpdaction on 0 = 0
  composition [0..*] of /PLCE/R_PDAction as _PDAction
{
  key 1 as SingletonID,
  _PDAction,
  max( /plce/cpdaction.last_changed_at ) as LastChangedAtMax,
  cast( '' as sxco_transport) as TransportRequestID,
  cast( 'X' as abap_boolean preserving type) as HideTransport
  
}
where I_Language.Language = $session.system_language

@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Planning and Dispatching Action text'
@Search.searchable: true
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #S,
  dataClass: #CUSTOMIZING
}
define view entity /PLCE/R_PDActionText
  as select from /plce/cpdactiont
  association        to parent /PLCE/R_PDAction as _Action on $projection.Action = _Action.Action
  association [0..1] to I_Language          as _Language on $projection.Language = _Language.Language
  association [1..1] to /PLCE/R_PDACTION_S      as _Action_S on $projection.SingletonID = _Action_S.SingletonID
{
      @ObjectModel.foreignKey.association: '_Action'
  key action                as Action,
      @ObjectModel.foreignKey.association: '_Language'
      @Semantics.language: true
  key spras                 as Language,
      @Semantics.text: true
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
      @Search.ranking: #HIGH
      description           as ActionDescription,
      1 as SingletonID,
      
      //Association
      _Action,
      _Action_S,
      _Language
}


<img width="1130" height="524" alt="image" src="https://github.com/user-attachments/assets/9f45fe64-14c1-410e-aded-897adb9f4846" />


<img width="468" height="355" alt="image" src="https://github.com/user-attachments/assets/b6b253ba-a1f9-42e8-8bc4-ca6b5ac49901" />



