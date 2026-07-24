Lösungsweg
Persistenz verwenden anstatt live zu abfragen.
Einen Abruf im Read (Virtual Element) verursacht Sortierung/Filter und erfordert für jedes OData Request den Aufruf des BMS, und macht somit die App bei Ausfall unbekanntbar. Persistenz ist auch die einfachere Realisierung.
Abruf:
Täglicher Job mit
deadline = heute
+ Button „Daten aktualisieren“. Keine Abruf beim Start der App (kein sauberes einzigartiges Event, parallele Nutzer, Wartezeit). App zeigt „Stand vom …“.
Technik:
SM59 Destination → Klasse
ZCL_BMS_CONTAINER_API
(Login, GET, generisches Parsing derdynamischen Depot Keys) → Z-Tabelle (Daten nach Tag, Prozess Protokoll, Depot mapping) → CDS liestdaraus. Das vorhandene RAP Verhalten wird beibehalten

Spalten:
feste Spalten für die relevanten Depots + „sonstige“ – vollständige Liste auf der Object Page –Fiori Elements kann keine dynamischen Spalten.
