Inhalte
Kopfinformationen
Was ist passiert?
Fehleranalyse
Informationen zur Abbruchstelle
Quelltextauszug
Aktive Aufrufe/Ereignisse
Kopfinformationen
Kurztext 	Funktionsparameter "EXTENSIONIN" ist nicht bekannt.
Laufzeitfehler 	CALL_FUNCTION_PARM_UNKNOWN
Ausnahme 	CX_SY_DYN_CALL_PARAM_NOT_FOUND
Programm 	ZBP_I_LANF_ROOT===============CP
Datum/Uhrzeit 	29.01.2026 01:31:46 (System)
Benutzer 	HWSB10035 (Elvis Mbah Forsab)
Mandant 	442
Host 	evhsap-srv08_RI4_01
Was ist passiert?
Fehler im ABAP-Anwendungsprogramm.
Das laufende ABAP-Programm "ZBP_I_LANF_ROOT===============CP" mußte abgebrochen werden, da es auf
eine Anweisung gestoßen ist, die leider nicht ausgeführt werden kann.
Es wurde der Funktionsbaustein "BAPI_SALESDOCU_CREATEFROMDATA" aufgerufen
und der Parameter "EXTENSIONIN" mitgegeben.
Dieser Parameter ist aber dort nicht definiert.
Fehleranalyse
Beschreibung nicht verfügbar
Parameter:
P1 EXTENSIONIN
P2 BAPI_SALESDOCU_CREATEFROMDATA
P3 ???
P4 ???
P5 ???
P6 ???
P7 ???
P8 ???
P9 "???"


Es wurde der Funktionsbaustein "BAPI_SALESDOCU_CREATEFROMDATA" aufgerufen
und der Parameter "EXTENSIONIN" mitgegeben.
Dieser Parameter ist aber dort nicht definiert.
Informationen zur Abbruchstelle
Der Abbruch trat im ABAP-Programm bzw. Include "ZBP_I_LANF_ROOT===============CP"
auf, und zwar in "CREATELANF". Das Hauptprogramm war "SAPMHTTP".
Im Quelltext befindet sich die Abbruchstelle in Zeile 163
des Includes "ZBP_I_LANF_ROOT===============CCIMP".
Quelltextauszug
153
154
155
156
157
158
159
160
161
162
>>>
164
165
166
167
168
169
170
171
172
173
  APPEND ls_extensionin TO lt_extensionin.
 
  CLEAR ls_extensionin.
  ls_extensionin-structure = 'BAPE_VBAKX'.
  ls_extensionin-valuepart1 = ls_bape_vbakx.
  APPEND ls_extensionin TO lt_extensionin.
 
  "-------------------------
  " Call BAPI (correct types!)
  "-------------------------
  CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
    EXPORTING
      order_header_in = ls_header_in
      business_object = lv_busobj
      without_commit = abap_true
    IMPORTING
      salesdocument = lv_vbeln
      return = ls_return1
    TABLES
      order_items_in = lt_items_in
      order_partners = lt_partners
Aktive Aufrufe/Ereignisse
Nr.	Ereignis	Programm	Include	Zeile
27	CREATELANF	ZBP_I_LANF_ROOT===============CP	ZBP_I_LANF_ROOT===============CCIMP	163
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
