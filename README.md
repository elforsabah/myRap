Was zu tun ist

Die Inbound-Seite braucht drei funktionale Endpunkte:

Endpunkt 1 – Auftragsstatus “begonnen”
BMS meldet für jeden Auftrag (Service/EAP), dass die Ausführung gestartet wurde. SAP setzt dann das Feld ZZ_BMS_STATUS in /plce/tpdsrvcst auf BEGONNEN. Technisch läuft das einfach als PATCH oder POST, Schlüssel ist die Auftragsnummer. Die Basis steht schon: es gibt den SICF-Handler ZCL_BMS_INBOUND_HANDLER, und die Tabellenerweiterung ist auch schon da.

Endpunkt 2 – Rückmeldedaten Auftrag (EAP)
Hier schickt BMS die Ausführungsdaten pro Service: tatsächliche Menge, Containertyp, Zeitstempel, bei Bedarf auch Abweichungen. SAP übernimmt diese Daten in die EAP (EWA_ORDER_OBJECT oder die passende Rückmeldetabelle). Das ist der komplizierteste Endpunkt, weil das Feldmapping schwierig ist. Mit Patrick muss noch abgestimmt werden, welche Felder BMS tatsächlich liefert.

Endpunkt 3 – Leistungsdaten Tour
BMS liefert die Gesamtdaten der Tour: Startzeit, Endzeit, gefahrene Kilometer und eventuell Ressourcendaten. SAP übernimmt das in den Kopf vom Entsorgungsauftrag oder in die Tagestour. Je nachdem, ob das ein oder zwei Endpunkte werden (Tour-Kopf separat von Ressourcen), variiert der Aufwand hier ein bisschen.
