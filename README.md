Der Sachbearbeiter öffnet den Vertrag in SAP und lädt den Lageplan per „Business Document ablegen“ als neue Dokumentenart „Logistikinformationen“ hoch. Die Datei landet direkt im d3-Archiv, was eigentlich niemand mitbekommt, weil SAP nur einen Verweis speichert. Für den Anwender fühlt sich das einfach wie ein normaler Upload am Vertrag an – vom d3 sieht er dabei nichts.

Der Hintergrundjob, der aus den Aufträgen die Services für die Dispo erstellt, schaut jetzt nicht nur wie gewohnt in die Auftragsdaten, sondern prüft auch den Vertrag. Wenn dort ein Dokument vom Typ „Logistikinformationen“ hinterlegt ist, holt er es aus dem d3 und hängt es automatisch an den erzeugten Service. Der Disponent kann es dann öffnen oder ausdrucken – so einfach ist das.

Die eigene Dokumentenart macht den Unterschied. Am Vertrag kann alles Mögliche hochgeladen werden: Angebote, Schriftwechsel, Fotos – aber nur „Logistikinformationen“ werden Richtung Dispo weitergereicht. Das System lässt hier ausschließlich PDFs zu, Fotos müssen vorher umgewandelt werden.

Dokumente legt man immer am Vertrag ab, nicht an einzelnen Positionen. Die Ablage funktioniert nur über die Vertragsnummer. Das heißt, das Dokument gilt automatisch für alle Positionen des Vertrags. Meistens passt das, solange ein Vertrag nur einen Einsatzort hat.

Wenn ein Vertrag mehrere Sammelstellen umfasst, gibt’s eine praktische Lösung: Der Sachbearbeiter nennt im Dokumentnamen die Position, zum Beispiel „Pos 10 – Lageplan Hofeinfahrt“. Das Programm erkennt das und schickt das Dokument nur an die Services dieser Position. Ohne Positionsangabe im Namen läuft alles wie bisher – das Dokument geht an alle Services des Vertrags.


Umsetzung
Nr	Arbeitspaket	Dauer
1	Ablage einrichten — Dokumentenart anlegen, mit Vertrag und d3 verbinden, Berechtigungen vergeben. Danach kann der Fachbereich bereits Lagepläne hinterlegen.	1,0 T
2	Analyse — feststellen, über welchen Weg ein Auftrag an der Vertragsposition und damit am Vertrag hängt.	1,5 T
3	Automatik bauen — Dokumente erkennen, aus dem d3 abrufen, Mehrfachversand verhindern, Ausfälle abfangen.	2,0 T
4	Übergabe an die Dispo — Anbindung an die Anhang-Schnittstelle von P&D.	1,0–2,0 T
5	Nachtragsreport — für Lagepläne, die erst hinterlegt werden, wenn die Aufträge schon laufen.	0,5 T
6	Test — inklusive Wiederholungslauf, nicht erreichbarem d3 und fehlender Berechtigung des Jobbenutzers.	1,5 T
7	Transport und Produktivsetzung	0,5 T
Summe	8,0–9,0 T
Risikozuschlag 20 %	2,0–3,0 T
Gesamt	10–12 PT
Zusätzlich +0,5 T, wenn die Regel für Verträge mit mehreren Sammelstellen umgesetzt werden soll.

Kalendarisch sind 3–4 Wochen einzuplanen. Nach der ersten Woche ist Akzeptanzkriterium 1 erfüllt, in Woche drei Akzeptanzkriterium 2. Paket 4 hängt von der Zuarbeit von PROLOGA ab — die Anfrage sollte am ersten Tag herausgehen.

Nicht enthalten sind der Aufwand bei PROLOGA für die Schnittstelle in P&D, die BMS-Anbindung zum Fahrer und der Löschreport für veraltete Dokumente. Alle drei sind in der Anforderung ausdrücklich ausgeklammert.
