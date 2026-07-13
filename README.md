managed implementation in class zbp_i_containerstock unique;
strict ( 2 );

define behavior for ZI_ContainerStock alias ContainerStock
persistent table zwrtcssafestock
lock master
authorization master ( global )
{
//  update;
//  delete;

// ( precheck, features : global )
  action sicherheitb_aendern
    parameter ZAE_D_SicherheitB
    result [0..*] $self;


  field ( readonly ) ContainerType;

  field ( readonly ) ContainerTypeName,
                     LagerA, LagerB, LagerC, WerkstattW, WerkstattX,
                     WithoutLocation, OtherLocations,
                     TotalOnStock, Difference, DifferenceCriticality,
                     SafetyStockCriticality;

  validation validateSafetyStock on save { field SafetyStock; create; update; }

  mapping for zwrtcssafestock corresponding
  {
    ContainerType = behtyp;
    SafetyStock   = safetystock;
  }
}
