## 7) Metadaten-Extension (UI-Annotationen)

Rechtsklick Paket → *New → Other → Core Data Services → Metadata Extension*.
Name `ZC_CONTAINERSTOCK` (extended entity = `ZC_ContainerStock`). Inhalt:

```abap
@Metadata.layer: #CORE

@UI: {
  headerInfo: {
    typeName:       'Behältertyp',
    typeNamePlural: 'Behälter-Lagerbestände',
    title:       { value: 'ContainerType' },
    description: { value: 'ContainerTypeName' }
  }
}
annotate entity ZC_ContainerStock with
{
  @UI.selectionField: [{ position: 10 }]
  @UI.lineItem:       [{ position: 10, label: 'Behältertyp', importance: #HIGH }]
  @EndUserText.label: 'Behältertyp'
  ContainerType;

  @UI.lineItem: [{ position: 20, label: 'Bezeichnung', importance: #HIGH }]
  @EndUserText.label: 'Bezeichnung'
  ContainerTypeName;

  @UI.lineItem: [{ position: 30, label: '1000 Hauptlager Halle A' }]
  Loc1000;
  @UI.lineItem: [{ position: 31, label: '1010 Wareneingang' }]
  Loc1010;
  @UI.lineItem: [{ position: 32, label: '1020 Kommissionierung' }]
  Loc1020;
  @UI.lineItem: [{ position: 33, label: '2000 Außenlager Nord' }]
  Loc2000;
  @UI.lineItem: [{ position: 34, label: '2010 Außenlager Süd' }]
  Loc2010;
  @UI.lineItem: [{ position: 35, label: '3000 Produktion Linie 1' }]
  Loc3000;

  @UI.lineItem: [{ position: 40, label: 'ohne Lagerort', importance: #HIGH }]
  @EndUserText.label: 'ohne Lagerort'
  WithoutLocation;

  @UI.lineItem: [{ position: 45, label: 'sonstige Lagerorte' }]
  @EndUserText.label: 'sonstige Lagerorte'
  OtherLocations;

  @UI.lineItem: [{ position: 50, label: 'Summe Lager', importance: #HIGH }]
  @EndUserText.label: 'Summe Lager'
  TotalOnStock;

  @UI.lineItem: [{ position: 60, label: 'Sicherheitsbestand', importance: #HIGH }]
  @EndUserText.label: 'Sicherheitsbestand'
  SafetyStock;

  @UI.lineItem: [{ position: 70, label: 'Differenz', criticality: 'DifferenceCriticality', importance: #HIGH }]
  @EndUserText.label: 'Differenz'
  Difference;

  @UI.hidden: true
  DifferenceCriticality;
}
```
