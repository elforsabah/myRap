Zusammenfassung:

Zusammen mit den Auftragsdaten SAP→BMS (vgl. #40067) wird für den Containerdienst eine eine bidirektionale Schnittstelle zwischen SAP und dem BMS-System vorgesehen.
Im SAP findet die Auftragserfassung, die Disposition in P&D und (nach Ausführung) die Rückmeldung und Abrechnung statt.
Das BMS-System ersetzt die Mobilgeräte. Das bedeutet die Fahrer haben ein Tablet mit einer BMS-App. HWS setzt das BMS bereits heute ein.
Wir übermitteln aus P&D heraus “fertig geplante” Touren mit den darauf geplanten Services an das BMS-System. 
Das BMS-System meldet uns später den Status “begonnen” pro Auftrag. Das Feld BMS-Status wird im P&D aktualisiert.
Das BMS-System meldet uns später die Rückmeldedaten aus der Ausführung. Wir übernehmen diese Daten direkt in die EAP.
Das BMS-System meldet später außerdem Leistungsdaten der gesamten Tour (z.B. Zeiten, gefahrene km). Wir übernehmen diese Daten direkt an die Tagestour bzw. Entsorgungsauftragskopf.

 

Wir haben für einen anderen Kunden (ERZ / Zürich) bereits einmal OData-Endpunkte für diese Rückdaten gebaut. Fachlich können wir uns daran orientieren, siehe Auszug aus der Doku im Anhang. Die technische Umsetzung können wir uns wohl nicht 1:1 als Vorbild nehmen (so die Einschätzung vom einem Entwickler aus dem ERZ-Projekt).

 

Was wird also insgesamt (vermutlich) gebraucht:

Ein Endpunkt für Rückmeldedaten Auftrag (EAP)
Ein Endpunkt für Status-Update “begonnen” → separat oder als eigenes Feld im 1. Endpunkt oben sinnvoll? (oder beides??)
Ein Endpunkt für Leistungsdaten der Tour (EA-Kopf/Ressourcen), ggf. zwei separate.
 

Kannst du hierfür bitte zuerst eine ganz grobe Größenordnung und Zeitplan für die Umsetzung einschätzen?
