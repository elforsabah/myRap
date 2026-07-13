@Metadata.layer: #CORE

@UI: {
  headerInfo: {
    typeName:       'Behältertyp',
    typeNamePlural: 'Behälter-Lagerbestände',
    title:       { value: 'ContainerType' },
    description: { value: 'ContainerTypeName' }
  },
  //=== Object Page: Aufbau der Detailseite (Klick auf Zeile) ===============
  facet: [
    { id: 'General',   purpose: #STANDARD, type: #IDENTIFICATION_REFERENCE,
      label: 'Allgemein',           position: 10 },
    { id: 'Locations', purpose: #STANDARD, type: #FIELDGROUP_REFERENCE,
      label: 'Bestand je Lagerort', position: 20, targetQualifier: 'Locations' }
  ]
}
annotate entity ZC_ContainerStock with
{
  //--- Kopf / Identification (Object Page: Abschnitt „Allgemein“) ---
  @UI.selectionField: [{ position: 10 }]
  @UI.lineItem:       [{ position: 10, label: 'Behältertyp', importance: #HIGH }]
  @UI.identification: [{ position: 10, label: 'Behältertyp' }]
  @EndUserText.label: 'Behältertyp'
  ContainerType;

  @UI.lineItem:       [{ position: 20, label: 'Bezeichnung', importance: #HIGH }]
  @UI.identification: [{ position: 20, label: 'Bezeichnung' }]
  @EndUserText.label: 'Bezeichnung'
  ContainerTypeName;

  //--- Lagerort-Spalten (List Report) + Field Group (Object Page) ---
  @UI.lineItem:   [{ position: 30, label: '1000 Hauptlager Halle A' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 10, label: '1000 Hauptlager Halle A' }]
  Loc1000;
  @UI.lineItem:   [{ position: 31, label: '1010 Wareneingang' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 20, label: '1010 Wareneingang' }]
  Loc1010;
  @UI.lineItem:   [{ position: 32, label: '1020 Kommissionierung' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 30, label: '1020 Kommissionierung' }]
  Loc1020;
  @UI.lineItem:   [{ position: 33, label: '2000 Außenlager Nord' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 40, label: '2000 Außenlager Nord' }]
  Loc2000;
  @UI.lineItem:   [{ position: 34, label: '2010 Außenlager Süd' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 50, label: '2010 Außenlager Süd' }]
  Loc2010;
  @UI.lineItem:   [{ position: 35, label: '3000 Produktion Linie 1' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 60, label: '3000 Produktion Linie 1' }]
  Loc3000;

  @UI.lineItem:   [{ position: 40, label: 'ohne Lagerort', importance: #HIGH }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 70, label: 'ohne Lagerort' }]
  @EndUserText.label: 'ohne Lagerort'
  WithoutLocation;

  @UI.lineItem:   [{ position: 45, label: 'sonstige Lagerorte' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 80, label: 'sonstige Lagerorte' }]
  @EndUserText.label: 'sonstige Lagerorte'
  OtherLocations;

  //--- Kennzahlen (List Report + Object Page „Allgemein“) ---
  @UI.lineItem:       [{ position: 50, label: 'Summe Lager', importance: #HIGH }]
  @UI.identification: [{ position: 30, label: 'Summe Lager' }]
  @EndUserText.label: 'Summe Lager'
  TotalOnStock;

  @UI.lineItem:       [{ position: 60, label: 'Sicherheitsbestand', importance: #HIGH }]
  @UI.identification: [{ position: 40, label: 'Sicherheitsbestand' }]
  @EndUserText.label: 'Sicherheitsbestand'
  SafetyStock;

  @UI.lineItem:       [{ position: 70, label: 'Differenz', criticality: 'DifferenceCriticality', importance: #HIGH }]
  @UI.identification: [{ position: 50, label: 'Differenz', criticality: 'DifferenceCriticality' }]
  @EndUserText.label: 'Differenz'
  Difference;

  @UI.hidden: true
  DifferenceCriticality;
}
