Inhalte
Kopfinformationen
Was ist passiert?
Fehleranalyse
Informationen zur Abbruchstelle
Quelltextauszug
Aktive Aufrufe/Ereignisse
Kopfinformationen
Kurztext 	Es wurde ein RAP-Kontrakt verletzt.
Laufzeitfehler 	BEHAVIOR_CONTRACT_VIOLATION
Programm 	ZBP_E_BP_R_PDTOUR=============CP
Datum/Uhrzeit 	25.11.2025 02:06:15 (System)
Benutzer 	HWSB10035 (Elvis Mbah Forsab)
Mandant 	442
Host 	evhsap-srv08_RI4_01
Was ist passiert?
Fehler im ABAP-Anwendungsprogramm.
Das laufende ABAP-Programm "ZBP_E_BP_R_PDTOUR=============CP" mußte abgebrochen werden, da es auf
eine Anweisung gestoßen ist, die leider nicht ausgeführt werden kann.
Es wurde ein RAP-Kontrakt verletzt: "CC/C:MISSING_CID".
Betroffene Behavior-Definition / Entität: "/PLCE/R_PDTOUR".
Operation / Wert: "CREATETOURWITHTEMPLATE".
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
Der Abbruch trat im ABAP-Programm bzw. Include "ZBP_E_BP_R_PDTOUR=============CP"
auf, und zwar in "CREATETOUR". Das Hauptprogramm war "SAPMHTTP".
Im Quelltext befindet sich die Abbruchstelle in Zeile 34
des Includes "ZBP_E_BP_R_PDTOUR=============CCIMP".
Quelltextauszug
24
25
26
27
28
29
30
31
32
33
>>
35
36
37
38
39
40
41
42
43
  ENDMETHOD.
 
  METHOD createtour.
 
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>)
         GROUP BY ( template = <key>-%param-tour_template
                    date = <key>-%param-start_date )
         ASSIGNING FIELD-SYMBOL(<group>).
 
      " Create new tour from template (child action)
      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour
          EXECUTE createTourWithTemplate
          FROM VALUE #(
            ( %param-without_draft = 'X'
              %param-tour_template = <group>-template
              %param-start_date = <group>-date ) )
        MAPPED DATA(mapped_tour)
        FAILED DATA(failed_tour)
        REPORTED DATA(reported_tour).
Aktive Aufrufe/Ereignisse
Nr.	Ereignis	Programm	Include	Zeile
31	CREATETOUR	ZBP_E_BP_R_PDTOUR=============CP	ZBP_E_BP_R_PDTOUR=============CCIMP	34
30	INVOKE	CL_ABAP_BEHAVIOR_HANDLER======CP	CL_ABAP_BEHAVIOR_HANDLER======CM002	4
29	CALL_HANDLER	CL_ABAP_BEHV_CTRL=============CP	CL_ABAP_BEHV_CTRL=============CM001	219
28	EXECUTE	CL_ABAP_BEHV_HANDLER_PROJ=====CP	CL_ABAP_BEHV_HANDLER_PROJ=====CM001	145
27	CALL_HANDLER	CL_ABAP_BEHV_CTRL=============CP	CL_ABAP_BEHV_CTRL=============CM001	206
26	CALL_HANDLERS_MODIFY	CL_RAP_BHV_PROCESSOR==========CP	CL_RAP_BHV_PROCESSOR==========CM00L	18
25	IF_RAP_TRANSACTION_PROCESSOR~MODIFY	CL_RAP_BHV_PROCESSOR==========CP	CL_RAP_BHV_PROCESSOR==========CM00B	77
24	IF_SADL_CHANGESET~MODIFY	CL_RAP_TRANSACTION_MANAGER====CP	CL_RAP_TRANSACTION_MANAGER====CM00E	71
23	_MODIFY	CL_SADL_TRANSACTION_MANAGER===CP	CL_SADL_TRANSACTION_MANAGER===CM00O	19
22	IF_SADL_CHANGESET~MODIFY	CL_SADL_TRANSACTION_MANAGER===CP	CL_SADL_TRANSACTION_MANAGER===CM00L	13
21	IF_SADL_CHANGESET~MODIFY	CL_SADL_CHANGESET=============CP	CL_SADL_CHANGESET=============CM006	23
20	IF_SADL_GW_V4_GENERIC_DPC~PROCESS_CHANGE_SET	CL_SADL_GW_V4_GENERIC_DPC=====CP	CL_SADL_GW_V4_GENERIC_DPC=====CM010	54
19	/IWBEP/IF_V4_DP_ADVANCED~EXECUTE_ACTION	CL_SADL_GW_V4_DPC_ADAPTER=====CP	CL_SADL_GW_V4_DPC_ADAPTER=====CM00I	49
18	EXECUTE_BATCH_OPERATION	/IWBEP/CL_V4_ABS_DATA_PROVIDERCP	/IWBEP/CL_V4_ABS_DATA_PROVIDERCM01O	43
17	/IWBEP/IF_V4_DP_BATCH~PROCESS_BATCH	/IWBEP/CL_V4_ABS_DATA_PROVIDERCP	/IWBEP/CL_V4_ABS_DATA_PROVIDERCM01K	81
16	/IWBEP/IF_V4_DP_BATCH~PROCESS_BATCH	CL_SADL_GW_V4_DPC_ADAPTER=====CP	CL_SADL_GW_V4_DPC_ADAPTER=====CM01T	4
15	/IWBEP/IF_V4_DATA_PROVIDER_FW~PROCESS_BATCH	/IWBEP/CL_V4_DP_PROXY=========CP	/IWBEP/CL_V4_DP_PROXY=========CM006	50
14	/IWBEP/IF_V4_DATA_PROVIDER_FW~PROCESS_BATCH	/IWBEP/CL_V4_LOCAL_DP_PROXY===CP	/IWBEP/CL_V4_LOCAL_DP_PROXY===CM009	5
13	/IWCOR/IF_OD_PROC_BATCH~EXECUTE	/IWBEP/CL_OD_PROCESSOR========CP	/IWBEP/CL_OD_PROCESSOR========CM00B	234
12	PROCESS_BATCH	/IWCOR/CL_OD_PROC_DISPATCHER==CP	/IWCOR/CL_OD_PROC_DISPATCHER==CM00F	26
11	/IWCOR/IF_OD_PROCESSOR~PROCESS	/IWCOR/CL_OD_PROC_DISPATCHER==CP	/IWCOR/CL_OD_PROC_DISPATCHER==CM005	125
10	DISPATCH	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM004	255
9	DISPATCH	/IWBEP/CL_OD_ROOT_HANDLER=====CP	/IWBEP/CL_OD_ROOT_HANDLER=====CM003	124
8	HANDLE_WITH_MODE	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM00L	209
7	/IWCOR/IF_REST_HANDLER~HANDLE	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM00F	3
6	IF_HTTP_EXTENSION~HANDLE_REQUEST	/IWCOR/CL_REST_HTTP_HANDLER===CP	/IWCOR/CL_REST_HTTP_HANDLER===CM001	121
5	IF_HTTP_EXTENSION~HANDLE_REQUEST	/IWBEP/CL_OD_HTTP_REQ_HANDLER=CP	/IWBEP/CL_OD_HTTP_REQ_HANDLER=CM007	70
4	IF_HTTP_EXTENSION~HANDLE_REQUEST	/IWBEP/CL_OD_ICF_HANDLER======CP	/IWBEP/CL_OD_ICF_HANDLER======CM001	39
3	EXECUTE_REQUEST	CL_HTTP_SERVER================CP	CL_HTTP_SERVER================CM00D	814
2	HTTP_DISPATCH_REQUEST	SAPLHTTP_RUNTIME	LHTTP_RUNTIMEU02	1655
1	%_HTTP_START	SAPMHTTP	SAPMHTTP	12
