@Metadata.layer: #CUSTOMER
annotate entity /PLCE/C_PDMNLServiceWR with {
  @UI.lineItem: [
    {
      type                : #FOR_ACTION,
      dataAction          : 'assignworkarea',   // bound action name
      label               : 'Change Service Type',
      requiresSelection   : false,              // => global action
      invocationGrouping  : #CHANGE_SET,        // optional
      emphasized          : true,               // optional
      position            : 100,
      importance          : #HIGH
    }
  ]
}
