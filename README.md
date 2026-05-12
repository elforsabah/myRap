Was wird gebaut – und warum wie lange?
1. Datenlogik im Hintergrund (~7–8 Tage)
Die App muss Daten aus verschiedenen SAP-Tabellen zusammensammeln, filtern und aufbereiten. Konkret: Welche Behälter sind auf Lager? Wo liegen sie? Sind sie echte Behälter oder nur Dummies im System? Wie hoch ist der Sicherheitsbestand? Das ist der Kern der Arbeit – hier steckt am meisten drin.
2. Eine kleine Hilfstabelle (~1 Tag)
Nicht jeder Lagerort im System ist auch ein Behälterlager. Damit die App nur die relevanten Lagerorte anzeigt, brauchen wir eine kleine Tabelle, in der man das pflegen kann – ähnlich wie ein Schalter: "Ja, dieser Lagerort soll in der App erscheinen."
3. Die Benutzeroberfläche (~3–4 Tage)
Die eigentliche App, die der Disponent sieht: eine Liste mit Filterleiste. Weil wir uns für eine einfache Listenansicht entschieden haben (statt einer Kreuztabelle), kann hier auf SAP-Standardkomponenten zurückgegriffen werden – das spart Zeit und ist stabiler.


Gesamt: ca. 11–14 Tage – also ungefähr 2,5 bis 3 Wochen für einen Entwickler.
