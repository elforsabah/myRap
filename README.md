@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Tour',
    typeNamePlural: 'Tours',
    title: {
      type: #STANDARD, value: 'TourId'
    },
    description: {
      type: #STANDARD, value: 'TourTemplateName'
    }
  },
  lineItem: [{ criticality: 'TourCriticality' }],                    //for TourDeviations

  presentationVariant: [{
    sortOrder: [{
      by: 'TourId', direction:  #ASC
    }], visualizations: [{ type: #AS_LINEITEM }]
  },{
    sortOrder: [{
      by: 'TourId', direction:  #ASC
    }], visualizations: [{ type: #AS_LINEITEM, qualifier: 'DefaultMap' }], qualifier: 'DefaultMap'
  }]
}
@Search.searchable: true
annotate entity /PLCE/C_PDMNLTourWR with
{
  @UI.facet: [
  {
    id: 'F0_CoreDefault',
    type: #FIELDGROUP_REFERENCE,
    targetElement: '_TourCore',
    targetQualifier: 'DefaultInformation',
    label: 'Tour Details'
  }]

  @UI.fieldGroup: [{ type: #FOR_ACTION, dataAction: 'releaseTour', label: 'Release Tour', qualifier: 'XXX'  }]

  @UI.lineItem: [{ position: 1 },
