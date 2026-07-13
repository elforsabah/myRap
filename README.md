# Behälter-Lagerbestände (RAP) — manuelle Anlage in ADT

Schritt-für-Schritt ohne abapGit. Alle Objekte werden in **Eclipse mit ADT**
angelegt (nicht in SAP GUI/SE80 – RAP-Objekte gibt es nur in ADT).

> **Namenskonvention:** unten sind Z-Namen verwendet. Passe sie bei Bedarf an
> eure Richtlinie an – dann überall konsistent umbenennen.

---

## 0) Vorbereitung

1. Eclipse mit **ADT** starten, mit dem System verbinden.
2. Im *Project Explorer* dein ABAP-Projekt öffnen → Ordner **Favorite Packages**.
3. Ein Entwicklungspaket wählen/anlegen (z. B. `ZWR_CONTAINER_STOCK`).
   - Neues Paket: Rechtsklick Paket → *New → ABAP Package*.
   - Für ersten Test reicht `$TMP` (lokal, nicht transportierbar).
4. Transportauftrag bereithalten (außer bei `$TMP`).

Reihenfolge unbedingt einhalten – jedes Objekt nach dem Einfügen mit
**Strg+S** speichern und **Strg+F3** aktivieren.

---

## 1) Datenbanktabelle `ZWRTCSSAFESTOCK`

**Die Tabelle existiert bereits – hier ist NICHTS zu tun.** Diese Anleitung nutzt
sie unverändert (ohne Verwaltungs-/Zeitstempelfelder). Es genügen der Schlüssel
und das Feld `safety_stock`.

> Nur zur Info – so sieht die genutzte Minimal-Struktur aus:
> ```abap
> key mandt          : mandt not null;
> key container_type : abap.char(18) not null;   // ggf. eure reale Domäne/Länge
> safety_stock       : abap.int4;
> ```
>
> **Konsequenz ohne Zeitstempelfeld:** keine optimistische Sperre. Ändern zwei
> Disponenten *gleichzeitig* denselben Behältertyp, gewinnt der zuletzt
> Speichernde. Für den Prototyp/Test unkritisch.

---

## 2) Interface-CDS `ZI_ContainerStock` (Root-View)

Rechtsklick Paket → *New → Other → Core Data Services → Data Definition*.
Name `ZI_ContainerStock`, Template *Define a View Entity* (Inhalt danach ersetzen):

```abap
@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Behälter-Lagerbestände (Interface, Root)'
@Metadata.allowExtensions: true
@ObjectModel.usageType: { serviceQuality: #A, sizeCategory: #S, dataClass: #MIXED }

define root view entity ZI_ContainerStock
  as select from zwrtcssafestock as SafeStock
{
  key SafeStock.container_type                                     as ContainerType,

      SafeStock.safety_stock                                       as SafetyStock,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( '' as abap.char( 40 ) )                               as ContainerTypeName,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as Loc1000,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as Loc1010,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as Loc1020,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as Loc2000,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as Loc2010,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as Loc3000,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as WithoutLocation,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as OtherLocations,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as TotalOnStock,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as Difference,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as DifferenceCriticality
}
```

> Aktivierung schlägt zunächst fehl, weil `ZCL_CONTAINER_STOCK_CALC` noch fehlt.
> Erst Schritt 3 anlegen, dann diese View aktivieren. (Reihenfolge: 2 anlegen →
> speichern → 3 anlegen+aktivieren → 2 aktivieren.)

---

## 3) Klasse `ZCL_CONTAINER_STOCK_CALC` (füllt die berechneten Spalten)

Rechtsklick Paket → *New → ABAP Class*. Name `ZCL_CONTAINER_STOCK_CALC`.
Gesamten Quelltext ersetzen:

```abap
CLASS zcl_container_stock_calc DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    INTERFACES if_sadl_exit_calc_element_read.

  PRIVATE SECTION.
    CONSTANTS:
      BEGIN OF gc_loc,
        l1000 TYPE lgort_d VALUE '1000',
        l1010 TYPE lgort_d VALUE '1010',
        l1020 TYPE lgort_d VALUE '1020',
        l2000 TYPE lgort_d VALUE '2000',
        l2010 TYPE lgort_d VALUE '2010',
        l3000 TYPE lgort_d VALUE '3000',
      END OF gc_loc.
ENDCLASS.

CLASS zcl_container_stock_calc IMPLEMENTATION.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    et_requested_orig_elements = VALUE #( ( `CONTAINERTYPE` )
                                          ( `SAFETYSTOCK` ) ).
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~calculate.

    DATA lt_result TYPE STANDARD TABLE OF zi_containerstock WITH DEFAULT KEY.
    lt_result = CORRESPONDING #( it_original_data ).

    IF lt_result IS INITIAL.
      RETURN.
    ENDIF.

    " Tagesfaktoren einmalig lesen – wie im Report ZWR_CS_CONTAINER_INVENTORY
    SELECT * FROM zwrtcsdayfactor INTO TABLE @DATA(lt_dayfactor). "#EC CI_NOWHERE

    LOOP AT lt_result ASSIGNING FIELD-SYMBOL(<row>).

      " === (A) TYP-EBENE: bestehende, geprüfte Logik wiederverwenden =========
      " ZCL_WR_WATP_CS_MISC=>CL_CALC_CONTAINER_INVENTORY liefert je Behältertyp
      " den Netto-Gesamtbestand (Dummies herausgerechnet) + Sicherheitsbestand.
      " (Nicht benötigte IMPORTING-Parameter können entfallen.)
      DATA(lv_total) = zcl_wr_watp_cs_misc=>cl_calc_container_inventory(
        EXPORTING
          par_date                = sy-datum
          par_behtyp              = CONV #( <row>-containertype )
          par_dayfactor           = lt_dayfactor
        IMPORTING
          par_current_inventory   = DATA(lv_current)
          par_delivered_inventory = DATA(lv_delivered)
          par_changed_inventory   = DATA(lv_changed)
          par_removed_inventory   = DATA(lv_removed)
          par_dummy_inventory     = DATA(lv_dummy)
          par_safetystock         = DATA(lv_safety) ).

      <row>-totalonstock = lv_total.                 " Netto-Gesamtbestand (ohne Dummies)
      <row>-difference   = lv_total - <row>-safetystock.
      <row>-differencecriticality =
        COND #( WHEN lv_total <  <row>-safetystock THEN 1
                WHEN lv_total =  <row>-safetystock THEN 2
                ELSE 3 ).
      <row>-containertypename = <row>-containertype.

      " === (B) LAGERORT-EBENE: NICHT in obiger Klasse enthalten ==============
      " Die Methode liefert KEINE Aufteilung je Lagerort. Die Spalten
      " Loc1000…/ohne Lagerort müssen aus der Selektion von ZWR_CONTAINER_OVW
      " (Programm ZWR_WATP_CONTAINER_OVERVIEW – Tabellen /watp/vcaequz + etyp +
      " Equipment-Stammdaten mit Lagerort/Status/Hersteller) befüllt werden.
      "
      " >>> TODO: Equipment-Selektion einsetzen und je Behältertyp zählen:
      "     - nur Status 'L' (auf Lager)
      "     - Hersteller <> 'DUMMY'
      "     - je Lagerort nach <row>-locXXXX, ohne Lagerort nach
      "       <row>-withoutlocation, übrige Lagerorte nach <row>-otherlocations
      CLEAR: <row>-loc1000, <row>-loc1010, <row>-loc1020,
             <row>-loc2000, <row>-loc2010, <row>-loc3000,
             <row>-withoutlocation, <row>-otherlocations.

    ENDLOOP.

    ct_calculated_data = CORRESPONDING #( lt_result ).

  ENDMETHOD.

ENDCLASS.
```

Aktivieren. Danach zurück zu **Schritt 2** und `ZI_ContainerStock` aktivieren.

> **Wichtig:** Teil (A) läuft sofort (Gesamtbestand, Sicherheitsbestand,
> Differenz). Teil (B) – die Lagerort-Spalten – bleibt zunächst `0`, bis die
> Selektion aus `ZWR_WATP_CONTAINER_OVERVIEW` eingesetzt ist. Ohne diese
> Selektion zeigt die App korrekte Typ-Summen, aber leere Lagerort-Spalten.

---

## 4) Behavior Definition + Behavior-Klasse

### 4a) Behavior Definition für `ZI_ContainerStock`
Rechtsklick auf die Data Definition `ZI_ContainerStock` → *New Behavior Definition*.
Implementation Type: **Managed**. Inhalt ersetzen:

```abap
managed implementation in class zbp_i_containerstock unique;
strict ( 2 );

define behavior for ZI_ContainerStock alias ContainerStock
persistent table zwrtcssafestock
lock master
authorization master ( global )
{
  update;

  field ( readonly ) ContainerType;

  field ( readonly ) ContainerTypeName,
                     Loc1000, Loc1010, Loc1020, Loc2000, Loc2010, Loc3000,
                     WithoutLocation, OtherLocations,
                     TotalOnStock, Difference, DifferenceCriticality;

  validation validateSafetyStock on save { field SafetyStock; create; update; }

  mapping for zwrtcssafestock corresponding
  {
    ContainerType = container_type;
    SafetyStock   = safety_stock;
  }
}
```

### 4b) Behavior-Klasse generieren
Speichern → im Editor erscheint eine Warnung/Quick-Fix zur Klasse
`ZBP_I_CONTAINERSTOCK`. Cursor auf `zbp_i_containerstock` → **Strg+1** →
*„Create behavior implementation class …“* → anlegen lassen.

Die generierte globale Klasse so lassen. In den **Local Types** (Tab unten im
Klassen-Editor) folgende Handler-Klasse einfügen:

```abap
CLASS lhc_containerstock DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    METHODS validateSafetyStock FOR VALIDATE ON SAVE
      IMPORTING keys FOR ContainerStock~validateSafetyStock.
ENDCLASS.

CLASS lhc_containerstock IMPLEMENTATION.

  METHOD validateSafetyStock.

    READ ENTITIES OF zi_containerstock IN LOCAL MODE
      ENTITY ContainerStock
        FIELDS ( SafetyStock )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_stock).

    LOOP AT lt_stock INTO DATA(ls_stock).
      IF ls_stock-SafetyStock < 0.
        APPEND VALUE #( %tky = ls_stock-%tky ) TO failed-containerstock.
        APPEND VALUE #( %tky                 = ls_stock-%tky
                        %element-SafetyStock = if_abap_behv=>mk-on
                        %msg = new_message_with_text(
                                 severity = if_abap_behv_message=>severity-error
                                 text     = 'Sicherheitsbestand darf nicht negativ sein' )
                      ) TO reported-containerstock.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

ENDCLASS.
```

Klasse aktivieren, dann Behavior Definition aktivieren.

---

## 5) Projektion `ZC_ContainerStock`

Rechtsklick Paket → *New → Data Definition*. Name `ZC_ContainerStock`,
Template *Define a Projection View* (Inhalt ersetzen):

```abap
@EndUserText.label: 'Behälter-Lagerbestände'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
@Search.searchable: true

define root view entity ZC_ContainerStock
  provider contract transactional_query
  as projection on ZI_ContainerStock
{
      @Search.defaultSearchElement: true
  key ContainerType,
      ContainerTypeName,

      Loc1000,
      Loc1010,
      Loc1020,
      Loc2000,
      Loc2010,
      Loc3000,
      WithoutLocation,
      OtherLocations,
      TotalOnStock,

      SafetyStock,

      Difference,
      DifferenceCriticality
}
```

---

## 6) Behavior Definition (Projektion) für `ZC_ContainerStock`

Rechtsklick auf `ZC_ContainerStock` → *New Behavior Definition*.
Implementation Type: **Projection**. Inhalt:

```abap
projection;
strict ( 2 );

define behavior for ZC_ContainerStock alias ContainerStock
{
  use update;
}
```

---

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

---

## 8) Service Definition `ZUI_ContainerStock`

Rechtsklick auf `ZC_ContainerStock` → *New → Service Definition*
(oder Paket → *New → Other → Business Services → Service Definition*).
Name `ZUI_ContainerStock`. Inhalt:

```abap
@EndUserText.label: 'Behälter-Lagerbestände'
define service ZUI_ContainerStock {
  expose ZC_ContainerStock as ContainerStock;
}
```

---

## 9) Service Binding + App starten

1. Rechtsklick auf `ZUI_ContainerStock` → *New → Service Binding*.
   - Name `ZUI_CONTAINERSTOCK_O4`.
   - Binding Type: **OData V4 – UI**.
2. Service Binding **aktivieren**, dann Button **Publish** (Local Service Endpoint).
3. Im Binding-Editor die Entität `ContainerStock` markieren → **Preview** →
   die generierte Fiori-Elements-App (List Report) öffnet sich im Browser.

Test: „Go“ zeigt die Behältertypen mit Lagerort-Spalten; „Bearbeiten“ →
Sicherheitsbestand ändern → „Sichern“.

---

## 10) Fiori Launchpad (produktiv)

1. Aus dem Service Binding eine **IBN/Tile** im Launchpad-Content anlegen
   (Business Catalog / SAP Fiori Launchpad Content Manager `/UI2/FLPCM_*`
   bzw. in S/4HANA: Business Catalog + Business Rolle über die IAM-App).
2. Berechtigung über `authorization master ( global )` bzw. die zugehörige
   IAM-App/Rolle vergeben.
3. Kachel testen → öffnet sich parallel zum Planungscockpit.

---

## Reihenfolge auf einen Blick

1 Tabelle (bereits vorhanden – nichts zu tun) → 2 ZI_ContainerStock (anlegen) → 3 ZCL_...CALC (aktiv.) →
2 aktivieren → 4 BDEF + ZBP-Klasse → 5 ZC_ContainerStock →
6 BDEF-Projektion → 7 Metadaten-Extension → 8 Service Definition →
9 Service Binding + Publish + Preview → 10 Launchpad.

## Noch anzupassen

- **Lagerort-Split (Teil B in Schritt 3):** Die eigentliche Aufteilung je
  Lagerort fehlt noch. Selektion aus `ZWR_WATP_CONTAINER_OVERVIEW`
  (`/watp/vcaequz` + `etyp` + Equipment-Stammdaten) einsetzen, je Behältertyp
  Status `L` zählen, Hersteller `DUMMY` ausschließen, nach Lagerort verteilen.
- **Behältertyp = `matnr` (`BEHTYPNEU`):** Im Report ist der Behältertyp das
  Material aus `etyp`. Den Schlüssel `container_type` in `ZWRTCSSAFESTOCK` bzw.
  `ZI_ContainerStock` an diese Domäne/Länge angleichen.
- **Zeilen = gepflegte Typen (Entscheidung):** Root ist `ZWRTCSSAFESTOCK`, es
  erscheinen also nur Behältertypen mit dortigem Sicherheitsbestand-Eintrag.
  Das ist gewollt. Neue Typen werden über die Pflege dieser Tabelle sichtbar.
- `ContainerTypeName`: echten Typtext statt Platzhalter lesen.
- Lagerort-Spalten (`Loc1000`…) auf eure realen Behälterlager anpassen
  (in ZI_ContainerStock, ZC_ContainerStock, der Metadaten-Extension und `gc_loc`).
```
