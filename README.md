Inhalte
Kopfinformationen
Was ist passiert?
Fehleranalyse
Informationen zur Abbruchstelle
Quelltextauszug
Aktive Aufrufe/Ereignisse
Kopfinformationen
Kurztext 	Anweisung "CALL FUNCTION .. IN BACKGROUND TASK" im aktuellen Zustand unzulässig
Laufzeitfehler 	BEHAVIOR_ILLEGAL_STATEMENT
Programm 	ZBP_E_BP_R_PDMNLSERVICE_EXT===CP
Datum/Uhrzeit 	20.03.2026 09:18:17 (System)
Benutzer 	HWSB10035 (Elvis Mbah Forsab)
Mandant 	442
Host 	evhsap-srv08_RI4_01
Was ist passiert?
Fehler im ABAP-Anwendungsprogramm.
Das laufende ABAP-Programm "ZBP_E_BP_R_PDMNLSERVICE_EXT===CP" mußte abgebrochen werden, da es auf
eine Anweisung gestoßen ist, die leider nicht ausgeführt werden kann.
Fehleranalyse
Die Ausführung befindet sich in einem transaktionalen Kontext:
Eine BO-Implementierung für "/PLCE/R_PDSERVICE" ist aktiv.
Daher ist die Anweisung "CALL FUNCTION .. IN BACKGROUND TASK" verboten.
Im Fall der Anweisung MESSAGE lautet der Nachrichtentext:
"???"
Der eigentliche Fehler liegt möglicherweise nicht in dem Programm, in
dem der Abbruch erfolgt, sondern in einer höheren Aufrufebene, von der
es in einer unzulässigen BEHAVIOR-Phase aufgerufen wird.
Der Grund für den Laufzeitfehler (siehe folgende Liste) ist: (0)
Gründe für Laufzeitfehler durch Kontraktverletzungen sind:
(0) Diese Kontraktverletzung führt immer zum Fehler.
(1) Die "strict"-Spezifikation im laufenden BO fordert es.
(2) Die "strict"-Spezifikation des aufgerufenen BOs fordert es.
(3) Die Kontraktverletzung erfolgt in Restricted ABAP.
(4) Die entsprechende Checkpoint-Gruppe ist auf "Fehler" gestellt.
(5) Die Verletzung erfolgt in einer BO-Erweiterung in Restricted ABAP.
Die folgenden ABAP-Anweisungen sind im transaktionalen Kontext
grundsätzlich verboten:
- COMMIT, ROLLBACK
- SUBMIT, CALL TRANSACTION, LEAVE
- alle DYNPRO-bezogenen, wie MESSAGE, CALL DIALOG, CALL SCREEN
- RAISE EXCEPTION, sofern es die BEHAVIOR-Implementierung verlassen
würde.
Die Anweisungen
- CALL FUNCTION IN UPDATE TASK
- CALL FUNCTION IN BACKGROUND TASK/UNIT
- PERFORM ON COMMIT/ROLLBACK
sind außerhalb der "Save"-Methode unzulässig.
Datenbank-Befehle INSERT/DELETE/UPDATE/MODIFY und andere Formen des
ändernden DB-Zugriffs (z.B. EXEC SQL, AMDP, EXPORT TO DATABASE) auf der
primären DB-Verbindung sind außerhalb der "Save"-Methode unzulässig.
(Änderungen auf einer sekundären DB-Verbindung sind jederzeit zulässig.)
Operationen, die zum Commit der primären DB-Verbindung führen (z.B. RFC,
HTTP, WAIT, Commit über EXEC SQL), sind in der "Late Save"-Phase
(Methoden "Adjust_Numbers" und "Save") unzulässig.
???APDOCU ABENDB_COMMIT
Die Anweisung AUTHORITY-CHECK oder die Verwendung einer DCL durch ein
SELECT ohne "Privileged Mode" ist während der Save-Sequenz nicht
zulässig (Prüfungen sollten in der Interaktionsphase stattfinden).
GET PERMISSIONS ist in der "Save"-Methode nicht zulässig (Prüfungen
sollten in der Interaktionsphase stattfinden).
SET LOCKS ist nur zulässig in MODIFY, LOCK und FINALIZE.
MODIFY ENTITIES ist unzulässig in BO-Implementierungen außer "Modify",
in klassifizierten Methoden mit einem anderen Kontrakt als "Modify" oder
"Surface", sowie in der "Update Task"-Verarbeitung.
Actions, die in der Definition als "save" gekennzeichnet sind, dürfen
nur in der entsprechenden Save-Methode aufgerufen werden.
RAISE EVENT ist nur zulässig in der "Late Save"-Phase.
???APDOCU ABAPRAP_LUW
Für (BAdI-)Methoden und Funktionen mit transaktionaler Klassifikation
gelten folgende Einschränkungen:
Der Aufruf einer als "Save" transaktional klassifizierten Methode ist
unzulässig aus der Interaktionsphase heraus, sowie aus einer Methode mit
einem anderen Kontrakt als "Save" oder "Surface".
Der Aufruf einer als "Modify" transaktional klassifizierten Methode ist
nur dann zulässig, wenn auch "MODIFY ENTITIES" erlaubt ist.
Der Aufruf einer als "Read" klassifizierten Methode ist nur dann
zulässig, wenn auch "READ ENTITIES" erlaubt ist.
Der Aufruf einer als "Surface" transaktional klassifizierten Methode ist
unzulässig aus einer BO-Implementierung heraus, sowie aus einer Methode
mit einem anderen Kontrakt als "Surface".
Informationen zur Abbruchstelle
Der Abbruch trat im ABAP-Programm bzw. Include "ZBP_E_BP_R_PDMNLSERVICE_EXT===CP"
auf, und zwar in "TERMINATESERVICE". Das Hauptprogramm war "SAPMHTTP".
Im Quelltext befindet sich die Abbruchstelle in Zeile 347
des Includes "ZBP_E_BP_R_PDMNLSERVICE_EXT===CCIMP".
Quelltextauszug
336
337
338
339
340
341
342
343
344
345
346
>>>
348
349
350
351
352
353
354
355
356
357
          et_return = lt_return.
 
      IF lt_return IS INITIAL.
        " --- SUCCESS ---
 
        " A. Update Status to '01' to keep Fiori happy for the next 3 seconds
        APPEND VALUE #( %tky = <ls_service>-%tky
                        servicestatus = '01' ) TO lt_service_update.
 
        " B. TRIGGER THE HARD DELETE IN THE BACKGROUND!
        " "IN BACKGROUND TASK" means this RFC won't start until the RAP COMMIT finishes.
        CALL FUNCTION 'Z_WR_DELAYED_HARD_DELETE' IN BACKGROUND TASK DESTINATION 'NONE'
          EXPORTING
            iv_serviceuuid = <ls_service>-serviceuuid.
 
        " C. Prepare Tour Unassignment
        SELECT SINGLE asgmt~serviceuuid, asgmt~touruuid
          FROM /plce/r_pdtourserviceasgmt AS asgmt
          WHERE asgmt~serviceuuid = @<ls_service>-serviceuuid
          INTO @DATA(ls_unassign).
 
        IF sy-subrc = 0.
Aktive Aufrufe/Ereignisse
Nr.	Ereignis	Programm	Include	Zeile
31	TERMINATESERVICE	ZBP_E_BP_R_PDMNLSERVICE_EXT===CP	ZBP_E_BP_R_PDMNLSERVICE_EXT===CCIMP	347
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
