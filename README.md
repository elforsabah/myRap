Header Information
Short Text 	The current program has intentionally triggered a termination with a short dump.
Runtime Error 	RAISE_SHORTDUMP
Exception 	CX_SADL_DUMP_APPL_MODEL_ERROR
Development Object 	CLAS ZCL_WR_TOUR_EXTEND_CALC
Date/Time 	10.02.2026 15:59:44 (System)

Client 	442
Host 	evhsap-srv08_RI4_01
What happened?
The current program has recognised an error that it could not handle
adequately. To reverse any database changes made and to save
information for further analysis, the program has intentionally
triggered a short dump using the language elements RAISE SHORTDUMP or
THROW SHORTDUMP.
???APDOCU ABAPRAISE_SHORTDUMP
Error analysis
The program has indicated exception "CX_SADL_DUMP_APPL_MODEL_ERROR" as the reason for the
termination:
Processing was aborted because of an application modeling error: See previous in exception chain.
The exception is related to the previous exception "CX_SADL_EXIT_PROCESSING_ERROR" that occurred in
program "CL_SADL_EXIT_HANDLER==========CP", in line 62 of include "CL_SADL_EXIT_HANDLER==========CM016". The reason for this exception
was:
Error caused by exit class ZCL_WR_TOUR_EXTEND_CALC, type CALCULATION, view /PLCE/C_PDMNLTOURWR, calculated element TOUR_CAPACITY_NEW
The exception is related to the previous exception "CX_SADL_EXIT_WRONG_ELMENT" that occurred in
program "CL_SADL_EXIT_HANDLER==========CP", in line 10 of include "CL_SADL_EXIT_HANDLER==========CM00X". The reason for this exception
was:
Entity /PLCE/C_PDMNLTOURWR does not contain element TourCapacity
Information on where terminated
The termination occurred in ABAP program or include "CL_SADL_SHORTDUMP=============CP", in "TRIGGER_APPL_MODEL_ERROR_DUMP". The
main program was "SAPMHTTP".

In the source code, the termination point is in line 2 of include "CL_SADL_SHORTDUMP=============CM006".
include "CL_SADL_SHORTDUMP=============CM006".
Source Code Extract
1
>
3
4
5
6
7
8
9
10
11
12
  METHOD trigger_appl_model_error_dump.
    DATA(_) = COND i( WHEN aunit_excptn_instead_of_dump <> abap_true THEN
     THROW SHORTDUMP cx_sadl_dump_appl_model_error( io_error )
"########################################################################################
"# !!! ATTENTION !!! #
"# Processing was deliberately stopped because an application modeling error has been #
"# detected. Please identify the root cause and contact the application owners: #
"# Carefully read the "Error Analysis" section of this dump ("Long text" tab in ADT #
"# or if viewed in SAP GUI (transaction ST22) in "ABAP Developer View"). #
"# For more detailed information refer to the exception chain contained in the dump. #
"# Identify the CDS view and/or service implementation classes to route this issue to #
"# the responsible application component. To facilitate the support incident please #
Active Calls/Events
No.	Event	Program	Include	Line
38	TRIGGER_APPL_MODEL_ERROR_DUMP	CL_SADL_SHORTDUMP=============CP	CL_SADL_SHORTDUMP=============CM006	2
37	RAISE_FOR_ERROR	CL_SADL_SHORTDUMP=============CP	CL_SADL_SHORTDUMP=============CM008	4
36	IF_SADL_EXCEPTION_HANDLER~HANDLE	CL_SADL_EXC_STRATEGY_SHORTDUMPCP	CL_SADL_EXC_STRATEGY_SHORTDUMPCM001	2
35	IF_SADL_EXCEPTION_HANDLER~HANDLE	CL_SADL_EXCEPTION_GATEKEEPER==CP	CL_SADL_EXCEPTION_GATEKEEPER==CCIMP	53
34	CONVERT_TO_GW_EXC_OR_HANDLE	CL_SADL_GW_V4_DPC_ADP_EXC_CONVCP	CL_SADL_GW_V4_DPC_ADP_EXC_CONVCM002	106
33	CONVERT_TO_GW_EXC_OR_HANDLE	CL_SADL_GW_V4_DPC_ADAPTER=====CP	CL_SADL_GW_V4_DPC_ADAPTER=====CM01O	2
32	/IWBEP/IF_V4_DP_ADVANCED~READ_ENTITY_LIST	CL_SADL_GW_V4_DPC_ADAPTER=====CP	CL_SADL_GW_V4_DPC_ADAPTER=====CM004	46
31	ADJUST_REQUESTED_SINGLE_LEVEL	CL_SADL_EXIT_HANDLER==========CP	CL_SADL_EXIT_HANDLER==========CM016	62
30	_CHECK_ORIG_ELEMENT	CL_SADL_EXIT_HANDLER==========CP	CL_SADL_EXIT_HANDLER==========CM00X	10
29	ADJUST_REQUESTED_SINGLE_LEVEL	CL_SADL_EXIT_HANDLER==========CP	CL_SADL_EXIT_HANDLER==========CM016	44
28	ADJUST_REQUESTED	CL_SADL_EXIT_HANDLER==========CP	CL_SADL_EXIT_HANDLER==========CM002	20
27	_APPLY_ELEMENT_EXITS_PRE_QUERY	CL_SADL_ABQI==================CP	CL_SADL_ABQI==================CM02D	26
26	HANDLE_PATHS_AND_PRE_SEL_EXITS	CL_SADL_ABQI==================CP	CL_SADL_ABQI==================CM02O	53
25	_SELECT	CL_SADL_ABQI==================CP	CL_SADL_ABQI==================CM01G	26
24	IF_SADL_ABQI~SELECT	CL_SADL_ABQI==================CP	CL_SADL_ABQI==================CM004	114
23	IF_SADL_QUERY_FETCH~FETCH	CL_SADL_ENTITY_RUNTIME========CP	CL_SADL_ENTITY_RUNTIME========CM002	22
22	_GET_ENTITYSET_VIA_FETCH	CL_SADL_GW_ODATA_RUNTIME======CP	CL_SADL_GW_ODATA_RUNTIME======CM00R	54
21	IF_SADL_GW_ODATA_RUNTIME~GET_ENTITY_SET	CL_SADL_GW_ODATA_RUNTIME======CP	CL_SADL_GW_ODATA_RUNTIME======CM001	9
20	IF_SADL_GW_V4_GENERIC_DPC~READ_ENTITY_LIST	CL_SADL_GW_V4_GENERIC_DPC=====CP	CL_SADL_GW_V4_GENERIC_DPC=====CM00G	49
19	/IWBEP/IF_V4_DP_ADVANCED~READ_ENTITY_LIST	CL_SADL_GW_V4_DPC_ADAPTER=====CP	CL_SADL_GW_V4_DPC_ADAPTER=====CM004	23
18	EXECUTE_BATCH_OPERATION	/IWBEP/CL_V4_ABS_DATA_PROVIDERCP	/IWBEP/CL_V4_ABS_DATA_PROVIDERCM01O	35
17	/IWBEP/IF_V4_DP_BATCH~PROCESS_BATCH	/IWBEP/CL_V4_ABS_DATA_PROVIDERCP	/IWBEP/CL_V4_ABS_DATA_PROVIDERCM01K	46
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
