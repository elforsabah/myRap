anbei schon ein Ausschnitt zum Swagger vom BMS-Endpunkt für die Behälter-Lager-Daten.

Der Aufruf erfolgt also über GET […]/api/container/depot-data-for-deadline?deadline=2026-07-22

Als Datum macht „heute“ immer Sinn. Eine Beispiel-Response ist ebenfalls im Anhang.

 

Was jetzt also für #41365 zu überlegen und einzuschätzen ist:

Abruf der Daten in der App über den Endpunkt oben.
Beim Öffnen der App + Aktualisieren-Button? Oder hast du eine bessere Idee?
Macht es Sinn die jeweils letzten abgerufenen Daten vom BMS im SAP zu persistieren? Oder lieber immer nur „live“ abfragen und anzeigen? Was ist technisch einfacher umzusetzen?
 
Der Projektleiter möchte dafür von uns nochmal sicherheitshalber den Lösungsweg mit grober Aufwandsschätzung schriftlich haben.
