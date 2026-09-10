@EndUserText.label : 'Service type print configuration'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
define structure zpd_s_srvprtconf {

  service_type : /plce/pdservice_type;
  material     : /plce/pdtransport_product;
  conf         : include /plcp/cwaprtconf;

}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Projection View of ZI_PRINTCONFIG'
@Metadata.ignorePropagatedAnnotations: true

@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}

define root view entity ZC_PRINTCONFIG 
provider contract transactional_query
as projection on ZI_PRINTCONFIG

{
   @UI.selectionField: [{ position: 10 }]
   @UI.lineItem: [{ position: 10 }]
    key Field,
    @UI.lineItem: [{ position: 20 }]
    key Printform,
    @UI.lineItem: [{ position: 30 }]
    key Formtype,
    @UI.lineItem: [{ position: 40 }]
    ParameterIn,
    @UI.lineItem: [{ position: 50 }]
    Filenameteemplate
}


@EndUserText.label : 'Tour template print configuration'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
define structure zpd_s_trtmpprtconf {

  tmpl : include /plce/spdttmpl_key;
  conf : include /plcp/cwaprtconf;

}
