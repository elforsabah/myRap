Schnittstelle SAP P&D → BMS: Vollständige Erläuterung Punkt für Punkt
1. Ausgangslage und Zielsetzung

Für den Containerdienst der HWS wird eine bidirektionale Schnittstelle zwischen SAP S/4HANA und dem BMS-System eingerichtet. Die Aufgabenteilung ist klar definiert: Im SAP finden die Auftragserfassung, die Disposition in P&D sowie nach der Ausführung die Rückmeldung und Abrechnung statt. Das BMS-System ersetzt die bisherigen Mobilgeräte — die Fahrer erhalten ihre Aufträge künftig auf einem Tablet in der BMS-App, die bei HWS bereits im Einsatz ist. Die hier beschriebene Ausbaustufe 1 umfasst die Hinrichtung: Fertig geplante Touren werden mit allen darauf disponierten Services aus dem P&D-Cockpit per Knopfdruck an das BMS übertragen.

2. Gesamtarchitektur

Die Schnittstelle besteht aus folgenden Bausteinen: Der Disponent löst im P&D-Cockpit die Aktion „An BMS freigeben" auf einer oder mehreren markierten Touren aus. Die Freigabelogik (eine RAP-Aktion auf dem Business-Objekt /PLCE/R_PDTOUR) liest daraufhin alle benötigten Daten aus den SAP-Datenquellen, baut pro Service einen JSON-Auftrag zusammen und übergibt ihn an den HTTP-Client (Hilfsklasse ZCL_WR_PD_TOUR_HELPER). Dieser authentifiziert sich einmal pro Tour per Bearer-Token am BMS und sendet die Aufträge per HTTPS an die BMS-REST-API. Das BMS legt die Aufträge an und stellt sie den Fahrern auf dem Tablet bereit. Jede Antwort des BMS wird ausgewertet, in ZBMS_API_LOG protokolliert und als Status am Service und an der Tour sichtbar gemacht.

3. Ablauf der Freigabe — Schritt für Schritt

Schritt 1 — Konfiguration lesen. Zu Beginn wird die zentrale Konfigurationstabelle ZTOUR_BMS_CFG gelesen: BMS-Endpunkt-URL, Zugangsdaten, Aktiv-Schalter sowie die Standard-Geschäftspartner für Transporteur (DEFAULT_CARRIER) und Entsorger (DEFAULT_RECYCLER). Ist die Schnittstelle nicht konfiguriert oder inaktiv, bricht die Verarbeitung mit einer Meldung ab.

Schritt 2 — Tour und Services lesen. Die markierten Touren werden über das RAP-Business-Objekt /PLCE/R_PDTOUR mit Tour-ID, Tourvorlage und Startdatum gelesen, dazu alle zugeordneten Services in ihrer Tourreihenfolge. Die Servicedaten selbst (Termine, Zeitfenster, Leistungsart, Zusatztexte) kommen aus /PLCE/TPDSRV mit einem Join auf /PLCE/TPDSRVWR für die Behälter-Identnummern (TIDNR).

Schritt 3 — Anmeldung am BMS. Pro Tour wird einmal ein Bearer-Token beim BMS angefordert, mit den Zugangsdaten aus ZTOUR_BMS_CFG. Schlägt die Anmeldung fehl, wird die gesamte Tour mit Status ERROR gekennzeichnet und übersprungen.

Schritt 4 — Daten je Service zusammenstellen. Für jeden Service werden die Auftragsdaten aus der Entsorgungsauftragsverwaltung EWA_ORDER_OBJECT gelesen (Auftraggeber, Warenempfänger, Behältertyp, Transporteur, Entsorger, AVV-Code, Termine, SD-Bezug). Die Geschäftspartner-Adressen für Kunde, Anfahrtstelle, Transporteur und Entsorger kommen aus BUT000 / BUT020 / ADRC. Aus ZTOUR_BMS_KOLONN wird über die Tourvorlage das BMS-Fahrerteam (Kolonne) ermittelt, aus ZWRTWAALAPRCTP die BMS-Bewegungsart zur SAP-Leistungsart. Die Abfallbezeichnung zum AVV-Code wird aus dem Materialstamm (MAKT) gelesen.

Schritt 5 — JSON senden. Der vollständige Auftrag wird als JSON im vom BMS geforderten Format (PascalCase-Feldnamen, bereinigte Nummern ohne führende Nullen und ohne Leerzeichen) per HTTP POST an die BMS-REST-API übertragen.

Schritt 6 — Antwort auswerten. HTTP-Status 200 oder 201 gilt als Erfolg. Bei Fehlern wird die konkrete Fehlermeldung aus der BMS-Antwort extrahiert (z. B. „Der Containertyp '05' wurde nicht gefunden") und dem Disponenten direkt im Cockpit angezeigt.

Schritt 7 — Status und Protokoll schreiben. Pro Service wird der BMS-Status im Feld ZZ_BMS_STATUS der Tabelle /PLCE/TPDSRVCST gesetzt (FREIGEGEBEN oder ERROR), und der komplette Aufruf wird in ZBMS_API_LOG protokolliert — ein Eintrag pro Tour und Service, bei Wiederholung aktualisiert. Wichtig: Ein fehlgeschlagener Service unterbricht die Tour nicht — die übrigen Services werden trotzdem übertragen.

Die Schritte 4 bis 7 wiederholen sich für jeden Service der Tour. Am Ende wird der Gesamtstatus der Tour abgeleitet und in /PLCE/TPDTOURCST fortgeschrieben: SENT, wenn alle Services erfolgreich waren, PARTIAL bei teilweisem Erfolg, ERROR, wenn kein Service übertragen werden konnte.

4. Datenmapping SAP → BMS

Kunde und Erzeuger werden aus dem Auftraggeber (EWA_ORDER_OBJECT-KUNNR) übernommen — im Entsorgungskontext ist der Auftraggeber zugleich der Abfallerzeuger. Anfahrtstelle und Standort kommen aus dem Warenempfänger (KUNWE), also dem Ort, an dem der Container physisch steht. Transporteur (TRANSPORTER) und Entsorger (DISPOSER) werden aus dem Entsorgungsauftrag gelesen; fehlen sie dort, greifen die in ZTOUR_BMS_CFG hinterlegten Standardpartner. Die Containerdaten umfassen Typ (BEH_TYPE_NEW), Anzahl (BEH_ANZAHL), Bewegungsart (Stellen, Abholen, Umleeren — aus ZWRTWAALAPRCTP) und die Behälternummern aus der Disposition (/PLCE/TPDSRVWR). Termine und Zeitfenster, der AVV-Abfallschlüssel mit Bezeichnung (aus MAKT), Entsorgungsnachweis- und Begleitscheinnummern sowie Auftragstexte werden mitgegeben. Zusätzlich werden ein Expressauftrag-Kennzeichen und die Information übertragen, ob eine Kundenunterschrift erforderlich ist.

5. Konfigurierbarkeit statt Festwerte

Die Schnittstelle ist bewusst ohne fest programmierte Werte gebaut. Alle Zuordnungen liegen in Pflegetabellen, die die Fachabteilung selbst über die SAP-Tabellenpflege (SM30) ändern kann — ohne Programmänderung und ohne Entwickler.

6. Tabellenpflege im Überblick

Zu pflegen durch die Fachabteilung (SM30):

ZTOUR_BMS_CFG — die zentrale Konfiguration: BMS-Endpunkt-URL, Benutzername und Passwort, Aktiv-Schalter sowie die Felder DEFAULT_CARRIER und DEFAULT_RECYCLER. Diese beiden Geschäftspartner-Nummern dienen als Rückfallwert, wenn am Entsorgungsauftrag kein Transporteur oder Entsorger hinterlegt ist. Wichtig: Die hinterlegten Geschäftspartner müssen vollständige Adressen im Stammsatz haben, sonst lehnt das BMS den Auftrag wegen fehlender Pflichtfelder ab.

ZTOUR_BMS_KOLONN — die Zuordnung von Tourvorlage zu BMS-Kolonne (Fahrerteam), Schlüssel TOUR_TEMPLATE, Wert BMS_TEAM. Fehlt der Eintrag für eine Tourvorlage, wird der Auftrag ohne Team gesendet und der Disponent erhält die Warnung „Tour hat keine BMS-Kolonne".

ZWRTWAALAPRCTP — die Übersetzung der SAP-Leistungsarten (z. B. 03, 05) in BMS-Bewegungsarten über die Spalte BMS_MOVEMENT_TYPE. Das BMS akzeptiert hier nur Werte aus seinem festen Wertekatalog (OptiAwiMovementType); die gültigen Werte müssen vom BMS-Team bestätigt und dann eingepflegt werden. Fehlt ein Eintrag, greift derzeit ein Fallback und der Disponent wird gewarnt.

Geplant: Containertyp-Mapping — die Übersetzung der PROLOGA-Behältertypen (z. B. „05") in die BMS-Containertypen. Umsetzung entweder als zusätzliche Spalte BMS_CONTAINER_TYPE in ZWRTWAALAPRCTP oder als eigene Tabelle ZWRBEHTYP_BMS. Voraussetzung ist die Containertyp-Liste aus dem BMS.

Automatisch befüllt — keine Pflege, nur Überwachung:

ZBMS_API_LOG — das Kommunikationsprotokoll. Ein Eintrag pro Tour und Service (Schlüssel TOUR_UUID + SERVICE_UUID); bei wiederholter Freigabe wird der Eintrag aktualisiert, es entstehen keine Duplikate. Gespeichert werden Zeitstempel, Benutzer, Auftragsnummer, der komplette gesendete JSON-Inhalt, der HTTP-Status und die aus der BMS-Antwort extrahierte Fehlermeldung im Klartext.

/PLCE/TPDSRVCST — der BMS-Status pro Service im Feld ZZ_BMS_STATUS (FREIGEGEBEN, ERROR, STORNIERT), sichtbar im Cockpit.

/PLCE/TPDTOURCST — der abgeleitete Gesamtstatus pro Tour (SENT, PARTIAL, ERROR, STORNIERT).

7. Status und Transparenz

Der BMS-Status existiert auf zwei Ebenen. Pro Service (/PLCE/TPDSRVCST) zeigt er, ob der einzelne Auftrag erfolgreich übertragen wurde. Pro Tour (/PLCE/TPDTOURCST) wird daraus der Gesamtstatus abgeleitet: SENT, PARTIAL oder ERROR. Der Disponent sieht direkt nach der Freigabe pro Auftrag eine Meldung im Cockpit — bei Erfolg mit Auftragsnummer, bei Fehlern mit der konkreten Ursache aus dem BMS. Für die tiefere Analyse steht das vollständige Protokoll in ZBMS_API_LOG bereit.

8. Storno

Bereits übertragene Aufträge können über eine eigene Aktion im Cockpit storniert werden. Dabei wird pro Service ein Storno-Aufruf an das BMS gesendet, das BMS entfernt den Auftrag vom Fahrer-Tablet, und der Status wird am Service (/PLCE/TPDSRVCST) und an der Tour (/PLCE/TPDTOURCST) auf STORNIERT gesetzt. Auch jeder Storno-Aufruf wird vollständig in ZBMS_API_LOG protokolliert.

9. Offene Punkte

Positionsdaten: Die Abrechnungspositionen aus dem SD-Dauervertrag (VBAP, ermittelt über EWA_ORDER_OBJECT-SDAUFNR + SDPOSNR) sind technisch angebunden, werden aber noch nicht übertragen, weil das BMS-Feld PositionType nur Werte aus einem festen Katalog (OptiAwiPositionType) akzeptiert und die gültigen Werte noch nicht vorliegen. Sobald die Werteliste da ist, wird entschieden, ob die Ableitung direkt im Code (z. B. über das Materialnummern-Präfix SP_/ABP_/TRP_) erfolgt oder über eine kleine Pflegetabelle.

Stammdaten-Abgleich: Die Containertypen und Bewegungsarten müssen mit dem BMS-Katalog abgeglichen und in ZWRTWAALAPRCTP bzw. dem geplanten Containertyp-Mapping gepflegt werden. Dafür wird die vollständige Werteliste vom BMS-Team benötigt sowie eine Auswertung, welche Behältertypen (BEH_TYPE_NEW) bei HWS tatsächlich vorkommen.

Ausbaustufe 2 — Rückrichtung BMS → SAP: Drei Endpunkte sind vorgesehen: die Statusmeldung „begonnen" pro Auftrag (aktualisiert ZZ_BMS_STATUS in /PLCE/TPDSRVCST), die Rückmeldedaten der Ausführung zur Übernahme in die EAP und die Leistungsdaten der Gesamttour (Zeiten, gefahrene Kilometer) für den Entsorgungsauftragskopf. Grobschätzung: 22–34 Entwicklertage. Empfohlener Einstieg ist der kleine Status-Endpunkt, da er schnell lieferbar ist und dem BMS-Team sofort eine Testmöglichkeit gibt.

Soll ich diesen Text als Word-Dokument formatieren oder die Präsentation damit abgleichen?

Möchtest du benachrichtigt werden, wenn Claude antwortet?
