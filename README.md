<img width="904" height="585" alt="image" src="https://github.com/user-attachments/assets/b86758b1-8452-4c3d-8b69-54ba43a6f58b" />


@EndUserText.label: 'Service Def für Behälter-Lagerbestände'
define service ZUI_ContainerStock {
  expose ZC_ContainerStock as ContainerStock;
}

projection;
strict ( 2 );

define behavior for ZC_ContainerStock //alias <alias_name>
{
//  use create;
 use update;
//  use delete;
   use action sicherheitb_aendern;
}


managed implementation in class zbp_i_containerstock unique;
strict ( 2 );

define behavior for ZI_ContainerStock alias ContainerStock
persistent table zwrtcssafestock
lock master
authorization master ( global )
{
  update;
//  delete;

// ( precheck, features : global )
  action sicherheitb_aendern
    parameter ZA_ContainerSafetyParam
    result [0..*] $self;
side effects {

 action sicherheitb_aendern affects $self;
 }

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


@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZA_ContainerSafetyParam
{
  @EndUserText.label: 'Neu Sicherheitsbestand'
  SafetyStock : zwr_d_cs_safetystock;
}


@EndUserText.label: 'Behälter-Lagerbestände'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
@Search.searchable: true

define root view entity ZC_ContainerStock
  provider contract transactional_query
  as projection on ZI_ContainerStock
{

 @Search.defaultSearchElement: true
 @Search.fuzzinessThreshold: 0.8
 @Consumption.valueHelpDefinition: [{ entity: { name: 'ZI_ContainerTypeVH', element: 'ContainerType' } }]
key ContainerType,
SafetyStock,
ContainerTypeName,
LagerA,
LagerB,
LagerC,
WerkstattW,
WerkstattX,
WithoutLocation,
OtherLocations,
TotalOnStock,
Difference,
DifferenceCriticality,
SafetyStockCriticality

}


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

  @UI.lineItem:       [{ position: 80, label: 'Sicherheitsbestand', importance: #HIGH,
                         criticality: 'SafetyStockCriticality' },
                       { type: #FOR_ACTION, dataAction: 'sicherheitb_aendern',
                         label: 'Sicherheitsbestand ändern' }]
  @UI.identification: [{ position: 40, label: 'Sicherheitsbestand',
                         criticality: 'SafetyStockCriticality' },
                       { type: #FOR_ACTION, dataAction: 'sicherheitb_aendern',
                         label: 'Sicherheitsbestand ändern' }]
  SafetyStock;

  // nur Object Page
  @UI.identification: [{ position: 50, label: 'Differenz', criticality: 'DifferenceCriticality' }]
  Difference;

  @UI.hidden: true
  DifferenceCriticality;

  @UI.hidden: true
  SafetyStockCriticality;
}

CLASS lhc_containerstock DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    " von "authorization master ( global )" gefordert:
    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR ContainerStock
      RESULT result.

    METHODS validateSafetyStock FOR VALIDATE ON SAVE
      IMPORTING keys FOR ContainerStock~validateSafetyStock.
    METHODS sicherheitb_aendern FOR MODIFY
      IMPORTING keys FOR ACTION ContainerStock~sicherheitb_aendern RESULT result.
ENDCLASS.

CLASS lhc_containerstock IMPLEMENTATION.

  METHOD get_global_authorizations.
    " Steuert das Ändern des Sicherheitsbestands. Prototyp: erlaubt.
    IF requested_authorizations-%update = if_abap_behv=>mk-on.
*     AUTHORITY-CHECK OBJECT 'Z_CONT_ST'
*       ID 'ACTVT' FIELD '02'.
*     IF sy-subrc = 0.
        result-%update = if_abap_behv=>auth-allowed.
*     ELSE.
*       result-%update = if_abap_behv=>auth-unauthorized.
*     ENDIF.
    ENDIF.
  ENDMETHOD.

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

  METHOD sicherheitb_aendern.

    " 1) neuen Sicherheitsbestand je selektierter Instanz setzen
    MODIFY ENTITIES OF zi_containerstock IN LOCAL MODE
      ENTITY ContainerStock
        UPDATE FIELDS ( SafetyStock )
        WITH VALUE #( FOR key IN keys
                      ( %tky        = key-%tky
                        SafetyStock = key-%param-SafetyStock ) )
      FAILED   failed
      REPORTED reported.

    " 2) geänderte Instanzen zurücklesen und als Ergebnis ($self) liefern
    READ ENTITIES OF zi_containerstock IN LOCAL MODE
      ENTITY ContainerStock
        ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_read).

    result = VALUE #( FOR row IN lt_read
                      ( %tky   = row-%tky
                        %param = row ) ).

  ENDMETHOD.


ENDCLASS.

CLASS zcl_container_stock_calc DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    INTERFACES if_sadl_exit_calc_element_read.

  PRIVATE SECTION.
    " Lagerorte (T001L, Werk 4410). Lager A/B/C bestätigt; Werkstätten offen.
    CONSTANTS:
      BEGIN OF gc_loc,
        lager_a     TYPE lgort_d VALUE '3002',   " 02 Lager A
        lager_b     TYPE lgort_d VALUE '3003',   " 03 Lager B
        lager_c     TYPE lgort_d VALUE '3004',   " 04 Lager C
        werkstatt_w TYPE lgort_d VALUE 'WKW',    " TODO: echten LGORT eintragen
        werkstatt_x TYPE lgort_d VALUE 'WKX',    " TODO: echten LGORT eintragen
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

    " "Lager"-Selektion 1:1 aus ZWR_WATP_CONTAINER_OVERVIEW (psel = 'L'),
    " ergänzt um Dummies-Ausschluss (EQUI~HERST) + Sicherheitsbestand (Tabelle).
    DATA lv_empty_logiknr TYPE logiknr.       " initial = auf Lager

    SELECT a~matnr   AS behtyp,
           s~b_lager AS lgort,
           e~herst   AS herst
      FROM /watp/vcaequz AS a
      INNER JOIN etyp AS t      ON a~matnr = t~matnr
      INNER JOIN equi AS e      ON e~equnr = a~equnr
      LEFT OUTER JOIN eqbs AS s ON a~equnr = s~equnr
      FOR ALL ENTRIES IN @lt_result
      WHERE a~matnr    =  @lt_result-containertype
        AND a~logiknr  =  @lv_empty_logiknr
        AND a~datab    <= @sy-datum
        AND a~datbi    >= @sy-datum
        AND t~kombinat =  'B'
      INTO TABLE @DATA(lt_equip).

    DELETE lt_equip WHERE herst = 'DUMMY'.

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
        COND #( WHEN <row>-safetystock = 0 THEN 1 ELSE 3 ).

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

