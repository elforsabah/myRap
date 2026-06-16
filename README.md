Content
Header Information
Service Information
Error Context
Source Code Extract
Active Calls/Events
Header Information
  Type	Frontend Error
  Short Text	You can't edit this instance at the moment. It's currently locked.
  Transaction ID	CA6D7138CBB4488284640108966C9588 (Replay in GW Client)
  Package	/IWBEP/V4S_RUNTIME
  Application Component	XX-PART-PLG-ISP
  Date/Time	16.06.2026 15:08:28 (System)
  Username	
  Client	110
  Request Kind	Server OData V4
Service Information
  Service Namespace	
  Service Name	/PLCE/UI_PDFUNCTIONALLOCATION
  Service Version	0001
  Group ID	/PLCE/UI_PDFUNCTIONALLOCATION
  Service Repository	SRVD
  Destination	NONE
Error Context
 –ERROR_CONTEXT	
      ERROR_INFO	You can't edit this instance at the moment. It's currently locked.
  –ERROR_RESOLUTION	
       SAP_NOTE	See SAP Note 1797736 for error analysis
       LINK_TO_SAP_NOTE	https://service.sap.com/sap/support/notes/1797736
       BATCH_SAP_NOTE	See SAP Note 1869434 for details about working with $batch
       LINK_TO_BATCH_SAP_NOTE	https://service.sap.com/sap/support/notes/1869434
  –Exceptions	
   –/IWCOR/CX_OD_HTTP_ERROR	
        Text	Unknown http error occurred in the context of Data Services
        Raise Location	/IWBEP/CL_OD_PROCESSOR
   –/IWBEP/CX_GATEWAY	
        Text	Unspecified provider error occurred. See Error Context and Call Stack.
    –Attributes	
         EXCEPTION_CATEGORY	Provider_Application_Error
         HTTP_STATUS_CODE	423
         IS_FOR_USER	X
         SAP_NOTE_ID	0000000000
        Raise Location	/IWBEP/CL_V4_RESPONSE_INFO
  –SERVICE_INFO	
       GROUP_ID	/PLCE/UI_PDFUNCTIONALLOCATION
       SERVICE_REPOSITORY	SRVD
       SERVICE_NAME	/PLCE/UI_PDFUNCTIONALLOCATION
       VERSION	0001
       SYSTEM_ALIAS	LOCAL
       DESTINATION	NONE
  –SYSTEM_INFO	
       REQUEST_URI	/sap/opu/odata4/plce/ui_pdfunctionallocation/***Hide_due_to_secure_log_level***
       REMOTE_ADDRESS	10.9.10.223
       APPLICATION_SERVER	vhmuzs4qci_S4Q_00
       HUB_VERSION_INFO	S4Q/110, Rel. 7.58, GWHUB Version 031
       BEP_VERSION_INFO	Rel. . , GWBEP Version 031
Source Code Extract
30
31
32
33
34
35
36
37
38
39
40
>>
42
43
44
45
        RAISE EXCEPTION lx_gateway.
 
      WHEN OTHERS.
        " Exception category "srv_alias_cache_outdated" is handled in /IWBEP/CL_OD_UTILS=>HANDLE_ERROR
        " Service alias cache only handled on HUB and hence not cleared here.
 
        /iwbep/cl_v4_logger=>set_error_in_bep( ).
 
        lx_gateway = NEW #( http_status_code = ms_response_info-exception-http_code
                            is_for_user = abap_true ).
        lx_gateway->/iwbep/if_gateway_exception_fw~set_response_info( io_response_info = me ).
        RAISE EXCEPTION lx_gateway.
 
    ENDCASE.
 
  ENDMETHOD.
Active Calls/Events
No.	Event	Program	Include	Line
13	METHOD	/IWBEP/CL_OD_PROCESSOR========CP	/IWBEP/CL_OD_PROCESSOR========CM00B	301
12	METHOD	/IWCOR/CL_OD_PROC_DISPATCHER==CP	/IWCOR/CL_OD_PROC_DISPATCHER==CM00F	26
11	METHOD	/IWCOR/CL_OD_PROC_DISPATCHER==CP	/IWCOR/CL_OD_PROC_DISPATCHER==CM005	125
10	METHOD	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM004	255
9	METHOD	/IWBEP/CL_OD_ROOT_HANDLER=====CP	/IWBEP/CL_OD_ROOT_HANDLER=====CM003	124
8	METHOD	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM00L	209
7	METHOD	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM00F	3
6	METHOD	/IWCOR/CL_REST_HTTP_HANDLER===CP	/IWCOR/CL_REST_HTTP_HANDLER===CM001	121
5	METHOD	/IWBEP/CL_OD_HTTP_REQ_HANDLER=CP	/IWBEP/CL_OD_HTTP_REQ_HANDLER=CM007	70
4	METHOD	/IWBEP/CL_OD_ICF_HANDLER======CP	/IWBEP/CL_OD_ICF_HANDLER======CM001	39
3	METHOD	CL_HTTP_SERVER================CP	CL_HTTP_SERVER================CM00D	814
2	FUNCTION	SAPLHTTP_RUNTIME	LHTTP_RUNTIMEU02	1655
1	MODULE (PBO)	SAPMHTTP	SAPMHTTP
