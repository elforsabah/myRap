Inhaltsverzeichnis
1	Ausgangssituation	4
2	Zielstellung	4
3	Aufbau des Datenexportes von opti.AWI	4
3.1	Aufbau EXP_AWI_AUFTRAG_<pkey Auftrag>_KOPFDATEN_<Zeitstempel>	5
3.1.1	Besondere Exportlogik für einzelne Felder	8
3.1.1.1	LEISTUNGSZEIT	8
3.1.1.2	Tausch Artikel und Abfall	9
3.1.1.3	KOLONNE	9
3.1.1.4	UNTERSCHRIFT	9
3.1.1.1	NOTIZ	9
3.2	Aufbau EXP_AWI_AUFTRAG_<pkey Auftrag>_BEHAELTER_<Zeitstempel>	10
3.3	Aufbau EXP_AWI_AUFTRAG_<pkey Auftrag>_POSITIONEN_<Zeitstempel>	11




 
1	Ausgangssituation
Die HWS plant die Erweiterung der BMS-Software um das Modul Containerdienst. In diesem Modul soll die komplette Auftragsdurchführung der Aufträge aus dem Bereich Containerdienst erfolgen. Da die Auftragserstellung und die Auftragsabrechnung aber weiterhin in der Software opti.AWI stattfinden, müssen die Auftragsinformationen von opti.AWI an das BMS übergeben werden. Nach der Bearbeitung im BMS müssen alle abrechnungsrelevanten Informationen zurück in opti.AWI. 
2	Zielstellung
Die Übergabe der Auftragsdaten von opti.AWI an das BMS erfolgt über einen automatischen Export der Daten aus opti.AWI beim Statuswechsel eines Auftrages auf GEDRUCKT und dem zeitlich gesteuerten Import in das BMS. 
Für die Rückführung der abrechnungsrelevanten Daten in opti.AWI gibt ist keine automatische Schnittstelle. Hier muss das BMS geeignete Übersichten bereitstellen, die neben den Abrechnungsinformationen in jedem Fall die Auftragsnummer aus opti.AWI enthält, welche beim Export an das BMS übergeben wurde. Das Einpflegen der Daten in opti.AWI geschieht dann manuell.

3	Aufbau des Datenexportes von opti.AWI
Die Arbeitsaufträge in opti.AWI setzen sich aus den Kopfdaten (Kunde, Leistungsort, Erzeuger, Beförderer, …), ein oder mehreren Behälterpositionen (Behältertyp, Bewegungsart, Anzahl,…) und ein oder mehreren Einzelpositionen (Artikel, Menge, Preis,…. oder Textposition) zusammen.

Aus opti.AWI werden daher pro Arbeitsauftrag 3 Dateien generiert:
EXP_AWI_AUFTRAG_<pkey Auftrag>_KOPFDATEN_<Zeitstempel>
EXP_AWI_AUFTRAG_<pkey Auftrag>_BEHAELTER_<Zeitstempel>
EXP_AWI_AUFTRAG_<pkey Auftrag>_POSITIONEN_<Zeitstempel>

Die Daten der Einzelpositionen werden aktuell zusätzlich in einem Textfeld in der Kopfdatendatei übergeben (da ursprünglich keine Einzelpositionsdatei übergeben werden sollte).

 
3.1	Aufbau EXP_AWI_AUFTRAG_<pkey Auftrag>_KOPFDATEN_<Zeitstempel>
Die Exportdatei ist eine CSV-Datei mit Kopfzeile. Als Trennzeichen wird das Semikolon verwendet. Die Datei wird im ANSI-Zeichensatz bereitgestellt.
Die Folgende Tabelle zeigt die Spaltennamen, den zugrundeliegenden Datentyp und ggf. eine kurze Beschreibung des Inhaltes.

Spaltenname csv-Datei	Datentyp in Datenbank	Bemerkungen
AA_PKEY	NUMBER	Primär Schlüssel Datenbanktabelle AA (Arbeitsauftrag)
AL_PKEY	NUMBER	Primärschlüssel Datenbanktabelle AL (Lieferschein) -> Fremdschlüssel für Auftragspositionen
STATUS	VARCHAR2(40)	Übergeben wird Status GEDRUCKT oder STORNIERT
AA_NR	VARCHAR2(20)	Nummer Arbeitsauftrag zum Suchen im AWI-Client
KUNDENNR	VARCHAR2(20)	 Kundennummer
SCHEIN_NR	VARCHAR2(40)	 Scheinnummer (die Art wird über Scheinart bestimmt)
SCHEINART	VARCHAR2(3)	LS = Lieferschein, ÜS = Übernehmeschen, BS = Begleitschein
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
PLANDATUM	DATE	 Wegen Kapazitätsüberlastung bereits beim Anlegen des Auftrages vom Solldatum abweichendes Plandatum
LEISTUNGSZEIT_VON	VARCHAR(5)	Enthalten  von- und bis-Wert die gleiche Zeit, dann gilt exakt dieser Zeitpunkt.
Ist nur der von-Wert gefüllt, dann kann ab diesem Zeitpunkt der Auftrag erfüllt werden.
Ist nur der bis-Wert gefüllt, dann muss der Auftrag bis spätestens zu diesem Zeitpunkt erfüllt werden.
Sind von- und bis-Wert gefüllt und unterschiedlich, dann muss der Auftrag innerhalb dieses Zeitfensters erledigt werden.
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
ENTSORGUNGSNACHWEIS_NR	VARCHAR2(20)	 Nummer des Entsorgungsnachweises
ABFALLSCHLUESSEL	CHAR(10)	 ACHTUNG!!! 
Im BMS wurden die Abfallartikel Zusätzlich in den Abfallschlüsselstammdaten hinterlegt. Auf den Tablets wird nur der Abfall angezeigt und nicht der spezifischere Abfallartikel. Da für die Fahler aber der Abfallartikel wesentlich aussagekräftiger ist, werden bei der Übergabe einfach beide Werte getauscht, so dass der Abfallartikel als Abfall interpretiert wird. 
Hier wird also die Artikelnummer des Abfalls übergeben und nicht die AVV-nummer 
ABFALLBEZEICHNUNG	VARCHAR2(254)	 ACHTUNG!!!:
Hier wird entsprechend der Bemerkung zum ABFALLSCHLUESSEL nicht die Abfallbezeichnung sondern die Bezeichnung des Abfallartikels übergeben.
ABFALLARTIKEL_NR	VARCHAR2(20)	 ACHTUNG!!!
Hier wird stattdessen die Abfallnummer übergeben (siehe Bemerkung zu ABFALLSCHLUESSEL)
ABFALLARTIKEL_BEZ	VARCHAR2(254)	  ACHTUNG!!!
Hier wird stattdessen die Abfallbezeichnung übergeben (siehe Bemerkung zu ABFALLSCHLUESSEL)
SAMMELBEGLEITSCHEIN_NR	VARCHAR2(40)	 Nummer eines eventuell zugeordneten Sammelbegleitscheins
HINWEIS_NACHWEISWESEN	VARCHAR2(254)	mögliche Textnotiz
KOLONNE	Kolonnenbezeichnung BMS	Hier wird ggf. die Bezeichnung einer Kolonne aus dem BMS mitgegeben. Wenn es die entsprechende Kolonne im BMS gibt, dann soll der Auftrag dieser Kolonne bereits zugeordnet werden. Ist der Wert leer oder die entsprechende Kolonne gibt es im BMS nicht, dann verbleibt der Auftrag ohne eine Kolonnenzuordnung im BMS.
EINZELPOSITIONEN	VARCHAR(500)	Für Einzelpositionen mit einem Artikel wird folgender Text hinzugefügt: <Artikelnummer>  <Artikelbezeichnung>  <Menge> <Mengeneinheit>#~#
Für reine Textpositionen wird folgender Text hinzugefügt: <Textposition>#~#

Als Trennzeichen für einzelne Zeilen wird also die Zeichenfolge #~# verwendet
VERTRAGSBEZUG	True oder False	True, wenn der Auftrag aus einer Vertragsposition stammt und somit eine feste Vereinbarung mit dem Kunden besteht. In diesem Fall darf kein anderer Behältertyp verwendet werden, als der im Auftrag vorgegebene 
UNTERSCHRIFT	True oder False	True, wenn für diesen Auftrag eine Unterschrift durch den Kunden auf dem Tablet zwingend für den Abschluss erforderlich ist

3.1.1	Besondere Exportlogik für einzelne Felder
3.1.1.1	LEISTUNGSZEIT
Im AWI gibt es bei der Auftragsannahme 3 Felder in welche Uhrzeiten eigetragen werden können „Sollzeit - von „ (ZEIT_VON) , „Sollzeit – bis“ (LZEIT_BIS) und „Planzeit“ (ZEIT).
Die Angabe im Feld LEISTUNGSZEIT basiert auf folgender Logik:

ZEIT_VON und ZEIT_BIS gefüllt und ZEIT_VON <> ZEIT_BIS  „ZEIT_VON bis ZEIT_BIS“
ZEIT_VON und ZEIT_BIS gefüllt und ZEIT_VON = ZEIT_BIS  „ZEIT_VON“
nur ZEIT_VON gefüllt  „ab ZEIT_VON“
nur ZEIT_BIS gefüllt  „bis ZEIT_BIS“
sonst „ZEIT“







3.1.1.2	Tausch Artikel und Abfall
Wie bereits in der Tabelle bei den Bemerkungen angegeben wurden im Projektverlauf beim Export die Angaben zu Abfall und Artikel getauscht. Damit sollte eine zu programmierende Anpassung auf Seiten des BMS umgangen werden und trotzdem der Artikel im Auftrag auf dem Tablet angezeigt werden. Da dort nur der Abfall angezeigt wird, wurden die Artikel einfach als Abfall im BMS hinterlegt und anstatt des Abfalls aus dem AWI wird an dieser Stelle im Export der Artikel übergeben.


3.1.1.3	KOLONNE
Den Aufträgen im AWI ist eine Standardtour mit einer entsprechenden Nummer zugeordnet. Beim Export erfolgt ein Mapping von der Standardtournummer zu der Kolonnenbezeichnung im BMS. Zwischen beiden besteht eine 1:1-Beziehung.

3.1.1.4	UNTERSCHRIFT
Im AWI gibt es kein extra Feld für ein Kennzeichen, dass eine Unterschrift durch den Kunden erforderlich ist. Daher wird im Notizfeld am Auftrag unter anderem der Text Unterschrift eingefügt. Dieses Notizfeld wird beim Export ausgewertet. Dazu werden alle Zeichen in Großbuchstaben umgewandelt und geprüft, ob der Teilstring UTERSCHR vorkommt. Ist dies der Fall, dann bekommt das Feld UNTERSCHRIFT im Export den Wert True sonst False.

3.1.1.1	NOTIZ
Das Feld NOTIZ enthält zunächst den gesamten Inhalt der Notiz aus dem Auftrag im AWI. Dieser Inhalt wird unter bestimmten Bedingungen um folgende Angaben ergänzt, da diese Für die Reporterstellung im BMS benötigt werden: 

enthalten das Feld Bestellnummer oder das Feld Besteller im AWI den Teilstring „IS“
oder ist die Kundennummer = D000113840
oder enthalten das Feld Bestellnummer oder das Feld Besteller im AWI den Teilstring „RKT“
oder ist die Kundennummer = D000125669
dann wird  die Notiz um den Eintrag “ AUFTRAG ZU KVV ” ergänzt. 
Die hier aufgeführten Merkmale sind Kennzeichen dafür, dass es sich um bestimmte Interseroh-Verträge handelt, bei denen eine Prüfung durch Abrechnung durch KVV erfolgen muss. Bei diesen Aufträgen muss beim Ausdruck aus dem BMS 

Ist das Feld Bestellnummer im Auftrag im AWI gefüllt, dann wird die Notiz um den Eintrag “ Bestellnummer: <BESTELLNUMMER> “ ergänzt

Ist der Preis für den Abfallartikel im AWI negative, dann wird die Notiz um den Eintrag “ VERGUETUNG “ ergänzt.

Ist die Entsorgungsnachweisnummer im Auftrag im AWI gefüllt, dann wird die Notiz um den Eintrag “ Entsorgungsnachweis: <ENTSORGUNGSNACHWEIS_NR> ergänzt. Für die Entsorgungsnachweisnummer existiert im Export zwar ein separates Feld, dieses wird durch BMS beim Import aber nicht berücksichtigt, da es kein entsprechendes Datenfeld im BMS gibt. Damit die Entsorgungsnachweisnummer auch auf den Tablets angezeigt wird, wird die Notiz entsprechend ergänzt.

3.2	Aufbau EXP_AWI_AUFTRAG_<pkey Auftrag>_BEHAELTER_<Zeitstempel>

Die Exportdatei ist eine CSV-Datei mit Kopfzeile. Als Trennzeichen wird das Semikolon verwendet. Die Datei wird im ANSI-Zeichensatz bereitgestellt.
Die Folgende Tabelle zeigt die Spaltennamen, den zugrundeliegenden Datentyp und ggf. eine kurze Beschreibung des Inhaltes.


Spaltenname csv-Datei	Datentyp in Datenbank	Bemerkungen
AA_PKEY	NUMBER	Mit dem Wert der Spalte AA_PKEY aus den Kopfdaten wird die Verbindung zu den Behälterdaten hergestellt
ANZAHL	NUMBER	aktuell ist es möglich, dass hier eine Anzahl>1 steht. Dann müssen im BMS entsprechend viele Leistungsobjekte (Container) am Vertrag hinterlegt werden 
BEWEGUNGSART	VARCHAR2(3)	S – für Stellen             wird Aufstellen im BMS
H – für Holen             wird Einzug im BMS
T – für Tauschen       wird Wechsel im BMS
U – Umleeren           wird Wechsel im BMS
F – Sofortbefüllung  wird Leerung im BMS 
BEHAELTER	VARCHAR2(40)	 Bezeichnung des Behältertyps (z.B. Presscontainer 16 cbm)
BEHAELTER_TYPNR	VARCHAR2(20)	Kurzbezeichnung des Behältertyps (z.B. PC 16)
IDENTNUMMER_AWI	NUMBER	aktuell sind Container im AWI nicht als Inventarbehälter gekennzeichnet. Somit ist dieses Feld derzeit nicht gefüllt
TRANSPONDER	VARCHAR2(40)	 aktuell sind Container im AWI nicht als Inventarbehälter gekennzeichnet. Somit ist dieses Feld derzeit nicht gefüllt
KUNDENEIGEN	True oder False	 True, wenn es ein Kundeneigener Behälter ist, bei dem kein Barcode zum Scannen vorhanden ist. Sonst False.

 
3.3	Aufbau EXP_AWI_AUFTRAG_<pkey Auftrag>_POSITIONEN_<Zeitstempel>

Die Exportdatei ist eine CSV-Datei mit Kopfzeile. Als Trennzeichen wird das Semikolon verwendet. Die Datei wird im ANSI-Zeichensatz bereitgestellt.
Die Folgende Tabelle zeigt die Spaltennamen, den zugrundeliegenden Datentyp und ggf. eine kurze Beschreibung des Inhaltes.


Spaltenname csv-Datei	Datentyp in Datenbank	Bemerkungen
AL_PKEY	NUMBER	Mit dem Wert der Spalte AL_PKEY aus den Kopfdaten wird die Verbindung zu den Positionsdaten hergestellt
REIHENFOLGE	NUMBER	Reihenfolge der Positionen (Sortiert nach Reihenfolge aufsteigend)
ARTIKEL_NR	VARCHAR2(20)	Artikelnummer
ARTIKEL_BEZ	VARCHAR2(254)	Artikelbezeichnung
MENGE	NUMBER(20,5)	Menge
MENGENEINHEIT	VARCHAR2(40)	Mengeneinheit
EINZELPREIS	NUMBER(20,5)	Preis für eine Mengeneinheit (Netto)
POSART	P, T oder A	P steht für Positionen mit Artikel, Preis, etc, T und A sind reine Textpositionen (A = Belegübergreifen, T = geht nicht in Rechnungsbeleg)
TEXT_POSART_A	VARCHAR2(2000)	Enthält den Text bei POSART A
TEXT_POSART_T	VARCHAR2(2000)	Enthält den Text bei POSART T

