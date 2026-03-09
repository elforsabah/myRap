Im Gegensatz zur Stadtreinigung (vgl. #39534) wird für den Containerdienst eine eine bidirektionale Schnittstelle zwischen SAP und dem BMS-System vorgesehen.
Im SAP findet die Auftragserfassung, die Disposition in P&D und (nach Ausführung) die Rückmeldung und Abrechnung statt.
Das BMS-System ersetzt die Mobilgeräte. Das bedeutet die Fahrer haben ein Tablet mit BMS. HWS setzt das BMS bereits heute ein.
Wir übermitteln aus P&D heraus “fertig geplante” Touren mit den darauf geplanten Services an das BMS-System. Dazu wird ein neuer Funktionsaufruf im Planungscockpit benötigt. ("Tour an BMS freigeben.")
Mit dieser Funktion soll ein Webservice (REST/SOAP) im BMS-System aufgerufen und die Daten als JSON-Payload übergeben werden. Eine erste Beispiel-JSON ist im Anhang. Zum ersten Verständnis der Felder liegt auch eine Dokumentation der alten Schnittstelle (Schnittstelle_AWI-BMS_31.docx) vor.
// Nachtrag: Ein Export der Swagger-Dokumentation des Endpunkts liegt jetzt auch vor (Swagger UI_Schema.pdf).
Dieser Webservice basiert auf dem heute von HWS eingesetzten Funktionsumfang. Anhand dieser ersten Version werden noch Erweiterungen folgen.
Die Standardfunktionen für den Einsatz der PROLOGA-Mobil-Lösung (Tour freigeben, Tour zurück rufen, Tour automatisch ordnen, Servicereihenfolge übertragen) können wir vorerst ausblenden. Bitte nicht für diese Schnittstelle verwenden, da in der Zukunft ein (paralleler) Einsatz der PROLOGA-Lösung möglich bleiben soll.




<img width="2535" height="533" alt="image" src="https://github.com/user-attachments/assets/c07ef517-fd23-41d8-9a28-41b61193d914" />



/ swagger: http://hws-srv17.hws.vvv.vvv-konzern.net/BmsApiSapTest/apidoc/index.html
// (scheint aktuell von uns oder SAP aus noch keinen Inhalt auszugeben)
//
//
// Ziel-URL (vermutlich): http://hws-srv17.hws.vvv.vvv-konzern.net/BmsApiSapTest/api/container/create-order-halle
{
 "status": "OK",
 "orderNumber": "string",
 "orderSheet": "string",
 "orderSheetType": "LS",
 "customer": {
   "number": "string",
   "name1": "string",
   "name2": "string",
   "street": "string",
   "streetNumber": "string",
   "zipCode": "string",
   "city": "string"
 },
 "placeOfDelivery": {
   "street": "string",
   "streetNumber": "string",
   "zipCode": "string",
   "city": "string"
 },
 "location": {
   "street": "string",
   "streetNumber": "string",
   "zipCode": "string",
   "city": "string"
 },
 "estimatedDuration": 0,
 "plannedDate": "2026-02-12T12:06:15.118Z",
 "executionDate": "2026-02-12T12:06:15.118Z",
 "executionTimeFrameStart": "string",
 "executionTimeFrameEnd": "string",
 "executionTime": "string",
 "notes": "string",
 "specialNotes": "string",
 "producer": {
   "number": "string",
   "name1": "string",
   "name2": "string",
   "street": "string",
   "streetNumber": "string",
   "zipCode": "string",
   "city": "string"
 },
 "recycler": {
   "name1": "string",
   "name2": "string",
   "street": "string",
   "streetNumber": "string",
   "zipCode": "string",
   "city": "string"
 },
 "carrier": {
   "number": "string",
   "name1": "string",
   "name2": "string",
   "street": "string",
   "streetNumber": "string",
   "zipCode": "string",
   "city": "string"
 },
 "garbageKey": "string",
 "garbageName": "string",
 "collectiveConsignmentNoteNumber": "string",
 "team": "string",
 "contractRelated": true,
 "signatureRequired": true,
 "positions": [
   {
     "sortNumber": 0,
     "itemNumber": "string",
     "itemDescription": "string",
     "quantity": 0,
     "unit": "string",
     "itemPrice": 0,
     "positionType": "P"
   }
 ],
 "containers": [
   {
     "quantity": 0,
     "movementType": "S",
     "containerTypeName": "string",
     "containerTypeNumber": "string",
     "customerOwned": true
   }
 ]
}


1	Ausgangssituation
Die HWS plant die Erweiterung der BMS-Software um das Modul Containerdienst. In diesem Modul soll die komplette Auftragsdurchführung der Aufträge aus dem Bereich Containerdienst erfolgen. Da die Auftragserstellung und die Auftragsabrechnung aber weiterhin in der Software opti.AWI stattfin-den, müssen die Auftragsinformationen von opti.AWI an das BMS übergeben werden. Nach der Bear-beitung im BMS müssen alle abrechnungsrelevanten Informationen zurück in opti.AWI. 
2	Zielstellung
Die Übergabe der Auftragsdaten von opti.AWI an das BMS erfolgt über einen automatischen Export der Daten aus opti.AWI beim Statuswechsel eines Auftrages auf GEDRUCKT und dem zeitlich gesteu-erten Import in das BMS. 
Für die Rückführung der abrechnungsrelevanten Daten in opti.AWI gibt ist keine automatische Schnitt-stelle. Hier muss das BMS geeignete Übersichten bereitstellen, die neben den Abrechnungsinformati-onen in jedem Fall die Auftragsnummer aus opti.AWI enthält, welche beim Export an das BMS über-geben wurde. Das Einpflegen der Daten in opti.AWI geschieht dann manuell.

3	Aufbau des Datenexportes von opti.AWI
Die Arbeitsaufträge in opti.AWI setzen sich aus den Kopfdaten (Kunde, Leistungsort, Erzeuger, Beförderer, …), ein oder mehreren Behälterpositionen (Behältertyp, Bewegungsart, Anzahl,…) und ein oder mehreren Einzelpositionen (Artikel, Menge, Preis,…. oder Textposition) zusammen.

Aus opti.AWI werden daher pro Arbeitsauftrag 3 Dateien generiert:
EXP_AWI_AUFTRAG_<pkey Auftrag>_KOPFDATEN_<Zeitstempel>
EXP_AWI_AUFTRAG_<pkey Auftrag>_BEHAELTER_<Zeitstempel>
EXP_AWI_AUFTRAG_<pkey Auftrag>_POSITIONEN_<Zeitstempel>

Die Daten der Einzelpositionen werden aktuell zusätzlich in einem Textfeld in der Kopfdatendatei übergeben (da ursprünglich keine Einzelpositionsdatei übergeben werden sollte).

 
3.1	Aufbau EXP_AWI_AUFTRAG_<pkey Auf-trag>_KOPFDATEN_<Zeitstempel>
Die Exportdatei ist eine CSV-Datei mit Kopfzeile. Als Trennzeichen wird das Semikolon verwen-det. Die Datei wird im ANSI-Zeichensatz bereitgestellt.
Die Folgende Tabelle zeigt die Spaltennamen, den zugrundeliegenden Datentyp und ggf. eine kurze Beschreibung des Inhaltes.

Spaltenname csv-Datei	Datentyp in Datenbank	Bemerkungen
AA_PKEY	NUMBER	Primär Schlüssel Datenbanktabelle AA (Arbeitsauftrag)
AL_PKEY	NUMBER	Primärschlüssel Datenbanktabelle AL (Lieferschein) -> Fremdschlüssel für Auftragspositionen
STATUS	VARCHAR2(40)	Übergeben wird Status GEDRUCKT oder STORNIERT
AA_NR	VARCHAR2(20)	Nummer Arbeitsauftrag zum Su-chen im AWI-Client
KUNDENNR	VARCHAR2(20)	 Kundennummer
SCHEIN_NR	VARCHAR2(40)	 Scheinnummer (die Art wird über Scheinart bestimmt)
SCHEINART	VARCHAR2(3)	LS = Lieferschein, ÜS = Überneh-meschen, BS = Begleitschein
BEARBEITER	VARCHAR2(40)	 Bearbeiter 
KUNDE_NAME1	VARCHAR2(40)	 Kundenname Teil 1
KUNDE_NAME2	VARCHAR2(40)	 Kundenname Teil 2
KUNDE_STRASSE	VARCHAR2(40)	 Kundenstraße
KUNDE_HANR	VARCHAR2(40)	 Kundenhausnummer
KUNDE_PLZ	VARCHAR2(40)	 Kundenpostleitzahl
KUNDE_ORT	VARCHAR2(40)	 Kundenort
LEISTUNGSORT_NAME1	VARCHAR2(40)	 Leistungsortname Teil 1
LEISTUNGSORT_NAME2	VARCHAR2(40)	 Leistungsortname Teil 2
LEISTUNGSORT_STRASSE	VARCHAR2(40)	 Leistungsortstraße
LEISTUNGSORT_HANR	VARCHAR2(40)	 Leistungsorthausnummer
LEISTUNGSORT_PLZ	VARCHAR2(40)	 Leistungsortpostleitzahl
LEISTUNGSORT_ORT	VARCHAR2(40)	 Leistungsortort
STANDPLATZ_IDENT	VARCHAR2(40)	 Standplatz Beschreibung
STANDPLATZ_STRASSE	VARCHAR2(40)	 Standplatzstraße
STANDPLATZ_HANR	VARCHAR2(40)	 Standplatzhausnummer
STANDPLATZ_PLZ	VARCHAR2(40)	 Standplatzpostleitzahl
STANDPLATZ_ORT	VARCHAR2(40)	 Standplatzort
DAUER_GEPLANT	NUMBER	geschätzte Dauer in Minuten. Wert=0, wenn noch nicht geschätzt.
SOLLDATUM	DATE	Das Datum zu dem der Kunde die Leistung ursprünglich haben wollte
PLANDATUM	DATE	 Wegen Kapazitätsüberlastung be-reits beim Anlegen des Auftrages vom Solldatum abweichendes Plandatum
LEISTUNGSZEIT_VON	VARCHAR(5)	Enthalten  von- und bis-Wert die gleiche Zeit, dann gilt exakt dieser Zeitpunkt.
Ist nur der von-Wert gefüllt, dann kann ab diesem Zeitpunkt der Auf-trag erfüllt werden.
Ist nur der bis-Wert gefüllt, dann muss der Auftrag bis spätestens zu diesem Zeitpunkt erfüllt werden.
Sind von- und bis-Wert gefüllt und unterschiedlich, dann muss der Auftrag innerhalb dieses Zeitfens-ters erledigt werden.
LEISTUNGSZEIT_BIS	VARCHAR(5)	
LEISTUNGSZEIT	VARCHAR2(15)	 enthält in Textform die Angabe eines möglichen Zeitpunktes oder Zeitfenster. Folgende Muster sind möglich:
14:00 bis 20:00
15:00
ab 16:00
bis 20:00
NOTIZ	VARCHAR2(2000)	 Notiz zum Auftrag
BES_HINWEISE	VARCHAR2(254)	besondere Hinweise aus dem Nachweiswesen
ERZEUGER_NR	VARCHAR2(20)	 Erzeugernummer
ERZEUGER_NAME1	VARCHAR2(40)	 Erzeugername Teil 1
ERZEUGER_NAME2	VARCHAR2(40)	 Erzeugername Teil 2
ERZEUGER_STRASSE	VARCHAR2(40)	 Erzeugerstraße
ERZEUGER_HANR	VARCHAR2(40)	 Erzeugerhausnummer
ERZEUGER_PLZ	VARCHAR2(40)	 Erzeugerpostleitzahl
ERZEUGER_ORT	VARCHAR2(40)	 Erzeugerort
DATUM_UEBERGABE	DATE	 Datum der Übergabe des Abfalls
ENTSORGER_NR	VARCHAR2(20)	 Entsorgernummer
ENTSORGER_NAME1	VARCHAR2(40)	 Entsorgername Teil 1
ENTSORGER_NAME2	VARCHAR2(40)	 Entsorgername Teil 2
ENTSORGER_STRASSE	VARCHAR2(40)	 Entsorgerstraße
ENTSORGER_HANR	VARCHAR2(40)	 Entsorgerhausnummer
ENTSORGER_PLZ	VARCHAR2(40)	 Entsorgerpostleitzahl
ENTSORGER_ORT	VARCHAR2(40)	 Entsorgerort
DATUM_ANNAHME	DATE	 Datum an dem der Entsorger den Abfall angenommen hat
BEFOERDERER_NR	VARCHAR2(20)	 Beförderernummer
BEFOERDERER_NAME1	VARCHAR2(40)	 Beförderername Teil 1
BEFOERDERER_NAME2	VARCHAR2(40)	 Beförderername Teil 2
BEFOERDERER_STRASSE	VARCHAR2(40)	 Befördererstraße
BEFOERDERER_HANR	VARCHAR2(40)	 Befördererhausnummer
BEFOERDERER_PLZ	VARCHAR2(40)	  Befördererpostleitzahl
BEFOERDERER_ORT	VARCHAR2(40)	 Befördererort
DATUM_UEBERNAHME	DATE	 Datum der Übernahme durch den Beförderer
ENTSORGUNGSNACHWEIS_NR	VARCHAR2(20)	 Nummer des Entsorgungsnachwei-ses
ABFALLSCHLUESSEL	CHAR(10)	 ACHTUNG!!! 
Im BMS wurden die Abfallartikel Zusätzlich in den Abfallschlüssel-stammdaten hinterlegt. Auf den Tablets wird nur der Abfall ange-zeigt und nicht der spezifischere Abfallartikel. Da für die Fahler aber der Abfallartikel wesentlich aussa-gekräftiger ist, werden bei der Übergabe einfach beide Werte getauscht, so dass der Abfallartikel als Abfall interpretiert wird. 
Hier wird also die Artikelnummer des Abfalls übergeben und nicht die AVV-nummer 
ABFALLBEZEICHNUNG	VARCHAR2(254)	 ACHTUNG!!!:
Hier wird entsprechend der Bemer-kung zum ABFALLSCHLUESSEL nicht die Abfallbezeichnung sondern die Bezeichnung des Abfallartikels übergeben.
ABFALLARTIKEL_NR	VARCHAR2(20)	 ACHTUNG!!!
Hier wird stattdessen die Abfall-nummer übergeben (siehe Bemer-kung zu ABFALLSCHLUESSEL)
ABFALLARTIKEL_BEZ	VARCHAR2(254)	  ACHTUNG!!!
Hier wird stattdessen die Abfallbe-zeichnung übergeben (siehe Be-merkung zu ABFALLSCHLUESSEL)
SAMMELBEGLEITSCHEIN_NR	VARCHAR2(40)	 Nummer eines eventuell zugeord-neten Sammelbegleitscheins
HINWEIS_NACHWEISWESEN	VARCHAR2(254)	mögliche Textnotiz
KOLONNE	Kolonnenbezeichnung BMS	Hier wird ggf. die Bezeichnung einer Kolonne aus dem BMS mitgegeben. Wenn es die entsprechende Kolon-ne im BMS gibt, dann soll der Auf-trag dieser Kolonne bereits zuge-ordnet werden. Ist der Wert leer oder die entsprechende Kolonne gibt es im BMS nicht, dann verbleibt der Auftrag ohne eine Kolonnenzu-ordnung im BMS.
EINZELPOSITIONEN	VARCHAR(500)	Für Einzelpositionen mit einem Ar-tikel wird folgender Text hinzuge-fügt: <Artikelnummer>  <Artikelbe-zeichnung>  <Menge> <Mengen-einheit>#~#
Für reine Textpositionen wird fol-gender Text hinzugefügt: <Textpo-sition>#~#

Als Trennzeichen für einzelne Zei-len wird also die Zeichenfolge #~# verwendet
VERTRAGSBEZUG	True oder False	True, wenn der Auftrag aus einer Vertragsposition stammt und somit eine feste Vereinbarung mit dem Kunden besteht. In diesem Fall darf kein anderer Behältertyp verwen-det werden, als der im Auftrag vor-gegebene 
UNTERSCHRIFT	True oder False	True, wenn für diesen Auftrag eine Unterschrift durch den Kunden auf dem Tablet zwingend für den Ab-schluss erforderlich ist

3.1.1	Besondere Exportlogik für einzelne Felder
3.1.1.1	LEISTUNGSZEIT
Im AWI gibt es bei der Auftragsannahme 3 Felder in welche Uhrzeiten eigetragen werden kön-nen „Sollzeit - von „ (ZEIT_VON) , „Sollzeit – bis“ (LZEIT_BIS) und „Planzeit“ (ZEIT).
Die Angabe im Feld LEISTUNGSZEIT basiert auf folgender Logik:

ZEIT_VON und ZEIT_BIS gefüllt und ZEIT_VON <> ZEIT_BIS  „ZEIT_VON bis ZEIT_BIS“
ZEIT_VON und ZEIT_BIS gefüllt und ZEIT_VON = ZEIT_BIS  „ZEIT_VON“
nur ZEIT_VON gefüllt  „ab ZEIT_VON“
nur ZEIT_BIS gefüllt  „bis ZEIT_BIS“
sonst „ZEIT“







3.1.1.2	Tausch Artikel und Abfall
Wie bereits in der Tabelle bei den Bemerkungen angegeben wurden im Projektverlauf beim Ex-port die Angaben zu Abfall und Artikel getauscht. Damit sollte eine zu programmierende Anpas-sung auf Seiten des BMS umgangen werden und trotzdem der Artikel im Auftrag auf dem Tablet angezeigt werden. Da dort nur der Abfall angezeigt wird, wurden die Artikel einfach als Abfall im BMS hinterlegt und anstatt des Abfalls aus dem AWI wird an dieser Stelle im Export der Artikel übergeben.


3.1.1.3	KOLONNE
Den Aufträgen im AWI ist eine Standardtour mit einer entsprechenden Nummer zugeordnet. Beim Export erfolgt ein Mapping von der Standardtournummer zu der Kolonnenbezeichnung im BMS. Zwischen beiden besteht eine 1:1-Beziehung.

3.1.1.4	UNTERSCHRIFT
Im AWI gibt es kein extra Feld für ein Kennzeichen, dass eine Unterschrift durch den Kunden erforderlich ist. Daher wird im Notizfeld am Auftrag unter anderem der Text Unterschrift einge-fügt. Dieses Notizfeld wird beim Export ausgewertet. Dazu werden alle Zeichen in Großbuchsta-ben umgewandelt und geprüft, ob der Teilstring UTERSCHR vorkommt. Ist dies der Fall, dann bekommt das Feld UNTERSCHRIFT im Export den Wert True sonst False.

3.1.1.1	NOTIZ
Das Feld NOTIZ enthält zunächst den gesamten Inhalt der Notiz aus dem Auftrag im AWI. Die-ser Inhalt wird unter bestimmten Bedingungen um folgende Angaben ergänzt, da diese Für die Reporterstellung im BMS benötigt werden: 

enthalten das Feld Bestellnummer oder das Feld Besteller im AWI den Teilstring „IS“
oder ist die Kundennummer = D000113840
oder enthalten das Feld Bestellnummer oder das Feld Besteller im AWI den Teilstring „RKT“
oder ist die Kundennummer = D000125669
dann wird  die Notiz um den Eintrag “ AUFTRAG ZU KVV ” ergänzt. 
Die hier aufgeführten Merkmale sind Kennzeichen dafür, dass es sich um bestimmte Interseroh-Verträge handelt, bei denen eine Prüfung durch Abrechnung durch KVV erfolgen muss. Bei die-sen Aufträgen muss beim Ausdruck aus dem BMS 

Ist das Feld Bestellnummer im Auftrag im AWI gefüllt, dann wird die Notiz um den Eintrag “ Be-stellnummer: <BESTELLNUMMER> “ ergänzt

Ist der Preis für den Abfallartikel im AWI negative, dann wird die Notiz um den Eintrag “ VERGUETUNG “ ergänzt.

Ist die Entsorgungsnachweisnummer im Auftrag im AWI gefüllt, dann wird die Notiz um den Ein-trag “ Entsorgungsnachweis: <ENTSORGUNGSNACHWEIS_NR> ergänzt. Für die Entsor-gungsnachweisnummer existiert im Export zwar ein separates Feld, dieses wird durch BMS beim Import aber nicht berücksichtigt, da es kein entsprechendes Datenfeld im BMS gibt. Damit die Entsorgungsnachweisnummer auch auf den Tablets angezeigt wird, wird die Notiz entspre-chend ergänzt.

3.2	Aufbau EXP_AWI_AUFTRAG_<pkey Auf-trag>_BEHAELTER_<Zeitstempel>

Die Exportdatei ist eine CSV-Datei mit Kopfzeile. Als Trennzeichen wird das Semikolon verwen-det. Die Datei wird im ANSI-Zeichensatz bereitgestellt.
Die Folgende Tabelle zeigt die Spaltennamen, den zugrundeliegenden Datentyp und ggf. eine kurze Beschreibung des Inhaltes.


Spaltenname csv-Datei	Datentyp in Datenbank	Bemerkungen
AA_PKEY	NUMBER	Mit dem Wert der Spalte AA_PKEY aus den Kopfdaten wird die Verbindung zu den Behäl-terdaten hergestellt
ANZAHL	NUMBER	aktuell ist es möglich, dass hier eine Anzahl>1 steht. Dann müssen im BMS entsprechend viele Leistungsobjekte (Container) am Vertrag hinter-legt werden 
BEWEGUNGSART	VARCHAR2(3)	S – für Stellen             wird Aufstellen im BMS
H – für Holen             wird Einzug im BMS
T – für Tauschen       wird Wechsel im BMS
U – Umleeren           wird Wechsel im BMS
F – Sofortbefüllung  wird Leerung im BMS 
BEHAELTER	VARCHAR2(40)	 Bezeichnung des Behältertyps (z.B. Presscon-tainer 16 cbm)
BEHAELTER_TYPNR	VARCHAR2(20)	Kurzbezeichnung des Behältertyps (z.B. PC 16)
IDENTNUMMER_AWI	NUMBER	aktuell sind Container im AWI nicht als Inventar-behälter gekennzeichnet. Somit ist dieses Feld derzeit nicht gefüllt
TRANSPONDER	VARCHAR2(40)	 aktuell sind Container im AWI nicht als Inven-tarbehälter gekennzeichnet. Somit ist dieses Feld derzeit nicht gefüllt
KUNDENEIGEN	True oder False	 True, wenn es ein Kundeneigener Behälter ist, bei dem kein Barcode zum Scannen vorhanden ist. Sonst False.

 
3.3	Aufbau EXP_AWI_AUFTRAG_<pkey Auf-trag>_POSITIONEN_<Zeitstempel>

Die Exportdatei ist eine CSV-Datei mit Kopfzeile. Als Trennzeichen wird das Semikolon verwen-det. Die Datei wird im ANSI-Zeichensatz bereitgestellt.
Die Folgende Tabelle zeigt die Spaltennamen, den zugrundeliegenden Datentyp und ggf. eine kurze Beschreibung des Inhaltes.


Spaltenname csv-Datei	Datentyp in Datenbank	Bemerkungen
AL_PKEY	NUMBER	Mit dem Wert der Spalte AL_PKEY aus den Kopfdaten wird die Verbindung zu den Positi-onsdaten hergestellt
REIHENFOLGE	NUMBER	Reihenfolge der Positionen (Sortiert nach Rei-henfolge aufsteigend)
ARTIKEL_NR	VARCHAR2(20)	Artikelnummer
ARTIKEL_BEZ	VARCHAR2(254)	Artikelbezeichnung
MENGE	NUMBER(20,5)	Menge
MENGENEINHEIT	VARCHAR2(40)	Mengeneinheit
EINZELPREIS	NUMBER(20,5)	Preis für eine Mengeneinheit (Netto)
POSART	P, T oder A	P steht für Positionen mit Artikel, Preis, etc, T und A sind reine Textpositionen (A = Belegüber-greifen, T = geht nicht in Rechnungsbeleg)
TEXT_POSART_A	VARCHAR2(2000)	Enthält den Text bei POSART A
TEXT_POSART_T	VARCHAR2(2000)	Enthält den Text bei POSART T


<img width="807" height="514" alt="image" src="https://github.com/user-attachments/assets/af02a65d-af86-4d56-9f6c-a551bacd0a25" />

<img width="644" height="870" alt="image" src="https://github.com/user-attachments/assets/e3898f94-0cf8-40b1-96f9-7fb4718a0cd9" />


<img width="629" height="866" alt="image" src="https://github.com/user-attachments/assets/af506159-cfd3-4b54-ab8f-c5e6efd051fa" />

<img width="646" height="911" alt="image" src="https://github.com/user-attachments/assets/d71b108a-eb1d-4c96-9db9-0e8a091b45a7" />

<img width="658" height="407" alt="image" src="https://github.com/user-attachments/assets/4be40490-be40-431d-a22a-b3c110fa4b74" />

<img width="634" height="897" alt="image" src="https://github.com/user-attachments/assets/11848e51-0bdf-458b-8035-c467ec9cdb7c" />





