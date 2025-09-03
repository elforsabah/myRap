Contents
Header Information
What happened?
Error analysis
Information on where terminated
Source Code Extract
Active Calls/Events
Header Information
Short Text 	Exception condition "TYPE_NOT_FOUND" triggered
Runtime Error 	RAISE_EXCEPTION
Program 	CL_SADL_GW_V4_PROPERTY_BUILDERCP
Application Component 	BC-ESI-ESF-GW
Date/Time 	03.09.2025 08:53:31 (System)
User 	PROLOGAEFO (Elvis Forsab)
Client 	100
Host 	vhmuzs4dci_S4D_00
What happened?
The current ABAP program has encountered an unexpected situation.
Error analysis
A RAISE statement in program "CL_ABAP_TYPEDESCR=============CP" has raised exception condition "TYPE_NOT_FOUND".
Since the exception was not caught by a program higher up in the call
hierarchy, processing was terminated.

Short text for exception condition:
You can find detailed documentation about the exception condition in
transaction SE37 (Function Library). You can find the name of the
function module called from the display of active calls.
Information on where terminated
The termination occurred in ABAP program or include "CL_ABAP_TYPEDESCR=============CP", in "DESCRIBE_BY_NAME". The
main program was "SAPMHTTP".

In the source code, the termination point is in line 32 of include "CL_ABAP_TYPEDESCR=============CM002".
include "CL_ABAP_TYPEDESCR=============CM002".
Source Code Extract
22
23
24
25
26
27
28
29
30
31
>>
33
34
35
36
      translate upper_name to upper case.
* get administration information
      system-call describe administration
        mode 'N' of upper_name into admin_tab_line-xtype crc admin_tab_line-kind.
      case admin_tab_line-kind.
        when_all.
          p_descr_ref = _get_by_xtype( p_xtype = admin_tab_line-xtype p_kind = admin_tab_line-kind ).
          return.
        when others.
* all others are erros
          raise type_not_found.
      endcase.
  endcase.
 
endmethod.
Active Calls/Events
No.	Event	Program	Include	Line
26	DESCRIBE_BY_NAME	CL_ABAP_TYPEDESCR=============CP	CL_ABAP_TYPEDESCR=============CM002	32
25	IF_SADL_GW_V4_ODATA_BUILDER~BUILD	CL_SADL_GW_V4_PROPERTY_BUILDERCP	CL_SADL_GW_V4_PROPERTY_BUILDERCM002	14
24	IF_SADL_GW_V4_METADATA~BUILD	CL_SADL_GW_V4_MDL_BUILDER=====CP	CL_SADL_GW_V4_MDL_BUILDER=====CM00A	37
23	IF_SADL_GW_V4_MODEL_EXPOSURE~EXPOSE	CL_SADL_GW_V4_MODEL_EXPOSURE==CP	CL_SADL_GW_V4_MODEL_EXPOSURE==CCIMP	91
22	_DEFINE_MODEL	CL_SADL_GW_V4_GENERIC_MPC=====CP	CL_SADL_GW_V4_GENERIC_MPC=====CM007	6
21	/IWBEP/IF_V4_MP_BASIC~DEFINE	CL_SADL_GW_V4_GENERIC_MPC=====CP	CL_SADL_GW_V4_GENERIC_MPC=====CM006	3
20	LOAD_MODEL_FROM_MPC	/IWBEP/CL_V4_LOCAL_MP_PROXY===CP	/IWBEP/CL_V4_LOCAL_MP_PROXY===CM002	40
19	/IWBEP/IF_V4_MODEL_PROVIDER_FW~LOAD_METADATA_CONDITIONAL	/IWBEP/CL_V4_LOCAL_MP_PROXY===CP	/IWBEP/CL_V4_LOCAL_MP_PROXY===CM001	111
18	CREATE_MODEL_INTERNAL	/IWBEP/CL_V4_MED_MODEL_FACTORYCP	/IWBEP/CL_V4_MED_MODEL_FACTORYCM00A	78
17	CREATE_MODEL	/IWBEP/CL_V4_MED_MODEL_FACTORYCP	/IWBEP/CL_V4_MED_MODEL_FACTORYCM001	12
16	GET_MODEL_BY_SERVICE_KEY_INT	/IWBEP/CL_V4_MED_MODEL_GROUP==CP	/IWBEP/CL_V4_MED_MODEL_GROUP==CM00G	41
15	/IWBEP/IF_V4_MED_MODEL_GROUP~GET_MODEL_BY_SERVICE_KEY	/IWBEP/CL_V4_MED_MODEL_GROUP==CP	/IWBEP/CL_V4_MED_MODEL_GROUP==CM00E	3
14	/IWBEP/IF_V4_REQUEST_INFO~GET_MODEL_OF_CURRENT_SERVICE	/IWBEP/CL_V4_REQUEST_INFO=====CP	/IWBEP/CL_V4_REQUEST_INFO=====CM03F	2
13	SET_REQUEST_INFO	/IWBEP/CL_OD_EDM_PROVIDER=====CP	/IWBEP/CL_OD_EDM_PROVIDER=====CM016	10
12	GET_INSTANCE	/IWBEP/CL_OD_EDM_PROVIDER=====CP	/IWBEP/CL_OD_EDM_PROVIDER=====CM00W	8
11	/IWCOR/IF_OD_SVC_FACTORY~CREATE_SERVICE	/IWBEP/CL_OD_SVC_FACTORY======CP	/IWBEP/CL_OD_SVC_FACTORY======CM001	20
10	DISPATCH	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM004	38
9	DISPATCH	/IWBEP/CL_OD_ROOT_HANDLER=====CP	/IWBEP/CL_OD_ROOT_HANDLER=====CM003	124
8	HANDLE_WITH_MODE	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM00L	209
7	/IWCOR/IF_REST_HANDLER~HANDLE	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM00F	3
6	IF_HTTP_EXTENSION~HANDLE_REQUEST	/IWCOR/CL_REST_HTTP_HANDLER===CP	/IWCOR/CL_REST_HTTP_HANDLER===CM001	121
5	IF_HTTP_EXTENSION~HANDLE_REQUEST	/IWBEP/CL_OD_HTTP_REQ_HANDLER=CP	/IWBEP/CL_OD_HTTP_REQ_HANDLER=CM007	70
4	IF_HTTP_EXTENSION~HANDLE_REQUEST	/IWBEP/CL_OD_ICF_HANDLER======CP	/IWBEP/CL_OD_ICF_HANDLER======CM001	39
3	EXECUTE_REQUEST	CL_HTTP_SERVER================CP	CL_HTTP_SERVER================CM00D	814
2	HTTP_DISPATCH_REQUEST	SAPLHTTP_RUNTIME	LHTTP_RUNTIMEU02	1655
1	%_HTTP_START	SAPMHTTP	SAPMHTTP	12
