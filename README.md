Inhalte
Kopfinformationen
Was ist passiert?
Fehleranalyse
Informationen zur Abbruchstelle
Quelltextauszug
Aktive Aufrufe/Ereignisse
Kopfinformationen
Kurztext 	Das laufende Programm hat bewusst einen Abbruch mit Kurzdump ausgelöst.
Laufzeitfehler 	RAISE_SHORTDUMP
Ausnahme 	CX_SADL_DUMP_APPL_MODEL_ERROR
Entwicklungsobjekt 	STOB /PLCE/C_PDMNLTOURWR
Anwendungskomponente 	XX-PART-PLG-ISP
Datum/Uhrzeit 	24.11.2025 15:55:26 (System)
Benutzer 	HWSB10035 (Elvis Mbah Forsab)
Mandant 	442
Host 	evhsap-srv08_RI4_01
Was ist passiert?
Das laufende Programm hat eine Fehlersituation erkannt, die es nicht
angemessen behandeln konnte.
Um bereits veranlasste Datenbankänderungen rückgängig zu machen und
Informationen für die weitere Analyse zu sichern, hat das Programm über
die Sprachelemente RAISE SHORTDUMP oder THROW SHORTDUMP bewusst einen
Abbruch mit Kurzdump ausgelöst.
???APDOCU ABAPRAISE_SHORTDUMP
Fehleranalyse
Als Grund für den Abbruch hat das Programm die Ausnahme "CX_SADL_DUMP_APPL_MODEL_ERROR" angegeben:
Processing was aborted because of an application modeling error: See previous in exception chain.
Die Ausnahme hängt mit einer vorhergehenden Ausnahme "CX_RAP_HANDLER_NOT_IMPLEMENTED" zusammen,
die im Programm "CL_RAP_BHV_PROCESSOR==========CP" auftrat, und zwar in Zeile 16 des Includes "CL_RAP_BHV_PROCESSOR==========CM01B".
Der Grund für diese Ausnahme war:
Handler nicht implementiert. Methode: GLOBAL_FEATURE, beteiligte Entitäten: /PLCE/C_PDMNLTOURWR
Informationen zur Abbruchstelle
Der Abbruch trat im ABAP-Programm bzw. Include "CL_SADL_SHORTDUMP=============CP"
auf, und zwar in "TRIGGER_APPL_MODEL_ERROR_DUMP". Das Hauptprogramm war "SAPMHTTP".
Im Quelltext befindet sich die Abbruchstelle in Zeile 2
des Includes "CL_SADL_SHORTDUMP=============CM006".
