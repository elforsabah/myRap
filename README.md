Was gebaut werden muss

Die Inbound-Seite besteht aus drei funktionalen Endpunkten:

Endpunkt 1 — Auftragsstatus "begonnen"
BMS meldet pro Auftrag (Service/EAP), dass die Ausführung gestartet wurde. SAP aktualisiert ZZ_BMS_STATUS auf BEGONNEN in /plce/tpdsrvcst. Technisch ein einfacher PATCH/POST mit Auftragsnummer als Schlüssel. Grundstruktur ist bereits vorhanden — der SICF-Handler ZCL_BMS_INBOUND_HANDLER und die Tabellenerweiterung existieren schon.

Endpunkt 2 — Rückmeldedaten Auftrag (EAP)
BMS liefert die Ausführungsdaten pro Service: tatsächliche Menge, Containertyp, Zeitstempel, ggf. Abweichungen. SAP schreibt diese Daten in die EAP (EWA_ORDER_OBJECT oder entsprechende Rückmeldetabelle). Das ist der komplexeste Endpunkt weil die Feldmapping-Logik aufwendig ist und mit Patrick geklärt werden muss welche Felder BMS tatsächlich zurückliefert.

Endpunkt 3 — Leistungsdaten Tour
BMS liefert Tourgesamtdaten: Startzeit, Endzeit, gefahrene km, ggf. Ressourcendaten. SAP schreibt in den Entsorgungsauftragskopf oder die Tagestour. Abhängig davon ob das ein oder zwei separate Endpunkte werden sollen (Tour-Kopf vs. Ressourcen) variiert der Aufwand.
