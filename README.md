<img width="1614" height="822" alt="image" src="https://github.com/user-attachments/assets/48099e55-2b3e-47c0-8102-fc4b92cdf25d" />

Anforderungsbeschreibung:

Wir haben eine Anforderung die Z-Logging-Tabelle "ZWR_CTP_OP_HDAY" im IC anzuzeigen. Schlüssel ist die Kundennr.

" Der Endnutzer muss sehen können, wer einen Urlaubstag zu letzt geändert hat" -> wird in obiger Tabelle bereits richtig erfasst, aber eben im IC nicht angezeigt

 

Dazu bitte einen neuen Tab ähnlich wie “Quality Notice” (Referenzaufgabe hier: https://project-redmine.hal.prologa.intern/issues/39321 - Transport  S4DK902986) einrichten. Diese Ansicht soll lediglich die Logs anzeigen und erstmal keinerlei Business-Funktion haben.

Mockpup:

image_20260803130110.png
Beispieldaten
BP: S4Q/110: 0052240024
S4D/110: 1000123

 

Akzeptanzkriterien:

Es werden nur die Urlaubstage vom aktuellen Kunden angezeigt
Urlaubstage von anderen Kunden werden nicht angezeigt.
Urlaubstage sollen nach Gültigkeit sortiert sein (neueste zuerst)
