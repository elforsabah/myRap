@Metadata.layer: #CORE
define metadata extension ZME_ZI_WR_WEIGHINGSESSION_UI
  for view entity ZI_WR_WEIGHINGSESSION
{
  // --- Object Page sections (wizard steps)
  @UI.facet: [
    { id: 'S1', label: 'Step 1 · Identification', type: #FIELDGROUP_REFERENCE, targetQualifier: 'S1', position: 10 },
    { id: 'S2', label: 'Step 2 · Load Type',      type: #FIELDGROUP_REFERENCE, targetQualifier: 'S2', position: 20 },
    { id: 'S3', label: 'Step 3 · Weighing',       type: #FIELDGROUP_REFERENCE, targetQualifier: 'S3', position: 30 }
  ];

  // --- Assign fields to FieldGroup S1
  @UI.fieldGroup: [{ qualifier: 'S1', position: 10 }]
  Vbeln;
  @UI.fieldGroup: [{ qualifier: 'S1', position: 20 }]
  Sessionid;

  // --- Assign fields to FieldGroup S2
  @UI.fieldGroup: [{ qualifier: 'S2', position: 10 }]
  LoadType;

  // --- Assign fields to FieldGroup S3
  @UI.fieldGroup: [{ qualifier: 'S3', position: 10 }]
  Grossweight;
  @UI.fieldGroup: [{ qualifier: 'S3', position: 11 }]
  Grossweightunit;
  @UI.fieldGroup: [{ qualifier: 'S3', position: 20 }]
  Tareweight;
  @UI.fieldGroup: [{ qualifier: 'S3', position: 21 }]
  Tareweightunit;
  @UI.fieldGroup: [{ qualifier: 'S3', position: 30 }]
  Netweight;
  @UI.fieldGroup: [{ qualifier: 'S3', position: 31 }]
  Netweightunit;

  // --- (Optional) gate steps by Step value
  @UI.hidden: : Step < 2
  LoadType;

  @UI.hidden: : Step < 3
  Grossweight, Grossweightunit, Tareweight, Tareweightunit, Netweight, Netweightunit;

  // --- (Optional) auto-refresh Netweight after Gross/Tare changes
  @Common.SideEffects: [
    { SourceProperties: ['Grossweight','Tareweight'], TargetProperties: ['Netweight'] }
  ];
}
