Zusammenfassung:

Im Gegensatz zur Stadtreinigung (vgl. #39534) wird für den Containerdienst eine eine bidirektionale Schnittstelle zwischen SAP und dem BMS-System vorgesehen.
Im SAP findet die Auftragserfassung, die Disposition in P&D und (nach Ausführung) die Rückmeldung und Abrechnung statt.
Das BMS-System ersetzt die Mobilgeräte. Das bedeutet die Fahrer haben ein Tablet mit BMS. HWS setzt das BMS bereits heute ein.
Wir übermitteln aus P&D heraus “fertig geplante” Touren mit den darauf geplanten Services an das BMS-System. Dazu wird ein neuer Funktionsaufruf im Planungscockpit benötigt. ("Tour an BMS freigeben.")
Mit dieser Funktion soll ein Webservice (REST/SOAP) im BMS-System aufgerufen und die Daten als JSON-Payload übergeben werden. Eine erste Beispiel-JSON ist im Anhang. Zum ersten Verständnis der Felder liegt auch eine Dokumentation der alten Schnittstelle (Schnittstelle_AWI-BMS_31.docx) vor.
// Nachtrag: Ein Export der Swagger-Dokumentation des Endpunkts liegt jetzt auch vor (Swagger UI_Schema.pdf).
Dieser Webservice basiert auf dem heute von HWS eingesetzten Funktionsumfang. Anhand dieser ersten Version werden noch Erweiterungen folgen.
Die Standardfunktionen für den Einsatz der PROLOGA-Mobil-Lösung (Tour freigeben, Tour zurück rufen, Tour automatisch ordnen, Servicereihenfolge übertragen) können wir vorerst ausblenden. Bitte nicht für diese Schnittstelle verwenden, da in der Zukunft ein (paralleler) Einsatz der PROLOGA-Lösung möglich bleiben soll.
 

Gern schon einmal die Informationen sichten. Weiteres folgt.
Komm bei Fragen wie gewohnt immer gern auf mich zu! :)
