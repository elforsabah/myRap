 // Actions visible/enabled rules (bound actions from RAP)
  @UI: { OperationGrouping: #CHANGE_SET, Importance: #HIGH, Position: 10 }
  @UI.operationAvailable: : ( Step = 1 and Step1Ok = true ) or ( Step = 2 and Step2Ok = true )
  action NextStep;

  @UI: { Importance: #HIGH, Criticality: #Positive, Position: 20 }
  @UI.operationAvailable: : Step = 3 and Netweight ne null and IsSummited = false
  action Submit;
