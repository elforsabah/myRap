opfinformationen
Was ist passiert?
Fehleranalyse
Informationen zur Abbruchstelle
Quelltextauszug
Aktive Aufrufe/Ereignisse
Kopfinformationen
Kurztext 	Es wurde ein RAP-Kontrakt verletzt.
Laufzeitfehler 	BEHAVIOR_CONTRACT_VIOLATION
Programm 	ZBP_E_BP_R_PDMNLSERVICE_EXT===CP
Datum/Uhrzeit 	02.10.2025 06:10:38 (System)
Benutzer 	HWSB10035 (Elvis Mbah Forsab)
Mandant 	442
Host 	evhsap-srv08_RI4_01
Was ist passiert?
Fehler im ABAP-Anwendungsprogramm.
Das laufende ABAP-Programm "ZBP_E_BP_R_PDMNLSERVICE_EXT===CP" mußte abgebrochen werden, da es auf
eine Anweisung gestoßen ist, die leider nicht ausgeführt werden kann.
Es wurde ein RAP-Kontrakt verletzt: "CC/C:EMPTY_UPDATE".
Betroffene Behavior-Definition / Entität: "/PLCE/R_PDSERVICE".
Operation / Wert: "UPDATE".
Fehleranalyse
Der Grund für den Laufzeitfehler (siehe folgende Liste) ist: (1)
Gründe für Laufzeitfehler durch Kontraktverletzungen sind:
(0) Diese Kontraktverletzung führt immer zum Fehler.
(1) Die "strict"-Spezifikation im laufenden BO fordert es.
(2) Die "strict"-Spezifikation des aufgerufenen BOs fordert es.
(3) Die Kontraktverletzung erfolgt in Restricted ABAP.
(4) Die entsprechende Checkpoint-Gruppe ist auf "Fehler" gestellt.
(5) Die Verletzung erfolgt in einer BO-Erweiterung in Restricted ABAP.
Das ABAP-Programm muss korrigiert werden.
Zusatzinformationen zu den einzelnen Kontraktverletzungen:
- "MISSING_CID":
Jede EML-Operation, die über das Feld %CID verfügt (instanzerzeugende
Operationen), sollte in diesem Feld mit einem eindeutigen Wert versorgt
werden.
???APDOCU ABAPRAP_BO_CONTRACT
- "EMPTY_UPDATE":
Jede EML-UPDATE-Operation sollte bei jedem Update-Satz mindestens ein
Nicht-Schlüsselfeld versorgen.
D.h., in der %CONTROL-Struktur jedes Update-Satzes sollte mindestens ein
Flag für ein Nicht-Schlüsselfeld gesetzt sein.
- "DUPLICATE_UPDATE", "DUPLICATE_DELETE", "DUPLICATE_EXECUTE":
Bei einer Update- oder Delete- oder Execute-Anweisung in EML sollten
keine duplikativen Sätze enthalten sein.
Betrachtet wird hierbei der transaktionale Schlüssel und das Feld
%CID_REF.
- "AUGM_RELATING":
Eine "Modify Augmenting"-Anweisung, die Operationen auf Instanzen
ausführt, welche nicht bereits im Original-Request enthalten sind,
sollte diese Instanzen über den Zusatz "RELATING" in Beziehung zu
Instanzen des Original-Requests setzen, weil Responses sonst nicht
zugeordnet werden können.
- "AUGM_OVERWRITE":
Eine "Modify Augmenting"-Anweisung darf keine Felder des
Original-Requests überschreiben. D.h., es darf nicht das gleiche
%CONTROL-Flag an der gleichen Instanz sowohl in der Original-Änderung
als auch in der "Augmenting"-Änderung gesetzt sein.
- "DERIVED_TYPE_CONFLICT":
Jede EML-Operation erwartet spezifische Typen für ihre Operanden; z.B.
UPDATE erwartet den entsprechenden Typ "TABLE FOR UPDATE <Entity>".
Bei dynamischer Angabe der Operanden kann der Typ nicht vom
ABAP-Compiler überprüft werden. Die ABAP-Laufzeit-Typprüfung erkennt
einen "falschen" Typ auch nicht in jedem Fall (sofern er technisch
kompatibel ist), was zu schwer analysierbaren Fehlern in der
Folgeverarbeitung führen kann. Die Kontraktprüfung erkennt den Fehler
beim Aufruf.
???APDOCU ABENRPM_DERIVED_TYPES
- "CONVERT_KEY_NOT_MAPPED":
In der "ADJUST_NUMBERS"-Phase muss ein BO mit "Late Numbering" die
Abbildung aller vergebenen vorläufigen Schlüssel (PID) auf endgültige
Schlüssel liefern, so dass diese mit der Anweisung CONVERT KEY abgeholt
werden können.
???APDOCU ABENBDL_LATE_NUMBERING
Informationen zur Abbruchstelle
Der Abbruch trat im ABAP-Programm bzw. Include "ZBP_E_BP_R_PDMNLSERVICE_EXT===CP"
auf, und zwar in "ASSIGNWORKAREA". Das Hauptprogramm war "SAPMHTTP".
Im Quelltext befindet sich die Abbruchstelle in Zeile 54
des Includes "ZBP_E_BP_R_PDMNLSERVICE_EXT===CCIMP".
Quelltextauszug
