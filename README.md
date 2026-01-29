Inhalte
Kopfinformationen
Was ist passiert?
Fehleranalyse
Informationen zur Abbruchstelle
Quelltextauszug
Aktive Aufrufe/Ereignisse
Kopfinformationen
Kurztext 	Anweisung "ROLLBACK WORK" im aktuellen Zustand unzulässig
Laufzeitfehler 	BEHAVIOR_ILLEGAL_STATEMENT
Programm 	SAPLBAPT
Anwendungskomponente 	BC-MID-API
Datum/Uhrzeit 	29.01.2026 02:27:59 (System)
Was ist passiert?
Fehler im ABAP-Anwendungsprogramm.
Das laufende ABAP-Programm "SAPLBAPT" mußte abgebrochen werden, da es auf
eine Anweisung gestoßen ist, die leider nicht ausgeführt werden kann.
Fehleranalyse
Die Ausführung befindet sich in einem transaktionalen Kontext:
Eine BO-Implementierung für "ZI_LANF_ROOT" ist aktiv.
Daher ist die Anweisung "ROLLBACK WORK" verboten.
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
Der Abbruch trat im ABAP-Programm bzw. Include "SAPLBAPT"
auf, und zwar in "BAPI_TRANSACTION_ROLLBACK". Das Hauptprogramm war "SAPMHTTP".
Im Quelltext befindet sich die Abbruchstelle in Zeile 8
des Includes "LBAPTU02".
Quelltextauszug
1
2
3
4
5
6
7
>
9
10
11
12
13
FUNCTION BAPI_TRANSACTION_ROLLBACK.
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*" EXPORTING
*" VALUE(RETURN) LIKE BAPIRET2 STRUCTURE BAPIRET2
*"----------------------------------------------------------------------
 
ROLLBACK WORK.
 
CALL FUNCTION 'BUFFER_REFRESH_ALL'.
 
 
ENDFUNCTION.
Aktive Aufrufe/Ereignisse
Nr.	Ereignis	Programm	Include	Zeile
28	BAPI_TRANSACTION_ROLLBACK	SAPLBAPT	LBAPTU02	8
27	CREATELANF	ZBP_I_LANF_ROOT===============CP	ZBP_I_LANF_ROOT===============CCIMP	290
26	INVOKE	CL_ABAP_BEHAVIOR_HANDLER======CP	CL_ABAP_BEHAVIOR_HANDLER======CM002	4
25	CALL_HANDLER	CL_ABAP_BEHV_CTRL=============CP	CL_ABAP_BEHV_CTRL=============CM001	219
24	CALL_HANDLERS_MODIFY	CL_RAP_BHV_PROCESSOR==========CP	CL_RAP_BHV_PROCESSOR==========CM00L	18
23	IF_RAP_TRANSACTION_PROCESSOR~MODIFY	CL_RAP_BHV_PROCESSOR==========CP	CL_RAP_BHV_PROCESSOR==========CM00B	77
22	IF_SADL_CHANGESET~MODIFY	CL_RAP_TRANSACTION_MANAGER====CP	CL_RAP_TRANSACTION_MANAGER====CM00E	71
21	_MODIFY	CL_SADL_TRANSACTION_MANAGER===CP	CL_SADL_TRANSACTION_MANAGER===CM00O	19
20	IF_SADL_CHANGESET~MODIFY	CL_SADL_TRANSACTION_MANAGER===CP	CL_SADL_TRANSACTION_MANAGER===CM00L	13
19	IF_SADL_CHANGESET~MODIFY	CL_SADL_CHANGESET=============CP	CL_SADL_CHANGESET=============CM006	23
18	IF_SADL_GW_V4_GENERIC_DPC~PROCESS_CHANGE_SET	CL_SADL_GW_V4_GENERIC_DPC=====CP	CL_SADL_GW_V4_GENERIC_DPC=====CM010	54
17	/IWBEP/IF_V4_DP_ADVANCED~EXECUTE_ACTION	CL_SADL_GW_V4_DPC_ADAPTER=====CP	CL_SADL_GW_V4_DPC_ADAPTER=====CM00I	49
16	/IWBEP/IF_V4_DATA_PROVIDER_FW~EXECUTE_ACTION	/IWBEP/CL_V4_DP_PROXY=========CP	/IWBEP/CL_V4_DP_PROXY=========CM003	36
15	/IWBEP/IF_V4_DATA_PROVIDER_FW~EXECUTE_ACTION	/IWBEP/CL_V4_LOCAL_DP_PROXY===CP	/IWBEP/CL_V4_LOCAL_DP_PROXY===CM007	5
14	EXECUTE_ACTION	/IWBEP/CL_OD_PROCESSOR========CP	/IWBEP/CL_OD_PROCESSOR========CM002	137
13	/IWCOR/IF_OD_PROC_COMPLEX_COLL~EXECUTE_BOUND_ACTION	/IWBEP/CL_OD_PROCESSOR========CP	/IWBEP/CL_OD_PROCESSOR========CM01H	4
12	PROCESS_COMPLEX_COLL	/IWCOR/CL_OD_PROC_DISPATCHER==CP	/IWCOR/CL_OD_PROC_DISPATCHER==CM00O	69
11	/IWCOR/IF_OD_PROCESSOR~PROCESS	/IWCOR/CL_OD_PROC_DISPATCHER==CP	/IWCOR/CL_OD_PROC_DISPATCHER==CM005	425
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
