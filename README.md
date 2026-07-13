 

1. Ziel / Kontext

Report / Fiori – Auswertung:  Behälter-Lagerbestände

Ziel: Auflistung der aktuellen IST-Bestände (Anzahl) je Behältertyp und Lagerort
Anwender: Disponenten (P&D-Nutzer)
Report soll über Fiori Launchpad geöffnet werden können und parallel zum Planungscockpit verwendet werden.
 

2. Datenbeschaffung

Anmerkung / nice to know: Transaktion ZWR_CONTAINER_OVW (bzw. Program ZWR_WATP_CONTAINER_OVERVIEW) listet alle Behälter einzeln auf:
zeigt Status (L = Behälter ist auf Lager / K = Behälter steht bei Kunde)
zeigt Lagerort für jeden Behälter
Hier allerdings alle Behälter einzeln. Daher separate App für übersichtliche Anzahl je Lagerort nötig.
Die Datenbeschaffung müsste also aus ZWR_CONTAINER_OVW (bzw. Program ZWR_WATP_CONTAINER_OVERVIEW) übernommen oder zumindest abgeschaut werden können. Dort fehlen nur:
Berücksichtigung Dummies
Sicherheitsbestände
→ Diese zwei Punkte hat dafür wiederum Pascal schon in einer anderen Entwicklung ("Behälterauslastungstool" → #40261) gelöst. Siehe Transaktion ZWR_CONTAINER_INV -> siehe CL_CALC_CONTAINER_INVENTORY.

 

 

3. Beschreibung / Anforderungen

Ausgabe:
Zeilen: Behältertypen
Selektion der Behältertypen muss möglich sein (keine=alle)
Spalten: Lagerorte (Lagerortbezeichnung)
Lagerort-Spalten müssen filterbar/selektierbar sein. Es gibt viele Lagerorte im System, die nicht als Behälterlager genutzt werden.
Zusätzliche Spalte: “ohne Lagerort”:
ein Behälter kann im SAP technisch gesehen im Lagerbestand verfügbar sein (vgl. Status “L” in ZWR_CONTAINER_OVW), ohne dass er einem Lagerort zugeordnet ist. Für diese Anzahl wäre eine eigene Spalte nötig. Im aktuellen Projektsystem gibt es viele solcher Behälter. Im Produktivbetrieb später soll dieser Zustand eine Ausnahme sein (z.B. fehlende Lagerbuchung oder neu beschaffte Behälter, die frisch im System angelegt sind).
Zusätzliche Spalte: Sicherheitsbestand zum Behältertyp (aus Pflege-View ZWRTCSSAFESTOCK) → Werte sollen hier änderbar sein(z.B. über Funktionsbutton)
 

Mockup:
image_20260626090325.png
 

Dummybehälter aus Beständen abziehen:
Jedes einzelne Behälter-Equipment kann ein Dummy sein. Zu erkennen anhand: Hersteller = "DUMMY"
Diese Dummys gehören nicht zum Behälterbestand, da diese keine echten/physischen Behälter sind. Deswegen sollen diese aus den Bestandszahlen herausgerechnet werden.
 

 

Komm für Fragen wie immer gern auf mich zu! :)
