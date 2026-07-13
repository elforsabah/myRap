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
`behtyp` und das Feld `safetystock`.

> Reale Struktur (SE11) – wird unverändert genutzt:
> ```abap
> key mandt   : mandt;                 " Mandant
> key behtyp  : zwr_d_cs_behtyp;       " Behältertyp, CHAR 40
> safetystock : zwr_d_cs_safetystock;  " Sicherheitsbestand, INT1
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
  key SafeStock.behtyp                                             as ContainerType,

      SafeStock.safetystock                                        as SafetyStock,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( '' as abap.char( 40 ) )                               as ContainerTypeName,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as LagerA,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as LagerB,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as LagerC,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as WerkstattW,
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as WerkstattX,

      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as WithoutLocation,

      // interne Sammelspalte (nicht angezeigt) für Bestände in sonstigen Lagern
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
      cast( 0 as abap.int4 )                                      as DifferenceCriticality,

      // Ampel für Sicherheitsbestand: 1=rot (=0) 3=grün (>0)  – wie im Mockup
      @ObjectModel.virtualElement: true
      @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CONTAINER_STOCK_CALC'
      cast( 0 as abap.int4 )                                      as SafetyStockCriticality
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
    " >>> Reale EQBS~B_LAGER-Codes eintragen (Platzhalter A/B/C/W/X anpassen!) <<<
    CONSTANTS:
      BEGIN OF gc_loc,
        lager_a     TYPE lgort_d VALUE 'A',   " Spalte "Lager A"
        lager_b     TYPE lgort_d VALUE 'B',   " Spalte "Lager B"
        lager_c     TYPE lgort_d VALUE 'C',   " Spalte "Lager C"
        werkstatt_w TYPE lgort_d VALUE 'W',   " Spalte "Werkstatt W"
        werkstatt_x TYPE lgort_d VALUE 'X',   " Spalte "Werkstatt X"
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

    " ============================================================
    "  Bestand je Behältertyp / Lagerort – Quelle wie ZWR_CONTAINER_OVW.
    "  Felder bestätigt gegen /watp/vcaequz (MATNR, TPLNR, DATAB, DATBI, EQUNR):
    "   - Status 'L' (auf Lager) = kein Aufstellort  (A~TPLNR initial)
    "   - Lagerort = EQBS~B_LAGER; kein/leeres B_LAGER => "ohne Lagerort"
    "   - HERST steht NICHT in /watp/vcaequz -> separater Join auf EQUI
    "   - Dummys (EQUI~HERST = 'DUMMY') werden herausgerechnet
    " ============================================================
    SELECT a~matnr   AS behtyp,
           s~b_lager AS lgort,
           e~herst   AS herst
      FROM /watp/vcaequz AS a
      INNER JOIN etyp AS t      ON a~matnr = t~matnr
      INNER JOIN equi AS e      ON e~equnr = a~equnr
      LEFT OUTER JOIN eqbs AS s ON s~equnr = a~equnr
      FOR ALL ENTRIES IN @lt_result
      WHERE a~matnr    =  @lt_result-containertype
        AND a~tplnr    =  @space          " nur Status 'L' (auf Lager)
        AND a~datab    <= @sy-datum
        AND a~datbi    >= @sy-datum
        AND t~kombinat =  'B'             " nur Behälter (wie im Report)
      INTO TABLE @DATA(lt_equip).

    DELETE lt_equip WHERE herst = 'DUMMY'.   " Dummys sind keine echten Behälter

    " Behältertyp-Bezeichnung (etyp: bautxt/bauform – wie im Report)
    SELECT matnr, bauform, bautxt FROM etyp
      FOR ALL ENTRIES IN @lt_result
      WHERE matnr = @lt_result-containertype
      INTO TABLE @DATA(lt_behtxt).
    SORT lt_behtxt BY matnr.

    LOOP AT lt_result ASSIGNING FIELD-SYMBOL(<row>).

      CLEAR: <row>-lagera, <row>-lagerb, <row>-lagerc,
             <row>-werkstattw, <row>-werkstattx,
             <row>-withoutlocation, <row>-otherlocations, <row>-totalonstock.

      LOOP AT lt_equip ASSIGNING FIELD-SYMBOL(<eq>) WHERE behtyp = <row>-containertype.
        <row>-totalonstock += 1.
        IF <eq>-lgort IS INITIAL.
          <row>-withoutlocation += 1.
        ELSE.
          CASE <eq>-lgort.
            WHEN gc_loc-lager_a.     <row>-lagera += 1.
            WHEN gc_loc-lager_b.     <row>-lagerb += 1.
            WHEN gc_loc-lager_c.     <row>-lagerc += 1.
            WHEN gc_loc-werkstatt_w. <row>-werkstattw += 1.
            WHEN gc_loc-werkstatt_x. <row>-werkstattx += 1.
            WHEN OTHERS.             <row>-otherlocations += 1.
          ENDCASE.
        ENDIF.
      ENDLOOP.

      <row>-difference = <row>-totalonstock - <row>-safetystock.
      <row>-differencecriticality =
        COND #( WHEN <row>-totalonstock <  <row>-safetystock THEN 1
                WHEN <row>-totalonstock =  <row>-safetystock THEN 2
                ELSE 3 ).
      <row>-safetystockcriticality =
        COND #( WHEN <row>-safetystock = 0 THEN 1 ELSE 3 ).   " rot bei 0, sonst grün

      READ TABLE lt_behtxt INTO DATA(ls_txt)
           WITH KEY matnr = <row>-containertype BINARY SEARCH.
      IF sy-subrc = 0.
        <row>-containertypename =
          COND #( WHEN ls_txt-bautxt IS NOT INITIAL THEN ls_txt-bautxt
                  ELSE ls_txt-bauform ).
      ENDIF.

    ENDLOOP.

    ct_calculated_data = CORRESPONDING #( lt_result ).

  ENDMETHOD.

ENDCLASS.
```

Aktivieren. Danach zurück zu **Schritt 2** und `ZI_ContainerStock` aktivieren.

> **Felder bestätigt** gegen `/watp/vcaequz`: `MATNR`, `TPLNR`, `DATAB`, `DATBI`,
> `EQUNR`, `LOGIKNR` sind vorhanden. `HERST` ist dort **nicht** enthalten →
> separater Join auf `EQUI`. Die Selektion ist damit feldkorrekt.
>
> **Falls trotzdem `$batch`-Fehler kommt**, liegt es NICHT an der Selektion,
> sondern an `authorization master ( global )` in der Behavior Definition
> (Schritt 4a) → diese Zeile entfernen, BDEF neu aktivieren und das Service
> Binding neu **publishen**.

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

## 5) Wertehilfe + Projektion `ZC_ContainerStock`

### 5a) Wertehilfe-View `ZI_ContainerTypeVH` (zuerst anlegen)
Rechtsklick Paket → *New → Other → Core Data Services → Data Definition*.
Name `ZI_ContainerTypeVH`. Listet die Behältertypen aus `etyp`
(nur Behälter, `kombinat = 'B'`) als F4-Hilfe. `ContainerType` erbt die
Domäne **MATNR** (Konv.-Routine `MATN1`) aus `etyp.matnr`:

```abap
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Wertehilfe: Behältertyp'
@ObjectModel.resultSet.sizeCategory: #XS
@Search.searchable: true
define view entity ZI_ContainerTypeVH
  as select from etyp
{
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
  key matnr   as ContainerType,
      bauform as BuildingForm,
      bautxt  as Description
}
where kombinat = 'B'
```

Aktivieren.

### 5b) Projektion `ZC_ContainerStock`
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
      @Consumption.valueHelpDefinition: [{ entity: { name: 'ZI_ContainerTypeVH', element: 'ContainerType' } }]
  key ContainerType,
      ContainerTypeName,

      LagerA,
      LagerB,
      LagerC,
      WerkstattW,
      WerkstattX,
      WithoutLocation,
      OtherLocations,
      TotalOnStock,

      SafetyStock,
      SafetyStockCriticality,

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
  //=== Object Page: facet MUSS am Schlüsselfeld stehen, NICHT im Header-Block!
  @UI.facet: [
    { id: 'General',   purpose: #STANDARD, type: #IDENTIFICATION_REFERENCE,
      label: 'Allgemein',           position: 10 },
    { id: 'Locations', purpose: #STANDARD, type: #FIELDGROUP_REFERENCE,
      label: 'Bestand je Lagerort', position: 20, targetQualifier: 'Locations' }
  ]
  //--- Behältertyp (Spalte 1 + Selektion + F4) ---
  @UI.selectionField: [{ position: 10 }]
  @UI.lineItem:       [{ position: 10, label: 'Behältertyp', importance: #HIGH }]
  @UI.identification: [{ position: 10, label: 'Behältertyp' }]
  ContainerType;

  // nur Object Page (im List Report laut Mockup keine eigene Spalte)
  @UI.identification: [{ position: 20, label: 'Bezeichnung' }]
  ContainerTypeName;

  //--- Lagerort-Spalten (genau wie Mockup) + Field Group (Object Page) ---
  @UI.lineItem:   [{ position: 20, label: 'Lager A' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 10, label: 'Lager A' }]
  LagerA;
  @UI.lineItem:   [{ position: 30, label: 'Lager B' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 20, label: 'Lager B' }]
  LagerB;
  @UI.lineItem:   [{ position: 40, label: 'Lager C' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 30, label: 'Lager C' }]
  LagerC;
  @UI.lineItem:   [{ position: 50, label: 'Werkstatt W' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 40, label: 'Werkstatt W' }]
  WerkstattW;
  @UI.lineItem:   [{ position: 60, label: 'Werkstatt X' }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 50, label: 'Werkstatt X' }]
  WerkstattX;

  @UI.lineItem:   [{ position: 70, label: 'ohne Lagerort', importance: #HIGH }]
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 60, label: 'ohne Lagerort' }]
  WithoutLocation;

  // nur Object Page (interne Sammelspalte)
  @UI.fieldGroup: [{ qualifier: 'Locations', position: 70, label: 'sonstige Lagerorte' }]
  OtherLocations;

  // nur Object Page
  @UI.identification: [{ position: 30, label: 'Summe Lager' }]
  @UI.fieldGroup:     [{ qualifier: 'Locations', position: 80, label: 'Summe Lager' }]
  TotalOnStock;

  //--- Sicherheitsbestand (letzte Spalte, editierbar, Ampel wie Mockup) ---
  @UI.lineItem:       [{ position: 80, label: 'Sicherheitsbestand', importance: #HIGH,
                         criticality: 'SafetyStockCriticality' }]
  @UI.identification: [{ position: 40, label: 'Sicherheitsbestand',
                         criticality: 'SafetyStockCriticality' }]
  SafetyStock;

  // nur Object Page
  @UI.identification: [{ position: 50, label: 'Differenz', criticality: 'DifferenceCriticality' }]
  Difference;

  @UI.hidden: true
  DifferenceCriticality;

  @UI.hidden: true
  SafetyStockCriticality;
}
```

> **Object Page:** Durch `facet` + `@UI.identification` + `@UI.fieldGroup`
> entsteht eine Detailseite. Ein Klick auf eine Zeile im List Report öffnet sie:
> Abschnitt **Allgemein** (Behältertyp, Bezeichnung, Summe Lager,
> Sicherheitsbestand, Differenz) und Abschnitt **Bestand je Lagerort**
> (alle Loc-Spalten, ohne/sonstige Lagerorte). `Sicherheitsbestand` ist auch
> hier über „Bearbeiten“ änderbar.

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

Test: „Go“ zeigt die Behältertypen mit Lagerort-Spalten; **Klick auf eine Zeile**
öffnet die Object Page (Abschnitte „Allgemein“ + „Bestand je Lagerort“);
„Bearbeiten“ → Sicherheitsbestand ändern → „Sichern“ (im List Report oder auf
der Object Page).

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
2 aktivieren → 4 BDEF + ZBP-Klasse → 5a ZI_ContainerTypeVH (Wertehilfe) →
5b ZC_ContainerStock → 6 BDEF-Projektion → 7 Metadaten-Extension →
8 Service Definition → 9 Service Binding + Publish + Preview → 10 Launchpad.

## Noch anzupassen

- **Dummy-Kennzeichen prüfen:** Die Selektion filtert `EQUI~HERST = 'DUMMY'`.
  Steht das Dummy-Merkmal in eurem System woanders, in Schritt 3 anpassen.
- **Reale Behälterlager-Codes eintragen:** In `gc_loc` (Schritt 3) stehen
  Platzhalter `A/B/C/W/X`. Ersetzt sie durch die echten `EQBS~B_LAGER`-Codes
  eurer Lagerorte „Lager A/B/C“ und „Werkstatt W/X“. Spaltentitel ggf. in der
  Metadaten-Extension (Schritt 7) anpassen.
- **Status-'L'-Filter:** `A~TPLNR = space` entspricht der Statuslogik des Reports
  (kein Aufstellort ⇒ Lager). Wollt ihr exakt die „Lager“-Selektion des Reports,
  zusätzlich `AND a~logiknr = @lv_empty_logiknr` verwenden.
- **Zeilen = gepflegte Typen (Entscheidung):** Root ist `ZWRTCSSAFESTOCK`, es
  erscheinen nur Behältertypen mit dortigem `SAFETYSTOCK`-Eintrag (aktuell 7).
- **Optional:** `ZCL_WR_WATP_CS_MISC=>CL_CALC_CONTAINER_INVENTORY` für eine
  zusätzliche Netto-Auslastungsspalte je Typ (nicht erforderlich).
```
