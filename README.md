Lösungsweg
Liebe Kollegin, lieber Kollege, persistieren Sie – live abfragen Sie nicht.
Sortieren/Filtern wird beim Read (Virtual Element) verhindert und beijedem OData-Request wird das BMS aufgerufen und macht die App im Ausfall unbenutzbar. Auch die Bauweise ist dereinfacher, nämlich persistenz.
Abruf:
täglicher Job mit
deadline = heute
+ Button „Daten aktualisieren“. Abfruf beim App-Start (no sauberer Einmal-Trigger, mehr als ein User, Wartezeit). App zeigt „Stand vom …“.
Technik:
SM59-Destination → Klasse
ZCL_BMS_CONTAINER_API
(Login, GET, generisches Parsing derdynamischen Depot-Keys) → Z-Tabellen (Daten je Stichtag, Lauf-Protokoll, Depot-Mapping) → CDS liestdaraus. Das bestehende RAP-Verhalten wird beibehalten.
Spalten:
Object Page –Fiori Elements can't support any dynamic Spalten for the relevant Depots + „sonstige“.
