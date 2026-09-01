<img width="1403" height="784" alt="image" src="https://github.com/user-attachments/assets/22af8035-55ac-40e4-957b-b9b1dd06febf" />


<img width="1357" height="670" alt="image" src="https://github.com/user-attachments/assets/dac66a9e-2d5b-463c-9865-51115a0bce05" />


BMS-Feld	SAP-Quelle	Regel
status	—	Konstante ok (Storno: cancelled)
externalSystemID	/PLCE/R_PDTOUR-TOURID	ALPHA = OUT, condensed
sortNumber	Tourzuordnung TOURSEQUENCE	unverändert
orderNumber	EWA_ORDER_OBJECT-SMAUFNR	erster gefüllter Wert aus SMAUFNR → ORDERNR → SDAUFNR → /PLCE/TPDSRV-REFERENCE_ID → SERVICE_ID, je ALPHA = OUT
orderSheet	EWA_ORDER_OBJECT-/WATP/NOTENR	condensed
orderSheetType	—	LS, wenn /WATP/NOTENR gefüllt; sonst weggelassen
customer.number	EWA_ORDER_OBJECT-KUNNR	ALPHA = OUT, condensed
customer.name1	ADRC-NAME1 über BUT020 zu KUNNR	condensed
customer.name2	ADRC-NAME2	condensed
customer.street	ADRC-STREET	condensed
customer.streetNumber	ADRC-HOUSE_NUM1	condensed
customer.zipCode	ADRC-POST_CODE1	condensed
customer.city	ADRC-CITY1	condensed — fehlt eines der Felder, wird der Service mit Meldung 030 übersprungen
placeOfDelivery.street	ADRC-STREET über BUT020 zu EWA-KUNWE	condensed
placeOfDelivery.streetNumber	ADRC-HOUSE_NUM1	condensed
placeOfDelivery.zipCode	ADRC-POST_CODE1	condensed
placeOfDelivery.city	ADRC-CITY1	condensed — gesamtes Objekt entfällt, wenn nicht alle vier gefüllt sind
location.street	ADRC-STREET über BUT020 zu EWA-KUNWE	identisch zu placeOfDelivery
location.streetNumber	ADRC-HOUSE_NUM1	identisch zu placeOfDelivery
location.zipCode	ADRC-POST_CODE1	identisch zu placeOfDelivery
location.city	ADRC-CITY1	identisch zu placeOfDelivery
estimatedDuration	EWA_ORDER_OBJECT-PLANNED_DURT	HH * 60 + MM; 0 bei nicht-numerischem Inhalt
plannedDate	/PLCE/TPDSRV-EARLIEST_DATE	erster gefüllter Wert aus EARLIEST_DATE → REQUESTED_DATE → EWA-ZZ_ORDER_DATE → EWA-OLD_ORDER_DATE → Tour STARTDATE, Format YYYY-MM-DDT00:00:00.000Z; keiner gefüllt → Meldung 028
executionTimeFrameStart	/PLCE/TPDSRV-SERVICE_WINDOW_START	HH:MM:SS
executionTimeFrameEnd	/PLCE/TPDSRV-SERVICE_WINDOW_END	HH:MM:SS
notes	EWA_ORDER_OBJECT-ORDERTXT	ergänzt um AUFTRAG ZU KVV (KVV-Regel), Bestellnummer: <SMAUFNR>, EXPRESSAUFTRAG, Entsorgungsnachweis: <APPNO/INTAPPNO>
specialNotes	/PLCE/TPDSRV-ADDITIONAL_TEXT	Präfix EXPRESSAUFTRAG - bei ORDER_TYPE = '02'
producer.*	wie customer.*	Auftraggeber ist zugleich Abfallerzeuger
recycler.name1	ADRC-NAME1 zu EWA-DISPOSER, sonst ZTOUR_BMS_CFG-DEFAULT_RECYCLER	kein number-Feld im Schema
recycler.name2	ADRC-NAME2	condensed
recycler.street	ADRC-STREET	condensed
recycler.streetNumber	ADRC-HOUSE_NUM1	condensed
recycler.zipCode	ADRC-POST_CODE1	condensed
recycler.city	ADRC-CITY1	condensed — Objekt entfällt, wenn name1 oder eine Adresszeile fehlt
carrier.number	EWA-TRANSPORTER, sonst ZTOUR_BMS_CFG-DEFAULT_CARRIER	ALPHA = OUT; Nummer und Adresse stammen immer vom selben Partner
carrier.name1	ADRC-NAME1 zum gewählten Partner	condensed
carrier.name2	ADRC-NAME2	condensed
carrier.street	ADRC-STREET	condensed
carrier.streetNumber	ADRC-HOUSE_NUM1	condensed
carrier.zipCode	ADRC-POST_CODE1	condensed
carrier.city	ADRC-CITY1	condensed — Objekt entfällt, wenn Nummer, name1 oder eine Adresszeile fehlt
garbageKey	EWA_ORDER_OBJECT-/WATP/AVVCODE	condensed
garbageName	MAKT-MAKTX	MATNR = /WATP/AVVCODE, SPRAS = SY-LANGU
collectiveConsignmentNoteNumber	EWA_ORDER_OBJECT-/WATP/NOTEINTNR	ABAP-Komponente coll_consignment_note_nr, nach der Serialisierung umbenannt (30-Zeichen-Grenze)
team	ZTOUR_BMS_KOLONN-BMS_TEAM, sonst /PLCE/R_PDTOUR-TOURTEMPLATE	Tabelle wirkt nur als Übersteuerung
signatureRequired	EWA-ORDERTXT / /PLCE/TPDSRV-ADDITIONAL_TEXT	true, wenn einer der Texte UNTERSCHR enthält (Großschreibung)
containers[].movementType	ZWRTWAALAPRCTP-BMS_MOVEMENT_TYPE	Schlüssel /PLCE/TPDSRV-ACTION; to_lower( condense( ) ); nur new/change/collect, sonst Meldung 026
containers[].containerTypeName	EWA_ORDER_OBJECT-BEH_TYPE	condensed, ohne Mapping durchgereicht; leer → Meldung 023
containers[].containerNumberOld	/PLCE/TPDSRVWR-CONTAINER_ATLOC_TIDNR	bei new geleert; bei collect Fallback EWA-CONTAINER; bei change Pflicht → sonst Meldung 034
containers[].containerNumberNew	/PLCE/TPDSRVWR-CONTAINER_NEW_TIDNR	bei collect geleert; Fallback EWA-CONTAINER; bei new und change Pflicht
containers[].containerMovementTypeInfo	abgeleitet aus movementType	Aufstellung / Wechsel / Einzug — Pflichtfeld der laufenden API
containers[].internalRemark	/PLCE/TPDSRV-ADDITIONAL_TEXT	keine, wenn leer — [Required] lehnt Leerstrings ab
containers[].customerOwned	—	Konstante false
