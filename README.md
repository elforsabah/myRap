1. Ziel / Kontext

Report / Fiori – Auswertung:  Behälter-Lagerbestände

Ziel insgesamt: Auflistung der aktuellen IST-Bestände (Anzahl) je Behältertyp und Lagerort
Anwender: Disponenten (P&D-Nutzer)
Report soll über Fiori Launchpad geöffnet werden können
Bonus (wenn möglich/geringer Aufwand): auch direkt aus Planungscockpit (in neuem Tab)
 

Tech. Anmerkung / nice to know: Transaktion ZWR_CONTAINER_OVW (bzw. Program ZWR_WATP_CONTAINER_OVERVIEW) listet alle Behälter einzeln auf
zeigt Status (L = Behälter ist auf Lager / K = Behälter steht bei Kunde)
zeigt Lagerort für jeden Behälter
Hier allerdings alle Behälter einzeln. Daher separate App für übersichtliche Anzahl je Lagerort gewünscht.
 

2. Beschreibung / Anforderungen

Ausgabe:

Zeilen: Behältertypen (Gerätetyp Tabelle IEDEVICECATEGORY bzw. Materialnr, siehe auch Transaktion ZWR_CONTAINER_INV)
Spalten: Lagerort (Lagerortbezeichnung)
Lagerort-Spalten müssen filterbar/selektierbar sein. Es gibt viele Lagerorte im System, die nicht als Behälterlager genutzt werden.
falls einfach: neue Customizing-Tabelle, um für jeden Lagerort zu verwalten, ob dieser ein Behälterlager ist?
Gern auch andere Ideen möglich. Lass uns gern darüber sprechen.
Zusätzliche Spalte: “ohne Lagerort”
ein Behälter kann im SAP technisch gesehen im Lagerbestand verfügbar sein (vgl. Status “L” in ZWR_CONTAINER_OVW), ohne dass er einem Lagerort zugeordnet ist. Für diese Anzahl wäre eine eigene Spalte nötig. Im aktuellen Projektsystem gibt es viele solcher Behälter. Im Produktiveinsätz soll dieser Zustand eine Ausnahme sein.
Zusätzliche Spalte: Sicherheitsbestand zum Behältertyp (aus Pflege-View ZWRTCSSAFESTOCK) → Werte hier änderbar machen (z.B. über Funktionsbutton)
 

Mockup:

image_20260512151217.png
 

Dummybehälter aus Beständen abziehen:
Jedes einzelne Behälter-Equipment kann ein Dummy sein. Zu erkennen anhand: Hersteller = DUMMY  (Feld EQUI-HERST)
Beispiel im RI4/442: Equipment 30000157
image_20260507212647.png
diese Prüfung wurde schon in Transaktion ZWR_CONTAINER_INV gelöst -> siehe CL_CALC_CONTAINER_INVENTORY → gern auch Pascal dazu ansprechen. Er hat das implementiert.
Diese Dummys gehören nicht zum Behälterbestand, da diese keine echten/physischen Behälter sind. Deswegen sollen diese aus den Bestandszahlen rausgerechnet werden.
 

Komm für Fragen wie immer gern auf mich zu! :)
