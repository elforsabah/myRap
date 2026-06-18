----------------------------------------------------------------------------------------------------
Category               ABAP programming error                                                       
Runtime Errors         UNCAUGHT_EXCEPTION                                                           
Except.                /PLCE/CX_BASEEXCEPTION                                                       
ABAP: Program          /PLCP/CL_TA_WA_ORDER_RSLT=====CP                                             
Application Component  XX-PART-PLG-ISP                                                              
Date and Time          18.06.2026 06:16:48 (CET)                                                    
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Short Text                                                                                        |
|    An exception has occurred that was not caught.                                                |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|What happened?                                                                                    |
|    The exception of class "/PLCE/CX_BASEEXCEPTION" was triggered but not caught                  |
|     anywhere in the                                                                              |
|    call hierarchy.                                                                               |
|                                                                                                  |
|    Since exceptions represent error situations, and this error was not                           |
|    adequately responded to, ABAP program "/PLCE/CL_APPLLOG_HELPER=======CP" had                  |
|     to be terminated.                                                                            |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|What can you do?                                                                                  |
|    Note which actions and entries caused the error to occur.                                     |
|                                                                                                  |
|    Consult your SAP administrator.                                                               |
|                                                                                                  |
|    Using transaction ST22 for ABAP dump analysis, you can view, manage,                          |
|    and retain termination messages for longer periods.                                           |
|    Note which actions and entries caused the error to occur.                                     |
|                                                                                                  |
|    Consult your SAP administrator.                                                               |
|                                                                                                  |
|    Using transaction ST22 for ABAP dump analysis, you can view, manage,                          |
|    and retain termination messages for longer periods.                                           |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Error analysis                                                                                    |
|    An exception has occurred in class "/PLCE/CX_BASEEXCEPTION". This exception                   |
|     was not caught                                                                               |
|    in procedure "CREATE_NOTIFICATION" "(METHOD)" or propagated by a RAISING                      |
|     clause.                                                                                      |
|    Since the caller of the procedure could not have anticipated this                             |
|    exception, the current program was terminated.                                                |
|    The reason for the exception occurring was:                                                   |
|    The reason for the exception is:                                                              |
|    An exception was raised                                                                       |
|                                                                                                  |
|    The exception is related to the previous exception "CX_BALI_NOT_FOUND" that                   |
|     occurred in                                                                                  |
|    program "CL_BALI_LOG_DB================CP", in line 24 of include                             |
|     "CL_BALI_LOG_DB================CM00H". The reason for this exception                         |
|    was:                                                                                          |
|    No log found in the database                                                                  |
|                                                                                                  |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|How to correct the error                                                                          |
|    If the exception cannot be prevented, "/PLCE/CX_BASEEXCEPTION" must be caught                 |
|     within                                                                                       |
|    procedure "CREATE_NOTIFICATION" "(METHOD)" or declared in the procedure's                     |
|     RAISING clause.                                                                              |
|    To prevent the exception, note the following:                                                 |
|                                                                                                  |
|    If the error occurs in a non-modfied SAP program, you might be able to                        |
|    find a solution in the SAP Notes system. If you have access to the SAP                        |
|    Notes system, check there first using the following keywords:                                 |
|                                                                                                  |
|    "UNCAUGHT_EXCEPTION" /PLCE/CX_BASEEXCEPTION                                                   |
|    "/PLCE/CL_APPLLOG_HELPER=======CP" bzw. /PLCE/CL_APPLLOG_HELPER=======CM004                   |
|    "CONSTRUCTOR"                                                                                 |
|    If you cannot solve the problem yourself, please send the following                           |
|    information to SAP:                                                                           |
|                                                                                                  |
|    1. The description of the problem (short dump)                                                |
|    Please press the "Local File” button in the current display.                                  |
|                                                                                                  |
|    2. The relevant system log                                                                    |
|    To do this, call the system log in transaction SM21. Restrict the time                        |
|    interval to ten minutes before the short dump and five minutes after                          |
|    it. In the display, choose System -> List -> Save -> Local File                               |
|    (unconverted).                                                                                |
|                                                                                                  |
|    3. If these are programs of your own, or modified SAP programs: The                           |
|    source text of the programs                                                                   |
|    To do this, please choose                                                                     |
|    "Other Utilities -> Upload/Download -> Download".                                             |
|                                                                                                  |
|    4. Details regarding the conditions under which the error occurred or                         |
|    which actions and input caused the error.                                                     |
|    which actions and input caused the error.                                                     |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|System environment                                                                                |
|    SAP Release..... 758                                                                          |
|    SAP Basis level 0003                                                                          |
|                                                                                                  |
|    Application server... vhmuzs4qci                                                              |
|    Instance name....... vhmuzs4qci_S4Q_00                                                        |
|    Network address...... 10.201.2.49                                                             |
|    Operating system... Linux                                                                     |
|    Release.............. 5.14.21-150500.55.12                                                    |
|    Hardware type....... x86_64                                                                   |
|    Character length..... 16 Bits                                                                 |
|    Pointer length........ 64 Bits                                                                |
|    Work process number... 43                                                                     |
|    Work process ID....... 593672                                                                 |
|    ABAP load version.... 3236                                                                    |
|    Shortdump setting. full                                                                       |
|                                                                                                  |
|    Database server... vhmuzh4qdb01                                                               |
|    Database type..... HDB                                                                        |
|    Database name..... S4Q                                                                        |
|    Database user ID SAPHANADB                                                                    |
|                                                                                                  |
|    Terminal.......... hal-w-elfo-2504                                                            |
|                                                                                                  |
|    Character set C                                                                               |
|                                                                                                  |
|    SAP kernel....... 793                                                                         |
|    Created on....... Mar 16 2026 15:07:54                                                        |
|    Created at....... Linux GNU SLES-15 x86_64  cc10.3.0 use-pr260316                             |
|    Database version SQLDBC 2.28.017.1770668846                                                   |
|    Patch level....... 352                                                                        |
|    Patch text.......                                                                             |
|    Hash............. 4861b2529e5a42dde3cd5b55896711903bce171e                                    |
|    Version.......... v7.93-8217-g4861b252                                                        |
|                                                                                                  |
|    Database............ HANA 1.0, HANA 2.0                                                       |
|    SAP database version. 793                                                                     |
|    Operating system... Linux,   GitVers: v7.93-8217-g4861b252,   GitHash:                        |
|     4861b2529e5a42dde3cd5b55896711903bce171e                                                     |
|                                                                                                  |
|    Memory consumption                                                                            |
|    EM...... 47475928 Bytes                                                                       |
|    Heap........................ 0 Bytes                                                          |
|    Page........................ 180224 Bytes                                                     |
|    MM used memory..... 32212584 Bytes                                                            |
|    MM free memory.......... 3393904 Bytes                                                        |
|    MM largest free block... 3391968 Bytes                                                        |
|    MM used blocks....... 51336                                                                   |
|    MM free blocks............ 4                                                                  |
|    MM maximum memory used... 32212584 Bytes                                                      |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|User and Transaction                                                                              |
|    Client................. 110                                                                   |
|    User.................. PROLOGAEFO                                                             |
|    Language key.......... E                                                                      |
|    Transaction......... SE37                                                                     |
|    Transaction ID...... 713C1E3CC46802B0E006A33513C9192B                                         |
|                                                                                                  |
|    EPP Root Context ID.... CB057860B5A71FE19AD85B0DC2F5F8BC                                      |
|    EPP Connection ID...... 00000000000000000000000000000000                                      |
|    EPP Connection Counter. 0                                                                     |
|    EPP Component Name..... S4Q/vhmuzs4qci_S4Q_00                                                 |
|                                                                                                  |
|    Program....................... /PLCE/CL_APPLLOG_HELPER=======CP                               |
|    Screen......................... SAPMSSY0                                1000                  |
|    Screen line.................... 6                                                             |
|    Debugger interpretor loop... "(TPDA)"                                                         |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Server-Side Connection Information                                                                |
|    Information on caller of Remote Function Call (RFC):                                          |
|    System.............. S4Q                                                                      |
|    Installation number 0021329263                                                                |
|    Database Release..... 758                                                                     |
|    Kernel Release...... 793                                                                      |
|    Connection type..... 3  (2=R/2, 3=ABAP-System, E=External,                                    |
|    R=Reg.External) call type....... synchronous and non-transactional (emode 2,                  |
|     imode 1)                                                                                     |
|    Inbound TID..........                                                                         |
|    Inbound queue name...                                                                         |
|    Outbound TID.........                                                                         |
|    Outbound queue name..                                                                         |
|                                                                                                  |
|    Client................. 110                                                                   |
|    User.................. PROLOGAEFO                                                             |
|    Transaction......... SBGRFCMON                                                                |
|    Call program...........CL_BGRFC_SUPPORTABILITY=======CP                                       |
|    Function module..... BGRFC_PREPARE_EXT_DEBUGGING                                              |
|    Call destination..... vhmuzs4qci_S4Q_00                                                       |
|    Source server...... vhmuzs4qci_S4Q_00                                                         |
|    Source IP address.. 10.201.2.49                                                               |
|                                                                                                  |
|    Additional information on RFC logon:                                                          |
|    Trusted relationship..                                                                        |
|    Logon return code.... 0                                                                       |
|    Trusted return code.. 0                                                                       |
|                                                                                                  |
|    Remarks:                                                                                      |
|    In Releases prior to 4.0, information about the RFC caller might be                           |
|    missing or incomplete.                                                                        |
|    - The installation number is provided in caller Release 700 and higher.                       |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Chain of Exception Objects                                                                        |
----------------------------------------------------------------------------------------------------
|Level        Class                                                                                |
|Attrib. Name                   Value                                                              |
----------------------------------------------------------------------------------------------------
|1            /PLCE/CX_BASEEXCEPTION                                                               |
|(Program)                      /PLCE/CL_APPLLOG_HELPER=======CP                                   |
|(Include)                      /PLCE/CL_APPLLOG_HELPER=======CM004                                |
|(Row)                          59                                                                 |
|(Short Text)                   An exception was raised                                            |
|T100_MSGID                     SY                                                                 |
|T100_MSGNO                     530                                                                |
|ATTRIBUTE_1                                                                                       |
|ATTRIBUTE_2                                                                                       |
|ATTRIBUTE_3                                                                                       |
|ATTRIBUTE_4                                                                                       |
|CLASS                                                                                             |
|FREE_TEXT                                                                                         |
|IS_RESUMABLE                                                                                      |
|KERNEL_ERRID                                                                                      |
|METHOD                                                                                            |
----------------------------------------------------------------------------------------------------
|2            CX_BALI_NOT_FOUND                                                                    |
|(Program)                      CL_BALI_LOG_DB================CP                                   |
|(Include)                      CL_BALI_LOG_DB================CM00H                                |
|(Row)                          24                                                                 |
|(Short Text)                   No log found in the database                                       |
|T100_MSGID                     BL                                                                 |
|T100_MSGNO                     208                                                                |
|IS_RESUMABLE                                                                                      |
|KERNEL_ERRID                                                                                      |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Information on where terminated                                                                   |
|    The termination occurred in ABAP program or include                                           |
|     "/PLCE/CL_APPLLOG_HELPER=======CP", in "CONSTRUCTOR". The                                    |
|    main program was "RS_TESTFRAME_CALL".                                                         |
|                                                                                                  |
|    In the source code, the termination point is in line 59 of include                            |
|     "/PLCE/CL_APPLLOG_HELPER=======CM004".                                                       |
|    include "/PLCE/CL_APPLLOG_HELPER=======CM004".                                                |
|    The termination is due to exception "/PLCE/CX_BASEEXCEPTION" occurring in                     |
|     procedure "CREATE_NOTIFICATION" "(METHOD)",                                                  |
|    but not being handled locally there or being declared in the RAISING                          |
|    clause.                                                                                       |
|    The procedure is in program "/PLCP/CL_TA_WA_ORDER_RSLT=====CP". Its source                    |
|     code begins in line 1 of                                                                     |
|    include "/PLCP/CL_TA_WA_ORDER_RSLT=====CM017".                                                |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Source Code Extract                                                                               |
----------------------------------------------------------------------------------------------------
|Line |Code                                                                                        |
----------------------------------------------------------------------------------------------------
|   29|*                                                                                           |
|   30|*    elseif iv_log_handle is initial and iv_external_id is not initial.                     |
|   31|*        data(lv_filter) = cl_bali_log_filter=>create( ).                                   |
|   32|*        try.                                                                               |
|   33|*            lv_filter = lv_filter->set_descriptor( object = iv_object subobject = iv_subob\|
|ject external_id = iv_external_id ).                                                              |
|   34|*            lt_logs = cl_bali_log_db=>get_instance( )->load_logs_via_filter( filter = lv_f\|
|ilter ).                                                                                          |
|   35|*          catch cx_bali_runtime INTO DATA(l_exception).                                    |
|   36|*          CLEANUP.                                                                         |
|   37|*            clear lt_logs.                                                                 |
|   38|*        endtry.                                                                            |
|   39|*                                                                                           |
|   40|*        if lines( lt_logs ) > 0.                                                           |
|   41|*            lv_log_handle = lt_logs[ 1 ]->get_handle( ).                                   |
|   42|*        endif.                                                                             |
|   43|*                                                                                           |
|   44|*    endif.                                                                                 |
|   45|*                                                                                           |
|   46|*    clear:                                                                                 |
|   47|*      FO_LOG.                                                                              |
|   48|                                                                                            |
|   49|    try.                                                                                    |
|   50|        if IV_LOG_HANDLE is initial.                                                        |
|   51|          FO_LOG = CL_BALI_LOG=>CREATE_WITH_HEADER( HEADER = CL_BALI_HEADER_SETTER=>CREATE(\|
| OBJECT = IV_OBJECT                                                                               |
|   52|                                                                                           \|
| SUBOBJECT = IV_SUBOBJECT                                                                         |
|   53|                                                                                           \|
| EXTERNAL_ID = IV_EXTERNAL_ID ) ).                                                                |
|   54|        else.                                                                               |
|   55|          FO_LOG_DB = CL_BALI_LOG_DB=>GET_INSTANCE( ).                                      |
|   56|          FO_LOG = FO_LOG_DB->LOAD_LOG( HANDLE = IV_LOG_HANDLE ).                           |
|   57|        endif.                                                                              |
|   58|      catch CX_BALI_RUNTIME into data(LRO_BALI_EXCEPTION).                                  |
|>>>>>|        raise exception type /PLCE/CX_BASEEXCEPTION                                         |
|   60|          exporting                                                                         |
|   61|            PREVIOUS = LRO_BALI_EXCEPTION.                                                  |
|   62|    endtry.                                                                                 |
|   63|                                                                                            |
|   64|    if FO_LOG is initial.                                                                   |
|   65|      raise exception type /PLCE/CX_BASEEXCEPTION                                           |
|   66|        message id /PLCE/CX_BASEEXCEPTION=>CS_NO_APPLICATION_LOG_CREATED-MSGID              |
|   67|        type IF_BALI_CONSTANTS=>C_SEVERITY_ERROR                                            |
|   68|        number /PLCE/CX_BASEEXCEPTION=>CS_NO_APPLICATION_LOG_CREATED-MSGNO.                 |
|   69|    endif.                                                                                  |
|   70|  endmethod.                                                                                |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Contents of system fields                                                                         |
----------------------------------------------------------------------------------------------------
|Name    |Val.                                                                                     |
----------------------------------------------------------------------------------------------------
|SY-SUBRC|0                                                                                        |
|SY-INDEX|1                                                                                        |
|SY-TABIX|3                                                                                        |
|SY-DBCNT|0                                                                                        |
|SY-FDPOS|0                                                                                        |
|SY-LSIND|0                                                                                        |
|SY-PAGNO|0                                                                                        |
|SY-LINNO|1                                                                                        |
|SY-COLNO|1                                                                                        |
|SY-PFKEY|STLI                                                                                     |
|SY-UCOMM|STRT                                                                                     |
|SY-TITLE|Call test environment                                                                    |
|SY-MSGTY|W                                                                                        |
|SY-MSGID|                                                                                         |
|SY-MSGNO|483                                                                                      |
|SY-MSGV1|438673                                                                                   |
|SY-MSGV2|                                                                                         |
|SY-MSGV3|                                                                                         |
|SY-MSGV4|                                                                                         |
|SY-MODNO|2                                                                                        |
|SY-DATUM|20260618                                                                                 |
|SY-UZEIT|061648                                                                                   |
|SY-XPROG|SAPLEEWA_EL_BASE                                                                         |
|SY-XFORM|CONVERSION_EXIT                                                                          |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Active Calls/Events                                                                               |
----------------------------------------------------------------------------------------------------
|No.   Ty.          Program                             Include                             Line   |
|      Name                                                                                        |
----------------------------------------------------------------------------------------------------
|   13 METHOD       /PLCE/CL_APPLLOG_HELPER=======CP    /PLCE/CL_APPLLOG_HELPER=======CM004    59  |
|      /PLCE/CL_APPLLOG_HELPER=>CONSTRUCTOR                                                        |
|   12 METHOD       /PLCP/CL_TA_WA_ORDER_RSLT=====CP    /PLCP/CL_TA_WA_ORDER_RSLT=====CM017    17  |
|      /PLCP/CL_TA_WA_ORDER_RSLT=>CREATE_NOTIFICATION                                              |
|   11 METHOD       /PLCP/CL_TA_WA_ORDER_RSLT=====CP    /PLCP/CL_TA_WA_ORDER_RSLT=====CM00Z    24  |
|      /PLCP/CL_TA_WA_ORDER_RSLT=>BOOK_IMPORT_SERVICE_SEQUENCES                                    |
|   10 METHOD       ZCL_PLCP_TA_WA_ORDER_RSLT=====CP    ZCL_PLCP_TA_WA_ORDER_RSLT=====CM001     2  |
|      ZCL_PLCP_TA_WA_ORDER_RSLT=>CALL_METHOD                                                      |
|    9 METHOD       CL_EEWA_TRANSACTION===========CP    CL_EEWA_TRANSACTION===========CM00V     8  |
|      CL_EEWA_TRANSACTION=>CALL                                                                   |
|    8 METHOD       CL_EEWA_TRANSACTION===========CP    CL_EEWA_TRANSACTION===========CM00B     4  |
|      CL_EEWA_TRANSACTION=>EXECUTE                                                                |
|    7 METHOD       /PLCP/CL_WA_CONFIRMATION======CP    /PLCP/CL_WA_CONFIRMATION======CM003    30  |
|      /PLCP/CL_WA_CONFIRMATION=>WRITE_DATA_TO_EEWA                                                |
|    6 METHOD       /PLCP/CL_WA_CONFIRMATION======CP    /PLCP/CL_WA_CONFIRMATION======CM007    15  |
|      /PLCP/CL_WA_CONFIRMATION=>SEQUENCE_SERVICES                                                 |
|    5 FUNCTION     /PLCP/SAPLEEWA_CONFIRMATION         /PLCP/LEEWA_CONFIRMATIONU01            39  |
|      /PLCP/EEWA_ORDER_CONFIRM_RFC                                                                |
|    4 FORM         /PLCP/EEWA_ORDER_CONFIRM_RFC==FT    /PLCP/EEWA_ORDER_CONFIRM_RFC==FT       62  |
|      FUNCTION_CALL                                                                               |
|    3 FUNCTION     SAPLSEUJ                            LSEUJU12                               31  |
|      SFCS_FA_FUNCTION_INVOCE                                                                     |
|    2 FUNCTION     SAPLSEUJ                            LSEUJU33                              216  |
|      SFCS_FA_TEST_FUNCTION                                                                       |
|    1 EVENT        RS_TESTFRAME_CALL                   RS_TESTFRAME_CALL                      16  |
|      START-OF-SELECTION                                                                          |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Selected Variables                                                                                |
----------------------------------------------------------------------------------------------------
|Name                                                                                              |
|    Val.                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      13 Ty.          METHOD                                                                   |
|Name  /PLCE/CL_APPLLOG_HELPER=>CONSTRUCTOR                                                        |
----------------------------------------------------------------------------------------------------
|IV_OBJECT                                                                                         |
|                                                                                                  |
|    22222222222222222222                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    20002000200020002000200020002000200020002000200020002000200020002000200020002000              |
|IV_SUBOBJECT                                                                                      |
|                                                                                                  |
|    22222222222222222222                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    20002000200020002000200020002000200020002000200020002000200020002000200020002000              |
|IV_EXTERNAL_ID                                                                                    |
|                                                                                                  |
|    222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222\|
|2222222                                                                                           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000                                                                                           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000                                                                                           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000                                                                                           |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000                                                                                  |
|IV_LOG_HANDLE                                                                                     |
|    omLuOBMd7z6QsP1FRmsEk0                                                                        |
|    6647444637357534567463                                                                        |
|    FDC5F2D47A6130162D35B0                                                                        |
|    0000000000000000000000                                                                        |
|    0000000000000000000000                                                                        |
|    6F006D004C0075004F0042004D00640037007A0036005100730050003100460052006D00730045006B003000      |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|/PLCE/CX_BASEEXCEPTION=>CS_APPL_LOG_ALREADY_CREATED-MSGID                                         |
|    /PLCE/MC_BASEMESSAGE                                                                          |
|    25444244544544455444                                                                          |
|    F0C35FD3F2135D533175                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    2F0050004C00430045002F004D0043005F0042004100530045004D00450053005300410047004500              |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|/PLCE/CX_BASEEXCEPTION=>CS_NO_APPLICATION_LOG_CREATED-MSGID                                       |
|    /PLCE/MC_BASEMESSAGE                                                                          |
|    25444244544544455444                                                                          |
|    F0C35FD3F2135D533175                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    2F0050004C00430045002F004D0043005F0042004100530045004D00450053005300410047004500              |
|LRO_BALI_EXCEPTION                                                                                |
|    {O:2720*\CLASS=CX_BALI_NOT_FOUND}                                                             |
|    5000A000                                                                                      |
|    F0000A00                                                                                      |
|    5F000000A00A0000                                                                              |
|SY-REPID                                                                                          |
|    /PLCE/CL_APPLLOG_HELPER=======CP                                                              |
|    2544424454554444544454533333334522222222                                                      |
|    F0C35F3CF100CCF7F85C052DDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430045002F0043004C005F004100500050004C004C004F0047005F00480045004C005000450052003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SY                                                                                                |
|    {1;0;3;1;0;0;0;0;0;0;0;0;1;0;1;0;0;0;0;94;0;0;20;0;3;36;8;0;0;0;0;0;0;0;0;0;0;0;29;188;0;3;0;\|
|3600;X;C;0000;4;0;0;;;;E;2;;;X;1000;;;;;;1;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;STLI;758;SE37;STRT;;;0000000000;0000000000;;00000000;2                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000050000000000010000\|
|000000020000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    1000000030001000000000000000000000000000000000001000000010000000000000000000E0000000000040000\|
|00030004000800000000000000000000000000000000000000000000000D000C0000000300000000E0080300040000C00\|
|00C000000050002000000080100000000000000000000000001000F0F03000000                                 |
|    010000000000000003000000010000000000000000000000000000000000000000000000000000000000000000000\|
|000010000000000000001000000000000000000000000000000000000005E000000000000000000000014000000000000\|
|0003000000240000000800000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001D000000BC000000000000000300000000000000100E000058004300000004000000000C00000000\|
|0C00200020002000450000000200000020002000580031003000300030002000200020002000200020002000200000000\|
|10000005F005F0053002000200020                                                                     |
|%_EXCP                                                                                            |
|    {O:2721*\CLASS=/PLCE/CX_BASEEXCEPTION}                                                        |
|    5000A000                                                                                      |
|    E0001A00                                                                                      |
|    5E000000A10A0000                                                                              |
|ME->FO_LOG                                                                                        |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|%_EXCP%_#E39                                                                                      |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
----------------------------------------------------------------------------------------------------
|No.      12 Ty.          METHOD                                                                   |
|Name  /PLCP/CL_TA_WA_ORDER_RSLT=>CREATE_NOTIFICATION                                              |
----------------------------------------------------------------------------------------------------
|PAR_USER_ID                                                                                       |
|    PROLOGAFN                                                                                     |
|    554444444222                                                                                  |
|    02FCF716E000                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    500052004F004C004F004700410046004E00200020002000                                              |
|PAR_NOTIFKEY                                                                                      |
|    WASTE_SERVICE_SAVE_SUCCESS                                                                    |
|    54554554554445545455544455222222                                                              |
|    71345F3526935F3165F3533533000000                                                              |
|    00000000000000000000000000000000                                                              |
|    00000000000000000000000000000000                                                              |
|    570041005300540045005F0053004500520056004900430045005F0053004100560045005F0053005500430043004\|
|50053005300200020002000200020002000                                                               |
|PAR_TOUR_ID                                                                                       |
|    00000000000000000852                                                                          |
|    33333333333333333333                                                                          |
|    00000000000000000852                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    30003000300030003000300030003000300030003000300030003000300030003000380035003200              |
|SYST-REPID                                                                                        |
|    /PLCP/CL_TA_WA_ORDER_RSLT=====CP                                                              |
|    2544524455455454544555545333334522222222                                                      |
|    F0C30F3CF41F71FF2452F23C4DDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F0043004C005F00540041005F00570041005F004F0052004400450052005F00520053004\|
|C0054003D003D003D003D003D004300500020002000200020002000200020002000                               |
|/PLCE/IF_NOTIFICATION_API=>GCS_PRIORITIES-NEUTRAL                                                 |
|    NEUTRAL                                                                                       |
|    44555442222222222222                                                                          |
|    E55421C0000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    4E00450055005400520041004C002000200020002000200020002000200020002000200020002000              |
|ME->FCALLBACK                                                                                     |
|    {O:1916*\CLASS=CL_EEWA_IMPL_BO_CALLBACK_MLOG}                                                 |
|    80007000                                                                                      |
|    3000C700                                                                                      |
|    830000007C070000                                                                              |
|CL_EEWA_BO_SERVICETYPE=>CSC_TRANSPORT                                                             |
|    02                                                                                            |
|    33                                                                                            |
|    02                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    30003200                                                                                      |
|ABAP_FALSE                                                                                        |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|LLOG_EEWA                                                                                         |
|    {O:1916*\CLASS=CL_EEWA_IMPL_BO_CALLBACK_MLOG}                                                 |
|    80007000                                                                                      |
|    3000C700                                                                                      |
|    830000007C070000                                                                              |
|SY-ZONLO                                                                                          |
|    CET                                                                                           |
|    445222                                                                                        |
|    354000                                                                                        |
|    000000                                                                                        |
|    000000                                                                                        |
|    430045005400200020002000                                                                      |
|LLOGHANDLE                                                                                        |
|    omLuOBMd7z6QsP1FRmsEk0                                                                        |
|    6647444637357534567463                                                                        |
|    FDC5F2D47A6130162D35B0                                                                        |
|    0000000000000000000000                                                                        |
|    0000000000000000000000                                                                        |
|    6F006D004C0075004F0042004D00640037007A0036005100730050003100460052006D00730045006B003000      |
|LLOG_EEWA->MSGLOG->HANDLE                                                                         |
|    omLuOBMd7z6QsP1FRmsEk0                                                                        |
|    6647444637357534567463                                                                        |
|    FDC5F2D47A6130162D35B0                                                                        |
|    0000000000000000000000                                                                        |
|    0000000000000000000000                                                                        |
|    6F006D004C0075004F0042004D00640037007A0036005100730050003100460052006D00730045006B003000      |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|LLOG_PLCE                                                                                         |
|    {O:2717*\CLASS=/PLCE/CL_APPLLOG_HELPER}                                                       |
|    60009000                                                                                      |
|    2000DA00                                                                                      |
|    620000009D0A0000                                                                              |
|CL_EEWA_BO_SERVICETYPE=>CSC_SWITCH                                                                |
|    07                                                                                            |
|    33                                                                                            |
|    07                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    30003700                                                                                      |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_##TVREG_001                                                                                     |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|SY-REPID                                                                                          |
|    /PLCP/CL_TA_WA_ORDER_RSLT=====CP                                                              |
|    2544524455455454544555545333334522222222                                                      |
|    F0C30F3CF41F71FF2452F23C4DDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F0043004C005F00540041005F00570041005F004F0052004400450052005F00520053004\|
|C0054003D003D003D003D003D004300500020002000200020002000200020002000                               |
----------------------------------------------------------------------------------------------------
|No.      11 Ty.          METHOD                                                                   |
|Name  /PLCP/CL_TA_WA_ORDER_RSLT=>BOOK_IMPORT_SERVICE_SEQUENCES                                    |
----------------------------------------------------------------------------------------------------
|IF_BALI_CONSTANTS=>C_SEVERITY_STATUS                                                              |
|    S                                                                                             |
|    5                                                                                             |
|    3                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5300                                                                                          |
|ME->ORDERRESULTS                                                                                  |
|    Table IT_28334[1x296]                                                                         |
|    {O:1920*\CLASS=ZCL_PLCP_TA_WA_ORDER_RSLT}\DATA=ORDERRESULTS                                   |
|    Table reference: 5641                                                                         |
|    TABH+ 0(20) = 08D365A5BF7F00000000000000000000B82367A5                                        |
|    TABH+20(20) = BF7F000000000000000000000100000044040000                                        |
|    TABH+40(20) = 0000000009160000AE6E00000100000028010000                                        |
|    TABH+60(20) = FFFFFFFF042A0200B0B706000400010602000000                                        |
|    store                   = 0x08D365A5BF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0xB82367A5BF7F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 5641  (0x09160000)                                                  |
|    label                   = 28334 (0xAE6E0000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 296   (0x28010000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000554/TYPE#000068                                             |
|    occu                    = 4     (0x04000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 0                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x001F67A5BF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 15011 (0xA33A0000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0x5802CDA0BF7F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|    {;00000000000000000852;20260616;0000005427;Table[initial];;}                                  |
|    202020202020202020202020202020202020202030303030303030303030303030303030303030303030303030303\|
|0303030303030303030303000000000FFFF20202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000008050202000206000601\|
|0600000000000005040207000000000FFFF00000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020003000300030003\|
|0003000300030003000300030003000300030003000300030003000380035003200320030003200360030003600310036\|
|0030003000300030003000300035003400320037000000000000000000FFFFFFFF2000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|ME->ORDERRESSRESULTS                                                                              |
|    Table[initial]                                                                                |
|                                                                                                  |
|ME->ORDERITEMRESULTS                                                                              |
|    Table IT_28335[1x608]                                                                         |
|    {O:1920*\CLASS=ZCL_PLCP_TA_WA_ORDER_RSLT}\DATA=ORDERITEMRESULTS                               |
|    Table reference: 5711                                                                         |
|    TABH+ 0(20) = F002CDA0BF7F00000000000000000000A8C566A5                                        |
|    TABH+20(20) = BF7F000000000000000000000100000044040000                                        |
|    TABH+40(20) = 000000004F160000AF6E00000100000060020000                                        |
|    TABH+60(20) = FFFFFFFF042A020070B506000400010602000000                                        |
|    store                   = 0xF002CDA0BF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0xA8C566A5BF7F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 5711  (0x4F160000)                                                  |
|    label                   = 28335 (0xAF6E0000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 608   (0x60020000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000554/TYPE#000056                                             |
|    occu                    = 4     (0x04000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 0                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x50307DA5BF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 15012 (0xA43A0000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0x68814FA7BF7F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|    {;00000000000000000852;20260616;0000005427;WE00000000000000006613;;0000;;0;;0;;;;0.000;;CB057\|
|860B5A71FD19AAA9FB772B9308E;0.0000000;Table[initial];X;Table[initial];0.000000000000;0.0000000000\|
|00;Table[initial]}                                                                                |
|    202020202020202020202020202020202020202030303030303030303030303030303030303030303030303030303\|
|0303030303030303030303050403030303030303030303030303030303030303030203030303020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202000000                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000008050202000206000601\|
|0600000000000005040207070500000000000000000000000000000000060601030000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020003000300030003\|
|0003000300030003000300030003000300030003000300030003000380035003200320030003200360030003600310036\|
|0030003000300030003000300035003400320037005700450030003000300030003000300030003000300030003000300\|
|0300030003000300036003600310033002000300030003000300020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020000000000000                                                                     |
|ME->ORDERWEIGHRESULTS                                                                             |
|    Table[initial]                                                                                |
|                                                                                                  |
|ME                                                                                                |
|    {O:1920*\CLASS=ZCL_PLCP_TA_WA_ORDER_RSLT}                                                     |
|    70008000                                                                                      |
|    F0000700                                                                                      |
|    7F00000080070000                                                                              |
|LRESULTREF                                                                                        |
|    {A:36*\TYPE=/PLCE/SWR_TOUR_RSLT}                                                              |
|    E0002000                                                                                      |
|    A0004000                                                                                      |
|    EA00000024000000                                                                              |
|ME->DO_COMMIT                                                                                     |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SYST                                                                                              |
|    {1;0;3;1;0;0;0;0;0;0;0;0;1;0;1;0;0;0;0;94;0;0;20;0;3;36;8;0;0;0;0;0;0;0;0;0;0;0;29;188;0;3;0;\|
|3600;X;C;0000;4;0;0;;;;E;2;;;X;1000;;;;;;1;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;STLI;758;SE37;STRT;;;0000000000;0000000000;;00000000;2                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000050000000000010000\|
|000000020000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    1000000030001000000000000000000000000000000000001000000010000000000000000000E0000000000040000\|
|00030004000800000000000000000000000000000000000000000000000D000C0000000300000000E0080300040000C00\|
|00C000000050002000000080100000000000000000000000001000F0F03000000                                 |
|    010000000000000003000000010000000000000000000000000000000000000000000000000000000000000000000\|
|000010000000000000001000000000000000000000000000000000000005E000000000000000000000014000000000000\|
|0003000000240000000800000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001D000000BC000000000000000300000000000000100E000058004300000004000000000C00000000\|
|0C00200020002000450000000200000020002000580031003000300030002000200020002000200020002000200000000\|
|10000005F005F0053002000200020                                                                     |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|ME->USER_ID                                                                                       |
|    PROLOGAFN                                                                                     |
|    554444444222                                                                                  |
|    02FCF716E000                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    500052004F004C004F004700410046004E00200020002000                                              |
|LRESULTREF->TOURID                                                                                |
|    00000000000000000852                                                                          |
|    33333333333333333333                                                                          |
|    00000000000000000852                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    30003000300030003000300030003000300030003000300030003000300030003000380035003200              |
|CL_EEWA_CONSTANTS_CONFIRMATION=>CWDOCNFSTATUS_ITEM-BILLED                                         |
|    IWA16                                                                                         |
|    45433                                                                                         |
|    97116                                                                                         |
|    00000                                                                                         |
|    00000                                                                                         |
|    49005700410031003600                                                                          |
----------------------------------------------------------------------------------------------------
|No.      10 Ty.          METHOD                                                                   |
|Name  ZCL_PLCP_TA_WA_ORDER_RSLT=>CALL_METHOD                                                      |
----------------------------------------------------------------------------------------------------
|PAR_PARAMS                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|PAR_NAME                                                                                          |
|    BOOK_IMPORT_SERVICE_SEQUENCES                                                                 |
|    44445445455554554445545544445                                                                 |
|    2FFBF9D0F24F3526935F35155E353                                                                 |
|    00000000000000000000000000000                                                                 |
|    00000000000000000000000000000                                                                 |
|    42004F004F004B005F0049004D0050004F00520054005F0053004500520056004900430045005F005300450051005\|
|50045004E00430045005300                                                                           |
|SY-REPID                                                                                          |
|    ZCL_PLCP_TA_WA_ORDER_RSLT=====CP                                                              |
|    5445544555455454544555545333334522222222                                                      |
|    A3CF0C30F41F71FF2452F23C4DDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5A0043004C005F0050004C00430050005F00540041005F00570041005F004F0052004400450052005F00520053004\|
|C0054003D003D003D003D003D004300500020002000200020002000200020002000                               |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ME                                                                                                |
|    {O:1920*\CLASS=ZCL_PLCP_TA_WA_ORDER_RSLT}                                                     |
|    70008000                                                                                      |
|    F0000700                                                                                      |
|    7F00000080070000                                                                              |
----------------------------------------------------------------------------------------------------
|No.       9 Ty.          METHOD                                                                   |
|Name  CL_EEWA_TRANSACTION=>CALL                                                                   |
----------------------------------------------------------------------------------------------------
|PAR_NAME                                                                                          |
|    BOOK_IMPORT_SERVICE_SEQUENCES                                                                 |
|    44445445455554554445545544445                                                                 |
|    2FFBF9D0F24F3526935F35155E353                                                                 |
|    00000000000000000000000000000                                                                 |
|    00000000000000000000000000000                                                                 |
|    42004F004F004B005F0049004D0050004F00520054005F0053004500520056004900430045005F005300450051005\|
|50045004E00430045005300                                                                           |
|PAR_PARAMS                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|SY-REPID                                                                                          |
|    CL_EEWA_TRANSACTION===========CP                                                              |
|    4454454555445445444333333333334522222222                                                      |
|    3CF5571F421E31349FEDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0045004500570041005F005400520041004E00530041004300540049004F004E003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SYST-REPID                                                                                        |
|    CL_EEWA_TRANSACTION===========CP                                                              |
|    4454454555445445444333333333334522222222                                                      |
|    3CF5571F421E31349FEDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0045004500570041005F005400520041004E00530041004300540049004F004E003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME->FPLUGINS                                                                                      |
|    {O:1924*\CLASS=CL_EEWA_PLUGINLIST_TA}                                                         |
|    70008000                                                                                      |
|    B0004700                                                                                      |
|    7B00000084070000                                                                              |
|SYST                                                                                              |
|    {1;0;3;1;0;0;0;0;0;0;0;0;1;0;1;0;0;0;0;94;0;0;20;0;3;36;8;0;0;0;0;0;0;0;0;0;0;0;29;188;0;3;0;\|
|3600;X;C;0000;4;0;0;;;;E;2;;;X;1000;;;;;;1;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;STLI;758;SE37;STRT;;;0000000000;0000000000;;00000000;2                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000050000000000010000\|
|000000020000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    1000000030001000000000000000000000000000000000001000000010000000000000000000E0000000000040000\|
|00030004000800000000000000000000000000000000000000000000000D000C0000000300000000E0080300040000C00\|
|00C000000050002000000080100000000000000000000000001000F0F03000000                                 |
|    010000000000000003000000010000000000000000000000000000000000000000000000000000000000000000000\|
|000010000000000000001000000000000000000000000000000000000005E000000000000000000000014000000000000\|
|0003000000240000000800000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001D000000BC000000000000000300000000000000100E000058004300000004000000000C00000000\|
|0C00200020002000450000000200000020002000580031003000300030002000200020002000200020002000200000000\|
|10000005F005F0053002000200020                                                                     |
|LHANDLED                                                                                          |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY-XFORM                                                                                          |
|    CONVERSION_EXIT                                                                               |
|    444545544454545222222222222222                                                                |
|    3FE65239FEF5894000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    43004F004E00560045005200530049004F004E005F004500580049005400200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|SY                                                                                                |
|    {1;0;3;1;0;0;0;0;0;0;0;0;1;0;1;0;0;0;0;94;0;0;20;0;3;36;8;0;0;0;0;0;0;0;0;0;0;0;29;188;0;3;0;\|
|3600;X;C;0000;4;0;0;;;;E;2;;;X;1000;;;;;;1;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;STLI;758;SE37;STRT;;;0000000000;0000000000;;00000000;2                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000050000000000010000\|
|000000020000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    1000000030001000000000000000000000000000000000001000000010000000000000000000E0000000000040000\|
|00030004000800000000000000000000000000000000000000000000000D000C0000000300000000E0080300040000C00\|
|00C000000050002000000080100000000000000000000000001000F0F03000000                                 |
|    010000000000000003000000010000000000000000000000000000000000000000000000000000000000000000000\|
|000010000000000000001000000000000000000000000000000000000005E000000000000000000000014000000000000\|
|0003000000240000000800000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001D000000BC000000000000000300000000000000100E000058004300000004000000000C00000000\|
|0C00200020002000450000000200000020002000580031003000300030002000200020002000200020002000200000000\|
|10000005F005F0053002000200020                                                                     |
|ME                                                                                                |
|    {O:1920*\CLASS=ZCL_PLCP_TA_WA_ORDER_RSLT}                                                     |
|    70008000                                                                                      |
|    F0000700                                                                                      |
|    7F00000080070000                                                                              |
|%_PRINT                                                                                           |
|    {;000;;;;;;0;0;0;;;;;;;;;;;0;;;0}                                                             |
|    202020203030302020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020203000000000\|
|00202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200030003000300020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000300000000000000000000000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       8 Ty.          METHOD                                                                   |
|Name  CL_EEWA_TRANSACTION=>EXECUTE                                                                |
----------------------------------------------------------------------------------------------------
|PAR_NAME                                                                                          |
|    BOOK_IMPORT_SERVICE_SEQUENCES                                                                 |
|    44445445455554554445545544445                                                                 |
|    2FFBF9D0F24F3526935F35155E353                                                                 |
|    00000000000000000000000000000                                                                 |
|    00000000000000000000000000000                                                                 |
|    42004F004F004B005F0049004D0050004F00520054005F0053004500520056004900430045005F005300450051005\|
|50045004E00430045005300                                                                           |
|ME->EXCEPTION                                                                                     |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|ME                                                                                                |
|    {O:1920*\CLASS=ZCL_PLCP_TA_WA_ORDER_RSLT}                                                     |
|    70008000                                                                                      |
|    F0000700                                                                                      |
|    7F00000080070000                                                                              |
|ME->DO_COMMIT                                                                                     |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|CL_ABAP_BADI_TRACER=>XTRACE_LEVEL                                                                 |
|    0                                                                                             |
|    00                                                                                            |
|    00                                                                                            |
|    0000                                                                                          |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|CL_ABAP_BADI_TRACER=>C_XTRACE-DEFAULT                                                             |
|    300                                                                                           |
|    20                                                                                            |
|    C1                                                                                            |
|    2C01                                                                                          |
|ME->DO_RAISE_EXCEPTION                                                                            |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
----------------------------------------------------------------------------------------------------
|No.       7 Ty.          METHOD                                                                   |
|Name  /PLCP/CL_WA_CONFIRMATION=>WRITE_DATA_TO_EEWA                                                |
----------------------------------------------------------------------------------------------------
|IT_TOUR_DATA                                                                                      |
|    Table IT_28321[1x744]                                                                         |
|    \CLASS=/PLCP/CL_WA_CONFIRMATION\METHOD=SEQUENCE_SERVICES\DATA=LTOURS_RES                      |
|    Table reference: 5670                                                                         |
|    TABH+ 0(20) = 489F4AA7BF7F0000000000000000000000FE3FA7                                        |
|    TABH+20(20) = BF7F000000000000000000000100000044040000                                        |
|    TABH+40(20) = 0000000026160000A16E000001000000E8020000                                        |
|    TABH+60(20) = FFFFFFFF042F0100305803000400010602000000                                        |
|    store                   = 0x489F4AA7BF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x00FE3FA7BF7F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 5670  (0x26160000)                                                  |
|    label                   = 28321 (0xA16E0000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 744   (0xE8020000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000303/TYPE#000015                                             |
|    occu                    = 4     (0x04000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 0                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x3832FFA6BF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 15000 (0x983A0000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0x10BD41A7BF7F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|    {00000000000000000852;20260615220000.0000000;0000005427;0000005427;;Table[initial];Table[init\|
|ial];Table[initial];Table[initial];IT_28322[1x1616]}                                              |
|    303030303030303030303030303030303030303022012000000030303030303030303030202020202020202020202\|
|0202020202020202020303030303030303030302020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    00000000000000000000000000000000008050200665200000C000000000000050402070000000000000000000000\|
|0000000000000000000000000000000504020700000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    300030003000300030003000300030003000300030003000300030003000300030003800350032002026061522000\|
|00000000C0030003000300030003000300035003400320037002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000300030003000300030003000350034003200370020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|IS_SEQUENCE_CONTROL                                                                               |
|    {X;X;PROLOGAFN}                                                                               |
|    5050505040404040404040202020                                                                  |
|    80800020F0C0F0701060E0000000                                                                  |
|    58005800500052004F004C004F004700410046004E00200020002000                                      |
|CT_BAPIRET                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY                                                                                                |
|    {1;0;3;1;0;0;0;0;0;0;0;0;1;0;1;0;0;0;0;94;0;0;20;0;3;36;8;0;0;0;0;0;0;0;0;0;0;0;29;188;0;3;0;\|
|3600;X;C;0000;4;0;0;;;;E;2;;;X;1000;;;;;;1;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;STLI;758;SE37;STRT;;;0000000000;0000000000;;00000000;2                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000050000000000010000\|
|000000020000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    1000000030001000000000000000000000000000000000001000000010000000000000000000E0000000000040000\|
|00030004000800000000000000000000000000000000000000000000000D000C0000000300000000E0080300040000C00\|
|00C000000050002000000080100000000000000000000000001000F0F03000000                                 |
|    010000000000000003000000010000000000000000000000000000000000000000000000000000000000000000000\|
|000010000000000000001000000000000000000000000000000000000005E000000000000000000000014000000000000\|
|0003000000240000000800000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001D000000BC000000000000000300000000000000100E000058004300000004000000000C00000000\|
|0C00200020002000450000000200000020002000580031003000300030002000200020002000200020002000200000000\|
|10000005F005F0053002000200020                                                                     |
|LTATA                                                                                             |
|    {O:1920*\CLASS=ZCL_PLCP_TA_WA_ORDER_RSLT}                                                     |
|    70008000                                                                                      |
|    F0000700                                                                                      |
|    7F00000080070000                                                                              |
|CL_ABAP_BEHV_CTRL=>BLOCKED                                                                        |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|IS_SEQUENCE_CONTROL-SEQUENCE_OPERATIONAL                                                          |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SYST                                                                                              |
|    {1;0;3;1;0;0;0;0;0;0;0;0;1;0;1;0;0;0;0;94;0;0;20;0;3;36;8;0;0;0;0;0;0;0;0;0;0;0;29;188;0;3;0;\|
|3600;X;C;0000;4;0;0;;;;E;2;;;X;1000;;;;;;1;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;STLI;758;SE37;STRT;;;0000000000;0000000000;;00000000;2                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000050000000000010000\|
|000000020000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    1000000030001000000000000000000000000000000000001000000010000000000000000000E0000000000040000\|
|00030004000800000000000000000000000000000000000000000000000D000C0000000300000000E0080300040000C00\|
|00C000000050002000000080100000000000000000000000001000F0F03000000                                 |
|    010000000000000003000000010000000000000000000000000000000000000000000000000000000000000000000\|
|000010000000000000001000000000000000000000000000000000000005E000000000000000000000014000000000000\|
|0003000000240000000800000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001D000000BC000000000000000300000000000000100E000058004300000004000000000C00000000\|
|0C00200020002000450000000200000020002000580031003000300030002000200020002000200020002000200000000\|
|10000005F005F0053002000200020                                                                     |
|IS_SEQUENCE_CONTROL-SEQUENCE_MASTERDATA                                                           |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|%_PRINT                                                                                           |
|    {;000;;;;;;0;0;0;;;;;;;;;;;0;;;0}                                                             |
|    202020203030302020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020203000000000\|
|00202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200030003000300020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000300000000000000000000000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       6 Ty.          METHOD                                                                   |
|Name  /PLCP/CL_WA_CONFIRMATION=>SEQUENCE_SERVICES                                                 |
----------------------------------------------------------------------------------------------------
|IV_PROFILE                                                                                        |
|    EWACS                                                                                         |
|    45445222222222222222                                                                          |
|    57133000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    45005700410043005300200020002000200020002000200020002000200020002000200020002000              |
|IT_KEYS                                                                                           |
|    Table IT_1622[1x32]                                                                           |
|    \FUNCTION=/PLCP/EEWA_ORDER_CONFIRM_RFC\DATA=IT_ASSIGNMENT_KEYS                                |
|    Table reference: 168                                                                          |
|    TABH+ 0(20) = 303EBC9CBF7F00000000000000000000907F3EA7                                        |
|    TABH+20(20) = BF7F000000000000000000000100000044010000                                        |
|    TABH+40(20) = 00000000A8000000560600000100000020000000                                        |
|    TABH+60(20) = FFFFFFFF042E0100C03600000100410602000000                                        |
|    store                   = 0x303EBC9CBF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x907F3EA7BF7F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 2     (cmpSingleMcmpR)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 168   (0xA8000000)                                                  |
|    label                   = 1622  (0x56060000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 32    (0x20000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000302/TYPE#000010                                             |
|    occu                    = 1     (0x01000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 1                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x5005B19CBF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 263   (0x07010000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 1     (0x01000000)                                                  |
|    lineAlloc               = 1     (0x01000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0x40C641A7BF7F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|    {CB057860B5A71FD19989D4C99A22601D;CB057860B5A71FD19AAA9FB772B9308E}                           |
|    C076BA1D98DC9261C076BA1D9A9B7B38                                                              |
|    B58057F19949A20DB58057F1AAF7290E                                                              |
|    CB057860B5A71FD19989D4C99A22601DCB057860B5A71FD19AAA9FB772B9308E                              |
|IS_SEQUENCE_CONTROL                                                                               |
|    {X;X;PROLOGAFN}                                                                               |
|    5050505040404040404040202020                                                                  |
|    80800020F0C0F0701060E0000000                                                                  |
|    58005800500052004F004C004F004700410046004E00200020002000                                      |
|CT_BAPIRET                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SYST-REPID                                                                                        |
|    /PLCP/CL_WA_CONFIRMATION======CP                                                              |
|    2544524455454444454454443333334522222222                                                      |
|    F0C30F3CF71F3FE692D149FEDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F0043004C005F00570041005F0043004F004E004600490052004D004100540049004F004\|
|E003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME                                                                                                |
|    {O:156*\CLASS=/PLCP/CL_WA_CONFIRMATION}                                                       |
|    60009000                                                                                      |
|    3000C000                                                                                      |
|    630000009C000000                                                                              |
|LTOURS_RES                                                                                        |
|    Table IT_28321[1x744]                                                                         |
----------------------------------------------------------------------------------------------------
|No.       5 Ty.          FUNCTION                                                                 |
|Name  /PLCP/EEWA_ORDER_CONFIRM_RFC                                                                |
----------------------------------------------------------------------------------------------------
|IS_ASSIGNMENT_CONTROL                                                                             |
|    {X;X;PROLOGAFN}                                                                               |
|    5050505040404040404040202020                                                                  |
|    80800020F0C0F0701060E0000000                                                                  |
|    58005800500052004F004C004F004700410046004E00200020002000                                      |
|IT_ASSIGNMENT_KEYS                                                                                |
|    Table IT_1622[1x32]                                                                           |
|IT_SERVICE_KEYS                                                                                   |
|    Table IT_1623[1x16]                                                                           |
|    \FUNCTION=/PLCP/EEWA_ORDER_CONFIRM_RFC\DATA=IT_SERVICE_KEYS                                   |
|    Table reference: 118                                                                          |
|    TABH+ 0(20) = 38BCD49CBF7F000000000000000000002053B3A0                                        |
|    TABH+20(20) = BF7F000000000000000000000100000044010000                                        |
|    TABH+40(20) = 0000000076000000570600000100000010000000                                        |
|    TABH+60(20) = FFFFFFFF042E0100603600000400410602000000                                        |
|    store                   = 0x38BCD49CBF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x2053B3A0BF7F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 2     (cmpSingleMcmpR)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 118   (0x76000000)                                                  |
|    label                   = 1623  (0x57060000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 16    (0x10000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000302/TYPE#000008                                             |
|    occu                    = 4     (0x04000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 1                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x1848BC9CBF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 757   (0xF5020000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0xA88DBDA1BF7F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|    {CB057860B5A71FD19AAA9FB772B9308E}                                                            |
|    C076BA1D9A9B7B38                                                                              |
|    B58057F1AAF7290E                                                                              |
|    CB057860B5A71FD19AAA9FB772B9308E                                                              |
|IT_TOUR_KEYS                                                                                      |
|    Table IT_1624[1x16]                                                                           |
|    \FUNCTION=/PLCP/EEWA_ORDER_CONFIRM_RFC\DATA=IT_TOUR_KEYS                                      |
|    Table reference: 126                                                                          |
|    TABH+ 0(20) = A8C4BC9CBF7F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000100000044010000                                        |
|    TABH+40(20) = 000000007E000000580600000100000010000000                                        |
|    TABH+60(20) = FFFFFFFF042E0100003600000100410602000000                                        |
|    store                   = 0xA8C4BC9CBF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 2     (cmpSingleMcmpR)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 126   (0x7E000000)                                                  |
|    label                   = 1624  (0x58060000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 16    (0x10000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000302/TYPE#000006                                             |
|    occu                    = 1     (0x01000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 1                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x6830BD9CBF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 262   (0x06010000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 1     (0x01000000)                                                  |
|    lineAlloc               = 1     (0x01000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    {CB057860B5A71FD19989D4C99A22601D}                                                            |
|    C076BA1D98DC9261                                                                              |
|    B58057F19949A20D                                                                              |
|    CB057860B5A71FD19989D4C99A22601D                                                              |
|IV_PROFILE                                                                                        |
|    EWACS                                                                                         |
|    45445222222222222222                                                                          |
|    57133000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    45005700410043005300200020002000200020002000200020002000200020002000200020002000              |
|CT_BAPIRET                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|SCREEN                                                                                            |
|    {%_17NNS0002966009_%_%_%_%_%_%_;;;;SRI;0;0;0;0;1;00;0;0;0;0;0;0}                              |
|    205030304040503030303030303030303050205020502050205020502050202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    50F01070E0E03000000020906060000090F050F050F050F050F050F050F0000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    25005F00310037004E004E00530030003000300032003900360036003000300039005F0025005F0025005F0025005\|
|F0025005F0025005F0025005F002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SYST-REPID                                                                                        |
|    /PLCP/SAPLEEWA_CONFIRMATION                                                                   |
|    2544525454445454444454454442222222222222                                                      |
|    F0C30F310C5571F3FE692D149FE0000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F005300410050004C0045004500570041005F0043004F004E004600490052004D0041005\|
|40049004F004E002000200020002000200020002000200020002000200020002000                               |
|LPROCESSOR                                                                                        |
|    {O:156*\CLASS=/PLCP/CL_WA_CONFIRMATION}                                                       |
|    60009000                                                                                      |
|    3000C000                                                                                      |
|    630000009C000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_FSREG_001                                                                                       |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|%_PRINT                                                                                           |
|    {;000;;;;;;0;0;0;;;;;;;;;;;0;;;0}                                                             |
|    202020203030302020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020203000000000\|
|00202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200030003000300020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000300000000000000000000000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SY-REPID                                                                                          |
|    /PLCP/SAPLEEWA_CONFIRMATION                                                                   |
|    2544525454445454444454454442222222222222                                                      |
|    F0C30F310C5571F3FE692D149FE0000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F005300410050004C0045004500570041005F0043004F004E004600490052004D0041005\|
|40049004F004E002000200020002000200020002000200020002000200020002000                               |
|SY                                                                                                |
|    {1;0;3;1;0;0;0;0;0;0;0;0;1;0;1;0;0;0;0;94;0;0;20;0;3;36;8;0;0;0;0;0;0;0;0;0;0;0;29;188;0;3;0;\|
|3600;X;C;0000;4;0;0;;;;E;2;;;X;1000;;;;;;1;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;STLI;758;SE37;STRT;;;0000000000;0000000000;;00000000;2                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000050000000000010000\|
|000000020000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    1000000030001000000000000000000000000000000000001000000010000000000000000000E0000000000040000\|
|00030004000800000000000000000000000000000000000000000000000D000C0000000300000000E0080300040000C00\|
|00C000000050002000000080100000000000000000000000001000F0F03000000                                 |
|    010000000000000003000000010000000000000000000000000000000000000000000000000000000000000000000\|
|000010000000000000001000000000000000000000000000000000000005E000000000000000000000014000000000000\|
|0003000000240000000800000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001D000000BC000000000000000300000000000000100E000058004300000004000000000C00000000\|
|0C00200020002000450000000200000020002000580031003000300030002000200020002000200020002000200000000\|
|10000005F005F0053002000200020                                                                     |
|%_EXCP%_#E8                                                                                       |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
----------------------------------------------------------------------------------------------------
|No.       4 Ty.          FORM                                                                     |
|Name  FUNCTION_CALL                                                                               |
----------------------------------------------------------------------------------------------------
|DESTI                                                                                             |
|                                                                                                  |
|    22222222222222222222222222222222                                                              |
|    00000000000000000000000000000000                                                              |
|    00000000000000000000000000000000                                                              |
|    00000000000000000000000000000000                                                              |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020002000                                                               |
|EX                                                                                                |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|DEBUG                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_OCT_BAPIRET                                                                                     |
|    Table[initial]                                                                                |
|                                                                                                  |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SY-REPID                                                                                          |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC==FT                                                              |
|    2544524454545445544444545544334522222222                                                      |
|    F0C30F5571FF2452F3FE692DF263DD6400000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F0045004500570041005F004F0052004400450052005F0043004F004E004600490052004\|
|D005F005200460043003D003D004600540020002000200020002000200020002000                               |
|TIME1                                                                                             |
|    0                                                                                             |
|    00000000                                                                                      |
|    0000000C                                                                                      |
|    000000000000000C                                                                              |
|SY-XFORM                                                                                          |
|    CONVERSION_EXIT                                                                               |
|    444545544454545222222222222222                                                                |
|    3FE65239FEF5894000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    43004F004E00560045005200530049004F004E005F004500580049005400200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|%_IIS_ASSIGNMENT_CONTROL                                                                          |
|    {X;X;PROLOGAFN}                                                                               |
|    5050505040404040404040202020                                                                  |
|    80800020F0C0F0701060E0000000                                                                  |
|    58005800500052004F004C004F004700410046004E00200020002000                                      |
|%_IIT_ASSIGNMENT_KEYS                                                                             |
|    Table IT_329[1x32]                                                                            |
|    \PROGRAM=/PLCP/EEWA_ORDER_CONFIRM_RFC==FT\DATA=%_IIT_ASSIGNMENT_KEYS                          |
|    Table reference: 58                                                                           |
|    TABH+ 0(20) = 303EBC9CBF7F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000100000044010000                                        |
|    TABH+40(20) = 000000003A000000490100000100000020000000                                        |
|    TABH+60(20) = FFFFFFFF04710000F07B00000100410602000000                                        |
|    store                   = 0x303EBC9CBF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 2     (cmpSingleMcmpR)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 58    (0x3A000000)                                                  |
|    label                   = 329   (0x49010000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 32    (0x20000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000113/TYPE#000028                                             |
|    occu                    = 1     (0x01000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 1                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x5005B19CBF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 263   (0x07010000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 1     (0x01000000)                                                  |
|    lineAlloc               = 1     (0x01000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    {CB057860B5A71FD19989D4C99A22601D;CB057860B5A71FD19AAA9FB772B9308E}                           |
|    C076BA1D98DC9261C076BA1D9A9B7B38                                                              |
|    B58057F19949A20DB58057F1AAF7290E                                                              |
|    CB057860B5A71FD19989D4C99A22601DCB057860B5A71FD19AAA9FB772B9308E                              |
|%_IIT_SERVICE_KEYS                                                                                |
|    Table IT_1044[1x16]                                                                           |
|    \PROGRAM=/PLCP/EEWA_ORDER_CONFIRM_RFC==FT\DATA=%_IIT_SERVICE_KEYS                             |
|    Table reference: 170                                                                          |
|    TABH+ 0(20) = 38BCD49CBF7F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000100000044010000                                        |
|    TABH+40(20) = 00000000AA000000140400000100000010000000                                        |
|    TABH+60(20) = FFFFFFFF04710000907B00000400410602000000                                        |
|    store                   = 0x38BCD49CBF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 2     (cmpSingleMcmpR)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 170   (0xAA000000)                                                  |
|    label                   = 1044  (0x14040000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 16    (0x10000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000113/TYPE#000026                                             |
|    occu                    = 4     (0x04000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 1                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x1848BC9CBF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 757   (0xF5020000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    {CB057860B5A71FD19AAA9FB772B9308E}                                                            |
|    C076BA1D9A9B7B38                                                                              |
|    B58057F1AAF7290E                                                                              |
|    CB057860B5A71FD19AAA9FB772B9308E                                                              |
|%_IIT_TOUR_KEYS                                                                                   |
|    Table IT_328[1x16]                                                                            |
|    \PROGRAM=/PLCP/EEWA_ORDER_CONFIRM_RFC==FT\DATA=%_IIT_TOUR_KEYS                                |
|    Table reference: 65                                                                           |
|    TABH+ 0(20) = A8C4BC9CBF7F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000100000044010000                                        |
|    TABH+40(20) = 0000000041000000480100000100000010000000                                        |
|    TABH+60(20) = FFFFFFFF04710000307B00000100410602000000                                        |
|    store                   = 0xA8C4BC9CBF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 2     (cmpSingleMcmpR)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 65    (0x41000000)                                                  |
|    label                   = 328   (0x48010000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 16    (0x10000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000113/TYPE#000024                                             |
|    occu                    = 1     (0x01000000)                                                  |
|    occu0                   = 1                                                                   |
|    stMode                  = 0                                                                   |
|    groupCntl               = 0                                                                   |
|    rfc                     = 0                                                                   |
|    unShareable             = 0                                                                   |
|    mightBeShared           = 1                                                                   |
|    sharedWithShmTab        = 0                                                                   |
|    isShmLockId             = 0                                                                   |
|    isUsed                  = 1                                                                   |
|    isCtfyAble              = 1                                                                   |
|    hasScndKeys             = 0                                                                   |
|    hasRowId                = 0                                                                   |
|    statScndKeysOutdated    = 0                                                                   |
|    statScndUniKeysOutdated = 0                                                                   |
|    primKeyIsEmpty          = ItPrimKeyIsEmptyNo                                                  |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x6830BD9CBF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 262   (0x06010000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 1     (0x01000000)                                                  |
|    lineAlloc               = 1     (0x01000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    {CB057860B5A71FD19989D4C99A22601D}                                                            |
|    C076BA1D98DC9261                                                                              |
|    B58057F19949A20D                                                                              |
|    CB057860B5A71FD19989D4C99A22601D                                                              |
|%_IIV_PROFILE                                                                                     |
|    EWACS                                                                                         |
|    45445222222222222222                                                                          |
|    57133000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    45005700410043005300200020002000200020002000200020002000200020002000200020002000              |
|TIME2                                                                                             |
|    0                                                                                             |
|    00000000                                                                                      |
|    0000000C                                                                                      |
|    000000000000000C                                                                              |
|RUNTIME                                                                                           |
|                                                                                                  |
|    222222222222222222222222222222                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ATRA                                                                                              |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SYST-REPID                                                                                        |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC==FT                                                              |
|    2544524454545445544444545544334522222222                                                      |
|    F0C30F5571FF2452F3FE692DF263DD6400000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F0045004500570041005F004F0052004400450052005F0043004F004E004600490052004\|
|D005F005200460043003D003D004600540020002000200020002000200020002000                               |
----------------------------------------------------------------------------------------------------
|No.       3 Ty.          FUNCTION                                                                 |
|Name  SFCS_FA_FUNCTION_INVOCE                                                                     |
----------------------------------------------------------------------------------------------------
|FUNCTION                                                                                          |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC                                                                  |
|    254452445454544554444454554422                                                                |
|    F0C30F5571FF2452F3FE692DF26300                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    2F0050004C00430050002F0045004500570041005F004F0052004400450052005F0043004F004E004600490052004\|
|D005F0052004600430020002000                                                                       |
|L_PROGNAME                                                                                        |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC==FT                                                              |
|    2544524454545445544444545544334522222222                                                      |
|    F0C30F5571FF2452F3FE692DF263DD6400000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F0045004500570041005F004F0052004400450052005F0043004F004E004600490052004\|
|D005F005200460043003D003D004600540020002000200020002000200020002000                               |
|SY-REPID                                                                                          |
|    SAPLSEUJ                                                                                      |
|    5454545422222222222222222222222222222222                                                      |
|    310C355A00000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C005300450055004A002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|VALUE_DEST                                                                                        |
|    <empty string>                                                                                |
|    10000000                                                                                      |
|    40000000                                                                                      |
|    1400000000000000                                                                              |
|SY-LINSZ                                                                                          |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|%_PRINT                                                                                           |
|    {;000;;;;;;0;0;0;;;;;;;;;;;0;;;0}                                                             |
|    202020203030302020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020203000000000\|
|00202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200030003000300020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000300000000000000000000000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SYST-REPID                                                                                        |
|    SAPLSEUJ                                                                                      |
|    5454545422222222222222222222222222222222                                                      |
|    310C355A00000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C005300450055004A002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_FUN*CONTROL_SEQFUNC                                                                             |
|    <initial>                                                                                     |
|    <initial>                                                                                     |
|    <initial>                                                                                     |
|    <<iinniittiiaall>>                                                                            |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY                                                                                                |
|    {1;0;3;1;0;0;0;0;0;0;0;0;1;0;1;0;0;0;0;94;0;0;20;0;3;36;8;0;0;0;0;0;0;0;0;0;0;0;29;188;0;3;0;\|
|3600;X;C;0000;4;0;0;;;;E;2;;;X;1000;;;;;;1;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;STLI;758;SE37;STRT;;;0000000000;0000000000;;00000000;2                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000050000000000010000\|
|000000020000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    1000000030001000000000000000000000000000000000001000000010000000000000000000E0000000000040000\|
|00030004000800000000000000000000000000000000000000000000000D000C0000000300000000E0080300040000C00\|
|00C000000050002000000080100000000000000000000000001000F0F03000000                                 |
|    010000000000000003000000010000000000000000000000000000000000000000000000000000000000000000000\|
|000010000000000000001000000000000000000000000000000000000005E000000000000000000000014000000000000\|
|0003000000240000000800000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001D000000BC000000000000000300000000000000100E000058004300000004000000000C00000000\|
|0C00200020002000450000000200000020002000580031003000300030002000200020002000200020002000200000000\|
|10000005F005F0053002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       2 Ty.          FUNCTION                                                                 |
|Name  SFCS_FA_TEST_FUNCTION                                                                       |
----------------------------------------------------------------------------------------------------
|FUNCNAME                                                                                          |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC                                                                  |
|    254452445454544554444454554422                                                                |
|    F0C30F5571FF2452F3FE692DF26300                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    2F0050004C00430050002F0045004500570041005F004F0052004400450052005F0043004F004E004600490052004\|
|D005F0052004600430020002000                                                                       |
|RUN_TIME_ON                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SEQUENCE                                                                                          |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|TRACE_SPECIAL_UNITS                                                                               |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SY-MSGID                                                                                          |
|                                                                                                  |
|    22222222222222222222                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    20002000200020002000200020002000200020002000200020002000200020002000200020002000              |
|RSGEN_FREE_CODE                                                                                   |
|    75                                                                                            |
|    33                                                                                            |
|    75                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    37003500                                                                                      |
|MSGID_RETT                                                                                        |
|                                                                                                  |
|    22222222222222222222                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    20002000200020002000200020002000200020002000200020002000200020002000200020002000              |
|MSGNO_RETT                                                                                        |
|    000                                                                                           |
|    333                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    300030003000                                                                                  |
|VAR1_RETT                                                                                         |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|VAR2_RETT                                                                                         |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|VAR3_RETT                                                                                         |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|VAR4_RETT                                                                                         |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|L_MSG                                                                                             |
|    <empty string>                                                                                |
|    10000000                                                                                      |
|    40000000                                                                                      |
|    1400000000000000                                                                              |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.       1 Ty.          EVENT                                                                    |
|Name  START-OF-SELECTION                                                                          |
----------------------------------------------------------------------------------------------------
|SY-LDBPG                                                                                          |
|    SAPDB__S                                                                                      |
|    5454455522222222222222222222222222222222                                                      |
|    31042FF300000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    530041005000440042005F005F0053002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|FUNCN                                                                                             |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC                                                                  |
|    254452445454544554444454554422                                                                |
|    F0C30F5571FF2452F3FE692DF26300                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    2F0050004C00430050002F0045004500570041005F004F0052004400450052005F0043004F004E004600490052004\|
|D005F0052004600430020002000                                                                       |
|SY-XPROG                                                                                          |
|    SAPLEEWA_EL_BASE                                                                              |
|    5454445454454454222222222222222222222222                                                      |
|    310C5571F5CF2135000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C0045004500570041005F0045004C005F00420041005300450020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|RUN_ON                                                                                            |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SY-XFORM                                                                                          |
|    CONVERSION_EXIT                                                                               |
|    444545544454545222222222222222                                                                |
|    3FE65239FEF5894000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    43004F004E00560045005200530049004F004E005F004500580049005400200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|SEQ                                                                                               |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_ARCHIVE                                                                                         |
|    {;;;;;;;;;;;;;;;;;;0}                                                                         |
|    202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|TRACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SYST-REPID                                                                                        |
|    RS_TESTFRAME_CALL                                                                             |
|    5555455454445444422222222222222222222222                                                      |
|    23F4534621D5F31CC00000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    520053005F0054004500530054004600520041004D0045005F00430041004C004C002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|SY-SUBRC                                                                                          |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|SY-BATCH                                                                                          |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_TRACE_%_APP_%                                                                                   |
|    {TRACE}                                                                                       |
|    505040404020202020202020202020202020202020202020202020202020                                  |
|    402010305000000000000000000000000000000000000000000000000000                                  |
|    540052004100430045002000200020002000200020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|SY-MSGID                                                                                          |
|                                                                                                  |
|    22222222222222222222                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    20002000200020002000200020002000200020002000200020002000200020002000200020002000              |
|FUNCN                                                                                             |
|VARI                                                                                              |
|    {;;;;0;00000000;000000;;0;0000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020000000303030303030303030303030303020202020202000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000000000000000300030003000300030003000300030003000300\|
|0300030003000300020002000200020002000200000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000                                                                     |
|%_SSCR                                                                                            |
|    {TRACE;1000003;P;1;C;CHAR;RS38L-HEAD;;;;0;2;;;;;;01;00;00;00}                                 |
|    505040404020202044005020000040404040505050303040204040404020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    402010305000000032F000001000303080102020303080C0D08050104000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    5400520041004300450020002000200043420F0050002000010000004300430048004100520052005300330038004\|
|C002D00480045004100440020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Information About Memory Usage                                                                    |
----------------------------------------------------------------------------------------------------
|Memory Consumption                                                                                |
----------------------------------------------------------------------------------------------------
|Memory Area                                                      Bytes                            |
----------------------------------------------------------------------------------------------------
|Extended Memory (EM)                                        47.475.928                            |
|PRIV Memory (Heap)                                                   0                            |
|SAP Paging                                                     180.224                            |
----------------------------------------------------------------------------------------------------
|MM Memory Management                                    Bytes / Number                            |
----------------------------------------------------------------------------------------------------
|Used Memory                                                 32.212.584                            |
|Free Memory                                                  3.393.904                            |
|Largest Free Block                                           3.391.968                            |
|Used Blocks                                                     51.336                            |
|Free Blocks                                                          4                            |
|Maximum Used Memory                                         32.212.584                            |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Shared Objects Locks                                                                              |
----------------------------------------------------------------------------------------------------
|ModeLockRe Area Name                                                                              |
|Instance Name                                                                    Clt Lock Type    |
|Version ID Label                Include                                  Line       Time Stamp    |
----------------------------------------------------------------------------------------------------
|         2 CL_GUI_MEMORY                                                                          |
|$DEFAULT_INSTANCE$                                                               110 R            |
|      3970               432237 CL_GUI_FRONTEND_SERVICES======CM01Z               6 20260618061533|
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Application Information                                                                           |
----------------------------------------------------------------------------------------------------
|Index|Information                                                                                 |
----------------------------------------------------------------------------------------------------
|    1|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|    2|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|    3|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|    4|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|    5|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|    6|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|    7|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|    8|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|    9|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
|   10|Inline hold, switch to SYNC_CPIC_NO_COMMIT                                                  |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Internal notes                                                                                    |
|    The termination was triggered in function "raiseExcp" of the SAP kernel, in                   |
|    line 3979 of module "//bas/GIT/krn/abap/runt/abexcp.c#0".                                     |
|    The internal operation just processed is "EXCP".                                              |
|    Internal mode started at 20260618061533.                                                      |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Active Calls in SAP Kernel                                                                        |
----------------------------------------------------------------------------------------------------
|Lines of C stack in kernel (structure different on each platform)                                 |
----------------------------------------------------------------------------------------------------
|# buildID: e10aa207a0f170ac13b6a9ee6c16f78676930516                                               |
|dw.sapS4Q_D00;0x974efe;[S](LinStackBacktrace(void**, int, int)+0xa8)[0x56225bf74efe]              |
|dw.sapS4Q_D00;0x979502;[S](LinStack(_IO_FILE*)+0x48)[0x56225bf79502]                              |
|dw.sapS4Q_D00;0x97497e;[S](CTrcStack2+0x4b)[0x56225bf7497e]                                       |
|dw.sapS4Q_D00;0xc60342;[S](rabax_CStackSave()+0x9e)[0x56225c260342]                               |
|dw.sapS4Q_D00;0xc5b78d;[S](rabax(char16_t const*, char16_t const*, int, char16_t const*, void con\|
|st*) [clone .cold]+0x102b)[0x56225c25b78d]                                                        |
|dw.sapS4Q_D00;0xf14d76;[S](raiseExcp() [clone .cold]+0x3ac)[0x56225c514d76]                       |
|dw.sapS4Q_D00;0xbe3e25;[S](ab_extri() [clone .cold]+0xff5)[0x56225c1e3e25]                        |
|dw.sapS4Q_D00;0x2d99981;[S](ab_xevent(char16_t const*)+0x31)[0x56225e399981]                      |
|dw.sapS4Q_D00;0x2d99f89;[S](ab_trigg()+0x49)[0x56225e399f89]                                      |
|dw.sapS4Q_D00;0x312121d;[S](ab_run+0x2d)[0x56225e72121d]                                          |
|dw.sapS4Q_D00;0x2d2e9a2;[S](dynpmcal(DINFDUMY*, STPDUMMY*)+0x752)[0x56225e32e9a2]                 |
|dw.sapS4Q_D00;0x2d2f2bc;[S](dynppai0(DINFDUMY*)+0x34c)[0x56225e32f2bc]                            |
|dw.sapS4Q_D00;0x2d2d669;[S](dynprctl(DINFDUMY*)+0x129)[0x56225e32d669]                            |
|dw.sapS4Q_D00;0x2d2c62e;[S](dynpen00(int)+0x3ce)[0x56225e32c62e]                                  |
|dw.sapS4Q_D00;0x30a3765;[S](ThStart()+0x7f5)[0x56225e6a3765]                                      |
|dw.sapS4Q_D00;0x306b8f4;[S](DpMain+0x3d4)[0x56225e66b8f4]                                         |
|dw.sapS4Q_D00;0x2cacc6e;[S](main+0x2e)[0x56225e2acc6e]                                            |
|# buildID: 2c8359b67579ed1cba5cce7875abfd60fa954ca7                                               |
|libc.so.6;0x351fd;[S](__libc_start_main+0xef)[0x7fc9459ea1fd]                                     |
|# buildID: e10aa207a0f170ac13b6a9ee6c16f78676930516                                               |
|dw.sapS4Q_D00;0x306b45a;[S](_start+0x2a)[0x56225e66b45a]                                          |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|List of ABAP programs affected                                                                    |
----------------------------------------------------------------------------------------------------
|Index |Ty.|Program                                 |Group |Date      |Time    |Size     |Lang.    |
----------------------------------------------------------------------------------------------------
|     0|Prg|RS_TESTFRAME_CALL                       |     0|15.03.2020|19:32:15|    27648|E        |
|     1|Prg|SAPMSSY0                                |     1|31.03.2023|15:25:25|   120832|E        |
|     2|Prg|SAPMSSYD                                |     1|03.05.2025|07:02:38|    34816|E        |
|     3|Prg|SAPFSYSCALLS                            |     1|01.06.2015|12:30:31|     7168|E        |
|     4|Prg|CL_DYNP_CUA_DEFAULT===========CP        |     4|22.01.2021|09:24:38|    10240|E        |
|     5|Prg|RSDBRUNT                                |     0|08.08.2024|10:13:36|   323584|E        |
|     6|Typ|RSSCR                                   |     0|31.05.2013|08:11:11|     6144|         |
|     7|Prg|RSDBSPBL                                |     0|01.04.2022|13:52:52|    82944|E        |
|     8|Prg|SAPDB__S                                |     0|15.03.2020|19:32:16|    21504|E        |
|     9|Typ|VARID                                   |     0|12.05.1997|16:51:30|     5120|         |
|    10|Typ|SSCRFIELDS                              |     0|31.05.2013|08:13:14|     5120|         |
|    11|Prg|RSDBSPVA                                |     0|03.05.2025|08:07:31|   221184|E        |
|    12|Prg|%_CSYDB0                                |     0|01.04.2022|13:52:51|    43008|E        |
|    13|Typ|RSVAMEMKEY                              |     0|07.05.1997|13:07:49|     3072|         |
|    14|Prg|RSDBSPMC                                |     0|01.04.2022|13:52:52|    94208|E        |
|    15|Typ|DDSHDESCR                               |     0|03.09.1997|03:05:16|     4096|         |
|    16|Typ|SPPARAMS                                |     0|07.05.1997|13:10:38|     3072|         |
|    17|Prg|SAPLICON                                |    17|01.06.2015|12:30:31|    54272|E        |
|    18|Prg|%_CICON                                 |    17|16.04.2021|19:43:34|   117760|E        |
|    19|Typ|ICONT                                   |     0|31.05.2013|08:04:28|     3072|         |
|    20|Prg|SAPLSABE                                |    20|07.04.2016|14:27:01|    15360|E        |
|    21|Prg|SAPLSECU                                |    21|15.06.2023|07:05:35|   156672|E        |
|    22|Typ|RSSUBINFO                               |     0|31.05.2013|08:11:14|     4096|         |
|    23|Typ|RSEXFCODE                               |     0|13.08.1997|12:52:57|     2048|         |
|    24|Prg|SAPLDSYA                                |    24|15.06.2023|07:05:19|    91136|E        |
|    25|Prg|SAPFSDS1                                |    24|19.11.2020|11:23:15|    76800|E        |
|    26|Typ|TDCLD                                   |     0|03.08.2015|16:12:59|     6144|         |
|    27|Prg|SAPLSDOD                                |    27|18.03.2023|11:02:31|   216064|E        |
|    28|Typ|DOKIL                                   |     0|12.05.1997|16:46:17|     4096|         |
|    29|Prg|SAPLSDOC                                |    29|15.06.2023|07:05:19|  1815552|E        |
|    30|Prg|SAPCNVE                                 |    30|26.01.2018|14:57:35|     8192|E        |
|    31|Prg|SAPLLANG                                |    31|26.01.2023|17:38:15|    18432|E        |
|    32|Typ|T002                                    |     0|21.01.2014|10:06:21|     3072|         |
|    33|Typ|RSPARINT                                |     0|10.04.1995|09:58:38|     2048|         |
|    34|Prg|SAPLSCNT                                |    34|28.05.2018|11:34:27|    32768|E        |
|    35|Typ|DYCBOX                                  |     0|20.08.1998|11:16:53|     3072|         |
|    36|Prg|SAPLSVSM                                |    36|10.11.2022|19:49:55|    31744|E        |
|    37|Prg|SAPLSGUI                                |    37|31.03.2023|15:25:25|   103424|E        |
|    38|Prg|SAPLSTTM                                |    38|10.11.2022|19:49:55|    80896|E        |
|    39|Prg|SAPLSBDC                                |    39|18.03.2023|11:02:33|    71680|E        |
|    40|Prg|CL_DATAPROVIDER===============CP        |    40|28.05.2018|11:34:26|    53248|E        |
|    41|Prg|SAPSHDTV                                |    34|31.03.2023|15:25:24|    91136|E        |
|    42|Prg|SAPFS_SECLOG                            |     1|10.11.2022|19:49:52|    25600|E        |
|    43|Typ|INDX                                    |     0|29.07.1998|18:38:15|     7168|         |
|    44|Prg|SDYNPRAL                                |     1|02.05.2016|13:52:39|     8192|E        |
|    45|Prg|SAPLSDYNPRAL                            |    45|10.11.2022|19:49:54|    16384|E        |
|    46|Typ|DYNP_RAL_ID                             |     0|27.11.2013|14:25:51|     3072|         |
|    47|Typ|DYNP_RAL_SCREEN                         |     0|31.05.2013|08:00:52|     3072|         |
|    48|Prg|CL_DYNP_RAL_API===============CP        |    48|26.01.2023|17:37:52|   119808|E        |
|    49|Prg|CL_SRAL_RECORDER_FACTORY_DYNP=CP        |    49|26.01.2023|17:38:14|    21504|E        |
|    50|Prg|SRAL_DYNP_ADAPTER                       |    48|10.11.2022|19:49:55|    27648|E        |
|    51|Prg|CL_SRAL_FACTORY_DYNP==========CP        |    51|26.01.2023|17:38:14|    45056|E        |
|    52|Prg|CL_SRAL_SWITCH_CLIENT=========CP        |    52|10.11.2022|19:49:43|    10240|E        |
|    53|Prg|CL_SRAL_DB_PARAMS=============CP        |    53|10.11.2022|19:49:43|    23552|E        |
|    54|Prg|RADSHPBOPREFLIGHT                       |     1|10.11.2022|19:49:51|    18432|E        |
|    55|Prg|RADSHPBOSTATUS                          |     1|10.11.2022|19:49:51|    20480|E        |
|    56|Prg|RADSHPBOSWITCH                          |     1|10.11.2022|19:49:51|    21504|E        |
|    57|Prg|RADSHUPGRADE                            |     1|10.11.2022|19:49:51|    21504|E        |
|    58|Prg|WPBHELP_REGISTRATION                    |     1|06.04.2021|14:01:49|    11264|E        |
|    59|Prg|SAPFGUICNTL                             |     1|17.12.2018|10:04:08|    28672|E        |
|    60|Prg|SAPLOLEA                                |    60|10.11.2022|19:49:53|   120832|E        |
|    61|Prg|SAPLSFES                                |    61|16.05.2023|13:23:16|   323584|E        |
|    62|Prg|CL_SYSTEM_TRANSACTION_STATE===CP        |    62|31.03.2023|15:25:24|   107520|E        |
|    63|Prg|SAPLTHFB                                |    63|15.06.2023|07:05:24|   518144|E        |
|    64|Prg|SAPLSPLUGIN                             |    64|03.11.2021|19:19:46|     8192|E        |
|    65|Typ|ARFCRDATA                               |     0|08.09.2011|14:25:38|     7168|         |
|    66|Prg|SAPLGRFC                                |    66|01.06.2015|12:30:31|    15360|E        |
|    67|Typ|SWCBCONT                                |     0|08.09.2011|14:24:16|     4096|         |
|    68|Typ|OLE_VERBS                               |     0|04.04.1995|16:02:20|     3072|         |
|    69|Typ|OLE_PA                                  |     0|04.04.1995|16:02:19|     3072|         |
|    70|Prg|SAPFS_DYNPRO_HOOKS                      |     1|26.01.2018|15:16:09|     7168|E        |
|    71|Prg|SAPLSALV_IDA_IMPL_UI_SCREEN             |    71|18.03.2023|11:03:29|    16384|E        |
|    72|Prg|CL_SALV_GUI_ROUNDTRIP_BROKER==CP        |    72|18.03.2023|11:03:29|    53248|E        |
|    73|Prg|SAPLSPOF                                |    73|18.07.2023|12:08:17|   220160|E        |
|    74|Typ|SSCRTEXTS                               |     0|03.09.1997|03:12:33|     4096|         |
|    75|Prg|SAPLSEUJ                                |    75|15.06.2023|07:05:35|   674816|E        |
|    76|Typ|D010SINF                                |     0|31.05.2013|08:19:01|     7168|         |
|    77|Prg|SAPLSUNI                                |    77|28.04.2023|07:29:53|   290816|E        |
|    78|Typ|TFDIR                                   |     0|29.03.2018|15:55:42|     4096|         |
|    79|Typ|TRDIR                                   |     0|31.05.2013|08:19:17|     7168|         |
|    80|Typ|STOKES                                  |     0|26.02.2001|12:01:12|     3072|         |
|    81|Prg|CL_R3STANDARD_PERSISTENCE=====CP        |    81|28.04.2023|07:26:24|   974848|E        |
|    82|Typ|ENHNAME_STRUC                           |     0|30.09.2004|13:14:49|     2048|         |
|    83|Typ|RS38L                                   |     0|29.03.2018|15:54:09|    33792|         |
|    84|Prg|SAPLSTRD                                |    84|15.06.2023|07:05:19|  1218560|E        |
|    85|Typ|TADIR                                   |     0|26.01.2018|14:08:30|     6144|         |
|    86|Typ|GTADIR                                  |     0|08.09.2011|14:20:45|     4096|         |
|    87|Prg|SAPLSTR9                                |    87|26.01.2023|17:38:07|    63488|E        |
|    88|Prg|CONTEXT_X_TR_SYS_PARAMS                 |    87|25.02.2022|14:55:08|    47104|E        |
|    89|Typ|TCONT                                   |     0|31.05.2013|08:14:44|     3072|         |
|    90|Typ|CONTEXTB                                |     0|31.05.2013|07:59:43|     7168|         |
|    91|Prg|SAPLSCP3                                |    91|01.06.2015|12:30:31|     9216|E        |
|    92|Prg|SAPLSCTS_UNICODE                        |    92|10.11.2022|19:49:54|    11264|E        |
|    93|Prg|CL_ABAP_CHAR_UTILITIES========CP        |    93|10.11.2022|19:49:27|    13312|E        |
|    94|Prg|SAPLSEUF                                |    94|15.06.2023|07:05:35|   843776|E        |
|    95|Prg|SAPMS38L                                |    94|16.05.2023|13:23:24|  1150976|E        |
|    96|Typ|ENLFDIR                                 |     0|01.06.2015|10:51:41|     4096|         |
|    97|Typ|RSINFDIR                                |     0|29.03.2018|15:55:38|     4096|         |
|    98|Typ|RSCHA                                   |     0|31.05.2013|08:10:18|     4096|         |
|    99|Typ|FUNCT                                   |     0|31.05.2013|08:03:33|     3072|         |
|   100|Typ|RSEXC                                   |     0|17.06.2009|13:36:19|     2048|         |
|   101|Typ|RSIMP                                   |     0|31.05.2013|08:10:43|     4096|         |
|   102|Typ|RSTBL                                   |     0|31.05.2013|08:11:14|     4096|         |
|   103|Typ|RSFDO                                   |     0|13.08.1997|12:52:58|     3072|         |
|   104|Prg|SAPLSEWORKINGAREA                       |   104|26.05.2023|05:06:55|   660480|E        |
|   105|Typ|FUPARAREF                               |     0|31.05.2013|07:57:17|     5120|         |
|   106|Typ|TFTIT                                   |     0|31.05.2013|08:14:59|     3072|         |
|   107|Prg|CL_FB_PARAMETER_DB============CP        |   107|03.05.2025|07:02:00|   216064|E        |
|   108|Typ|RSEXP                                   |     0|31.05.2013|08:10:41|     4096|         |
|   109|Typ|FUNCTDIR                                |     0|13.08.1997|12:33:07|     3072|         |
|   110|Typ|EUFUNC                                  |     0|08.09.2011|14:26:21|     8192|         |
|   111|Prg|%_CNF2TY                                |    75|29.03.2018|15:54:08|     8192|E        |
|   112|Prg|CL_ABAP_TYPEDESCR=============CP        |   112|16.05.2023|13:23:14|    29696|E        |
|   113|Prg|/PLCP/EEWA_ORDER_CONFIRM_RFC==FT        |    75|12.05.2025|15:47:46|    40960|E        |
|   114|Prg|SAPLSNF2                                |   114|18.03.2023|11:02:55|   418816|E        |
|   115|Prg|%_CSYDES                                |   114|01.06.2015|12:30:30|     9216|E        |
|   116|Typ|/PLCE/SPDTOUR_KEY                       |     0|08.08.2024|08:36:18|     2048|         |
|   117|Prg|IF_WB_CONSTANTS===============IP        |   114|01.06.2015|12:30:31|     5120|E        |
|   118|Typ|/PLCE/SPDSRV_KEY                        |     0|08.08.2024|08:36:18|     2048|         |
|   119|Typ|/PLCE/SPDTASSRVC_KEY                    |     0|08.08.2024|08:36:18|     3072|         |
|   120|Typ|/PLCE/D_PDTOURTRANSFERSEQUENCE          |     0|07.05.2025|00:28:31|     3072|         |
|   121|Typ|BAPIRET2                                |     0|28.05.2018|11:29:59|     5120|         |
|   122|Prg|SAPLSMON                                |   122|16.05.2023|13:24:04|   614400|E        |
|   123|Typ|CPU_ALL                                 |     0|28.05.2018|11:30:25|     4096|         |
|   124|Typ|RFCDES                                  |     0|08.09.2011|14:16:06|    13312|         |
|   125|Prg|SAPLSETF                                |   125|03.03.2023|16:55:31|    99328|E        |
|   126|Prg|SAPLWB_CUSTOMIZING                      |   126|16.05.2023|13:25:05|   315392|E        |
|   127|Typ|RSEUMOD                                 |     0|10.10.2014|18:27:11|    55296|         |
|   128|Typ|SCXTAB_TABSTRIP                         |     0|06.11.2003|20:31:00|     3072|         |
|   129|Prg|%_CSETST                                |   125|16.12.2022|11:36:11|    39936|E        |
|   130|Prg|SAPLSYMB                                |   130|18.03.2023|11:02:55|   126976|E        |
|   131|Prg|CL_ABAP_LIST_UTIL_INTERNAL====CP        |   131|01.06.2015|12:30:30|    17408|E        |
|   132|Prg|CL_GUI_PROPS_CONSUMER=========CP        |   132|28.05.2018|11:34:26|    33792|E        |
|   133|Typ|EUDB                                    |     0|31.05.2013|08:02:19|     9216|         |
|   134|Prg|%_CCNTL                                 |   132|28.05.2018|11:34:26|    16384|E        |
|   135|Prg|CL_ABAP_LIST_PARSER===========CP        |   135|18.03.2023|11:02:55|    26624|E        |
|   136|Prg|SCL_ABAP_LIST_PARSER==========CP        |   136|28.05.2018|11:33:55|     9216|E        |
|   137|Prg|SAPLSTR4                                |   137|10.11.2022|19:49:55|    93184|E        |
|   138|Prg|RADSHPBO                                |     1|18.03.2023|11:02:30|    62464|E        |
|   139|Prg|CL_DSH_SCREEN_ASSIGNMENT======CP        |   139|16.05.2023|13:23:10|    43008|E        |
|   140|Prg|CL_ABAP_MESSAGE_DIGEST========CP        |   140|16.05.2023|13:24:01|    46080|E        |
|   141|Prg|/PERSONAS/DYNP_INTEGRATION              |     1|12.02.2025|20:56:50|    24576|E        |
|   142|Prg|/PERSONAS/CL_API==============CP        |   142|12.02.2025|20:56:50|    12288|E        |
|   143|Prg|/PERSONAS/CL_HANDLER_FACTORY==CP        |   143|12.02.2025|20:56:50|    35840|E        |
|   144|Prg|/PERSONAS/IF_CONTEXT_TYPES====IP        |   142|12.02.2025|18:36:08|    64512|E        |
|   145|Prg|/PERSONAS/CL_SETTINGS_DATA====CP        |   145|12.02.2025|20:56:50|   178176|E        |
|   146|Prg|/PERSONAS/IF_SETTINGS_TYPES===IP        |   145|12.02.2025|18:35:57|    16384|E        |
|   147|Prg|/PERSONAS/CL_AUTH_UTILS=======CP        |   147|12.02.2025|18:36:08|    50176|E        |
|   148|Prg|/PERSONAS/IF_AUTH_TYPES=======IP        |   147|12.02.2025|18:36:08|    19456|E        |
|   149|Prg|/PERSONAS/CL_AUTHENTICATOR====CP        |   149|12.02.2025|18:35:43|    15360|E        |
|   150|Prg|%_CABAP                                 |   149|10.11.2022|19:49:06|    36864|E        |
|   151|Typ|USVALUES                                |     0|31.05.2013|08:16:39|     3072|         |
|   152|Prg|SAPLSUSE                                |   152|17.06.2024|13:21:25|   271360|E        |
|   153|Typ|USER12                                  |     0|08.09.2011|14:28:36|     3072|         |
|   154|Prg|CL_ABAP_SYST==================CP        |   154|24.03.2016|13:14:20|    13312|E        |
|   155|Prg|CL_SUSR_AUTHORIZATIONS========CP        |   155|17.06.2026|07:11:43|   125952|E        |
|   156|Prg|CL_SUID_TOOLS=================CP        |   156|17.06.2026|07:11:43|   311296|E        |
|   157|Typ|SUID_ST_BNAME                           |     0|08.09.2011|14:23:36|     2048|         |
|   158|Prg|CL_SUSR_AUTH_TOOLS============CP        |   158|26.01.2023|17:38:07|   174080|E        |
|   159|Prg|CL_RBAMRT=====================CP        |   159|10.11.2022|19:49:39|     8192|E        |
|   160|Prg|/PERSONAS/CL_SESSION_MEMORY===CP        |   160|12.02.2025|20:56:50|    68608|E        |
|   161|Typ|/PERSONAS/T_MEMO                        |     0|12.02.2025|18:29:06|     6144|         |
|   162|Typ|DYNPRO_ID                               |     0|09.11.2000|14:07:21|     3072|         |
|   163|Prg|/PERSONAS/CL_DYNP_CONTEXT=====CP        |   163|12.02.2025|20:56:50|    83968|E        |
|   164|Prg|/PERSONAS/CL_CONTEXT==========CP        |   164|12.02.2025|20:56:50|    29696|E        |
|   165|Prg|/PERSONAS/CL_EWM_RF_DATA======CP        |   165|12.02.2025|18:35:42|    13312|E        |
|   166|Prg|CL_DYNPRO=====================CP        |   166|10.11.2022|19:49:34|    38912|E        |
|   167|Prg|/PERSONAS/IF_EWM_RF_DATA======IP        |   165|12.02.2025|18:35:41|    10240|E        |
|   168|Typ|DYNPVALUE                               |     0|26.02.2001|12:00:36|     2048|         |
|   169|Typ|DYNP_EL_ID                              |     0|18.05.2004|14:07:47|     3072|         |
|   170|Prg|/PERSONAS/CL_CONTEXT_DATA=====CP        |   170|12.02.2025|20:56:50|    90112|E        |
|   171|Prg|CL_SYSTEM_UUID================CP        |   171|26.01.2023|17:38:07|    52224|E        |
|   172|Prg|IF_SYSTEM_UUID================IP        |   170|03.11.2021|19:19:43|     6144|E        |
|   173|Prg|IF_SYSTEM_UUID_STATIC=========IP        |   170|10.11.2022|19:49:51|     7168|E        |
|   174|Prg|/PERSONAS/CL_OVERWRITE_DATA===CP        |   174|12.02.2025|20:56:50|   398336|E        |
|   175|Prg|/PERSONAS/CL_DYNP_AUTH_HNDL===CP        |   175|12.02.2025|18:33:49|    32768|E        |
|   176|Prg|/PERSONAS/CL_AUTH_HNDL========CP        |   176|12.02.2025|20:56:50|    51200|E        |
|   177|Prg|/PERSONAS/CL_GUI_XML_HNDL=====CP        |   177|12.02.2025|20:56:52|   268288|E        |
|   178|Prg|/PERSONAS/CL_DYNP_XML_HNDL====CP        |   178|12.02.2025|20:56:52|   353280|E        |
|   179|Prg|/PERSONAS/CL_XML_HNDL=========CP        |   179|12.02.2025|20:56:52|   353280|E        |
|   180|Prg|/PERSONAS/CL_LOG_MANAGER======CP        |   180|12.02.2025|18:32:43|    20480|E        |
|   181|Prg|/PERSONAS/CL_LOGGER===========CP        |   181|12.02.2025|18:32:43|    58368|E        |
|   182|Prg|/PERSONAS/IF_MESSAGE_TYPES====IP        |   181|12.02.2025|18:35:57|   620544|E        |
|   183|Prg|/PERSONAS/CL_APP_LOGGER=======CP        |   183|12.02.2025|18:32:38|    21504|E        |
|   184|Prg|/PERSONAS/CL_COMMAND_LOGGER===CP        |   184|12.02.2025|20:56:50|    19456|E        |
|   185|Prg|/PERSONAS/CL_PERSONAS_LOGGER==CP        |   185|12.02.2025|20:56:50|    21504|E        |
|   186|Prg|/PERSONAS/CL_MSG_HNDL=========CP        |   186|12.02.2025|20:56:52|    47104|E        |
|   187|Prg|/PERSONAS/CL_DYNP_FLV_FACTORY=CP        |   187|12.02.2025|20:56:52|    80896|E        |
|   188|Prg|/PERSONAS/CL_FLAVOR_FACTORY===CP        |   188|12.02.2025|20:56:52|   203776|E        |
|   189|Prg|/PERSONAS/CL_TRANSPORT_FACTORYCP        |   189|12.02.2025|20:56:52|    13312|E        |
|   190|Prg|/PERSONAS/CL_NO_AUTH_TR_CLIENTCP        |   190|12.02.2025|20:56:52|    26624|E        |
|   191|Prg|/PERSONAS/CL_DYNP_THM_FACTORY=CP        |   191|12.02.2025|20:56:50|    61440|E        |
|   192|Prg|/PERSONAS/CL_THEME_FACTORY====CP        |   192|12.02.2025|20:56:52|   172032|E        |
|   193|Prg|/PERSONAS/CL_USER_PROFILE=====CP        |   193|12.02.2025|20:56:50|   201728|E        |
|   194|Prg|CL_GUI_FRONTEND_SERVICES======CP        |   194|28.04.2023|07:29:38|   165888|E        |
|   195|Prg|CL_GUI_OBJECT=================CP        |   195|10.11.2022|19:49:36|   446464|E        |
|   196|Prg|CL_GUI_CFW====================CP        |   196|17.12.2018|10:03:42|   185344|E        |
|   197|Prg|CL_DYNAMIC_GUI_EXTENSIONS=====CP        |   197|01.06.2015|12:30:30|    41984|E        |
|   198|Typ|OBJ_RECORD                              |     0|28.05.2018|11:34:27|     3072|         |
|   199|Typ|TOLE                                    |     0|29.07.1998|19:56:01|     4096|         |
|   200|Prg|%_COLE2                                 |    60|28.05.2018|11:34:26|    12288|E        |
|   201|Prg|SAPLSCP2                                |   201|15.06.2023|07:05:35|   197632|E        |
|   202|Typ|SWCONT                                  |     0|08.09.2011|14:24:14|     3072|         |
|   203|Prg|CL_GUI_MEMORY=================CP        |   203|10.11.2022|19:49:36|    47104|E        |
|   204|Prg|CL_SHM_AREA===================CP        |   204|10.11.2022|19:49:42|    63488|E        |
|   205|Prg|CL_ABAP_MEMORY_AREA===========CP        |   205|10.11.2022|19:49:27|    12288|E        |
|   206|Prg|CL_SHM_SERVICE================CP        |   206|10.11.2022|19:49:42|    55296|E        |
|   207|Prg|CL_GUI_MEMORY_ROOT============CP        |   207|10.11.2022|19:49:36|    15360|E        |
|   208|Prg|/PERSONAS/CL_GUI_RUNTIME_MODE=CP        |   208|12.02.2025|20:56:52|   166912|E        |
|   209|Prg|/PERSONAS/CL_DYNP_RUNTIME_MODECP        |   209|12.02.2025|20:56:52|   243712|E        |
|   210|Prg|/PERSONAS/CL_RUNTIME_MODE=====CP        |   210|12.02.2025|20:56:52|   315392|E        |
|   211|Prg|/PERSONAS/CL_MODE=============CP        |   211|12.02.2025|20:56:52|   248832|E        |
|   212|Prg|/PERSONAS/CL_FLAVOR_DATA======CP        |   212|12.02.2025|20:56:52|  1162240|E        |
|   213|Prg|/PERSONAS/IF_CONSTANTS========IP        |   212|12.02.2025|18:35:43|    76800|E        |
|   214|Prg|/PERSONAS/CL_ROLE_DATA========CP        |   214|12.02.2025|20:56:52|   194560|E        |
|   215|Typ|AGR_USERS                               |     0|31.05.2013|07:57:04|     4096|         |
|   216|Prg|/PERSONAS/INIT_ROLE_ASSIGNMENTCP        |   216|12.02.2025|20:56:50|    15360|E        |
|   217|Prg|CL_BADI_BASE==================CP        |   217|16.12.2022|13:58:24|    32768|E        |
|   218|Prg|CL_ABAP_BADI_TRACER===========CP        |   218|10.11.2022|19:49:27|    18432|E        |
|   219|Prg|CL_SXT_TRACE_OPTIONS==========CP        |   219|10.11.2022|19:49:45|    11264|E        |
|   220|Prg|IF_SXT_TYPES==================IP        |   219|03.11.2021|19:22:04|    10240|E        |
|   221|Prg|CL_SXT_TRACER_FACTORY=========CP        |   221|10.11.2022|19:49:45|    20480|E        |
|   222|Prg|CL_SXT_TRACER=================CP        |   222|16.05.2023|13:23:10|    68608|E        |
|   223|Prg|CL_SXT_RT_DB_TRACE_WRITER=====CP        |   223|26.01.2023|17:38:15|    56320|E        |
|   224|Prg|CL_SXT_ACTIVATION_HANDLER=====CP        |   224|26.01.2023|17:38:15|    27648|E        |
|   225|Prg|CL_SXT_MEMORY_LIMITS==========CP        |   225|26.01.2023|17:38:15|    29696|E        |
|   226|Prg|CL_REQUEST_ENTRY_POINT========CP        |   226|26.01.2023|17:38:13|    16384|E        |
|   227|Typ|SXT_ACT                                 |     0|19.11.2020|09:54:38|     5120|         |
|   228|Prg|CL_SXT_E2E_ACTIVATION=========CP        |   228|10.11.2022|19:50:11|    18432|E        |
|   229|Prg|CL_EPP_GLOBAL_FACTORY=========CP        |   229|16.05.2023|13:23:53|    12288|E        |
|   230|Prg|CL_EPP_GLOBAL=================CP        |   230|26.01.2023|17:37:53|    33792|E        |
|   231|Prg|CL_EPP_CORE===================CP        |   231|16.05.2023|13:23:16|   105472|E        |
|   232|Prg|IF_EPP_GLOBAL_SECTION=========IP        |   230|10.11.2022|19:50:11|     9216|E        |
|   233|Prg|CX_BADI_NOT_IMPLEMENTED=======CP        |   233|10.11.2022|19:49:47|    10240|E        |
|   234|Prg|CX_BADI_NOT_SINGLE_USE========CP        |   234|10.11.2022|19:49:47|    10240|E        |
|   235|Prg|CX_BADI=======================CP        |   235|10.11.2022|19:49:47|    10240|E        |
|   236|Prg|CX_NO_CHECK===================CP        |   236|10.11.2022|19:49:48|    10240|E        |
|   237|Prg|CX_ROOT=======================CP        |   237|10.11.2022|19:49:48|    14336|E        |
|   238|Prg|/PERSONAS/CL_TRANSLATION_UTILSCP        |   238|12.02.2025|18:32:43|    24576|E        |
|   239|Prg|SAPLSOTR_DB_READ                        |   239|03.11.2021|19:19:46|    98304|E        |
|   240|Prg|/PERSONAS/CL_DYNP_FLAVOR======CP        |   240|12.02.2025|20:56:50|   152576|E        |
|   241|Prg|/PERSONAS/CL_FLAVOR===========CP        |   241|12.02.2025|20:56:52|   406528|E        |
|   242|Prg|/PERSONAS/IF_FLAVOR_TYPES=====IP        |   241|12.02.2025|18:35:43|   190464|E        |
|   243|Prg|/PERSONAS/CL_DYNP_FLV_VERSION=CP        |   243|12.02.2025|20:56:50|    79872|E        |
|   244|Prg|/PERSONAS/CL_FLAVOR_VERSION===CP        |   244|12.02.2025|20:56:50|   108544|E        |
|   245|Prg|/PERSONAS/CL_BASIC_UTILS======CP        |   245|12.02.2025|20:56:49|    73728|E        |
|   246|Prg|/PERSONAS/CL_USAGE_DATA=======CP        |   246|12.02.2025|20:56:50|   275456|E        |
|   247|Prg|/PERSONAS/CL_RESOURCE_DATA====CP        |   247|12.02.2025|20:56:52|   390144|E        |
|   248|Prg|/PERSONAS/CL_THEME_DATA=======CP        |   248|12.02.2025|20:56:52|   467968|E        |
|   249|Prg|/PERSONAS/CL_DYNP_THEME=======CP        |   249|12.02.2025|20:56:50|    49152|E        |
|   250|Prg|/PERSONAS/CL_THEME============CP        |   250|12.02.2025|20:56:52|    99328|E        |
|   251|Prg|/PERSONAS/CL_THEMES_UTILS=====CP        |   251|12.02.2025|20:56:50|    47104|E        |
|   252|Prg|/PERSONAS/IF_THEME_TYPES======IP        |   250|12.02.2025|20:56:51|   120832|E        |
|   253|Prg|/PERSONAS/CL_SETTINGS=========CP        |   253|12.02.2025|20:56:52|    30720|E        |
|   254|Typ|/PERSONAS/STTNGS                        |     0|12.02.2025|18:29:05|     5120|         |
|   255|Prg|CL_GUI_CONTROL================CP        |   255|10.11.2022|19:49:36|   121856|E        |
|   256|Prg|/PERSONAS/CL_CONFIG_DATA======CP        |   256|12.02.2025|18:35:38|    56320|E        |
|   257|Typ|/PERSONAS/C_MDUI                        |     0|12.02.2025|18:29:01|     5120|         |
|   258|Typ|/PERSONAS/FLVSCR                        |     0|12.02.2025|18:29:02|     3072|         |
|   259|Prg|/PERSONAS/CL_WIN_FLV_SCREEN===CP        |   259|12.02.2025|20:56:50|    86016|E        |
|   260|Prg|/PERSONAS/CL_DYNP_FLV_SCREEN==CP        |   260|12.02.2025|20:56:50|   220160|E        |
|   261|Prg|/PERSONAS/CL_FLAVOR_SCREEN====CP        |   261|12.02.2025|20:56:50|   194560|E        |
|   262|Prg|/PERSONAS/CL_NO_FLV_CHNG_STRATCP        |   262|12.02.2025|18:34:42|    13312|E        |
|   263|Prg|CL_ABAP_GET_CALL_STACK========CP        |   263|26.01.2023|17:38:04|    17408|E        |
|   264|Typ|CALL_STACK_ENTRY                        |     0|17.06.2009|13:31:55|     3072|         |
|   265|Prg|/PERSONAS/CL_CHANGE_DATA======CP        |   265|12.02.2025|20:56:50|   571392|E        |
|   266|Typ|ARFCCALLID                              |     0|08.09.2011|14:18:31|     3072|         |
|   267|Typ|ARFCTID                                 |     0|04.04.1995|15:40:33|     3072|         |
|   268|Typ|ARFCDATA                                |     0|04.04.1995|15:40:32|     5120|         |
|   269|Prg|CL_ABAP_STRUCTDESCR===========CP        |   269|16.05.2023|13:24:02|    51200|E        |
|   270|Prg|CL_ABAP_COMPLEXDESCR==========CP        |   270|16.05.2023|13:23:14|    16384|E        |
|   271|Prg|CL_ABAP_DATADESCR=============CP        |   271|16.05.2023|13:23:14|    26624|E        |
|   272|Typ|SYCH_TABLES_ENTRY                       |     0|17.06.2009|13:37:47|     2048|         |
|   273|Typ|SRFC_SMETH                              |     0|06.11.2003|20:31:19|     3072|         |
|   274|Prg|CL_ABAP_SOFT_REFERENCE========CP        |   274|26.11.2018|11:01:11|     9216|E        |
|   275|Prg|CL_ABAP_REFERENCE=============CP        |   275|01.06.2015|12:30:30|     7168|E        |
|   276|Prg|CL_GUI_DATAMANAGER============CP        |   276|10.11.2022|19:49:36|    87040|E        |
|   277|Prg|CL_IXML_MINI==================CP        |   277|28.05.2018|11:32:46|     9216|E        |
|   278|Prg|CL_IXML_MINI_DOM==============CP        |   278|18.02.2005|14:13:24|    18432|E        |
|   279|Prg|IF_IXML_MINI_NODE=============IP        |   278|09.09.2004|14:18:21|     8192|E        |
|   280|Typ|IXMLTDOM                                |     0|18.05.2004|14:07:00|     3072|         |
|   281|Prg|CL_IXML_MINI_NODE=============CP        |   281|18.02.2005|14:13:24|    22528|E        |
|   282|Prg|CL_IXML_MINI_RENDERER=========CP        |   282|18.02.2005|14:13:24|    12288|E        |
|   283|Typ|RSSOURCEL                               |     0|09.11.2000|14:11:36|     2048|         |
|   284|Typ|RSCONVERT                               |     0|28.05.2018|11:35:51|     4096|         |
|   285|Prg|RSDYNSS0                                |    75|10.11.2022|19:49:51|    67584|E        |
|   286|Prg|SAPLSEUT                                |   286|26.01.2023|17:37:29|   362496|E        |
|   287|Typ|SNODE                                   |     0|08.11.2019|08:56:18|     7168|         |
|   288|Typ|SEUTEXPAND                              |     0|04.04.1995|16:16:35|     2048|         |
|   289|Typ|SEUCOMM                                 |     0|14.02.2002|14:04:48|    24576|         |
|   290|Prg|%_CSTREE                                |   286|19.11.2020|09:53:19|    34816|E        |
|   291|Typ|TREEV_NODE                              |     0|28.05.2018|11:38:03|     5120|         |
|   292|Typ|SNODEITEM                               |     0|28.05.2018|11:37:23|     6144|         |
|   293|Typ|SYMSG                                   |     0|31.05.2013|08:13:41|     4096|         |
|   294|Prg|SAPLSLST                                |   294|16.05.2023|13:24:21|   312320|E        |
|   295|Typ|SMP_DYNTXT                              |     0|25.01.1999|13:03:11|     3072|         |
|   296|Typ|LINEFORMAT                              |     0|18.05.2004|14:06:58|     3072|         |
|   297|Typ|SLINE                                   |     0|14.02.2002|14:04:52|    23552|         |
|   298|Prg|SAPLSUGS                                |   298|02.05.2025|11:07:09|   173056|E        |
|   299|Typ|DSH_INDX                                |     0|21.01.2014|10:04:47|     6144|         |
|   300|Prg|CL_SAL_WRITE_EVENT============CP        |   300|08.08.2024|10:13:22|    86016|E        |
|   301|Prg|CL_SAL_SHM_ACCESS=============CP        |   301|26.01.2023|17:38:08|    27648|E        |
|   302|Prg|/PLCP/SAPLEEWA_CONFIRMATION             |   302|07.05.2025|00:32:17|    17408|E        |
|   303|Prg|/PLCP/CL_WA_CONFIRMATION======CP        |   303|17.06.2026|07:15:00|   301056|E        |
|   304|Prg|/PLCE/R_PDMTOUR===============BD        |   303|17.06.2026|07:15:00|  1430528|E        |
|   305|Prg|CL_ABAP_BEHV_RJOB=============CP        |   305|16.12.2022|13:57:53|    63488|E        |
|   306|Prg|CL_ABAP_BEHV_JOB_BASE=========CP        |   306|31.03.2023|15:25:20|   103424|E        |
|   307|Prg|CL_ABAP_BEHV_CTRL=============CP        |   307|15.06.2023|07:05:18|   486400|E        |
|   308|Prg|CL_ABAP_BEHV_LOG==============CP        |   308|03.03.2023|16:55:27|    33792|E        |
|   309|Prg|IF_ABAP_BEHV_LOG==============IP        |   308|03.11.2021|19:21:30|     8192|E        |
|   310|Typ|ABP_BEHV_ACTIVITIES                     |     0|08.11.2019|08:51:24|     4096|         |
|   311|Prg|CL_ABAP_BEHV_AUTH_CONTEXT=====CP        |   311|10.11.2022|19:49:27|    26624|E        |
|   312|Prg|CL_ABAP_BEHV_AUTH_CC_EXCEPTIONCP        |   312|10.11.2022|19:49:27|    47104|E        |
|   313|Prg|CL_ABAP_BEHV_AUTH_CC_EXCEPT_SOCP        |   313|03.05.2022|08:37:40|    10240|E        |
|   314|Prg|CL_ABAP_BEHV==================CP        |   314|28.04.2023|07:29:44|    30720|E        |
|   315|Prg|CL_ABAP_BEHV_TRACER===========CP        |   315|16.05.2023|13:23:33|    69632|E        |
|   316|Prg|CL_ABAP_BEHV_AUX==============CP        |   316|28.04.2023|07:29:35|    18432|E        |
|   317|Prg|CL_ABAP_BEHV_LOAD=============CP        |   317|03.05.2025|08:07:01|   139264|E        |
|   318|Prg|CL_ABAP_BEHVDESCR=============CP        |   318|30.05.2023|13:35:50|    88064|E        |
|   319|Prg|CL_ABAP_DBFEATURES============CP        |   319|26.01.2023|17:38:09|    19456|E        |
|   320|Prg|CL_ABAP_CLASSDESCR============CP        |   320|10.11.2022|19:49:27|    30720|E        |
|   321|Prg|CL_ABAP_OBJECTDESCR===========CP        |   321|31.03.2023|15:25:34|    32768|E        |
|   322|Prg|CL_RAP_EML_PROCESSOR_FACTORY==CP        |   322|19.11.2020|11:23:13|    55296|E        |
|   323|Prg|CL_SADL_EXCEPTION_GATEKEEPER==CP        |   323|12.02.2025|20:54:28|    47104|E        |
|   324|Prg|IF_SADL_EXCEPTION_HANDLER=====IP        |   322|13.01.2020|14:13:19|     7168|E        |
|   325|Prg|CL_SADL_EXC_STRATEGY_SHORTDUMPCP        |   325|15.06.2023|07:05:20|    26624|E        |
|   326|Prg|CL_SADL_TRACE_FACTORY=========CP        |   326|26.01.2023|17:38:14|    27648|E        |
|   327|Prg|CL_SALV_LOGGER================CP        |   327|10.11.2022|19:49:41|    16384|E        |
|   328|Prg|CL_SALV_CSQ_PARAMS_DB=========CP        |   328|10.11.2022|19:49:41|    89088|E        |
|   329|Prg|CL_SALV_CSQ_PARAMS============CP        |   329|10.11.2022|19:49:41|    24576|E        |
|   330|Prg|CL_SADL_TRACE_FORMATTING======CP        |   330|26.01.2023|17:38:14|    16384|E        |
|   331|Prg|CL_SADL_TRACE=================CP        |   331|28.04.2023|07:29:48|    68608|E        |
|   332|Prg|CL_SADL_UTIL==================CP        |   332|16.05.2023|13:23:10|    36864|E        |
|   333|Prg|CL_SADL_ENTITY_TRANS_FACTORY==CP        |   333|21.08.2023|10:35:32|    23552|E        |
|   334|Prg|SADL_ENTITY_TRANSACT_PROVIDER=CP        |   334|21.08.2023|10:35:32|    26624|E        |
|   335|Prg|CL_SADL_ENTITY_TRANS_PROV_CDS=CP        |   335|15.06.2023|07:05:35|    25600|E        |
|   336|Prg|CL_RAP_BHV_RT_ENTITY_METADATA=CP        |   336|12.02.2025|20:54:26|   159744|E        |
|   337|Prg|CL_SADL_TRACE_SXT_COMP_PROV===CP        |   337|26.01.2023|17:38:14|    13312|E        |
|   338|Prg|CL_RAP_ENTITY_TRANSACTIONAL===CP        |   338|03.11.2021|19:20:41|   112640|E        |
|   339|Prg|CL_RAP_TRANSACTION_MANAGER====CP        |   339|16.05.2023|13:23:32|   122880|E        |
|   340|Prg|CL_SADL_FRWK_TRANSACT_MANAGER=CP        |   340|16.05.2023|13:23:47|    35840|E        |
|   341|Prg|CL_RAP_BHV_PROCESSOR==========CP        |   341|16.05.2023|13:23:27|   194560|E        |
|   342|Prg|CL_SADL_RUNTIME_CONTEXT=======CP        |   342|26.01.2023|17:37:35|    21504|E        |
|   343|Prg|IF_RAP_TRANSACTION_PROCESSOR==IP        |   341|16.05.2023|13:23:47|    43008|E        |
|   344|Prg|CL_SADL_TRNSACT_MNGR_EXC_HNDLRCP        |   344|13.01.2020|14:10:56|    11264|E        |
|   345|Prg|CL_RAP_RT_MESSAGE_HANDLER=====CP        |   345|19.11.2020|11:23:47|   146432|E        |
|   346|Prg|CL_SADL_FRWK_MESSAGE_HANDLER==CP        |   346|19.11.2020|11:23:00|    41984|E        |
|   347|Prg|CL_SADL_MESSAGE_HANDLER_TRACE=CP        |   347|19.11.2020|11:23:10|    48128|E        |
|   348|Prg|CL_SADL_ASSERT================CP        |   348|26.01.2023|17:38:13|    31744|E        |
|   349|Prg|CL_SADL_DEV_ASSIST============CP        |   349|26.01.2023|17:37:55|     8192|E        |
|   350|Prg|IF_RAP_EML_PROCESSOR==========IP        |   339|04.04.2023|11:24:32|    10240|E        |
|   351|Prg|CL_RAP_BHV_LOG_FACTORY========CP        |   351|03.11.2021|19:21:31|    51200|E        |
|   352|Prg|CL_RAP_BHV_EML_FAIL_ONLY_LOG==CP        |   352|22.01.2021|09:24:25|    11264|E        |
|   353|Prg|/PLCE/R_PDMTOUR===============BDI       |   307|07.05.2025|00:31:51|     3072|E        |
|   354|Prg|/PLCE/BP_R_PDMTOUR============CP        |   354|17.06.2026|07:15:00|   637952|E        |
|   355|Typ|D010BEHV                                |     0|26.01.2018|14:09:25|     5120|         |
|   356|Prg|IF_ABAP_BEHV==================IP        |   306|28.04.2023|07:29:45|    17408|E        |
|   357|Prg|CL_ABAP_BEHV_SAVER_MNGD_CSP===CP        |   357|10.11.2022|19:49:06|    58368|E        |
|   358|Prg|CL_ABAP_BEHV_SAVER_MANAGED====CP        |   358|26.11.2018|11:05:00|    32768|E        |
|   359|Prg|CL_ABAP_BEHAVIOR_SAVER========CP        |   359|10.11.2022|19:48:14|    24576|E        |
|   360|Prg|CL_ABAP_BEHV_HANDLER_MNGD_CSP=CP        |   360|15.03.2020|19:36:41|    44032|E        |
|   361|Prg|CL_ABAP_BEHV_HANDLER_MANAGED==CP        |   361|22.01.2021|09:24:46|    46080|E        |
|   362|Prg|CL_ABAP_BEHV_HANDLER_GENERIC==CP        |   362|31.03.2023|15:25:26|    35840|E        |
|   363|Prg|CL_ABAP_BEHAVIOR_HANDLER======CP        |   363|23.04.2019|09:25:09|    34816|E        |
|   364|Prg|CL_ABAP_BEHV_HANDLER_ASSOC====CP        |   364|28.04.2023|07:29:46|   134144|E        |
|   365|Prg|CL_CSP_BEHV_HANDLER_FACTORY===CP        |   365|10.11.2022|19:49:17|    41984|E        |
|   366|Prg|CL_CSP_UTL_X_TRACE_FACTORY====CP        |   366|28.04.2023|07:29:50|    19456|E        |
|   367|Prg|CL_CSP_UTL_X_TRACE============CP        |   367|28.04.2023|07:29:35|    30720|E        |
|   368|Prg|CL_CSP_BEHV_A_INTERACT_HANDLERCP        |   368|19.11.2020|11:23:20|    37888|E        |
|   369|Prg|CL_CSP_BEHV_INTERACT_HANDLER==CP        |   369|19.11.2020|11:23:20|    38912|E        |
|   370|Prg|CL_CSP_RSP_PRC_CTX_WRP_FACTORYCP        |   370|19.11.2020|11:23:43|    14336|E        |
|   371|Prg|CL_CSP_RSP_PRC_CTX_WRAPPER====CP        |   371|09.07.2021|09:00:40|    51200|E        |
|   372|Prg|CL_CSP_PRC_FACTORY============CP        |   372|16.05.2023|13:23:42|    76800|E        |
|   373|Prg|CL_CSP_PRC_BEHV_READ==========CP        |   373|03.03.2023|16:55:56|    61440|E        |
|   374|Prg|CL_CSP_CATALOG_PROCESS_START==CP        |   374|16.05.2023|13:23:42|    19456|E        |
|   375|Prg|CL_CSP_PROCESS_CATALOG_FACT===CP        |   375|16.05.2023|13:23:42|    17408|E        |
|   376|Prg|CL_CSP_RUNTIME_CATALOG========CP        |   376|08.08.2024|10:13:14|   183296|E        |
|   377|Prg|CL_CSP_PROCESS_CATALOG_BUILDERCP        |   377|16.05.2023|13:23:42|    17408|E        |
|   378|Prg|CL_CSP_GENERIC_PROCESS_CATALOGCP        |   378|16.05.2023|13:23:42|    17408|E        |
|   379|Prg|IF_CSP_RT_CATALOG=============IP        |   377|16.05.2023|13:23:42|    12288|E        |
|   380|Prg|CL_CSP_RT_FACTORY=============CP        |   380|16.05.2023|13:23:42|    21504|E        |
|   381|Prg|CL_CSP_RT_PROCESS_ENGINE======CP        |   381|16.05.2023|13:23:42|    21504|E        |
|   382|Prg|IF_CSP_RT_FACTORY_HANDSHAKE===IP        |   380|14.03.2022|08:09:01|     5120|E        |
|   383|Prg|CL_CSP_RT_PLUGIN_MANAGER======CP        |   383|16.05.2023|13:23:42|    30720|E        |
|   384|Prg|CL_CSP_PLUGIN_CATALOG_FACTORY=CP        |   384|31.03.2023|15:25:24|    11264|E        |
|   385|Prg|CL_CSP_PLUGIN_CATALOG=========CP        |   385|28.04.2023|07:29:50|    14336|E        |
|   386|Prg|CL_CSP_ACT_CHECK_RBA_SUPPORTEDCP        |   386|03.03.2023|16:55:42|    60416|E        |
|   387|Prg|IF_CSP_PRC_ACT_BEHV_READ======IP        |   373|03.03.2023|16:55:56|    29696|E        |
|   388|Prg|CL_CSP_MD_ENTITY_FACTORY======CP        |   388|16.12.2022|13:58:23|    34816|E        |
|   389|Prg|CL_CSP_MD_ENTITY_FACT_WO_DECO=CP        |   389|16.12.2022|13:58:17|    28672|E        |
|   390|Prg|CL_CSP_MD_METADATA_FACTORY====CP        |   390|17.05.2022|15:17:11|    44032|E        |
|   391|Prg|CL_CSP_MD_ENTITY_MD_PROXY=====CP        |   391|16.12.2022|13:58:23|    96256|E        |
|   392|Prg|CL_CSP_PRC_RUNTIME_MD_FACTORY=CP        |   392|16.05.2023|13:23:42|    35840|E        |
|   393|Prg|CL_CSP_ADP_FACTORY_PROVIDER===CP        |   393|16.05.2023|13:23:41|    26624|E        |
|   394|Prg|CL_CSP_ADP_FACT_PROV_WO_CACHE=CP        |   394|16.05.2023|13:23:41|    21504|E        |
|   395|Prg|CL_CSP_ADP_BEHAVIOR_FACTORY===CP        |   395|16.05.2023|13:23:33|    37888|E        |
|   396|Prg|CL_CSP_WRP_BEHAVIOR_FACTORY===CP        |   396|16.05.2023|13:23:33|    28672|E        |
|   397|Prg|CL_CSP_WRP_BEHAVIOR===========CP        |   397|16.05.2023|13:23:33|    67584|E        |
|   398|Prg|CL_CSP_ADP_BEHAVIOR_ENTITY====CP        |   398|10.11.2022|19:49:15|    68608|E        |
|   399|Prg|CL_CSP_MD_ENTITY==============CP        |   399|16.05.2023|13:23:41|   173056|E        |
|   400|Prg|CL_CSP_MD_ABSTRACT_ENTITY=====CP        |   400|17.12.2018|10:04:52|     9216|E        |
|   401|Prg|CL_CSP_MD_ASSOCIATION=========CP        |   401|31.03.2023|15:25:30|    76800|E        |
|   402|Prg|CL_CSP_MD_ABSTRACT_ASSOCIATIONCP        |   402|01.02.2019|14:42:13|    49152|E        |
|   403|Prg|CL_CSP_PRC_MD_GET_ENTITY_LOAD=CP        |   403|01.04.2022|13:58:32|    79872|E        |
|   404|Prg|CL_CSP_RUNTIM_METADATA_CATALOGCP        |   404|16.05.2023|13:23:42|    46080|E        |
|   405|Prg|CL_CSP_ACT_MD_GET_ENT_BUF_LOADCP        |   405|03.11.2021|19:20:49|    67584|E        |
|   406|Prg|IF_CSP_PRC_ACT_MD_GET_ENT_LOADIP        |   403|20.12.2021|08:33:09|    53248|E        |
|   407|Prg|CL_CSP_MD_LOAD_READ_ACCESS====CP        |   407|20.12.2021|08:33:17|    79872|E        |
|   408|Prg|CL_CSP_MD_LOAD_SERIALIZER=====CP        |   408|03.11.2021|19:21:12|    29696|E        |
|   409|Prg|CL_CSP_DEC_MD_LOAD_EXISTS=====CP        |   409|03.11.2021|19:21:01|    22528|E        |
|   410|Prg|IF_CSP_PRC_DEC_MD_GET_ENT_LOADIP        |   403|03.11.2021|19:21:31|     9216|E        |
|   411|Prg|CL_CSP_ACT_MD_GET_ACT_VALIDITYCP        |   411|20.12.2021|08:33:16|    97280|E        |
|   412|Prg|CL_CSP_MD_LOAD_VALIDITY_DEF===CP        |   412|03.11.2021|19:21:13|    13312|E        |
|   413|Prg|CL_CSP_MD_LOAD_VALIDITY_DETERMCP        |   413|03.11.2021|19:21:14|    15360|E        |
|   414|Prg|CL_CSP_MD_LOAD_GET_ITEM_VAL_FACP        |   414|03.11.2021|19:21:11|    16384|E        |
|   415|Prg|CL_CSP_MD_LOAD_GET_BEHV_LD_VERCP        |   415|07.02.2022|10:19:49|    55296|E        |
|   416|Prg|CL_CSP_MD_LOAD_GET_CDS_ANNO_TSCP        |   416|03.05.2025|08:07:41|    15360|E        |
|   417|Prg|CL_CSP_ADP_DD_ANNO_FACTORY====CP        |   417|03.05.2022|08:42:37|    13312|E        |
|   418|Prg|CL_CSP_WRP_DD_ANNO_FACTORY====CP        |   418|01.04.2022|13:59:03|    11264|E        |
|   419|Prg|CL_CSP_WRP_DD_ANNOTATION======CP        |   419|03.05.2025|08:07:41|    24576|E        |
|   420|Prg|CL_CSP_ADP_DD_ANNO_ENTITY=====CP        |   420|03.05.2025|08:07:37|    28672|E        |
|   421|Prg|CL_DD_DDL_ANNO_CACHE_HANDLER==CP        |   421|15.06.2023|07:05:20|    91136|E        |
|   422|Prg|CL_CSP_MD_LOAD_GET_ENT_VER_NUMCP        |   422|03.11.2021|19:21:10|    12288|E        |
|   423|Prg|CL_CSP_MD_LOAD_VERSION========CP        |   423|03.11.2021|19:21:14|    10240|E        |
|   424|Prg|CL_CSP_DEC_MD_LOAD_IS_VALID===CP        |   424|03.11.2021|19:21:02|    22528|E        |
|   425|Prg|CL_CSP_PRC_MD_GET_ASSOC_LOAD==CP        |   425|26.01.2023|17:37:33|    98304|E        |
|   426|Prg|CL_CSP_ACT_MD_GET_ASC_BUF_LOADCP        |   426|03.11.2021|19:20:47|    81920|E        |
|   427|Prg|IF_CSP_PRC_ACT_MD_GET_ASC_LOADIP        |   425|20.12.2021|08:33:08|    70656|E        |
|   428|Prg|IF_CSP_PRC_DEC_MD_GET_ASC_LOADIP        |   425|20.12.2021|08:33:09|     9216|E        |
|   429|Prg|CL_CSP_ACT_MAP_READ_OPS=======CP        |   429|03.03.2023|16:55:45|    41984|E        |
|   430|Prg|CL_CSP_UTL_OP_MAPPER_FACTORY==CP        |   430|19.11.2020|11:23:44|    38912|E        |
|   431|Prg|CL_CSP_UTL_READ_MAPPER========CP        |   431|03.03.2023|16:56:01|    34816|E        |
|   432|Prg|CL_CSP_UTL_RBA_MAPPER=========CP        |   432|03.03.2023|16:56:01|    35840|E        |
|   433|Prg|CL_CSP_ACT_EXEC_READ_PROCESS==CP        |   433|03.03.2023|16:55:43|    74752|E        |
|   434|Prg|CL_CSP_UTL_PREF_MODE_SELECTOR=CP        |   434|03.11.2021|19:21:25|    43008|E        |
|   435|Prg|CL_CSP_MD_ENTITY_RT===========CP        |   435|03.05.2025|08:07:40|   206848|E        |
|   436|Prg|IF_CSP_MD_ENTITY_RT===========IP        |   433|03.05.2025|08:10:16|    19456|E        |
|   437|Prg|CL_CSP_PRC_READ===============CP        |   437|14.03.2022|08:08:44|    70656|E        |
|   438|Prg|CL_CSP_ACT_MERGE_DUP_REQ_PKY==CP        |   438|05.03.2021|08:21:16|    51200|E        |
|   439|Prg|IF_CSP_PRC_ACT_READ===========IP        |   437|05.03.2021|08:21:28|    12288|E        |
|   440|Prg|CL_CSP_DEC_PREFERRED_MODE=====CP        |   440|22.01.2021|09:24:54|    19456|E        |
|   441|Prg|IF_CSP_PRC_DEC_READ===========IP        |   437|14.05.2021|06:39:38|    11264|E        |
|   442|Prg|CL_CSP_DEC_IMAGE==============CP        |   442|19.11.2020|11:23:24|    24576|E        |
|   443|Prg|CL_CSP_DEC_DCL_RELEVANT=======CP        |   443|14.05.2021|06:39:43|    38912|E        |
|   444|Prg|CL_CSP_ACT_READ_FROM_BUFFERS==CP        |   444|03.11.2021|19:20:55|    93184|E        |
|   445|Prg|CL_CSP_SRV_MODIFY_BUF_FACTORY=CP        |   445|16.03.2020|10:27:43|    49152|E        |
|   446|Prg|IF_CSP_SRV_A_MODIFY_BUF_FACTRYIP        |   445|14.01.2020|13:52:20|     6144|E        |
|   447|Prg|CL_CSP_SRV_CHANGE_BUF_MEMBERS=CP        |   447|16.03.2020|10:27:42|    31744|E        |
|   448|Prg|IF_CSP_SRV_CHANGE_BUF_MEMBERS=IP        |   447|26.01.2023|17:37:59|    12288|E        |
|   449|Typ|ABP_BEHV_MNGD_ADMIN                     |     0|13.01.2020|13:37:29|     3072|         |
|   450|Prg|CL_CSP_SRV_A_CHANGE_BUFFER====CP        |   450|16.05.2023|13:23:24|    81920|E        |
|   451|Prg|CL_CSP_SRV_CHANGE_BUF_COMMON==CP        |   451|28.04.2023|07:29:48|    97280|E        |
|   452|Prg|CL_CSP_DEC_INSTANCE_KIND======CP        |   452|22.01.2021|09:25:13|    21504|E        |
|   453|Prg|CL_CSP_ACT_FAIL_UNSAVED=======CP        |   453|22.01.2021|09:24:50|    45056|E        |
|   454|Prg|CL_CSP_ACT_READ_FROM_DB=======CP        |   454|16.12.2022|13:58:09|    75776|E        |
|   455|Prg|CL_CSP_UTL_DB_RESULT_ELEMENT==CP        |   455|16.12.2022|13:58:31|    35840|E        |
|   456|Prg|CT_CSP_FLAT_ELEMENT_NAME======CP        |   456|28.04.2023|07:29:46|    17408|E        |
|   457|Prg|CL_CSP_DAC_FACTORY_PROVIDER===CP        |   457|10.11.2022|19:49:19|    39936|E        |
|   458|Prg|CL_CSP_DAC_ENTITY_WO_DAO_FACT=CP        |   458|03.05.2025|08:07:38|    32768|E        |
|   459|Prg|CL_CSP_DAC_RB_ELEMENTS_SQL====CP        |   459|16.12.2022|13:58:14|    75776|E        |
|   460|Typ|/PLCE/R_PDMTOUR                         |     0|25.02.2026|18:02:25|     5120|         |
|   461|Prg|CL_CSP_ACT_MERGE_LOADED=======CP        |   461|07.02.2022|10:19:45|    68608|E        |
|   462|Prg|CL_CSP_ACT_FILL_READ_FAILED===CP        |   462|08.11.2019|10:02:58|    22528|E        |
|   463|Prg|CL_CSP_ACT_EXEC_RBA_PROCESS===CP        |   463|03.03.2023|16:55:42|    74752|E        |
|   464|Prg|CL_CSP_PRC_READ_BY_ASSOCIATIONCP        |   464|26.01.2023|17:37:33|    86016|E        |
|   465|Prg|CL_CSP_MD_ASSOCIATION_RT======CP        |   465|03.05.2025|08:07:39|    97280|E        |
|   466|Prg|IF_CSP_PRC_ACT_RBA============IP        |   464|08.08.2024|10:14:05|    13312|E        |
|   467|Prg|CL_CSP_ACT_RBA_RESOLVE_SOURCE=CP        |   467|19.11.2020|11:23:15|    57344|E        |
|   468|Prg|CL_ABAP_CORRESPONDING=========CP        |   468|03.11.2021|19:19:30|    18432|E        |
|   469|Prg|IF_CSP_SRV_A_MODIFY_BUFFER====IP        |   445|28.04.2023|07:29:49|     6144|E        |
|   470|Prg|CL_CSP_DEC_SOURCES_EXIST======CP        |   470|19.11.2020|11:23:24|    23552|E        |
|   471|Prg|IF_CSP_PRC_DEC_RBA============IP        |   464|10.11.2022|19:50:11|    12288|E        |
|   472|Prg|CL_CSP_DEC_ASSOCIATION_TYPE===CP        |   472|19.11.2020|11:23:23|    45056|E        |
|   473|Prg|CL_CSP_DEC_ASSOC_TGT_EXIST_DEPCP        |   473|08.08.2024|10:13:41|    44032|E        |
|   474|Prg|CL_CSP_ACT_FILTER_CREATED_INSTCP        |   474|08.08.2024|10:13:41|    56320|E        |
|   475|Prg|CL_CSP_ACT_RBA_ADD_TARGETS_DB=CP        |   475|19.11.2020|11:23:13|    69632|E        |
|   476|Prg|CL_CSP_DAC_ASC_TGT_WO_DAO_FACTCP        |   476|03.05.2025|08:07:38|    34816|E        |
|   477|Prg|CL_CSP_DAC_READ_PKY_BY_ASC_SQLCP        |   477|17.05.2022|15:17:06|    41984|E        |
|   478|Prg|CL_CSP_MD_CLAUSE_PLAN_CONV_FACCP        |   478|25.02.2022|16:23:14|    12288|E        |
|   479|Prg|CL_CSP_MD_CLAUSE_PLAN_CONVERTRCP        |   479|25.02.2022|16:23:22|    26624|E        |
|   480|Prg|CL_CSP_ACT_RBA_ADD_TGTS_CRE===CP        |   480|26.01.2023|17:37:33|    56320|E        |
|   481|Prg|CL_CSP_UTL_BUF_READ_STRAT_FACTCP        |   481|26.01.2023|17:37:50|    31744|E        |
|   482|Prg|CT_CSP_INSTANCE_KIND==========CP        |   482|28.02.2019|16:18:23|     8192|E        |
|   483|Prg|CT_CSP_COMPONENT_POSITION=====CP        |   483|15.03.2020|19:37:14|     8192|E        |
|   484|Prg|CT_CSP_BUFFER_OPERATION=======CP        |   484|19.11.2020|11:22:11|     8192|E        |
|   485|Prg|CT_CSP_TABLE_KEY_NAME=========CP        |   485|20.12.2021|08:33:11|    10240|E        |
|   486|Prg|CT_CSP_OPERATION_ON_ACTIVE====CP        |   486|19.05.2020|14:44:33|     8192|E        |
|   487|Prg|CL_CSP_UTL_BUF_READ_CREATED===CP        |   487|26.01.2023|17:37:48|    57344|E        |
|   488|Prg|CL_CSP_UTL_ASC_TGT_BUF_READER=CP        |   488|19.11.2020|11:23:41|    35840|E        |
|   489|Prg|CL_CSP_ACT_RBA_ADD_TGTS_UPD===CP        |   489|19.11.2020|11:23:14|    47104|E        |
|   490|Prg|CL_CSP_UTL_BUF_READ_U_ASC_TGT=CP        |   490|26.01.2023|17:37:52|    57344|E        |
|   491|Prg|CL_CSP_DEC_IS_CROSS_PROVIDER==CP        |   491|10.11.2022|19:49:23|    47104|E        |
|   492|Prg|CL_CSP_DEC_READ_INTRA_BO_TGT==CP        |   492|10.11.2022|19:49:25|    53248|E        |
|   493|Prg|CL_CSP_ACT_RBA_READ_TARGETS===CP        |   493|22.01.2021|09:24:51|    55296|E        |
|   494|Prg|CL_CSP_ACT_RBA_LINK_REV_FK====CP        |   494|03.11.2021|19:20:54|    67584|E        |
|   495|Prg|CL_CSP_DEC_CLEAR_RBA_RESULT===CP        |   495|10.11.2022|19:49:21|    43008|E        |
|   496|Prg|CL_CSP_ACT_EXEC_READ_BY_AKFUNCCP        |   496|18.03.2023|11:02:38|    53248|E        |
|   497|Prg|/PLCE/R_PDTOUR================BD        |   317|17.06.2026|07:15:00|  2052096|E        |
|   498|Typ|DFIES                                   |     0|25.02.2015|23:57:18|    11264|         |
|   499|Prg|SAPLSDIFRUNTIME                         |   499|16.05.2023|13:23:52|    43008|E        |
|   500|Typ|X031L                                   |     0|26.02.2015|00:15:45|     7168|         |
|   501|Prg|CL_DD_DFIES_PROVIDER==========CP        |   501|16.05.2023|13:23:52|    41984|E        |
|   502|Prg|SAPLSDNT                                |   502|18.04.2025|12:30:38|   373760|E        |
|   503|Prg|CL_DD_ENV_INFO_INTERNAL=======CP        |   503|03.05.2025|07:02:00|    14336|E        |
|   504|Prg|CL_DD_TESTPROPERTIES_UPG======CP        |   504|03.05.2025|07:02:00|    19456|E        |
|   505|Prg|CL_DD_TESTPROPERTIES_ABS======CP        |   505|03.03.2023|16:56:04|    32768|E        |
|   506|Prg|CL_DD_ENV_INFO================CP        |   506|03.05.2025|08:07:03|    24576|E        |
|   507|Prg|SABP_DD_CATALOG_ACCESS========CP        |   507|16.05.2023|13:23:55|    31744|E        |
|   508|Prg|SAPLSABP_DD_CATALOG                     |   508|16.05.2023|13:23:55|   162816|E        |
|   509|Typ|X030L                                   |     0|02.05.2016|13:50:58|     7168|         |
|   510|Prg|CL_DD_DATATYPE================CP        |   510|16.05.2023|13:23:45|    76800|E        |
|   511|Prg|CL_DD_NAMETAB_INTERNAL========CP        |   511|16.05.2023|13:23:52|   156672|E        |
|   512|Prg|IF_DD_CONSTANTS===============IP        |   511|16.05.2023|13:23:52|    10240|E        |
|   513|Prg|CL_DD_DFIES_PROVIDER_STRU=====CP        |   513|16.05.2023|13:23:52|    45056|E        |
|   514|Prg|CL_DD_DFIES_CACHE=============CP        |   514|10.11.2022|19:49:32|    12288|E        |
|   515|Prg|CL_DD_EXPIMP_CACHES===========CP        |   515|08.11.2019|10:05:11|     8192|E        |
|   516|Typ|DDDFIESCACHE                            |     0|08.11.2019|08:53:27|     3072|         |
|   517|Prg|CL_DD_DDL_ANNOTATION_SERVICE==CP        |   517|15.06.2023|07:05:23|  1347584|E        |
|   518|Typ|ABAPTEXT                                |     0|16.03.1993|18:19:31|     2048|         |
|   519|Prg|CL_DD_DRTY_ANNO_PROVIDER======CP        |   519|15.06.2023|07:05:20|    47104|E        |
|   520|Prg|CL_DD_DTEL_ANNO_PROVIDER======CP        |   520|12.02.2025|20:57:20|    25600|E        |
|   521|Prg|CL_DD_EXPRESSION_TYPE_INFO====CP        |   521|16.05.2023|13:04:57|    37888|E        |
|   522|Prg|CL_DD_EXPRESSION_DTEL_INFO====CP        |   522|18.04.2025|12:30:37|    53248|E        |
|   523|Prg|CL_DD_EXPRSSN_SIMPLE_TYPE_INFOCP        |   523|03.05.2025|08:07:03|    89088|E        |
|   524|Prg|CL_DD_DDL_ANNOTATION_LEAVES===CP        |   524|15.06.2023|07:05:20|    74752|E        |
|   525|Typ|ABP_BEHV_RETRIEVALS                     |     0|26.11.2018|09:59:51|     3072|         |
|   526|Prg|/PLCE/R_PDTOUR================BDI       |   307|02.03.2026|18:00:44|     3072|E        |
|   527|Prg|/PLCE/BP_R_PDTOUR=============CP        |   527|17.06.2026|07:15:00|  1106944|E        |
|   528|Prg|/PLCE/BP_R_PDTOUR_ASSIGNMENT==CP        |   528|17.06.2026|07:15:00|   675840|E        |
|   529|Prg|/PLCE/BP_R_PDTOUR_RESOURCE====CP        |   529|17.06.2026|07:15:00|   501760|E        |
|   530|Prg|ZZBP_ZPD_R_TOUR===============CP        |   530|17.06.2026|07:15:00|   585728|E        |
|   531|Typ|/PLCE/R_PDTOUR                          |     0|25.02.2026|18:02:45|     9216|         |
|   532|Typ|ABAP_SORTORDER                          |     0|13.02.2005|17:31:39|     3072|         |
|   533|Prg|/PLCE/R_MDRESOURCE_S==========BD        |   303|07.05.2025|00:31:51|   356352|E        |
|   534|Typ|/PLCE/R_PDCONFNOTE                      |     0|16.09.2025|12:32:36|     5120|         |
|   535|Typ|/PLCE/SWR_TOUR_RSLT                     |     0|25.02.2026|18:02:36|    17408|         |
|   536|Prg|CL_EEWA_IMPL_BO_CALLBACK_MLOG=CP        |   536|05.09.2025|15:09:31|    20480|E        |
|   537|Prg|CL_EEWA_OBJECT================CP        |   537|05.09.2025|15:09:31|    11264|E        |
|   538|Prg|CL_EEWA_MSGLOG================CP        |   538|28.04.2023|12:46:46|   116736|E        |
|   539|Prg|CL_EEWA_MSGLOG_REDIRECTLIST===CP        |   539|26.02.2015|05:40:46|    14336|E        |
|   540|Typ|BAL_S_PROF                              |     0|03.03.2023|14:53:21|    49152|         |
|   541|Prg|SAPLSBAL_PROFILE                        |   541|03.03.2023|14:53:21|    21504|E        |
|   542|Typ|BAL_S_FCAT                              |     0|31.05.2013|07:57:43|     5120|         |
|   543|Prg|SAPLSBAL                                |   543|26.01.2023|17:38:07|   387072|E        |
|   544|Typ|BAL_S_LOG                               |     0|21.01.2014|10:04:42|     8192|         |
|   545|Prg|SAPLSBAL_SERVICE                        |   545|26.01.2023|17:37:48|   218112|E        |
|   546|Typ|BAL_S_PARM                              |     0|08.09.2011|14:18:31|     4096|         |
|   547|Typ|BAL_S_SCNT                              |     0|08.09.2011|14:25:35|     4096|         |
|   548|Typ|BAL_S_SDEF                              |     0|04.12.2000|12:45:15|     3072|         |
|   549|Prg|SAPLSYGU                                |   549|10.11.2022|19:49:55|    24576|E        |
|   550|Typ|BAL_S_CONT                              |     0|02.11.1998|09:43:07|     3072|         |
|   551|Typ|BAL_S_CLBK                              |     0|08.09.2011|14:18:31|     3072|         |
|   552|Prg|CL_EEWA_BO_OBJTRANSACTION=====CP        |   552|05.09.2025|15:09:31|   117760|E        |
|   553|Prg|CL_EEWA_BO_BASE===============CP        |   553|05.09.2025|15:09:31|   315392|E        |
|   554|Prg|/PLCP/CL_TA_WA_ORDER_RSLT=====CP        |   554|17.06.2026|07:11:41|   567296|E        |
|   555|Prg|CL_EEWA_TRANSACTION===========CP        |   555|05.09.2025|15:09:31|    38912|E        |
|   556|Prg|CL_EEWA_BO_OBJTYPE============CP        |   556|05.09.2025|15:09:31|   110592|E        |
|   557|Prg|CL_EEWA_CACHE_BO==============CP        |   557|05.09.2025|15:09:31|   124928|E        |
|   558|Prg|CL_EEWA_IMPL_BO_CALLBACK_CACH=CP        |   558|05.09.2025|15:09:31|    21504|E        |
|   559|Prg|CL_EEWA_BULKDATAACCESSLIST====CP        |   559|26.02.2015|07:25:25|    13312|E        |
|   560|Prg|CL_EEWA_PLUGINLIST_BO=========CP        |   560|05.09.2025|15:09:31|    40960|E        |
|   561|Prg|IF_EEWA_BO_CALLBACK_DETAIL====IP        |   553|31.01.2018|17:57:10|     5120|E        |
|   562|Prg|CX_SY_MOVE_CAST_ERROR=========CP        |   562|10.11.2022|19:49:48|    11264|E        |
|   563|Prg|CX_DYNAMIC_CHECK==============CP        |   563|10.11.2022|19:49:48|    10240|E        |
|   564|Prg|CL_EEWA_MISC==================CP        |   564|10.11.2025|17:51:03|   197632|E        |
|   565|Prg|CL_ISU_EHP4_SFW_TOOLS=========CP        |   565|26.02.2015|05:42:51|    15360|E        |
|   566|Prg|SAPLSFW_COMMON                          |   566|15.06.2023|07:05:35|   239616|E        |
|   567|Prg|CL_ISU_EHP5_SFW_TOOLS=========CP        |   567|26.02.2015|05:42:51|    13312|E        |
|   568|Typ|SFW_STATELINE                           |     0|10.10.2014|18:27:23|     3072|         |
|   569|Prg|CL_ABAP_SWITCH================CP        |   569|28.04.2023|07:29:35|    62464|E        |
|   570|Prg|CL_ISU_EHP6_SFW_TOOLS=========CP        |   570|26.02.2015|07:25:56|    12288|E        |
|   571|Prg|CL_EEWA_BULKDATAACCESS_GEN====CP        |   571|26.02.2015|07:25:25|    31744|E        |
|   572|Prg|CL_EEWA_BULKDATAACCESS_BASE===CP        |   572|26.02.2015|07:45:05|    27648|E        |
|   573|Prg|IF_EEWA_BULKDATAACCESS========IP        |   559|29.05.2015|12:35:10|     7168|E        |
|   574|Typ|EWASOBJTYPE_KEY                         |     0|25.02.2015|23:58:20|     2048|         |
|   575|Prg|CL_EEWA_DDICINFO==============CP        |   575|06.03.2023|05:37:30|   277504|E        |
|   576|Prg|CL_ABAP_REFDESCR==============CP        |   576|16.05.2023|13:23:14|    30720|E        |
|   577|Typ|EWA_EL_OBJECTTYPE_BO                    |     0|25.02.2015|23:58:23|     9216|         |
|   578|Prg|CL_ABAP_RELEASE_BLACKLIST=====CP        |   578|10.11.2022|19:49:27|    10240|E        |
|   579|Typ|TEWA_EL_OBJTYPE                         |     0|26.02.2015|01:02:09|     5120|         |
|   580|Typ|TEWA_EL_OBJTYPEM                        |     0|26.02.2015|01:02:10|     4096|         |
|   581|Prg|CL_ABAP_ELEMDESCR=============CP        |   581|16.05.2023|13:24:01|    47104|E        |
|   582|Typ|EWASDDICINCLUDES                        |     0|25.02.2015|23:58:19|     2048|         |
|   583|Typ|EWA_EL_OBJECTTYPES_BO                   |     0|25.02.2015|23:58:23|     4096|         |
|   584|Typ|EWA_EL_BO_INITDATA                      |     0|16.05.2021|13:14:20|     5120|         |
|   585|Typ|EWASBO_LANGFIELD                        |     0|25.02.2015|23:58:19|     4096|         |
|   586|Prg|EEWA_PLUGIN_BO================CP        |   586|06.03.2025|15:17:15|    15360|E        |
|   587|Prg|CL_BADI_CACHE_HANDLER=========CP        |   587|26.01.2023|17:38:04|    15360|E        |
|   588|Prg|/WATP/CL_IM_EEWA_PLUGIN_BO====CP        |   588|12.02.2025|20:57:09|    22528|E        |
|   589|Prg|IF_EX_EEWA_PLUGIN_BO==========IP        |   553|29.06.2015|14:08:25|     5120|E        |
|   590|Prg|/WATP/CL_BO_IMPLEMENTATION====CP        |   590|12.02.2025|20:57:09|   690176|E        |
|   591|Prg|/WATP/CL_BO_TRANSPORT_BASE====CP        |   591|12.02.2025|20:57:09|   157696|E        |
|   592|Prg|/WATP/CL_BO_BASE==============CP        |   592|12.02.2025|20:57:08|   462848|E        |
|   593|Prg|/WATP/CL_OBJECT===============CP        |   593|12.02.2025|18:37:16|    10240|E        |
|   594|Typ|/WATP/TTIMPL                            |     0|08.08.2024|08:30:14|     6144|         |
|   595|Typ|/WATP/TTIMPLMAN                         |     0|08.08.2024|08:30:14|     3072|         |
|   596|Typ|TEWA_EL_OBJTYPET                        |     0|26.02.2015|01:02:10|     3072|         |
|   597|Prg|CL_ABAP_DYN_PRG===============CP        |   597|18.03.2023|11:02:27|    71680|E        |
|   598|Typ|EWASOBJTRA_KEY                          |     0|25.02.2015|23:58:20|     2048|         |
|   599|Typ|EWA_EL_OBJECTTRANSACTION_BO             |     0|25.02.2015|23:58:23|    10240|         |
|   600|Typ|TEWA_EL_OBJTRA                          |     0|26.02.2015|01:02:08|     6144|         |
|   601|Typ|TEWA_EL_OBJTRAM                         |     0|26.02.2015|01:02:09|     5120|         |
|   602|Typ|TEWA_EL_OBJTRAT                         |     0|26.02.2015|01:02:09|     3072|         |
|   603|Prg|ZCL_PLCP_TA_WA_ORDER_RSLT=====CP        |   603|17.06.2026|07:14:58|   291840|E        |
|   604|Prg|EEWA_PLUGIN_TA================CP        |   604|21.08.2023|09:25:42|    15360|E        |
|   605|Prg|CL_EEWA_PLUGINLIST_TA=========CP        |   605|05.09.2025|15:09:31|    22528|E        |
|   606|Prg|/WATP/CL_IM_EWAPLGTA_CACHE_BO=CP        |   606|08.08.2024|10:54:03|     8192|E        |
|   607|Prg|/WATP/CL_IM_EEWA_PLUGIN_TA====CP        |   607|12.02.2025|20:57:09|    21504|E        |
|   608|Prg|IF_EX_EEWA_PLUGIN_TA==========IP        |   555|29.06.2015|14:08:26|     5120|E        |
|   609|Prg|/WATP/CL_EEWA_PLG_TA_CACHE_BO=CP        |   609|12.02.2025|20:55:25|    15360|E        |
|   610|Prg|/WATP/CL_CACHE_BO=============CP        |   610|12.02.2025|20:57:09|   126976|E        |
|   611|Prg|/WATP/CL_IMPL_BO_CALLBACK_CACHCP        |   611|12.02.2025|18:36:48|    24576|E        |
|   612|Prg|CL_EEWA_STRINGS===============CP        |   612|08.08.2024|10:25:44|    78848|E        |
|   613|Prg|CL_EEWA_XML_TRANSFORMER=======CP        |   613|26.02.2015|05:41:03|    17408|E        |
|   614|Prg|CL_IXML=======================CP        |   614|10.11.2022|19:49:37|   257024|E        |
|   615|Prg|CL_IXML_UNKNOWN===============CP        |   615|01.06.2015|12:30:30|     8192|E        |
|   616|Typ|/PLCP/SWA_WDO_RSLT                      |     0|07.11.2025|23:02:05|     6144|         |
|   617|Prg|CL_EEWA_CONSTANTS_CONFIRMATIONCP        |   617|15.11.2022|09:55:25|    12288|E        |
|   618|Prg|CL_EEWA_CONSTANTS=============CP        |   618|26.02.2015|07:25:25|     7168|E        |
|   619|Prg|CL_EEWA_BO_WDORDER============CP        |   619|17.06.2026|07:11:42|   843776|E        |
|   620|Prg|IF_DD_NAMETAB_CONSTANTS=======IP        |   513|16.12.2022|13:58:23|    10240|E        |
|   621|Prg|CL_EEWA_TA_SUBCONTRACTING_BASECP        |   621|17.06.2026|07:11:42|   274432|E        |
|   622|Typ|EWASWDORDER                             |     0|05.09.2025|15:07:44|    23552|         |
|   623|Typ|EWA_ORDER_HEAD                          |     0|05.09.2025|15:09:30|    21504|         |
|   624|Typ|EWASBO_RELATION                         |     0|16.05.2021|13:14:20|     4096|         |
|   625|Typ|EWASJSTAT                               |     0|25.02.2015|23:58:20|     3072|         |
|   626|Typ|EWASORDFILLK                            |     0|20.11.2020|06:09:32|     7168|         |
|   627|Typ|EWA_ORDER_RESOBJ                        |     0|08.08.2024|08:36:18|     8192|         |
|   628|Prg|CL_EEWA_BO_WDORDERITEM========CP        |   628|17.06.2026|07:11:56|  1520640|E        |
|   629|Typ|EWASWDORDERITEMDET                      |     0|09.04.2026|00:02:04|    56320|         |
|   630|Typ|EWASSUB_WDOSUB_DMRPOS_DET               |     0|22.01.2021|17:14:16|     5120|         |
|   631|Typ|EWAS_SUB_WDO_PO_DET                     |     0|05.03.2021|19:56:40|     4096|         |
|   632|Prg|CX_SY_DYN_CALL_ILLEGAL_CLASS==CP        |   632|10.11.2022|19:49:48|    11264|E        |
|   633|Prg|CX_SY_DYN_CALL_ERROR==========CP        |   633|10.11.2022|19:49:48|    11264|E        |
|   634|Prg|CL_EXITHANDLER================CP        |   634|31.03.2023|15:25:35|    51200|E        |
|   635|Prg|SAPLSEXV                                |   635|16.05.2023|13:23:36|   163840|E        |
|   636|Prg|CL_EX_EEWA_BA_WDORDER=========CP        |   636|26.02.2015|05:41:07|    14336|E        |
|   637|Prg|CL_BADI_FLT_DATA_TRANS_AND_DB=CP        |   637|15.06.2023|07:05:34|    47104|E        |
|   638|Typ|SXC_EXIT                                |     0|09.11.2000|14:23:43|     3072|         |
|   639|Prg|SAPLSEXE                                |   639|15.06.2023|07:05:35|   137216|E        |
|   640|Prg|IF_EX_EEWA_BA_WDORDER=========IP        |   553|26.02.2015|05:43:33|     5120|E        |
|   641|Prg|%_CSXRT                                 |   636|01.06.2015|12:30:30|    16384|E        |
|   642|Prg|CL_EXIT_MASTER================CP        |   642|16.12.2022|11:36:12|    39936|E        |
|   643|Prg|CL_EX_BADI_LAYER==============CP        |   643|16.12.2022|11:36:12|    30720|E        |
|   644|Prg|IF_EX_BADI_LAYER==============IP        |   634|16.12.2022|11:36:13|     7168|E        |
|   645|Prg|/WATP/CL_IM_BA_WDORDER========CP        |   645|08.08.2024|10:53:29|     8192|E        |
|   646|Prg|SAPLASTAT_TRIG                          |   646|01.06.2015|12:30:31|    17408|E        |
|   647|Prg|/WATP/CL_BA_WDORDER===========CP        |   647|09.04.2026|00:02:53|    28672|E        |
|   648|Prg|CL_EEWA_BD_BO_BASE============CP        |   648|26.02.2015|05:40:14|    15360|E        |
|   649|Prg|IF_EX_EEWAEL_BA_BASE==========IP        |   553|01.04.2022|21:44:02|     8192|E        |
|   650|Prg|IF_EX_EEWA_BO_WDORDER=========IP        |   619|26.02.2015|05:43:34|     8192|E        |
|   651|Prg|/WATP/CL_CALLBACK_CONNECTOR===CP        |   651|12.02.2025|20:57:09|    22528|E        |
|   652|Prg|/WATP/CL_BO_OBJTYPE===========CP        |   652|12.02.2025|20:57:09|   350208|E        |
|   653|Prg|/WATP/CL_PLUGINLIST_BO========CP        |   653|12.02.2025|20:57:11|    58368|E        |
|   654|Prg|/WATP/CL_MISC=================CP        |   654|12.02.2025|20:57:09|   353280|E        |
|   655|Prg|/WATP/CL_SAPSYSTEMINFO========CP        |   655|12.02.2025|20:57:11|   125952|E        |
|   656|Prg|SAPLTIMEEXT                             |   656|01.06.2015|12:30:31|    12288|E        |
|   657|Prg|%_CTRNSP                                |   137|28.02.2019|15:30:04|    10240|E        |
|   658|Typ|TRNSPACE                                |     0|28.02.2019|15:30:04|     3072|         |
|   659|Prg|/WATP/IF_BO_CALLBACK_DETAIL===IP        |   592|08.08.2024|09:25:10|     5120|E        |
|   660|Prg|/WATP/CL_DDICINFO=============CP        |   660|12.02.2025|20:56:03|   292864|E        |
|   661|Typ|/WATP/SOBJTYPE_BO                       |     0|08.08.2024|08:35:48|    15360|         |
|   662|Typ|/WATP/TTOBJTYPE                         |     0|08.08.2024|08:30:14|     9216|         |
|   663|Typ|/WATP/TTOBJTYPEM                        |     0|08.08.2024|08:30:14|     5120|         |
|   664|Typ|/WATP/SDDICINCLUDES                     |     0|08.08.2024|08:35:51|     2048|         |
|   665|Typ|/WATP/SBO_RELATION                      |     0|08.08.2024|08:35:49|     4096|         |
|   666|Typ|/WATP/TTOBJTYPEB                        |     0|08.08.2024|08:30:14|     3072|         |
|   667|Typ|/WATP/TTOBJTYPED                        |     0|24.10.2024|09:00:49|     5120|         |
|   668|Typ|/WATP/SBO_INITDATA                      |     0|08.08.2024|08:35:49|     5120|         |
|   669|Typ|/WATP/TTOBJTYPEA                        |     0|08.08.2024|08:30:14|     3072|         |
|   670|Typ|/WATP/SBO_LANGFIELD                     |     0|08.08.2024|08:35:49|     4096|         |
|   671|Typ|/WATP/SRANGE                            |     0|08.08.2024|08:35:49|     3072|         |
|   672|Prg|/WATP/CL_OBJECTREADER=========CP        |   672|12.02.2025|20:57:09|    87040|E        |
|   673|Typ|/WATP/SOBJREAD_KEY                      |     0|08.08.2024|08:35:48|     3072|         |
|   674|Prg|/WATP/IF_INVOKE===============IP        |   320|08.08.2024|09:25:19|     6144|E        |
|   675|Prg|/WATP/IF_PROGRESS=============IP        |   320|08.08.2024|09:25:22|     5120|E        |
|   676|Prg|/WATP/IF_PROTOCOL=============IP        |   320|08.08.2024|09:25:22|     6144|E        |
|   677|Prg|/WATP/IF_RUNTIME_ANALYSIS=====IP        |   320|08.08.2024|09:25:22|     5120|E        |
|   678|Prg|/WATP/IF_STRINGS_SUBSTITUTION=IP        |   320|08.08.2024|09:25:04|     5120|E        |
|   679|Typ|/WATP/TTOBJTYPET                        |     0|08.08.2024|08:30:14|     4096|         |
|   680|Typ|/WATP/TTOBJTYPMT                        |     0|08.08.2024|08:30:14|     4096|         |
|   681|Typ|/WATP/SBO_DETAILRECORD                  |     0|08.08.2024|08:35:49|     4096|         |
|   682|Prg|/WATP/CL_STRINGS==============CP        |   682|12.02.2025|20:55:26|   135168|E        |
|   683|Prg|CL_ABAP_CONV_UC_NUMBER========CP        |   683|10.11.2022|19:49:27|    14336|E        |
|   684|Typ|CODEPAGES                               |     0|09.07.1997|23:51:03|     2048|         |
|   685|Prg|CL_ABAP_CONV_OBJ==============CP        |   685|16.05.2023|13:23:09|    31744|E        |
|   686|Prg|SAPLSCPA                                |   686|26.01.2023|17:38:15|    65536|E        |
|   687|Typ|TCP00                                   |     0|12.02.2001|19:31:42|     6144|         |
|   688|Prg|/WATP/CL_XML_TRANSFORMER======CP        |   688|12.02.2025|18:36:51|    24576|E        |
|   689|Prg|/WATP/CL_BO_OBJREADER=========CP        |   689|12.02.2025|20:57:09|   216064|E        |
|   690|Typ|/WATP/SOBJREAD                          |     0|24.10.2024|08:59:55|     9216|         |
|   691|Typ|/WATP/TTOBJREAD                         |     0|24.10.2024|09:00:48|     6144|         |
|   692|Typ|/WATP/TTOBJREADM                        |     0|08.08.2024|08:30:14|     4096|         |
|   693|Typ|/WATP/TTOBJREADT                        |     0|08.08.2024|08:30:14|     4096|         |
|   694|Prg|/WATP/CL_OR_IMPLEMENTBOADDON==CP        |   694|12.02.2025|20:57:10|    56320|E        |
|   695|Prg|/WATP/IF_OBJECTREADER=========IP        |   689|08.08.2024|09:25:20|     9216|E        |
|   696|Prg|CL_ABAP_TABLEDESCR============CP        |   696|16.05.2023|13:23:14|    36864|E        |
|   697|Prg|/WATP/CL_RANGES===============CP        |   697|12.02.2025|20:56:08|   106496|E        |
|   698|Prg|/WATP/CL_BO_EWAORDER==========CP        |   698|09.04.2026|00:02:54|   262144|E        |
|   699|Prg|/WATP/CL_BO_EWAORDER_BASE=====CP        |   699|09.04.2026|00:02:54|   146432|E        |
|   700|Prg|/WATP/IF_EX_BA_BASE===========IP        |   647|08.08.2024|09:25:11|     8192|E        |
|   701|Typ|/WATP/SBO_EWAORDER                      |     0|05.09.2025|15:07:27|    23552|         |
|   702|Typ|/WATP/TSAPJOBJNR                        |     0|08.08.2024|08:30:13|     3072|         |
|   703|Typ|EWATORDFILLK                            |     0|20.11.2020|06:41:55|     7168|         |
|   704|Prg|/WATP/CL_SAPSTATUS============CP        |   704|12.02.2025|20:55:26|    51200|E        |
|   705|Prg|SAPLBSVT                                |   705|21.08.2023|09:57:35|    79872|E        |
|   706|Typ|TJ03T                                   |     0|31.05.2013|08:15:13|     3072|         |
|   707|Prg|IF_EEWA_PLUGIN_BO_BASE========IP        |   553|16.06.2015|09:33:31|    11264|E        |
|   708|Prg|CL_EEWA_SAPSTATUS=============CP        |   708|05.09.2025|15:09:31|    55296|E        |
|   709|Prg|CL_EEWA_TRANSACTION_STATE=====CP        |   709|29.05.2015|12:31:23|    12288|E        |
|   710|Prg|SAPLEEWA_EL_BASE                        |   710|05.09.2025|15:09:32|   251904|E        |
|   711|Typ|JSTO_PRE                                |     0|30.11.1993|14:11:48|     2048|         |
|   712|Prg|SAPLBSVA                                |   712|01.06.2015|15:05:09|   701440|E        |
|   713|Typ|JCDS                                    |     0|28.05.2015|10:40:20|     4096|         |
|   714|Typ|JSTAT                                   |     0|30.11.1993|14:11:44|     2048|         |
|   715|Prg|SAPLSMSG                                |   715|03.03.2023|14:53:21|   141312|E        |
|   716|Typ|JSTO                                    |     0|28.05.2015|10:40:20|     3072|         |
|   717|Typ|JEST                                    |     0|28.05.2015|10:40:20|     3072|         |
|   718|Prg|SAPLAIEV                                |   718|10.11.2022|19:49:53|    14336|E        |
|   719|Typ|ISU_AUTHOR                              |     0|24.02.2005|13:45:13|     3072|         |
|   720|Prg|SAPLEE00                                |   720|17.06.2026|07:11:44|   345088|E        |
|   721|Prg|/1BCDWBEN/SAPLEEN0000                   |   721|05.09.2025|15:10:17|   266240|E        |
|   722|Prg|SAPLSENA                                |   722|16.05.2023|13:23:57|    60416|E        |
|   723|Prg|CL_RLFW_SERVER_GROUP==========CP        |   723|03.03.2023|16:55:44|    32768|E        |
|   724|Typ|EEWABO_DETAILRECORD                     |     0|26.11.2018|19:13:29|     4096|         |
|   725|Typ|EWA_ORDER_OBJECT                        |     0|09.04.2026|00:02:53|    51200|         |
|   726|Typ|EWA_ORDER_OBJ_SD                        |     0|13.04.2016|15:02:09|     4096|         |
|   727|Typ|EWA_ORDER_BULKY                         |     0|03.05.2025|05:24:22|     7168|         |
|   728|Typ|/WATP/SJSTAT                            |     0|08.08.2024|08:35:52|     3072|         |
|   729|Typ|/PLCP/SWDO_RSLT_ENH                     |     0|08.08.2024|08:36:19|     4096|         |
|   730|Prg|SAPLEEWA_ORDER_INT                      |   730|17.06.2026|07:11:44|  1637376|E        |
|   731|Typ|EWASSUBCONTRACTINGSLSCNTRPKEY           |     0|02.06.2015|08:58:41|     3072|         |
|   732|Prg|SAPLEEWA_BRF                            |   732|17.06.2026|07:11:44|    63488|E        |
|   733|Prg|CL_CONTROLLER_POOL_BRF========CP        |   733|08.09.2011|14:59:05|    12288|E        |
|   734|Prg|CL_APPLICATION_CLASS_BRF======CP        |   734|08.09.2011|15:08:06|    10240|E        |
|   735|Prg|CL_DB_BRF=====================CP        |   735|30.01.2017|10:07:36|  3148800|E        |
|   736|Typ|TBRF100T                                |     0|08.09.2011|14:24:30|     3072|         |
|   737|Typ|TBRF133                                 |     0|31.05.2013|08:14:35|     5120|         |
|   738|Typ|TBRF210                                 |     0|31.05.2013|08:14:35|     9216|         |
|   739|Typ|TBRF110T                                |     0|31.05.2013|08:14:35|     4096|         |
|   740|Typ|TABLENAME                               |     0|01.04.1998|01:54:17|     3072|         |
|   741|Prg|CL_CONTROLLER_BRF=============CP        |   741|08.09.2011|14:59:04|    31744|E        |
|   742|Prg|IF_CONTROLLER_BRF=============IP        |   733|08.09.2011|15:50:36|     9216|E        |
|   743|Prg|CL_ACTION_PROCESSOR_BRF=======CP        |   743|22.01.2021|09:24:26|    23552|E        |
|   744|Prg|CL_TRACE_POOL_BRF=============CP        |   744|08.09.2011|15:28:39|    11264|E        |
|   745|Prg|CL_TRACE_BRF==================CP        |   745|08.09.2011|15:28:38|    32768|E        |
|   746|Prg|IF_TRACE_BRF==================IP        |   744|08.09.2011|15:46:18|    15360|E        |
|   747|Typ|BRF_TRACE_OBJECTS                       |     0|08.09.2011|14:17:37|     3072|         |
|   748|Typ|BRF_CONTROLLER_POOL                     |     0|08.09.2011|14:17:33|     3072|         |
|   749|Typ|EWASWDORDERITEM                         |     0|09.04.2026|00:02:03|    55296|         |
|   750|Prg|CL_OBJECT_POOL_BRF============CP        |   750|08.09.2011|15:15:00|    15360|E        |
|   751|Prg|CL_CLASS_BRF==================CP        |   751|01.06.2015|11:33:46|    22528|E        |
|   752|Typ|TBRF180T                                |     0|31.05.2013|08:14:35|     5120|         |
|   753|Prg|CL_EVENT_BRF==================CP        |   753|08.09.2011|15:04:40|    20480|E        |
|   754|Prg|CL_EVENT_BASE_BRF=============CP        |   754|08.09.2011|15:04:34|    38912|E        |
|   755|Prg|IF_EVENT_BRF==================IP        |   750|08.09.2011|15:51:59|    16384|E        |
|   756|Typ|SBRF110A                                |     0|08.09.2011|14:28:07|     7168|         |
|   757|Prg|IF_RULE_COMPONENT_BRF=========IP        |   754|08.09.2011|15:39:52|     7168|E        |
|   758|Typ|TBRF185                                 |     0|31.05.2013|08:14:35|     3072|         |
|   759|Typ|TBRF110                                 |     0|31.05.2013|08:14:35|     6144|         |
|   760|Prg|CL_DET_ASSIGNMNT_RL_BRF=======CP        |   760|08.09.2011|15:14:21|    19456|E        |
|   761|Prg|IF_DETERMINE_ASSIGNMENT_BRF===IP        |   754|08.09.2011|15:51:04|     7168|E        |
|   762|Prg|CL_DET_ASSIGNMNT_RS_BRF=======CP        |   762|08.09.2011|15:14:21|    19456|E        |
|   763|Typ|TBRF280                                 |     0|31.05.2013|08:14:35|     6144|         |
|   764|Typ|TBRF280T                                |     0|31.05.2013|08:14:35|     4096|         |
|   765|Typ|SBRF210A                                |     0|08.09.2011|14:23:54|    11264|         |
|   766|Prg|CL_EXPRESSION_POOL_BRF========CP        |   766|01.06.2015|11:34:00|    47104|E        |
|   767|Prg|CX_SY_PROGRAM_NOT_FOUND=======CP        |   767|18.02.2005|14:13:43|    10240|E        |
|   768|Prg|IF_T100_MESSAGE===============IP        |   766|10.11.2022|19:49:51|     7168|E        |
|   769|Typ|EWASORDFILLK_DATA                       |     0|20.11.2020|06:09:32|     6144|         |
|   770|Typ|EWASBO_DETAILINDEX                      |     0|25.02.2015|23:58:19|     2048|         |
|   771|Typ|/WATP/AEWAWDOI_INFO                     |     0|24.10.2024|08:59:52|     3072|         |
|   772|Typ|ZWR_AEWMD_EWAELOCSD_NEW                 |     0|09.04.2026|00:02:44|     3072|         |
|   773|Typ|EWASRANGE                               |     0|25.02.2015|23:58:20|     3072|         |
|   774|Typ|EWASDDICFKEY                            |     0|25.02.2015|23:58:19|     4096|         |
|   775|Typ|EWASDDICFKEYTABLE                       |     0|25.02.2015|23:58:19|     4096|         |
|   776|Typ|EWASCOPYFIELDNAME                       |     0|25.02.2015|23:58:19|     3072|         |
|   777|Typ|ISU_ADMIN                               |     0|08.09.2011|14:21:07|     3072|         |
|   778|Prg|SAPLE71WASTE                            |   778|17.06.2026|07:11:44|   374784|E        |
|   779|Prg|SAPLRSA2                                |   779|16.12.2022|11:36:13|   522240|E        |
|   780|Prg|SAPLRSAN                                |   780|18.06.2009|10:06:03|    54272|E        |
|   781|Prg|SAPLRSSCM                               |   781|17.06.2024|13:17:00|  1064960|E        |
|   782|Typ|RSSCM_BWCLNT                            |     0|25.03.2003|17:47:51|     2048|         |
|   783|Typ|T000                                    |     0|08.09.2011|14:23:54|     5120|         |
|   784|Prg|SAPLSCD0                                |   784|12.02.2025|20:54:40|   197632|E        |
|   785|Typ|CDPOS_UID                               |     0|01.06.2015|16:00:31|     4096|         |
|   786|Typ|CDPOS_STR                               |     0|01.06.2015|16:00:31|     6144|         |
|   787|Typ|CDTXT                                   |     0|31.05.2013|07:59:10|     3072|         |
|   788|Typ|CDPOS                                   |     0|03.06.2015|15:16:20|     6144|         |
|   789|Prg|SAPLSCD8                                |   789|10.11.2022|19:49:54|    94208|E        |
|   790|Prg|CL_CHDO_OBJECT_TOOLS==========CP        |   790|15.06.2023|07:05:36|   337920|E        |
|   791|Prg|CL_CBO_CHANGE_DOC_INQUIRY=====CP        |   791|12.02.2025|20:53:50|    22528|E        |
|   792|Prg|IF_CBO_CONSTANTS==============IP        |   791|17.05.2022|15:15:56|    29696|E        |
|   793|Prg|CL_ABAP_BEHV_TRANSACTION_NTFR=CP        |   793|16.04.2021|19:43:16|     9216|E        |
|   794|Prg|CL_OS_TRANSACTION_END_NOTIFIERCP        |   794|05.03.2021|08:21:43|     9216|E        |
|   795|Typ|JCDO                                    |     0|28.05.2015|10:40:19|     4096|         |
|   796|Typ|ONR00                                   |     0|14.12.1992|11:16:42|     2048|         |
|   797|Typ|TJ01T                                   |     0|31.05.2013|08:15:13|     3072|         |
|   798|Typ|THEAD                                   |     0|31.05.2013|08:15:00|     8192|         |
|   799|Typ|TLINE                                   |     0|08.03.1992|00:19:59|     3072|         |
|   800|Prg|SAPLBSVX                                |   800|08.09.2011|15:57:51|    95232|E        |
|   801|Prg|ZCL_WR_EEWA_BO_WDORDERITEM====CP        |   801|17.06.2026|21:16:28|   453632|E        |
|   802|Prg|CL_EX_ISU_WA_HAZARD_WASTE=====CP        |   802|17.06.2026|07:11:10|   273408|E        |
|   803|Prg|IF_EX_ISU_WA_HAZARD_WASTE=====IP        |   634|17.06.2026|07:11:10|   196608|E        |
|   804|Prg|/PLCE/R_PDSERVICE=============BD        |   306|17.06.2026|07:15:00|  1362944|E        |
|   805|Prg|/PLCE/R_PDSERVICE=============BDI       |   307|17.06.2026|07:15:00|     3072|E        |
|   806|Prg|/PLCE/BP_R_PDSERVICE==========CP        |   806|17.06.2026|07:15:00|   749568|E        |
|   807|Prg|/PLCE/BP_R_PDSERVICEUNI=======CP        |   807|17.06.2026|07:15:00|    12288|E        |
|   808|Prg|/PLCE/BP_R_PDSERVICEWR========CP        |   808|17.06.2026|07:15:00|   140288|E        |
|   809|Prg|ZBP_ZPD_BP_R_PDSERVICE========CP        |   809|17.06.2026|07:15:00|   319488|E        |
|   810|Prg|ZBP_ZPD_R_SERVICE=============CP        |   810|17.06.2026|07:15:00|   273408|E        |
|   811|Typ|/PLCE/R_PDSERVICE                       |     0|14.05.2026|18:01:18|     8192|         |
|   812|Typ|/PLCE/R_PDSERVICEATTACHMENT             |     0|25.02.2026|18:02:26|     5120|         |
|   813|Prg|CL_CSP_UTL_INSTANCE_KIND======CP        |   813|03.11.2021|19:21:24|    13312|E        |
|   814|Typ|/PLCE/R_PDMTOURCONFNOTE                 |     0|25.02.2026|18:02:25|     8192|         |
|   815|Typ|/PLCE/R_PDMTOURDOCRESULT                |     0|25.02.2026|18:02:25|     7168|         |
|   816|Typ|/PLCE/R_PDMTOURSERVICERESULT            |     0|25.02.2026|18:02:25|     5120|         |
|   817|Typ|/PLCE/R_PDMTOURACTIVITY                 |     0|25.02.2026|18:02:25|     7168|         |
|   818|Typ|/PLCE/R_PDMTOURADDRESULT                |     0|25.02.2026|18:02:25|     5120|         |
|   819|Typ|/PLCE/R_PDMTOURSERVICERESULTWR          |     0|07.05.2025|00:28:57|     6144|         |
|   820|Typ|/PLCE/P_PDTOURASSIGNMENT                |     0|25.02.2026|18:02:25|     4096|         |
|   821|Prg|/PLCE/R_MDRESOURCE_S==========BDI       |   307|07.05.2025|00:31:50|     3072|E        |
|   822|Prg|/PLCE/BP_R_MDRESOURCE_S=======CP        |   822|07.05.2025|00:31:50|   186368|E        |
|   823|Prg|CL_CSP_ACT_CHECK_ACL==========CP        |   823|03.11.2021|19:20:39|    92160|E        |
|   824|Typ|/PLCE/R_MDRESOURCE                      |     0|07.05.2025|00:30:45|     5120|         |
|   825|Typ|/PLCE/R_PDMTOURWEIGHING                 |     0|25.02.2026|18:02:26|     7168|         |
|   826|Typ|/PLCE/SWR_SERVICE_RSLT                  |     0|25.02.2026|18:02:36|    17408|         |
|   827|Typ|/PLCE/SWR_SERVICE_RSLT_DATA             |     0|08.01.2025|16:36:15|     5120|         |
|   828|Typ|/PLCE/SWR_SERVICE_ATTA_RSLT             |     0|08.08.2024|08:36:18|     4096|         |
|   829|Prg|/PLCE/CL_PD_CONSTANTS=========CP        |   829|07.05.2025|00:31:45|    41984|E        |
|   830|Typ|/PLCE/SWR_SERVICE_ADD_RSLT              |     0|08.01.2025|16:36:15|     3072|         |
|   831|Typ|/PLCP/SWA_WDOI_RSLT                     |     0|07.11.2025|23:02:04|    12288|         |
|   832|Typ|/PLCE/TPDSRVATTM                        |     0|12.02.2025|18:29:15|     6144|         |
|   833|Typ|/PLCP/SWA_ATTACHMENT_RSLT               |     0|19.11.2025|17:33:41|     6144|         |
|   834|Prg|ZCL_WR_MISC_GOS===============CP        |   834|14.05.2026|18:01:39|    47104|E        |
|   835|Typ|/PLCP/SWA_WDOIADDSRV_RSLT               |     0|08.01.2025|16:36:20|     3072|         |
|   836|Typ|/PLCP/SWA_WDOH_KEY                      |     0|08.08.2024|08:36:19|     3072|         |
|   837|Typ|EWA_EL_BO_RETAINDATA                    |     0|28.05.2018|11:32:15|     3072|         |
|   838|Typ|EWASORDOBJFILLK                         |     0|20.11.2020|06:09:32|     9216|         |
|   839|Typ|EWASWDORDERITEM_VBAP                    |     0|11.12.2025|08:31:12|   100352|         |
|   840|Typ|EWASORDERDMRPOS_DET                     |     0|15.03.2020|23:41:25|     5120|         |
|   841|Typ|EWASSUB_WDOISUB_DMRPOS_DET              |     0|22.01.2021|17:14:16|     5120|         |
|   842|Typ|EWAS_SUB_WDOI_PO_DET                    |     0|05.03.2021|19:56:40|     4096|         |
|   843|Typ|ZWR_SWDOITEM_DIFFCULT                   |     0|16.09.2025|12:32:28|     4096|         |
|   844|Prg|CL_EX_EEWA_BA_WDORDERITEM=====CP        |   844|26.02.2015|05:41:07|    14336|E        |
|   845|Prg|IF_EX_EEWA_BA_WDORDERITEM=====IP        |   553|26.02.2015|05:43:33|     5120|E        |
|   846|Typ|EWA_ORDER_OBJECT_IKEY                   |     0|25.02.2015|23:58:23|     3072|         |
|   847|Prg|SAPLEEWA_WDOITEM                        |   847|17.06.2026|07:11:44|   702464|E        |
|   848|Prg|CL_EEWA_DCFL==================CP        |   848|17.06.2026|07:11:42|   581632|E        |
|   849|Prg|ZCL_WR_MISC_EEWA==============CP        |   849|08.05.2026|09:28:15|    26624|E        |
|   850|Prg|CL_EEWA_IMPL_BO_CALLBACK_LIST=CP        |   850|26.02.2015|05:40:44|    16384|E        |
|   851|Prg|BSV_OBJECT_ACTIVATION=========CP        |   851|21.08.2023|09:57:35|    14336|E        |
|   852|Typ|TJ05                                    |     0|30.11.1993|16:45:10|     2048|         |
|   853|Typ|TJ01                                    |     0|01.03.1999|13:50:12|     6144|         |
|   854|Typ|TJ07                                    |     0|30.11.1993|16:45:13|     3072|         |
|   855|Prg|BSV_STATUS_PROFILE============CP        |   855|21.08.2023|09:27:45|    14336|E        |
|   856|Typ|TJ02                                    |     0|27.03.1996|18:28:28|     3072|         |
|   857|Typ|TJ03                                    |     0|22.02.2001|10:04:57|     4096|         |
|   858|Typ|VBAP                                    |     0|11.12.2025|08:34:38|    99328|         |
|   859|Prg|SAPLV45I                                |   859|21.05.2026|18:09:56|   982016|E        |
|   860|Typ|VBAPVB                                  |     0|11.12.2025|08:33:43|   126976|         |
|   861|Prg|CL_DCFL_UTILITIES_MODE========CP        |   861|31.05.2013|09:53:12|    17408|E        |
|   862|Prg|BADI_DCFL_GET_BF_SWITCH=======CP        |   862|21.08.2023|09:26:35|    15360|E        |
|   863|Prg|BADI_DCFL_SET_DECOUPLING_MODE=CP        |   863|21.08.2023|09:26:35|    13312|E        |
|   864|Prg|CL_DCFL_GET_BF_SWITCH=========CP        |   864|14.09.2011|14:40:16|     8192|E        |
|   865|Prg|IF_BADI_INTERFACE=============IP        |   861|01.06.2015|12:30:31|     5120|E        |
|   866|Prg|IF_DCFL_GET_BF_SWITCH=========IP        |   861|08.09.2011|17:12:43|     5120|E        |
|   867|Prg|CL_FIN_DCFL_SWITCH_CHECK======CP        |   867|14.09.2011|14:40:16|     8192|E        |
|   868|Typ|EWATORDERDMRPOS                         |     0|15.03.2020|23:55:25|     5120|         |
|   869|Typ|EWATDEVLOC                              |     0|16.06.2015|08:35:55|     7168|         |
|   870|Prg|ZCX_EEWA_BO_ORD===============CP        |   870|05.05.2026|18:06:03|    39936|E        |
|   871|Prg|CX_EEWA_BO_BASE===============CP        |   871|08.08.2024|10:25:27|    29696|E        |
|   872|Prg|CX_EEWA_BASE==================CP        |   872|14.11.2019|00:09:51|    35840|E        |
|   873|Typ|SCX_T100KEY                             |     0|31.05.2013|08:12:18|     3072|         |
|   874|Prg|IF_EEWA_PROTOCOL==============IP        |   801|31.01.2018|17:57:10|     6144|E        |
|   875|Prg|CX_EEWA_SILENT================CP        |   875|27.11.2018|02:20:11|    20480|E        |
|   876|Prg|CL_MESSAGE_HELPER=============CP        |   876|10.11.2022|19:49:38|    47104|E        |
|   877|Prg|IF_MESSAGE====================IP        |   237|10.11.2022|19:49:50|     5120|E        |
|   878|Prg|IF_SERIALIZABLE_OBJECT========IP        |   320|01.06.2015|12:30:31|     5120|E        |
|   879|Prg|IF_T100_DYN_MSG===============IP        |   320|10.11.2022|19:49:51|     6144|E        |
|   880|Prg|CX_SY_DYN_CALL_ILLEGAL_TYPE===CP        |   880|10.11.2022|19:49:48|    13312|E        |
|   881|Prg|CX_SY_DYN_CALL_PARAMETER_ERRORCP        |   881|10.11.2022|19:49:48|    12288|E        |
|   882|Prg|SAPLMESS                                |   882|25.03.2019|14:20:51|    18432|E        |
|   883|Typ|T100C                                   |     0|03.08.2015|16:04:01|     3072|         |
|   884|Prg|SAPLSBAL_DB_INTERNAL                    |   884|10.11.2022|19:49:54|   343040|E        |
|   885|Typ|BAL_S_MSG                               |     0|31.05.2013|07:57:43|     8192|         |
|   886|Prg|SAPLRHFE                                |   886|29.03.2018|15:55:41|    21504|E        |
|   887|Prg|SAPLSETRM                               |   887|16.05.2023|13:23:54|   327680|E        |
|   888|Prg|SAPLSBAL_DB                             |   888|16.12.2022|11:24:16|   337920|E        |
|   889|Prg|IF_DATA_TEMPERATURE===========IP        |   888|01.06.2015|12:30:31|     5120|E        |
|   890|Prg|SAPLSBAL_TOOLBOX                        |   890|10.11.2022|19:49:54|   251904|E        |
|   891|Prg|SAPLSNR3                                |   891|28.04.2023|07:29:38|   248832|E        |
|   892|Typ|BNRIV                                   |     0|08.09.2011|14:25:41|     4096|         |
|   893|Typ|BAL_S_LGNM                              |     0|31.05.2013|07:57:43|     3072|         |
|   894|Typ|BALHDR                                  |     0|03.11.2021|18:10:15|    10240|         |
|   895|Typ|BALDAT                                  |     0|10.10.2014|18:26:25|     4096|         |
|   896|Typ|BALMSGHNDL                              |     0|02.11.1998|09:43:06|     3072|         |
|   897|Prg|/SDF/SAPLSMD_E2E_TRACE                  |   897|16.05.2023|13:23:52|   274432|E        |
|   898|Typ|BAL_S_DB                                |     0|03.11.2021|18:01:47|    10240|         |
|   899|Prg|CL_DATA_TEMPERATURE===========CP        |   899|01.06.2015|12:30:30|     9216|E        |
|   900|Prg|CL_ABAP_STACK_TEMPERATURE=====CP        |   900|10.11.2022|19:49:27|    17408|E        |
|   901|Typ|SEQTA                                   |     0|10.10.2014|18:27:21|     3072|         |
|   902|Prg|CL_EEWA_HASH==================CP        |   902|26.02.2015|07:25:05|    13312|E        |
|   903|Prg|SAPLSECH                                |   903|18.03.2023|11:02:28|    65536|E        |
|   904|Typ|EWA_ROUTE_DISPO                         |     0|14.07.2016|07:52:36|     3072|         |
|   905|Prg|/WATP/CL_IM_ARBHAZARDWASTE====CP        |   905|17.06.2026|07:11:41|   378880|E        |
|   906|Prg|/WATP/CL_IMPL_BO_CALLBACK_BAPICP        |   906|12.02.2025|18:36:48|    16384|E        |
|   907|Prg|/WATP/CL_MSGLOG_REDIRECT_PROTOCP        |   907|12.02.2025|20:55:25|    17408|E        |
|   908|Typ|EWA_EL_ABAPMETHODDESCRIPTION            |     0|25.02.2015|23:58:23|     4096|         |
|   909|Typ|EWA_EL_ABAPMETHODPARAMVALUE             |     0|28.05.2018|11:32:15|     6144|         |
|   910|Prg|CL_EEWA_BO_ROUTE==============CP        |   910|05.05.2026|18:05:14|   150528|E        |
|   911|Typ|EWAROUTE                                |     0|03.05.2025|05:24:20|    16384|         |
|   912|Prg|EEWA_BA_ROUTE=================CP        |   912|21.08.2023|09:27:05|    14336|E        |
|   913|Prg|CL_EEWA_RFC_WDO===============CP        |   913|17.06.2026|07:11:56|   486400|E        |
|   914|Prg|CL_EEWA_BO_SERVICETYPE========CP        |   914|05.09.2025|15:09:31|    92160|E        |
|   915|Prg|SAPLE10Z                                |   915|21.08.2023|10:01:17|   118784|E        |
|   916|Typ|ETYP                                    |     0|05.03.2021|20:17:08|    16384|         |
|   917|Typ|TEWASERVICETMPL                         |     0|05.09.2025|15:09:37|     6144|         |
|   918|Prg|SAPLEEWA_DB_CUSTOMIZING                 |   918|05.09.2025|15:09:38|   124928|E        |
|   919|Prg|SAPLEGPL                                |   919|17.06.2024|13:32:13|   109568|E        |
|   920|Prg|SAPLITO3                                |   920|09.08.2024|13:43:01|   200704|E        |
|   921|Typ|IFLOS                                   |     0|24.04.2018|15:15:54|     4096|         |
|   922|Typ|ITOB                                    |     0|17.06.2024|13:32:10|    41984|         |
|   923|Prg|SAPLITO1                                |   923|17.06.2024|14:33:12|   103424|E        |
|   924|Prg|%_CITOB                                 |   923|17.06.2024|14:33:09|    96256|E        |
|   925|Prg|CL_ITOB_FACTORY===============CP        |   925|17.06.2024|14:33:11|    27648|E        |
|   926|Prg|SAPLITOBS                               |   926|17.06.2024|14:33:12|   206848|E        |
|   927|Prg|SAPLITOBBUFEQ                           |   927|17.06.2024|14:33:12|   230400|E        |
|   928|Prg|SAPLITOBBUFFL                           |   928|17.06.2024|14:33:12|   153600|E        |
|   929|Prg|SAPLITOBBUFIL                           |   929|17.06.2024|14:33:12|    47104|E        |
|   930|Typ|EQKT                                    |     0|24.04.2018|15:15:30|     4096|         |
|   931|Typ|EQUI                                    |     0|17.06.2024|12:59:38|    22528|         |
|   932|Typ|EQUZ                                    |     0|17.06.2024|12:59:47|    13312|         |
|   933|Typ|IFLO                                    |     0|15.11.2022|06:19:09|    21504|         |
|   934|Typ|IFLOTX                                  |     0|24.04.2018|15:15:56|     4096|         |
|   935|Typ|ILOA                                    |     0|31.05.2013|08:04:35|    10240|         |
|   936|Typ|IRLO                                    |     0|16.06.2015|08:40:46|     8192|         |
|   937|Typ|IRLOT                                   |     0|16.06.2015|08:36:25|     7168|         |
|   938|Typ|IRLOTX                                  |     0|31.05.2013|08:04:39|     3072|         |
|   939|Typ|V_EQUI                                  |     0|17.06.2024|13:13:48|    39936|         |
|   940|Typ|IFLOT                                   |     0|15.11.2022|06:07:32|    14336|         |
|   941|Typ|ITOBREAD_INFO                           |     0|16.06.2015|08:29:04|     4096|         |
|   942|Prg|SAPLITO2                                |   942|20.05.2026|12:31:45|   619520|E        |
|   943|Prg|CL_ISU_DM_CONTEXT=============CP        |   943|29.05.2015|12:33:47|     9216|E        |
|   944|Typ|EGPLTX                                  |     0|25.02.2015|23:58:01|     3072|         |
|   945|Typ|TEWACONFTYPE                            |     0|06.04.2021|21:38:35|     5120|         |
|   946|Typ|ISUWA_ORDER                             |     0|17.06.2026|07:11:10|   548864|         |
|   947|Typ|ISUWA_ORDER_LINES                       |     0|09.04.2026|00:02:26|   113664|         |
|   948|Typ|ISUWA_ORDER_OBJECT                      |     0|09.04.2026|00:02:26|    63488|         |
|   949|Typ|TESERVICETYPE                           |     0|01.04.2022|19:32:23|     4096|         |
|   950|Prg|SAPLSWCD                                |   950|10.11.2022|19:49:55|   109568|E        |
|   951|Prg|SAPLEWW1                                |   951|06.06.2015|16:50:28|    94208|E        |
|   952|Typ|SWHACTOR                                |     0|04.04.1995|16:18:07|     2048|         |
|   953|Prg|SAPLSWEA                                |   953|08.09.2011|15:17:10|   155648|E        |
|   954|Prg|CL_SWF_EVT_CHECK_AUTHORITY====CP        |   954|15.02.2018|11:09:34|    14336|E        |
|   955|Prg|CL_SWF_UTL_RFC================CP        |   955|10.11.2022|19:49:45|    10240|E        |
|   956|Prg|CL_RFC========================CP        |   956|26.01.2023|17:38:13|    18432|E        |
|   957|Prg|CL_SWF_CNT_FACTORY============CP        |   957|28.04.2023|07:29:50|   146432|E        |
|   958|Prg|CL_SWF_IFS_CONTAINER_BASE=====CP        |   958|10.11.2022|19:52:51|    10240|E        |
|   959|Prg|CL_SWF_CNT_INSTANCE_FACTORY===CP        |   959|10.11.2022|19:52:51|    17408|E        |
|   960|Prg|CL_SWF_EVT_UTILITIES==========CP        |   960|18.03.2023|11:02:54|    75776|E        |
|   961|Prg|CL_SWF_UTL_DEF_SERVICES=======CP        |   961|15.06.2023|07:05:35|   222208|E        |
|   962|Typ|SWF_RETURN                              |     0|31.05.2013|08:13:30|     6144|         |
|   963|Typ|SWOTLQ                                  |     0|31.05.2013|08:13:33|     9216|         |
|   964|Prg|SAPLSWOR                                |   964|31.03.2023|15:25:25|   480256|E        |
|   965|Typ|SWOTRETURN                              |     0|31.05.2013|08:13:33|     4096|         |
|   966|Prg|SAPLSWOH                                |   966|01.06.2015|12:30:31|     9216|E        |
|   967|Typ|SWOTENV                                 |     0|08.12.2004|18:00:33|     5120|         |
|   968|Prg|SAPLSWOD                                |   968|16.05.2023|13:23:18|   886784|E        |
|   969|Prg|SAPLRHS0                                |   969|03.03.2023|16:55:28|    26624|E        |
|   970|Prg|CL_COS_UTILITIES==============CP        |   970|31.03.2023|13:50:55|    32768|E        |
|   971|Typ|TOJTD                                   |     0|08.09.2011|14:24:02|     3072|         |
|   972|Typ|TOJTB                                   |     0|18.08.2015|12:17:42|    10240|         |
|   973|Prg|CL_SWO_KEY_HANDLING===========CP        |   973|26.01.2023|17:38:15|    64512|E        |
|   974|Typ|SWOTLV                                  |     0|31.05.2013|08:13:33|    12288|         |
|   975|Typ|SIBFLPOR                                |     0|15.11.2000|17:53:54|     3072|         |
|   976|Typ|SIBFLPORB                               |     0|15.11.2000|17:53:55|     3072|         |
|   977|Typ|SWFUTLPOR                               |     0|04.12.2000|14:54:42|     3072|         |
|   978|Prg|CL_SWF_CNT_CONTAINER==========CP        |   978|31.03.2023|15:25:35|   335872|E        |
|   979|Prg|IF_SWF_CNT_CONTAINER==========IP        |   957|10.11.2022|19:52:51|    41984|E        |
|   980|Prg|CL_IMODE_AREA=================CP        |   980|10.11.2022|19:49:37|    12288|E        |
|   981|Typ|SWCELEMENT                              |     0|08.09.2011|14:24:15|     3072|         |
|   982|Prg|CL_SWF_CNT_SERVICE============CP        |   982|16.05.2023|13:23:28|   185344|E        |
|   983|Typ|SWFDELMDEF                              |     0|28.02.2019|15:28:59|     8192|         |
|   984|Prg|CL_SWF_CNT_ELEMENT============CP        |   984|18.03.2023|11:02:32|   108544|E        |
|   985|Typ|SWF_T100MS                              |     0|31.05.2013|08:13:30|     4096|         |
|   986|Prg|SAPLSWOL                                |   986|07.02.2022|08:59:47|    90112|E        |
|   987|Typ|TOJTT                                   |     0|31.05.2013|08:15:31|     3072|         |
|   988|Typ|HRS1212                                 |     0|31.05.2013|08:04:08|    11264|         |
|   989|Typ|WFSYST                                  |     0|08.09.2011|14:25:30|     7168|         |
|   990|Typ|SYST                                    |     0|10.10.2014|18:27:32|    32768|         |
|   991|Typ|SWFRCNTRST                              |     0|08.09.2011|14:28:16|     4096|         |
|   992|Typ|SWOTOBJID                               |     0|04.05.1995|15:20:52|     3072|         |
|   993|Prg|IF_SWF_IFS_PARAMETER_CONTAINERIP        |   953|10.11.2022|19:52:51|     8192|E        |
|   994|Prg|CL_SWF_EVT_EVENT==============CP        |   994|18.03.2023|11:02:32|    40960|E        |
|   995|Prg|CL_SWF_CNT_PERSISTENCE_XML====CP        |   995|15.03.2020|19:37:10|    32768|E        |
|   996|Prg|CL_SWF_CNT_BASE_PERSISTENCE===CP        |   996|18.03.2023|11:02:32|    45056|E        |
|   997|Prg|IF_SWF_CNT_PERSISTENCE========IP        |   978|10.11.2022|19:52:51|    11264|E        |
|   998|Prg|CL_SWF_EXP_SERVICE============CP        |   998|16.05.2023|13:23:14|   283648|E        |
|   999|Prg|CL_SWF_EVT_REQUESTER==========CP        |   999|17.04.2020|19:41:33|    12288|E        |
|  1000|Prg|CL_SWF_EVT_EVENT_MANAGER======CP        |  1000|21.08.2023|10:26:52|   134144|E        |
|  1001|Prg|CL_SWF_EVT_EVENT_HANDLER======CP        |  1001|31.03.2023|15:25:21|    40960|E        |
|  1002|Typ|SWFTRCLPNT                              |     0|08.09.2011|14:23:42|     3072|         |
|  1003|Prg|CL_SWF_TRC_MANAGER============CP        |  1003|18.03.2023|11:02:32|   104448|E        |
|  1004|Prg|CL_SWF_UTL_TIME===============CP        |  1004|26.01.2023|17:38:15|    30720|E        |
|  1005|Typ|SWWINDX                                 |     0|29.07.1998|19:15:49|     7168|         |
|  1006|Prg|CL_ABAP_TSTMP=================CP        |  1006|08.08.2024|10:13:12|    64512|E        |
|  1007|Prg|SWF_EVT_PO_BADI===============CP        |  1007|21.08.2023|09:54:23|    16384|E        |
|  1008|Prg|CL_POC_SWF_EVT================CP        |  1008|21.08.2023|10:20:00|    22528|E        |
|  1009|Prg|IF_SWF_EVT_PO=================IP        |  1000|16.12.2022|11:23:53|     8192|E        |
|  1010|Prg|CL_POC_SETTINGS===============CP        |  1010|21.08.2023|10:20:00|    17408|E        |
|  1011|Prg|CL_POC_APPLICATION_LOG========CP        |  1011|03.03.2023|14:53:21|    27648|E        |
|  1012|Prg|IF_POC_MAIN_C=================IP        |  1010|10.11.2022|19:49:50|    33792|E        |
|  1013|Prg|SWF_EVT_PO_BADI_V02===========CP        |  1013|21.08.2023|10:26:52|    14336|E        |
|  1014|Typ|SWOTSUPER                               |     0|04.04.1995|16:18:16|     2048|         |
|  1015|Typ|SWOTRE                                  |     0|31.05.2013|08:13:33|     4096|         |
|  1016|Prg|ZCL_WR_CPT_EVENT_HELPER=======CP        |  1016|17.06.2026|21:56:05|    33792|E        |
|  1017|Prg|ZCL_WR_CPT_HELPER=============CP        |  1017|05.06.2026|18:01:58|    43008|E        |
|  1018|Prg|SAPLEEWA_ORDER                          |  1018|17.06.2026|07:15:00|  2672640|E        |
|  1019|Typ|ISUWA_ORDER_AUTO                        |     0|09.04.2026|00:02:24|    36864|         |
|  1020|Typ|ISUWA_BULK_ORDER                        |     0|13.04.2016|15:02:18|     9216|         |
|  1021|Typ|EMSG                                    |     0|31.05.2013|08:01:32|     4096|         |
|  1022|Typ|ISU_RANGES                              |     0|24.02.2005|13:45:22|     3072|         |
|  1023|Typ|V_EWAOBJ                                |     0|05.09.2025|15:09:41|    21504|         |
|  1024|Prg|CL_ISU_WA_DISPSL_ORDER_COMMON=CP        |  1024|26.02.2015|06:36:53|    72704|E        |
|  1025|Prg|CL_EEWA_RCI_COMMON============CP        |  1025|17.06.2026|07:11:56|  1340416|E        |
|  1026|Prg|CX_EEWA_BO_RCI================CP        |  1026|05.11.2021|11:38:36|    48128|E        |
|  1027|Prg|SAPLEEWA_DB_ORDER                       |  1027|17.06.2026|07:11:44|   330752|E        |
|  1028|Typ|EWATSUB_WDOISPOS                        |     0|22.01.2021|17:32:21|     5120|         |
|  1029|Typ|EWA_ORDER_WEIGH                         |     0|26.05.2025|11:31:26|    25600|         |
|  1030|Prg|SAPLEEWA_RESPL                          |  1030|17.06.2026|07:11:44|  1157120|E        |
|  1031|Typ|EWATSUB_WDOSPOS                         |     0|22.01.2021|17:32:21|     5120|         |
|  1032|Typ|ISUWA_EORDER                            |     0|09.04.2026|00:02:10|    53248|         |
|  1033|Typ|EWACONTAINER                            |     0|16.06.2015|08:28:23|    10240|         |
|  1034|Prg|CL_EEWA_CONTAINERGRP==========CP        |  1034|17.06.2026|07:11:56|   222208|E        |
|  1035|Prg|SAPLIBCO                                |  1035|17.06.2024|13:15:24|   343040|E        |
|  1036|Prg|SAPLIBXX                                |  1036|16.06.2015|10:29:41|    13312|E        |
|  1037|Prg|CL_IBASE_ENGINE===============CP        |  1037|17.06.2024|13:14:48|    86016|E        |
|  1038|Prg|CL_EX_BADI_IBASE_AUTHORITY====CP        |  1038|16.06.2015|09:57:12|    26624|E        |
|  1039|Prg|SAPLPA_SWITCH_EXTENSIONS                |  1039|10.11.2022|19:49:53|    18432|E        |
|  1040|Prg|CL_PAK_INACTIVE_EXT_PACKAGES==CP        |  1040|26.01.2023|17:38:13|   107520|E        |
|  1041|Typ|PAKDATBUF                               |     0|28.09.2001|11:44:22|     6144|         |
|  1042|Prg|IF_EX_BADI_IBASE_AUTHORITY====IP        |   634|16.06.2015|10:38:47|     8192|E        |
|  1043|Prg|CL_EX_BADI_IBASE_IBCO=========CP        |  1043|16.06.2015|09:57:12|    20480|E        |
|  1044|Prg|CL_IBASE_STRUC================CP        |  1044|16.06.2015|09:58:47|    96256|E        |
|  1045|Prg|CL_IBASE_STRUC_BUF============CP        |  1045|31.07.2015|13:33:44|   120832|E        |
|  1046|Prg|CL_IBASE_IBSTREF==============CP        |  1046|16.06.2015|09:58:46|    60416|E        |
|  1047|Prg|CL_IBASE_ADDRESS==============CP        |  1047|17.06.2024|13:14:48|    79872|E        |
|  1048|Prg|CL_IBASE_IBADDR_BUF===========CP        |  1048|17.06.2024|13:14:48|    74752|E        |
|  1049|Prg|IF_IBASE_REQ_IBIN=============IP        |  1037|31.07.2015|13:50:06|     5120|E        |
|  1050|Prg|SAPLIBIBF                               |  1050|17.06.2024|13:15:24|   233472|E        |
|  1051|Prg|%_CIBIB                                 |  1050|03.04.2018|21:11:11|    25600|E        |
|  1052|Prg|SAPLIBINF                               |  1052|17.06.2024|13:15:24|   570368|E        |
|  1053|Prg|CL_IBASE_IBINTX===============CP        |  1053|17.06.2024|13:14:48|    74752|E        |
|  1054|Prg|CL_IBASE_IBINTX_BUF===========CP        |  1054|16.06.2015|09:58:44|    64512|E        |
|  1055|Prg|CL_IBASE_IBIN=================CP        |  1055|31.07.2015|13:33:43|    15360|E        |
|  1056|Prg|%_CIBIN                                 |  1052|03.04.2018|21:11:11|    73728|E        |
|  1057|Prg|%_CIBCO                                 |  1035|16.06.2015|10:58:02|    23552|E        |
|  1058|Prg|%_CIBXX                                 |  1035|16.06.2015|10:58:03|    82944|E        |
|  1059|Prg|%_CIBCO1                                |  1035|16.06.2015|10:58:02|    23552|E        |
|  1060|Prg|CL_IBASE_STRUC_CONTAINER======CP        |  1060|16.06.2015|09:58:48|    16384|E        |
|  1061|Prg|CL_IBASE_SERVICE==============CP        |  1061|31.07.2015|13:33:44|    34816|E        |
|  1062|Prg|SAPLIBCUT                               |  1062|16.06.2015|10:29:32|    47104|E        |
|  1063|Typ|V_IBINR                                 |     0|15.02.2018|20:29:00|     7168|         |
|  1064|Typ|IBIBCOM                                 |     0|26.11.2018|19:19:15|     7168|         |
|  1065|Typ|IBIB                                    |     0|26.11.2018|20:39:07|     8192|         |
|  1066|Prg|SAPLIBCU                                |  1066|16.06.2015|10:29:31|    14336|E        |
|  1067|Typ|IBIBCUST                                |     0|05.06.2015|20:18:30|     5120|         |
|  1068|Prg|CL_IBASE_STATUS_CUST==========CP        |  1068|16.06.2015|09:58:47|    46080|E        |
|  1069|Typ|T371STV                                 |     0|09.04.2005|14:25:14|     3072|         |
|  1070|Typ|IBINVAL                                 |     0|11.11.2019|13:57:40|     3072|         |
|  1071|Typ|IBINCOM                                 |     0|31.05.2013|08:04:19|     4096|         |
|  1072|Typ|IBINCONF                                |     0|16.06.2015|08:28:57|     3072|         |
|  1073|Typ|IBRECADM                                |     0|08.09.2011|14:20:54|     3072|         |
|  1074|Typ|V_IBINR_CM                              |     0|16.06.2015|08:41:49|     7168|         |
|  1075|Typ|IBIN                                    |     0|11.11.2019|13:57:40|     9216|         |
|  1076|Typ|IBINTX                                  |     0|15.02.2018|20:23:21|     5120|         |
|  1077|Typ|IBSP                                    |     0|11.11.2019|13:57:40|     8192|         |
|  1078|Prg|SAPLIBARC                               |  1078|21.08.2023|09:39:26|    71680|E        |
|  1079|Typ|IBADDR                                  |     0|16.06.2015|12:23:38|     5120|         |
|  1080|Typ|IBIBOBS                                 |     0|19.02.2005|20:45:27|     3072|         |
|  1081|Typ|IBIBT                                   |     0|15.02.2018|20:23:21|     4096|         |
|  1082|Typ|IBINDOMAINS                             |     0|15.04.1998|00:20:38|     4096|         |
|  1083|Typ|IBINOBS                                 |     0|15.02.2018|20:23:21|     3072|         |
|  1084|Typ|IBINOWN                                 |     0|15.02.2018|20:23:21|     3072|         |
|  1085|Typ|IBINT                                   |     0|21.04.1999|13:24:34|     4096|         |
|  1086|Typ|IBINVALUES                              |     0|15.02.2018|20:23:21|     7168|         |
|  1087|Typ|IBST                                    |     0|11.11.2019|13:57:40|     6144|         |
|  1088|Typ|IBSTREF                                 |     0|15.02.2018|20:23:22|     3072|         |
|  1089|Typ|IBSYMBOL                                |     0|26.11.2018|20:39:08|     4096|         |
|  1090|Prg|CL_IBASE_T371D_BUF============CP        |  1090|16.06.2015|09:58:49|    21504|E        |
|  1091|Typ|IBASE_S_T371D                           |     0|05.06.2015|20:18:30|     3072|         |
|  1092|Prg|CL_IBASE_R3_MATERIAL==========CP        |  1092|10.11.2025|17:51:09|    80896|E        |
|  1093|Prg|IF_CM_BASELINE================IP        |  1061|05.04.2016|15:22:58|     9216|E        |
|  1094|Prg|IF_IBASE_IBOF_TREE_OBJECT=====IP        |  1061|16.06.2015|10:39:18|     7168|E        |
|  1095|Prg|IF_IBASE_INSTALL==============IP        |  1061|16.06.2015|10:39:18|     6144|E        |
|  1096|Prg|IF_IBASE_MATERIAL=============IP        |  1061|16.06.2015|10:39:18|     5120|E        |
|  1097|Prg|IF_IBASE_OBJECT===============IP        |  1061|16.06.2015|10:39:18|     6144|E        |
|  1098|Prg|IF_EX_CM_BADI_VERSION=========IP        |   576|06.06.2015|17:52:49|     5120|E        |
|  1099|Prg|CL_ABAP_INTFDESCR=============CP        |  1099|10.11.2022|19:49:27|    24576|E        |
|  1100|Prg|CL_EX_CM_BADI_VERSION=========CP        |  1100|06.06.2015|15:07:09|    15360|E        |
|  1101|Prg|CL_IM_CM_BADI_VERSION_DEF=====CP        |  1101|06.06.2015|15:15:28|     8192|E        |
|  1102|Prg|SAPLBOMA                                |  1102|31.07.2015|13:43:17|    32768|E        |
|  1103|Typ|IONRA                                   |     0|03.05.2025|05:24:37|    30720|         |
|  1104|Prg|%_CIBINX                                |  1052|16.06.2015|10:58:03|    19456|E        |
|  1105|Typ|EWA_IBASE_CONTGRP                       |     0|16.06.2015|08:28:25|     4096|         |
|  1106|Typ|MARA                                    |     0|10.11.2025|17:50:47|    67584|         |
|  1107|Typ|PRE03                                   |     0|16.06.2015|08:31:03|     2048|         |
|  1108|Prg|SAPLMG21                                |  1108|10.11.2025|17:51:51|   590848|E        |
|  1109|Prg|CL_WRT_REUSE_FAE_HINT=========CP        |  1109|06.06.2015|15:30:00|    12288|E        |
|  1110|Prg|CL_SPFL_PROFILE_PARAMETER=====CP        |  1110|31.03.2023|15:25:29|   190464|E        |
|  1111|Typ|PRE09                                   |     0|16.06.2015|08:31:03|     3072|         |
|  1112|Typ|MAKT                                    |     0|16.06.2015|08:37:30|     3072|         |
|  1113|Prg|SAPLMG23                                |  1113|10.11.2025|17:51:38|    83968|E        |
|  1114|Typ|ISUWA_DEVGRP                            |     0|16.06.2015|08:29:02|    16384|         |
|  1115|Typ|EWADCFL                                 |     0|26.02.2015|00:20:48|     4096|         |
|  1116|Prg|/WATP/IF_BO_CALLBACK_CHAIN====IP        |   611|08.08.2024|10:53:29|     5120|E        |
|  1117|Prg|/WATP/CL_BO_EWAORDERPOS=======CP        |  1117|17.06.2026|07:11:41|   475136|E        |
|  1118|Typ|EWATSUBCNTCNF                           |     0|07.02.2022|20:01:39|     5120|         |
|  1119|Typ|/WATP/SBO_EWAORDERPOS                   |     0|09.04.2026|00:01:50|    52224|         |
|  1120|Typ|EWATORDOBJFILLK                         |     0|20.11.2020|06:41:55|     9216|         |
|  1121|Typ|/WATP/TOBJCSTSRV                        |     0|08.08.2024|08:30:13|     3072|         |
|  1122|Prg|/WATP/CL_PLG_ARB_ST_ORDERPOS==CP        |  1122|09.04.2026|00:02:54|    30720|E        |
|  1123|Prg|/WATP/CL_PLUGIN_BO_BASE=======CP        |  1123|12.02.2025|20:57:11|    55296|E        |
|  1124|Prg|/WATP/CL_BO_ARB_ST_CTTYPE_M===CP        |  1124|21.05.2026|18:09:17|   140288|E        |
|  1125|Prg|/WATP/CL_OBJSTATUSMGR=========CP        |  1125|12.02.2025|20:57:09|   136192|E        |
|  1126|Prg|/WATP/CL_OR_IMPLEMENTATION====CP        |  1126|12.02.2025|20:57:10|    97280|E        |
|  1127|Typ|/WATP/SIMPL_KEY                         |     0|08.08.2024|08:35:52|     2048|         |
|  1128|Typ|/WATP/SARBST_CONTRTYPE_MS               |     0|08.08.2024|08:35:52|     2048|         |
|  1129|Typ|/WATP/SARBST_CONTRTYPE_DET              |     0|08.08.2024|08:35:52|     4096|         |
|  1130|Typ|/WATP/SARBST_CONTRTYPE                  |     0|08.08.2024|08:35:52|     3072|         |
|  1131|Typ|/WATP/SBO_DETAILINDEX                   |     0|08.08.2024|08:35:49|     2048|         |
|  1132|Typ|/WATP/TARBST_CT                         |     0|08.08.2024|08:30:54|     3072|         |
|  1133|Prg|/WATP/CL_BO_ARB_ST_CONFIG=====CP        |  1133|12.02.2025|20:57:08|   113664|E        |
|  1134|Typ|/WATP/SARBST_CONFIG                     |     0|08.08.2024|08:35:52|     3072|         |
|  1135|Typ|/WATP/TARBST_CG                         |     0|08.08.2024|08:30:54|     3072|         |
|  1136|Prg|/WATP/IF_PLUGIN_BO_BASE=======IP        |   653|08.08.2024|09:25:21|    13312|E        |
|  1137|Prg|SAPLBSVU                                |  1137|21.08.2023|09:49:27|   124928|E        |
|  1138|Prg|BSV_STATUS_UPDATE=============CP        |  1138|21.08.2023|09:29:59|    14336|E        |
|  1139|Prg|/MERP/CL_PM_TECHOBJ_EXCH_IMPL=CP        |  1139|20.05.2022|05:06:17|    18432|E        |
|  1140|Prg|IF_BSV_STATUS_UPDATE==========IP        |  1137|08.09.2011|16:04:57|     8192|E        |
|  1141|Prg|SAPLEEWA_ORDER_UPD                      |  1141|05.05.2026|18:05:16|   858112|E        |
|  1142|Typ|VEWA_ORDER_HEAD                         |     0|05.09.2025|15:09:10|    22528|         |
|  1143|Typ|VEWA_ORDER_OBJECT                       |     0|09.04.2026|00:02:40|    51200|         |
|  1144|Typ|VEWA_ORDER_RESOBJ                       |     0|08.08.2024|08:37:19|     9216|         |
|  1145|Typ|VEWA_ORDER_WEIGH                        |     0|26.05.2025|11:31:26|    25600|         |
|  1146|Prg|CL_RSSCD_ENHANCE==============CP        |  1146|12.02.2025|20:54:27|   264192|E        |
|  1147|Typ|CDHDR                                   |     0|01.06.2015|16:00:31|     5120|         |
|  1148|Prg|SAPLSUGZ                                |  1148|15.06.2023|07:05:25|    86016|E        |
|  1149|Prg|CL_SOI_DEPLOY_PARAMETERS======CP        |  1149|03.03.2023|16:56:24|    41984|E        |
|  1150|Prg|CL_SOI_LOGGER=================CP        |  1150|03.03.2023|16:56:25|    67584|E        |
|  1151|Typ|UVERS                                   |     0|09.11.2000|14:16:07|     4096|         |
|  1152|Prg|SAPLRLFW                                |  1152|03.03.2023|16:55:27|    66560|E        |
|  1153|Prg|CX_SY_DYNAMIC_OSQL_SEMANTICS==CP        |  1153|10.11.2022|19:49:48|    15360|E        |
|  1154|Prg|CX_SY_DYNAMIC_OSQL_ERROR======CP        |  1154|10.11.2022|19:49:48|    13312|E        |
|  1155|Prg|CX_SY_OPEN_SQL_ERROR==========CP        |  1155|10.11.2022|19:49:48|    11264|E        |
|  1156|Prg|CX_SY_SQL_ERROR===============CP        |  1156|10.11.2022|19:49:48|    10240|E        |
|  1157|Prg|SAPLSCD1                                |  1157|12.02.2025|20:54:40|   300032|E        |
|  1158|Typ|BDI_CDPOS                               |     0|20.08.2001|12:12:41|     6144|         |
|  1159|Prg|SAPLSCP1                                |  1159|16.12.2022|11:36:13|    53248|E        |
|  1160|Typ|TBDA1                                   |     0|04.04.1995|17:42:19|     2048|         |
|  1161|Typ|TBDA2                                   |     0|09.11.2000|14:23:55|     3072|         |
|  1162|Typ|TBD62                                   |     0|20.08.1998|11:28:11|     3072|         |
|  1163|Typ|IONR                                    |     0|28.08.1997|08:49:59|     2048|         |
|  1164|Typ|EWASORDOBJFILLK_DATA                    |     0|20.11.2020|06:09:32|     8192|         |
|  1165|Prg|CL_EEWA_BO_WEIGHINGPROCESS====CP        |  1165|17.06.2026|07:11:42|   772096|E        |
|  1166|Typ|EWA_WA_WEIGHPROC                        |     0|26.05.2025|11:31:28|    39936|         |
|  1167|Typ|EWAWA_SWEIGHTEXT                        |     0|18.05.2020|09:06:19|    12288|         |
|  1168|Typ|EWASWA_WEIGHPC_B                        |     0|25.02.2015|23:58:21|     6144|         |
|  1169|Prg|CL_EX_EEWAWA_BA_WEIGHPROC=====CP        |  1169|26.02.2015|05:41:06|    16384|E        |
|  1170|Prg|IF_EX_EEWAWA_BA_WEIGHPROC=====IP        |   553|26.02.2015|05:43:33|     5120|E        |
|  1171|Prg|CL_EEWA_BO_WDORDERWEIGH=======CP        |  1171|17.06.2026|07:11:42|   422912|E        |
|  1172|Typ|EWASWDORDERWEIGH                        |     0|26.05.2025|11:31:15|    25600|         |
|  1173|Typ|EWASORDWEIGHFILLK                       |     0|30.04.2022|09:31:48|    11264|         |
|  1174|Typ|EWASSUB_WDOWSUB_DMRPOS_DET              |     0|22.01.2021|17:14:16|     6144|         |
|  1175|Typ|EWAS_SUB_WDOW_PO_DET                    |     0|05.03.2021|19:56:40|     4096|         |
|  1176|Prg|CL_EX_EEWA_BA_WDORDERWEIGH====CP        |  1176|26.02.2015|05:41:07|    14336|E        |
|  1177|Prg|IF_EX_EEWA_BA_WDORDERWEIGH====IP        |   553|26.02.2015|05:43:33|     5120|E        |
|  1178|Typ|EWAELOCSD_NEW                           |     0|05.06.2026|18:01:57|    16384|         |
|  1179|Typ|EWA_ORDER_RESULT_EMPTYING               |     0|26.02.2026|21:09:11|    17408|         |
|  1180|Prg|/MERP/CL_PM_STAT_EFI_EVT======CP        |  1180|20.05.2022|05:06:15|    14336|E        |
|  1181|Prg|/SMFND/CL_CORE_EFI_EVENT_BASE=CP        |  1181|14.11.2019|01:46:43|    38912|E        |
|  1182|Prg|/SYCLO/CL_CORE_MDO_TOOLS======CP        |  1182|17.12.2018|18:25:42|   356352|E        |
|  1183|Prg|SAPLSYUT                                |  1183|31.03.2023|15:25:20|    22528|E        |
|  1184|Prg|/SYCLO/CL_CORE_APPL_LOGGER====CP        |  1184|06.03.2023|02:23:04|    86016|E        |
|  1185|Typ|BALOBJ                                  |     0|17.05.2020|15:52:40|     3072|         |
|  1186|Prg|/SYCLO/CL_CORE_CONSTANTS======CP        |  1186|03.05.2025|07:15:06|    25600|E        |
|  1187|Typ|BAL_S_PAR                               |     0|02.11.1998|09:43:07|     3072|         |
|  1188|Typ|/SYCLO/CA001                            |     0|13.11.2019|21:52:13|     5120|         |
|  1189|Prg|/SMFND/CORE_EFI_EVT_HNDLR_BADICP        |  1189|21.08.2023|09:19:42|    15360|E        |
|  1190|Prg|/SMFND/CL_CORE_EXOBJ_TOOLS====CP        |  1190|03.05.2025|07:15:05|   203776|E        |
|  1191|Prg|/MFND/CL_CORE_MAPP_CONFIG_MGR=CP        |  1191|03.05.2025|07:15:05|   140288|E        |
|  1192|Prg|/SYCLO/CL_CORE_RUNTIME_PARAMS=CP        |  1192|03.05.2025|08:29:23|    46080|E        |
|  1193|Typ|/SYCLO/CORE_RANGE_STR                   |     0|17.12.2018|14:08:27|     3072|         |
|  1194|Typ|/SYCLO/CORE_USER_GUID_LIST_STR          |     0|17.12.2018|14:08:27|     2048|         |
|  1195|Typ|DDFIXVALUE                              |     0|27.08.1999|18:04:56|     3072|         |
|  1196|Prg|CL_DD_TEXT_PROVIDER===========CP        |  1196|31.03.2023|13:47:28|    61440|E        |
|  1197|Prg|CL_DD_DFIES_PROVIDER_DTEL=====CP        |  1197|31.03.2023|13:47:28|    23552|E        |
|  1198|Prg|CL_EEWA_BO_WDORESULTTYP=======CP        |  1198|05.09.2025|15:09:31|   100352|E        |
|  1199|Prg|SAPLSLVC                                |  1199|15.06.2023|07:05:35|   544768|E        |
|  1200|Prg|SAPLSKBB                                |  1200|15.06.2023|07:05:35|   104448|E        |
|  1201|Prg|CL_ALV_VARIANT================CP        |  1201|08.08.2024|10:13:13|   306176|E        |
|  1202|Prg|SAPLSKBH                                |  1202|15.06.2023|07:05:35|  1029120|E        |
|  1203|Prg|SAPLSSN1                                |  1203|16.12.2022|13:58:47|    32768|E        |
|  1204|Prg|%_CSLIS                                 |  1203|16.12.2022|13:58:46|   102400|E        |
|  1205|Prg|IF_ALV_Z======================IP        |  1202|03.05.2025|07:11:14|    20480|E        |
|  1206|Prg|%_CKKBLO                                |  1202|16.12.2022|13:58:46|   161792|E        |
|  1207|Typ|DTC_S_LAYO                              |     0|28.05.2018|11:31:36|     7168|         |
|  1208|Typ|ALV_S_PCTL                              |     0|10.10.2014|18:26:23|     9216|         |
|  1209|Typ|PRI_PARAMS                              |     0|31.05.2013|08:08:32|     6144|         |
|  1210|Typ|ARC_PARAMS                              |     0|10.10.2014|18:26:25|     6144|         |
|  1211|Typ|DISVARIANT                              |     0|29.07.1998|18:25:39|     3072|         |
|  1212|Typ|LVC_S_LAYO                              |     0|28.05.2018|11:34:03|    16384|         |
|  1213|Typ|LVC_S_PRNT                              |     0|10.10.2014|18:26:55|    11264|         |
|  1214|Typ|LVC_S_RPRP                              |     0|04.12.2000|12:48:05|     3072|         |
|  1215|Typ|LVC_S_TOTO                              |     0|28.05.2018|11:34:07|     5120|         |
|  1216|Typ|LVC_S_FCAT                              |     0|28.05.2018|11:34:00|    21504|         |
|  1217|Typ|TEWARESULTZ                             |     0|30.01.2017|19:22:57|     5120|         |
|  1218|Prg|CL_EEWA_ORDER_FILLUP_RESULT===CP        |  1218|17.06.2026|07:11:42|    48128|E        |
|  1219|Prg|EEWA_BF_WDOC_FILLUP_RESULTS===CP        |  1219|21.08.2023|09:18:33|    14336|E        |
|  1220|Prg|CL_EX_EEWA_BF_WDOC_FILLUP_RESDCP        |  1220|09.04.2026|00:02:55|    34816|E        |
|  1221|Prg|IF_EEWA_WDOC_FILLUPMETHOD=====IP        |   628|09.04.2026|00:02:56|    10240|E        |
|  1222|Prg|IF_EX_EEWA_BF_WDOC_FILLUP_RES=IP        |  1218|02.06.2015|09:22:50|     8192|E        |
|  1223|Prg|ZCL_WR_WDOC_FILLUP_CONTCOUNT==CP        |  1223|09.04.2026|00:02:57|    28672|E        |
|  1224|Prg|CL_EEWA_WDOC_FILLUP_CONTCOUNT=CP        |  1224|09.04.2026|00:02:55|    30720|E        |
|  1225|Prg|CL_EEWA_WDOC_FILLUPMETHOD=====CP        |  1225|09.04.2026|00:02:55|    50176|E        |
|  1226|Prg|CL_EEWA_WDOC_FILLUP_CONTTYPE==CP        |  1226|09.04.2026|00:02:55|    31744|E        |
|  1227|Prg|CL_EEWA_WDOC_FILLUP_ORDDURAT==CP        |  1227|17.06.2026|07:11:42|    52224|E        |
|  1228|Prg|CL_EEWA_WDOC_FILLUP_WASTE=====CP        |  1228|09.04.2026|00:02:55|    38912|E        |
|  1229|Prg|ZCL_WR_WDOC_FILLUP_WEIGHTS====CP        |  1229|09.04.2026|00:02:57|    31744|E        |
|  1230|Prg|CL_EEWA_WDOC_FILLUP_WEIGHTS===CP        |  1230|17.06.2026|07:11:42|    53248|E        |
|  1231|Prg|CL_EEWA_BO_MEASUREMENT========CP        |  1231|05.09.2025|15:09:31|    98304|E        |
|  1232|Prg|SAPLSCV0                                |  1232|10.11.2022|19:49:54|   187392|E        |
|  1233|Typ|T006D                                   |     0|09.11.2000|14:23:49|     4096|         |
|  1234|Prg|CL_EEWA_WDOC_FILLUP_REQVERIFY=CP        |  1234|17.06.2026|07:11:42|    72704|E        |
|  1235|Prg|CL_EEWA_ORDER_FILLUP_CONTEXT==CP        |  1235|17.06.2026|07:11:42|   122880|E        |
|  1236|Typ|EWA_ORDER_RESULT_HEAD                   |     0|05.09.2025|15:07:54|    10240|         |
|  1237|Typ|EWA_ORDER_RESULT_WEIGH_EXT              |     0|26.05.2025|11:31:19|    20480|         |
|  1238|Prg|CL_EEWA_BO_WDPLANT============CP        |  1238|17.06.2026|07:11:42|   372736|E        |
|  1239|Prg|CL_EEWA_WDOC_MISC=============CP        |  1239|26.02.2015|07:25:25|    13312|E        |
|  1240|Typ|EWA_ORDER_HEAD_IKEY                     |     0|25.02.2015|23:58:23|     2048|         |
|  1241|Typ|EWASORDFILLRESULT                       |     0|05.09.2025|15:07:42|    11264|         |
|  1242|Prg|IF_EEWA_ORDER_FILLUP_CONTEXT==IP        |  1235|09.04.2026|00:02:56|    37888|E        |
|  1243|Typ|EWASORDOBJFILLRESULT                    |     0|26.02.2026|21:09:02|    17408|         |
|  1244|Typ|EWA_ORDER_WEIGH_IKEY                    |     0|25.02.2015|23:58:23|     3072|         |
|  1245|Prg|SAPLMDR1                                |  1245|05.05.2026|14:22:47|   580608|E        |
|  1246|Typ|MDR1OUTALLOWEDQTUNIT                    |     0|16.06.2015|08:30:07|     5120|         |
|  1247|Prg|SAPLMG25                                |  1247|10.11.2025|17:51:38|   161792|E        |
|  1248|Typ|SMEINH                                  |     0|08.07.2019|09:17:40|    13312|         |
|  1249|Typ|MARM                                    |     0|08.07.2019|09:18:50|    11264|         |
|  1250|Prg|SAPLOMCV                                |  1250|21.08.2023|09:17:43|    54272|E        |
|  1251|Prg|BADI_MATN1====================CP        |  1251|21.08.2023|09:29:37|    15360|E        |
|  1252|Prg|CL_EX_BADI_MATN1==============CP        |  1252|19.11.2020|17:33:17|    19456|E        |
|  1253|Typ|TMCNV                                   |     0|15.06.2015|16:51:13|     5120|         |
|  1254|Prg|SAPLSCVU                                |  1254|10.11.2022|19:49:54|   195584|E        |
|  1255|Typ|T006A                                   |     0|31.05.2013|08:13:44|     3072|         |
|  1256|Typ|ZSWDOICHECKLIMIT                        |     0|05.09.2025|15:09:10|     6144|         |
|  1257|Typ|ZWR_SWDOI_CHECKLIMIT_DATA               |     0|05.09.2025|15:09:24|     5120|         |
|  1258|Prg|CL_EEWA_RANGES================CP        |  1258|02.06.2015|09:22:26|    61440|E        |
|  1259|Rty|type000000000000                        |     0|18.06.2026|06:16:42|        0|         |
|  1260|Prg|CL_EEWA_OBJECTREADER==========CP        |  1260|26.02.2015|07:25:07|    87040|E        |
|  1261|Typ|EWA_EL_CLASSNAME                        |     0|25.02.2015|23:58:23|     3072|         |
|  1262|Prg|ZCL_WR_EEWA_OR_WDORDERITEM====CP        |  1262|21.05.2026|18:09:40|    48128|E        |
|  1263|Prg|CL_EEWA_OR_WDORDERITEM========CP        |  1263|09.04.2026|00:02:55|   294912|E        |
|  1264|Prg|IF_EEWA_OBJECTREADER==========IP        |   556|14.11.2019|00:11:53|     9216|E        |
|  1265|Prg|CL_EEWA_OR_WDORDERITEM_SD=====CP        |  1265|17.06.2026|07:11:42|   165888|E        |
|  1266|Typ|EWA_ORDER_OBJECT_KEY                    |     0|25.02.2015|23:58:23|     3072|         |
|  1267|Typ|EWA_ORDER_OBJECT_MAIN                   |     0|20.11.2020|06:09:35|    22528|         |
|  1268|Typ|EWA_WDOITEM_INCL_EEW_PS                 |     0|03.05.2025|05:24:22|     2048|         |
|  1269|Typ|/WATP/AORDERTXT                         |     0|24.10.2024|08:59:52|     3072|         |
|  1270|Typ|/WATP/APRINTINFO                        |     0|08.08.2024|08:35:47|     3072|         |
|  1271|Typ|/WATP/ATPWDOITEM                        |     0|08.08.2024|08:35:47|     5120|         |
|  1272|Typ|/WATP/AWAMM_EWA_ORDER_OBJECT            |     0|08.08.2024|08:35:47|     5120|         |
|  1273|Typ|ZWR_AEEWA_EWA_ORDER_OBJECT              |     0|09.04.2026|00:02:40|     7168|         |
|  1274|Typ|EWA_EHS_DATA                            |     0|25.02.2015|23:58:23|     6144|         |
|  1275|Typ|EWASSUBCONTRACTINGDATA                  |     0|20.11.2020|06:09:32|     3072|         |
|  1276|Typ|EWASSUBCONTRACTINGDATAVAL               |     0|13.04.2016|15:02:09|     3072|         |
|  1277|Typ|EKPO_KEY                                |     0|13.08.1997|12:31:17|     2048|         |
|  1278|Typ|EWAS_MATDOC_DATA                        |     0|20.11.2020|06:09:33|     3072|         |
|  1279|Typ|SALES_ITEM_KEY                          |     0|13.04.2016|15:03:44|     2048|         |
|  1280|Typ|CI_EWA_ORDER_RESULT_EMPTYING            |     0|26.02.2026|21:08:57|     3072|         |
|  1281|Typ|EWASSUBCONTRACTINGRESULTDATA            |     0|25.02.2015|23:58:21|     3072|         |
|  1282|Typ|EWASSUBCONTRACTINGRSLTDATAVAL           |     0|02.06.2015|08:58:41|     3072|         |
|  1283|Typ|/PLCP/AEWA_ORDER_RESULT_ITEM            |     0|08.01.2025|16:36:16|     3072|         |
|  1284|Typ|/WATP/AEWA_ORDER_RESULT                 |     0|08.08.2024|08:35:47|     5120|         |
|  1285|Typ|ZWR_SEWA_ORDER_RESULT_ITEM              |     0|26.02.2026|21:09:49|     3072|         |
|  1286|Typ|EWASPURCHASECONTRACTPOSKEY              |     0|25.02.2015|23:58:20|     3072|         |
|  1287|Typ|/PLCP/SWDOI_RSLT_ENH                    |     0|08.01.2025|16:36:20|     3072|         |
|  1288|Typ|/WATP/SAEWA_ORDER_RESULT_AT             |     0|08.08.2024|08:35:50|     3072|         |
|  1289|Typ|/WATP/SORDERTXT                         |     0|24.10.2024|08:59:59|     3072|         |
|  1290|Typ|/WATP/SPRINTINFO                        |     0|08.08.2024|08:35:49|     3072|         |
|  1291|Typ|/WATP/STPWDOITEM                        |     0|08.08.2024|08:35:50|     4096|         |
|  1292|Typ|/WATP/SWAMM_EWA_ORDER_OBJECT            |     0|08.08.2024|08:35:51|     5120|         |
|  1293|Typ|ZWR_SEEWA_EWA_ORDER_OBJECT_EXT          |     0|09.04.2026|00:02:47|     7168|         |
|  1294|Typ|ZWR_SEEWA_CUSTFIELDS_PDSERVICE          |     0|23.10.2025|13:03:21|     2048|         |
|  1295|Typ|EWASADMIN                               |     0|25.02.2015|23:58:19|     4096|         |
|  1296|Typ|ISU_ADMINL                              |     0|24.02.2005|13:45:13|     3072|         |
|  1297|Prg|CL_EEWA_SQL_MISC==============CP        |  1297|06.04.2021|23:23:44|    26624|E        |
|  1298|Prg|CL_EEWA_WDORDER_MISC==========CP        |  1298|17.06.2026|07:11:56|   177152|E        |
|  1299|Prg|CL_EEWA_BO_MARA===============CP        |  1299|09.04.2026|00:02:55|   115712|E        |
|  1300|Prg|CL_EEWA_BO_PARTNER============CP        |  1300|17.06.2026|07:11:56|   270336|E        |
|  1301|Prg|SAPLBSVY                                |  1301|18.06.2009|09:49:29|    34816|E        |
|  1302|Typ|BSVWCOUP5                               |     0|08.09.2011|14:17:39|     4096|         |
|  1303|Prg|SAPLSWEB                                |  1303|18.03.2023|11:02:32|   143360|E        |
|  1304|Prg|CD_BADI_DATA1=================CP        |  1304|21.08.2023|10:32:05|    14336|E        |
|  1305|Prg|/SPE/CL_EI_EWM_MAT_DISTR_FULL=CP        |  1305|26.01.2023|17:38:09|    21504|E        |
|  1306|Prg|CL_BEL_CHGDOC_CONNECTOR=======CP        |  1306|03.05.2025|07:24:40|    23552|E        |
|  1307|Prg|CL_BEL_CHGDOC_EVENT_ADAPTER===CP        |  1307|03.05.2025|07:24:40|   112640|E        |
|  1308|Prg|CL_POC_CD_DATA================CP        |  1308|21.08.2023|10:20:00|    24576|E        |
|  1309|Prg|CL_DFS_ORGM_CD_IMPL===========CP        |  1309|13.07.2023|08:37:15|    26624|E        |
|  1310|Prg|CL_MEP_BADI_CLG===============CP        |  1310|10.11.2022|19:49:38|    13312|E        |
|  1311|Prg|CD_IF_DATA====================IP        |   784|31.05.2013|08:58:58|     8192|E        |
|  1312|Prg|CL_MEP_ADMIN==================CP        |  1312|10.11.2022|19:49:38|    72704|E        |
|  1313|Prg|CL_MEP_CLG_EVENT_PROVIDER=====CP        |  1313|16.05.2023|13:23:13|    72704|E        |
|  1314|Prg|CL_MEP_EVENT_PROVIDER_BASE====CP        |  1314|10.11.2022|19:49:38|    11264|E        |
|  1315|Prg|CL_MEP_PROVIDER_BASE==========CP        |  1315|10.11.2022|19:49:38|     8192|E        |
|  1316|Prg|SAPLARFC                                |  1316|31.03.2023|15:25:25|   713728|E        |
|  1317|Typ|QSENDDEST                               |     0|31.05.2013|08:08:53|     5120|         |
|  1318|Prg|CL_DBI_TRANSACTION_STATE======CP        |  1318|01.06.2015|12:30:30|     8192|E        |
|  1319|Typ|ZWR_GOS_CREATE_INFO                     |     0|14.05.2026|18:01:20|     8192|         |
|  1320|Prg|CL_FS_PATH====================CP        |  1320|26.01.2023|17:38:05|    62464|E        |
|  1321|Typ|SWOTRK                                  |     0|08.09.2011|14:28:18|     6144|         |
|  1322|Prg|SAPLZWR_GOS_FUNCTIONALITY               |  1322|17.06.2026|07:14:59|   304128|E        |
|  1323|Prg|ZCL_WR_BO_METAGOS_WDOI========CP        |  1323|17.06.2026|07:14:58|   107520|E        |
|  1324|Prg|ZCL_WR_BO_METAGOS_BASE========CP        |  1324|17.06.2026|07:14:58|   190464|E        |
|  1325|Typ|ZWR_GOS_META_BO                         |     0|14.05.2026|18:01:20|    16384|         |
|  1326|Typ|ZWR_TGOS_META                           |     0|14.05.2026|18:01:23|    13312|         |
|  1327|Typ|ZWR_GOS_META_CONTENT_DATA_BO            |     0|16.09.2025|12:32:19|     3072|         |
|  1328|Typ|ZWR_GOS_META_CONTENT_DATA               |     0|16.09.2025|12:32:19|     2048|         |
|  1329|Prg|/WATP/CL_BO_OBJNR=============CP        |  1329|12.02.2025|20:57:09|   169984|E        |
|  1330|Typ|/WATP/TOBJNR                            |     0|08.08.2024|08:30:13|     3072|         |
|  1331|Prg|/1BCDWBEN/SAPL/WATP/EN0000              |  1331|12.02.2025|18:38:16|   317440|E        |
|  1332|Prg|/WATP/CL_BO_OBJSTATUS=========CP        |  1332|12.02.2025|20:57:09|   227328|E        |
|  1333|Typ|/WATP/SBO_IMPL                          |     0|08.08.2024|08:35:49|     9216|         |
|  1334|Typ|/WATP/SIMPLWRP                          |     0|08.08.2024|08:35:52|     4096|         |
|  1335|Typ|/WATP/TTIMPLWRP                         |     0|08.08.2024|08:30:14|     3072|         |
|  1336|Typ|/WATP/SIMPLSCR                          |     0|08.08.2024|08:35:52|     4096|         |
|  1337|Typ|/WATP/TTIMPLSCR                         |     0|08.08.2024|08:30:14|     3072|         |
|  1338|Typ|/WATP/TTIMPLFLD                         |     0|08.08.2024|08:30:14|     8192|         |
|  1339|Typ|/WATP/TTIMPLPRT                         |     0|08.08.2024|08:30:14|     4096|         |
|  1340|Typ|/WATP/TTIMPLBFM                         |     0|08.08.2024|08:30:14|     5120|         |
|  1341|Typ|/WATP/TTIMPLRPT                         |     0|08.08.2024|08:30:14|     4096|         |
|  1342|Typ|/WATP/TTIMPLNR                          |     0|08.08.2024|08:30:14|     4096|         |
|  1343|Typ|/WATP/TTIMPLBOK                         |     0|08.08.2024|08:30:14|    19456|         |
|  1344|Typ|/WATP/SIMPLDTA                          |     0|08.08.2024|08:35:52|    20480|         |
|  1345|Typ|/WATP/TTIMPLDTA                         |     0|08.08.2024|08:30:14|    19456|         |
|  1346|Typ|/WATP/SIMPLPRA                          |     0|08.08.2024|08:35:52|    20480|         |
|  1347|Typ|/WATP/TTIMPLPRA                         |     0|08.08.2024|08:30:14|    19456|         |
|  1348|Typ|/WATP/SIMPLRNG                          |     0|08.08.2024|08:35:52|     4096|         |
|  1349|Typ|/WATP/TTIMPLRNG                         |     0|08.08.2024|08:30:14|     3072|         |
|  1350|Typ|/WATP/SIMPLNAV                          |     0|08.08.2024|08:35:52|     6144|         |
|  1351|Typ|/WATP/TTIMPLNAV                         |     0|08.08.2024|08:30:14|     5120|         |
|  1352|Typ|/WATP/SIMPLSET                          |     0|08.08.2024|08:35:52|     3072|         |
|  1353|Typ|/WATP/TTIMPLSET                         |     0|08.08.2024|08:30:14|     3072|         |
|  1354|Typ|/WATP/SIMPLPRF                          |     0|08.08.2024|08:35:52|     3072|         |
|  1355|Typ|/WATP/TTIMPLPRF                         |     0|08.08.2024|08:30:14|     3072|         |
|  1356|Prg|/WATP/CL_BO_IMPLEMENTGROUP====CP        |  1356|12.02.2025|20:57:09|   193536|E        |
|  1357|Prg|/WATP/CL_BO_OBJMODULE=========CP        |  1357|12.02.2025|20:57:09|   289792|E        |
|  1358|Typ|/WATP/SOBJSTAT                          |     0|08.08.2024|08:35:48|    10240|         |
|  1359|Typ|/WATP/TTOBJSTAT                         |     0|08.08.2024|08:30:14|     7168|         |
|  1360|Typ|/WATP/TTOBJSTATM                        |     0|08.08.2024|08:30:14|     4096|         |
|  1361|Typ|/WATP/TTOBJSTATF                        |     0|08.08.2024|08:30:14|     8192|         |
|  1362|Prg|/WATP/CL_BO_OBJSHSTRY=========CP        |  1362|12.02.2025|20:57:09|   139264|E        |
|  1363|Typ|/WATP/TOBJSHSTRY                        |     0|08.08.2024|08:30:13|     4096|         |
|  1364|Typ|/WATP/SOBJSTAT_KEY                      |     0|08.08.2024|08:35:48|     3072|         |
|  1365|Prg|/WATP/CL_BO_OBJPROCESS========CP        |  1365|12.02.2025|20:57:09|   199680|E        |
|  1366|Typ|/WATP/SOBJPROC_KEY                      |     0|08.08.2024|08:35:48|     3072|         |
|  1367|Prg|/1BCDWBEN/SAPLZEN0000                   |  1367|14.05.2026|18:01:49|    19456|E        |
|  1368|Typ|ZWR_GOS_MSG_KEY                         |     0|05.09.2025|15:09:16|     2048|         |
|  1369|Typ|ZWR_GOS_ALINK_KEY                       |     0|05.09.2025|15:09:16|     3072|         |
|  1370|Prg|/WATP/CL_BO_SO_DOCUMENT=======CP        |  1370|12.02.2025|20:57:09|   129024|E        |
|  1371|Typ|/WATP/SSO_DOCUMENT                      |     0|08.08.2024|08:35:50|    18432|         |
|  1372|Typ|SOFOLENTI1                              |     0|07.02.2006|10:08:30|    17408|         |
|  1373|Typ|SOOBJINFI1                              |     0|07.02.2006|10:08:30|     7168|         |
|  1374|Typ|SOSENDERI1                              |     0|07.02.2006|10:08:31|     3072|         |
|  1375|Typ|SOFORWI1                                |     0|07.02.2006|10:08:30|     3072|         |
|  1376|Typ|SORECI1                                 |     0|07.02.2006|10:08:30|     3072|         |
|  1377|Typ|SOSNDATTI1                              |     0|04.04.1995|16:17:23|     3072|         |
|  1378|Typ|SOSNDCNTI1                              |     0|04.04.1995|16:17:23|     4096|         |
|  1379|Typ|SOPROCESI1                              |     0|12.10.1997|12:09:15|     3072|         |
|  1380|Typ|SOSTATUSI1                              |     0|04.04.1995|16:17:23|     3072|         |
|  1381|Typ|SOCREATRI1                              |     0|07.02.2006|10:08:30|     3072|         |
|  1382|Typ|SOCHANGEI1                              |     0|07.02.2006|10:08:30|     3072|         |
|  1383|Typ|SOODK                                   |     0|08.03.1992|00:09:04|     3072|         |
|  1384|Prg|SAPLSOC3                                |  1384|16.05.2023|13:06:54|   207872|E        |
|  1385|Prg|SAPLSOA3                                |  1385|17.06.2024|13:06:43|   330752|E        |
|  1386|Typ|SOPRD                                   |     0|31.05.2013|08:12:44|    18432|         |
|  1387|Prg|SAPFSSO0                                |  1385|17.06.2024|12:55:12|  1054720|E        |
|  1388|Typ|SOPR                                    |     0|31.05.2013|08:12:44|    17408|         |
|  1389|Prg|SAPFSSO3                                |  1385|16.05.2023|13:06:54|   214016|E        |
|  1390|Typ|TSOUX                                   |     0|31.05.2013|08:15:54|     2048|         |
|  1391|Typ|SOSUD                                   |     0|14.08.1995|10:10:30|    10240|         |
|  1392|Prg|SAPFSSO5                                |  1385|17.06.2024|13:06:42|   619520|E        |
|  1393|Prg|SAPLSX04                                |  1393|16.05.2023|13:06:54|   326656|E        |
|  1394|Typ|SXADDRTYPE                              |     0|13.08.1997|12:55:49|     3072|         |
|  1395|Typ|SOADRBUF                                |     0|28.08.1997|09:29:20|     6144|         |
|  1396|Typ|SOADDR                                  |     0|07.02.2006|10:08:30|     3072|         |
|  1397|Typ|SOUC                                    |     0|08.02.1994|19:12:37|     3072|         |
|  1398|Typ|SOUIO                                   |     0|07.02.2006|10:08:31|     3072|         |
|  1399|Typ|V_SOUDADCP                              |     0|17.05.2022|14:47:49|    20480|         |
|  1400|Typ|SOXAU                                   |     0|14.10.1999|22:01:41|     5120|         |
|  1401|Prg|SAPLSO36                                |  1401|10.11.2022|19:49:54|    17408|E        |
|  1402|Typ|SOUDK                                   |     0|04.04.1995|16:17:32|     3072|         |
|  1403|Prg|CL_BCS_CONVERT================CP        |  1403|16.05.2023|13:23:12|    37888|E        |
|  1404|Typ|SOLIX                                   |     0|11.09.1997|10:17:06|     3072|         |
|  1405|Typ|SOLI                                    |     0|08.03.1992|00:08:59|     3072|         |
|  1406|Prg|SAPLSOI1                                |  1406|16.05.2023|13:06:54|   560128|E        |
|  1407|Typ|SOLISTI1                                |     0|04.04.1995|16:17:08|     3072|         |
|  1408|Typ|SOPARAI1                                |     0|04.04.1995|16:17:14|     3072|         |
|  1409|Typ|SOPARBI1                                |     0|04.04.1995|16:17:14|     3072|         |
|  1410|Prg|CL_BCS_VSI_PROFILE============CP        |  1410|26.01.2023|15:40:21|    15360|E        |
|  1411|Prg|SAPFSSOA                                |  1406|16.05.2023|13:06:54|   187392|E        |
|  1412|Prg|SAPLSBCOMSUT                            |  1412|17.06.2024|13:23:29|   580608|E        |
|  1413|Prg|CL_BCS_BODYPART_DB============CP        |  1413|03.03.2023|16:55:27|    46080|E        |
|  1414|Prg|CL_TRACE_BCS==================CP        |  1414|26.01.2023|15:40:41|    54272|E        |
|  1415|Prg|CL_BCS_OBJHEAD================CP        |  1415|10.11.2022|19:49:28|    25600|E        |
|  1416|Prg|SAPLSOC8                                |  1416|16.05.2023|13:06:54|   111616|E        |
|  1417|Prg|SAPFSSO1                                |  1416|16.05.2023|13:06:54|  1338368|E        |
|  1418|Typ|TSOTD                                   |     0|07.05.1997|11:28:11|     3072|         |
|  1419|Prg|SAPLSOKP                                |  1419|16.05.2023|13:06:54|   153600|E        |
|  1420|Prg|CL_BCS_UTILITIES==============CP        |  1420|03.03.2023|16:55:27|    44032|E        |
|  1421|Prg|SAPLSDC_MIME                            |  1421|17.06.2024|13:17:01|  1078272|E        |
|  1422|Prg|SAPLSOA1                                |  1422|08.09.2011|15:07:18|   274432|E        |
|  1423|Typ|TSOTT                                   |     0|31.05.2013|08:15:54|     3072|         |
|  1424|Prg|SAPFSSO6                                |  1422|16.05.2023|13:23:44|   488448|E        |
|  1425|Typ|SOFDD                                   |     0|12.05.1997|16:48:13|     9216|         |
|  1426|Typ|SOFD                                    |     0|12.05.1997|16:48:13|     9216|         |
|  1427|Prg|SAPFSSO2                                |  1422|16.05.2023|13:06:54|   174080|E        |
|  1428|Typ|SOFMK                                   |     0|17.03.1993|01:15:56|     4096|         |
|  1429|Prg|SAPLSYST                                |  1429|10.11.2022|19:49:55|    52224|E        |
|  1430|Typ|SDOKPROPTY                              |     0|07.05.1997|13:08:54|     3072|         |
|  1431|Prg|SAPLSDCL                                |  1431|16.05.2023|13:23:22|  2754560|E        |
|  1432|Typ|SDOKLOIO                                |     0|15.11.2000|17:53:21|     6144|         |
|  1433|Typ|SDOKLOCL                                |     0|24.02.1999|03:16:29|     4096|         |
|  1434|Prg|%_CSDOKI                                |  1431|17.04.2020|19:51:54|    48128|E        |
|  1435|Prg|SAPLSDCI                                |  1435|10.11.2022|19:49:54|   377856|E        |
|  1436|Typ|SDOKRELIST                              |     0|07.05.1997|13:08:54|     3072|         |
|  1437|Typ|SDOKERRMSG                              |     0|10.10.2014|18:27:20|     5120|         |
|  1438|Typ|SDOKOBJECT                              |     0|07.05.1997|13:08:53|     3072|         |
|  1439|Prg|SAPLSDC_DYNAMIC_ATTRIBUTES              |  1439|29.03.2018|15:55:41|    40960|E        |
|  1440|Typ|SDOKIOCL                                |     0|11.04.2001|12:40:46|     5120|         |
|  1441|Typ|SDOKLOTAB                               |     0|09.11.2000|14:21:50|     4096|         |
|  1442|Prg|SAPLSDOK_UPDTSK                         |  1442|10.11.2022|19:49:54|   178176|E        |
|  1443|Typ|SDOKLOPR                                |     0|07.05.1997|13:46:01|     3072|         |
|  1444|Typ|SOFFLOIO                                |     0|15.11.2000|17:54:23|     6144|         |
|  1445|Prg|SAPLSDCD                                |  1445|03.11.2021|19:19:46|    77824|E        |
|  1446|Prg|SAPLSYDB                                |  1446|03.05.2025|07:10:17|    71680|E        |
|  1447|Typ|SDOKPHIO                                |     0|31.05.2013|08:12:21|     7168|         |
|  1448|Typ|SDOKERRKEY                              |     0|10.10.2014|18:27:20|     3072|         |
|  1449|Typ|SDOKPHCL_C                              |     0|19.11.2020|09:33:13|     4096|         |
|  1450|Typ|SDOKREPOS                               |     0|24.02.1999|03:16:31|     3072|         |
|  1451|Typ|SDOKPHTAB                               |     0|09.11.2000|14:21:53|     5120|         |
|  1452|Typ|SOFFPHIO                                |     0|31.05.2013|08:12:44|     7168|         |
|  1453|Typ|SDOKRECL                                |     0|24.02.1999|03:16:31|     3072|         |
|  1454|Typ|SDOKFILACI                              |     0|18.11.1998|16:24:10|     4096|         |
|  1455|Prg|CL_SX_PARAM===================CP        |  1455|26.01.2023|15:40:21|    23552|E        |
|  1456|Typ|SDOKCOMPRU                              |     0|04.12.2000|14:54:19|    12288|         |
|  1457|Typ|SDOKSTCA                                |     0|24.02.1999|03:16:32|     3072|         |
|  1458|Prg|SAPLSCMS_SDOKSTRE                       |  1458|03.03.2023|16:55:30|   104448|E        |
|  1459|Typ|CREP                                    |     0|04.12.2000|14:56:32|     3072|         |
|  1460|Typ|CREP_STOR                               |     0|01.06.2015|10:56:58|     5120|         |
|  1461|Typ|SDOKSTRE                                |     0|13.02.2005|17:31:45|     6144|         |
|  1462|Prg|SAPLSCMS_API                            |  1462|28.04.2023|07:29:46|   326656|E        |
|  1463|Prg|SCMS                                    |  1462|03.05.2025|08:07:27|    55296|E        |
|  1464|Prg|SAPLSCMS_HTTP                           |  1464|10.11.2022|19:49:54|   324608|E        |
|  1465|Prg|SAPLSCMS_URL                            |  1465|26.01.2018|14:57:36|   145408|E        |
|  1466|Typ|SCMS_SEATR                              |     0|18.11.1998|16:24:08|     3072|         |
|  1467|Typ|SSFBIN                                  |     0|13.05.1997|12:54:27|     3072|         |
|  1468|Typ|CREP_HTTP                               |     0|23.06.2016|13:29:36|     5120|         |
|  1469|Prg|SCMS_HTTP                               |  1465|01.06.2015|11:55:55|    13312|E        |
|  1470|Prg|SAPLSCMS_LOCATION                       |  1470|16.05.2023|13:23:54|    28672|E        |
|  1471|Prg|SAPLSNI_ADDR                            |  1471|31.03.2023|15:25:25|    72704|E        |
|  1472|Prg|SAPLSCMS_CONVERSION                     |  1472|09.09.2004|14:18:36|    15360|E        |
|  1473|Prg|SAPLSCMS_CERT                           |  1473|08.09.2011|15:04:58|    75776|E        |
|  1474|Prg|SAPLSSFG                                |  1474|18.03.2023|11:02:29|   202752|E        |
|  1475|Typ|SSFARGS                                 |     0|08.09.2011|14:28:14|     6144|         |
|  1476|Prg|SAPLSCMS_CVT                            |  1476|05.07.2005|13:10:18|    14336|E        |
|  1477|Prg|SAPLSCMS_CONV                           |  1477|10.11.2022|19:49:54|    48128|E        |
|  1478|Prg|CL_ABAP_CONV_OUT_CE===========CP        |  1478|10.11.2022|19:49:27|    19456|E        |
|  1479|Typ|SSFIDLIN                                |     0|28.08.1997|09:03:27|     4096|         |
|  1480|Typ|SDOKLOC                                 |     0|24.02.1999|03:30:24|     2048|         |
|  1481|Prg|SAPLSDC_PROF                            |  1481|01.06.2015|12:30:31|    15360|E        |
|  1482|Prg|CL_SCMS_VIRUS_SCAN_INFO=======CP        |  1482|03.11.2021|19:21:01|    13312|E        |
|  1483|Prg|CL_VSI========================CP        |  1483|16.05.2023|13:23:14|   120832|E        |
|  1484|Typ|VSCAN_PROF                              |     0|17.05.2022|14:44:06|     4096|         |
|  1485|Prg|SAPLSCMS_HTTP2                          |  1485|31.05.2013|08:35:19|   153600|E        |
|  1486|Prg|SAPLSFTP                                |  1486|31.05.2013|08:35:35|   164864|E        |
|  1487|Prg|SAPLCRFC                                |  1487|15.06.2023|07:05:35|   854016|E        |
|  1488|Typ|RFCDISPLAY                              |     0|10.11.2022|16:51:47|    24576|         |
|  1489|Typ|RFCDISPLAZ                              |     0|26.02.2015|00:09:07|     8192|         |
|  1490|Typ|ST4QRFC                                 |     0|14.02.2005|17:06:38|     2048|         |
|  1491|Typ|THTTP                                   |     0|06.05.1998|02:22:27|     3072|         |
|  1492|Typ|HTTP_COMP                               |     0|18.05.2004|14:08:19|     3072|         |
|  1493|Typ|SDOKCNTBIN                              |     0|10.07.1997|00:08:33|     3072|         |
|  1494|Prg|SAPLSCMS_LOG                            |  1494|03.11.2021|18:10:15|    21504|E        |
|  1495|Typ|SPAR                                    |     0|27.08.1994|17:26:58|     3072|         |
|  1496|Prg|SAPLSLG0                                |  1496|03.11.2021|18:10:15|    75776|E        |
|  1497|Typ|BAL_S_LFIL                              |     0|03.11.2021|18:01:47|    18432|         |
|  1498|Typ|BAL_S_LOGH                              |     0|18.11.1998|16:20:52|     3072|         |
|  1499|Typ|BAL_S_LOGN                              |     0|18.11.1998|16:20:52|     3072|         |
|  1500|Typ|BAL_S_EXTN                              |     0|18.11.1998|16:20:52|     3072|         |
|  1501|Typ|BAL_S_OBJ                               |     0|18.11.1998|16:20:52|     3072|         |
|  1502|Typ|BAL_S_EPP                               |     0|03.11.2021|18:01:47|     3072|         |
|  1503|Prg|SAPLSCMS_MON                            |  1503|31.03.2023|15:25:26|   200704|E        |
|  1504|Prg|SAPLSALI                                |  1504|26.01.2023|17:38:07|   230400|E        |
|  1505|Prg|SAPLSALC                                |  1505|26.01.2023|17:38:07|   869376|E        |
|  1506|Prg|SAPLSRFC                                |  1506|18.03.2023|11:02:29|    52224|E        |
|  1507|Prg|SAPLSCSMROUTE                           |  1507|03.11.2021|19:19:46|   675840|E        |
|  1508|Prg|SAPLSALS                                |  1508|03.11.2021|19:19:45|   197632|E        |
|  1509|Typ|ALMONSE40B                              |     0|31.05.2013|07:57:09|    10240|         |
|  1510|Prg|SAPLSALJ                                |  1510|17.04.2020|19:44:33|   161792|E        |
|  1511|Typ|ALMTNAME_L                              |     0|07.05.1997|12:39:59|     3072|         |
|  1512|Typ|ALNAMESPLT                              |     0|31.05.2013|07:57:12|     3072|         |
|  1513|Typ|CSMSEGM                                 |     0|31.05.2013|08:00:06|     6144|         |
|  1514|Prg|SAPLSCSM_UTILITIES                      |  1514|01.06.2015|12:30:31|     9216|E        |
|  1515|Typ|CSM_UPD_C                               |     0|31.05.2013|08:00:06|     7168|         |
|  1516|Typ|MSXXLIST_V6                             |     0|10.10.2014|18:26:57|     5120|         |
|  1517|Prg|CL_STSK_UTIL==================CP        |  1517|10.11.2022|19:49:44|     9216|E        |
|  1518|Typ|ALCONSEG                                |     0|31.05.2013|07:57:06|     8192|         |
|  1519|Typ|ALGTIDLNRC                              |     0|31.05.2013|07:57:08|     4096|         |
|  1520|Typ|ALRFCLNAME                              |     0|31.05.2013|07:57:13|     9216|         |
|  1521|Typ|ALSMSGRVAL                              |     0|31.05.2013|07:57:14|     7168|         |
|  1522|Typ|T100                                    |     0|31.05.2013|08:13:47|     3072|         |
|  1523|Typ|SXMIMSGRAW                              |     0|04.06.1997|19:24:12|     5120|         |
|  1524|Typ|ALGTIDRC                                |     0|31.05.2013|07:57:08|     4096|         |
|  1525|Typ|ALRFCSRVAL                              |     0|31.05.2013|07:57:14|    13312|         |
|  1526|Typ|BALNRI                                  |     0|13.08.1997|12:18:17|     3072|         |
|  1527|Typ|BAL_S_SUB                               |     0|18.11.1998|16:20:52|     3072|         |
|  1528|Typ|BAL_S_DATE                              |     0|18.11.1998|16:20:51|     3072|         |
|  1529|Typ|BAL_S_TIME                              |     0|18.11.1998|16:20:53|     3072|         |
|  1530|Typ|BAL_S_PROG                              |     0|18.11.1998|16:20:52|     3072|         |
|  1531|Typ|BAL_S_TCDE                              |     0|18.11.1998|16:20:53|     3072|         |
|  1532|Typ|BAL_S_USER                              |     0|08.09.2011|14:25:39|     3072|         |
|  1533|Typ|BAL_S_MODE                              |     0|21.01.2014|10:04:42|     3072|         |
|  1534|Typ|BAL_S_PRCL                              |     0|08.09.2011|14:18:33|     3072|         |
|  1535|Prg|/WATP/CX_BO_SAPOFFICE=========CP        |  1535|08.08.2024|09:24:54|    30720|E        |
|  1536|Prg|/WATP/CX_BO_BASE==============CP        |  1536|08.08.2024|09:24:52|    32768|E        |
|  1537|Prg|/WATP/CX_BASE=================CP        |  1537|12.02.2025|20:55:26|    38912|E        |
|  1538|Typ|SBTFR_PARAM                             |     0|06.05.2008|14:19:04|     2048|         |
|  1539|Prg|SAPLSOTR_SERVICES                       |  1539|23.04.2019|09:17:19|    22528|E        |
|  1540|Prg|SAPLSBAL_TXT                            |  1540|10.11.2022|19:49:54|    75776|E        |
|  1541|Typ|/PLCE/R_PDTOURSERVICEASGMT              |     0|25.02.2026|18:02:34|     4096|         |
|  1542|Typ|ISU_EE00_U                              |     0|24.02.2005|13:45:19|     2048|         |
|  1543|Prg|CL_EEWA_OR_SERVICEADDRESS=====CP        |  1543|17.06.2026|07:11:42|   209920|E        |
|  1544|Prg|CL_EEWA_BO_SERVFREQHEAD=======CP        |  1544|17.06.2026|07:11:42|   418816|E        |
|  1545|Prg|CL_EEWA_BO_ISUOBJ=============CP        |  1545|05.09.2025|15:09:31|    78848|E        |
|  1546|Typ|EWASSERVFREQHEAD                        |     0|03.05.2025|05:24:20|    20480|         |
|  1547|Typ|EWAOBJ                                  |     0|03.05.2025|05:24:19|    19456|         |
|  1548|Prg|CL_EEWA_BO_SERVFREQITEM=======CP        |  1548|17.06.2026|07:11:42|   309248|E        |
|  1549|Typ|EWASSERVFREQITEM_DET                    |     0|25.02.2015|23:58:21|     5120|         |
|  1550|Prg|CL_EEWA_BO_CLASSIFICATION=====CP        |  1550|05.09.2025|15:09:31|   131072|E        |
|  1551|Prg|SAPLCLSE                                |  1551|17.06.2024|13:21:07|   626688|E        |
|  1552|Typ|USR01                                   |     0|03.05.2022|08:37:48|     5120|         |
|  1553|Prg|SAPLSUU1                                |  1553|17.06.2024|13:06:33|   528384|E        |
|  1554|Prg|CL_SUSR_BASIC_TOOLS===========CP        |  1554|17.06.2026|07:11:43|    79872|E        |
|  1555|Typ|SUSR_S_REF_2_RNG                        |     0|13.01.2010|09:06:27|     2048|         |
|  1556|Typ|SUSR_S_RANGE_4_XUBNAME                  |     0|08.09.2011|14:24:15|     3072|         |
|  1557|Typ|USR02                                   |     0|08.09.2011|14:17:43|    10240|         |
|  1558|Typ|USR21                                   |     0|15.02.2018|10:23:44|     5120|         |
|  1559|Typ|EWASCLASSIFICATION_KEY                  |     0|31.07.2015|12:00:10|     3072|         |
|  1560|Typ|EWASCLASSIFICATION                      |     0|31.07.2015|12:00:10|     3072|         |
|  1561|Typ|EWASCLASSIFICATIONVALUE_DET             |     0|31.07.2015|12:00:10|     7168|         |
|  1562|Prg|EEWA_BA_CLASSIFICATION========CP        |  1562|21.08.2023|09:23:07|    14336|E        |
|  1563|Prg|EEWA_BA_SERVFREQHEAD==========CP        |  1563|21.08.2023|09:18:45|    14336|E        |
|  1564|Prg|IF_EEWA_CLASSIFICATION========IP        |   320|26.02.2015|07:26:08|     5120|E        |
|  1565|Prg|IF_EEWA_KEYDATE===============IP        |   320|26.02.2015|05:43:30|     5120|E        |
|  1566|Prg|CL_EEWA_RFC_SERVICE_FREQUENCY=CP        |  1566|17.06.2026|07:11:56|  1498112|E        |
|  1567|Prg|/1BCDWBEN/SAPLEEN0003                   |  1567|17.06.2024|14:33:37|   303104|E        |
|  1568|Prg|SAPLEEWA_MD_RO                          |  1568|17.06.2026|07:12:08|  2492416|E        |
|  1569|Typ|ISUWA_OBJ_AUTO                          |     0|05.06.2026|18:01:43|   108544|         |
|  1570|Typ|ISUWA_ROB                               |     0|17.06.2026|07:11:11|  1127424|         |
|  1571|Typ|ISUWA_SFRO                              |     0|03.05.2025|05:24:37|    15360|         |
|  1572|Typ|ISUWA_SLOC                              |     0|05.06.2026|18:01:48|    50176|         |
|  1573|Typ|ISUWA_XVABK                             |     0|21.05.2026|18:06:08|    56320|         |
|  1574|Typ|EWAELOCSD_ALL                           |     0|05.06.2026|18:01:32|    16384|         |
|  1575|Typ|VISER02                                 |     0|06.04.2021|21:49:14|     6144|         |
|  1576|Typ|ISUWA_OBJ                               |     0|17.06.2026|07:11:07|   465920|         |
|  1577|Typ|ISU_ROBJ_TSL                            |     0|05.09.2025|15:09:00|    11264|         |
|  1578|Typ|ISUWA_SELDAT                            |     0|16.06.2015|08:29:02|     4096|         |
|  1579|Typ|EWAOBJD                                 |     0|05.09.2025|15:07:38|    39936|         |
|  1580|Prg|CX_EEWA_MD====================CP        |  1580|28.04.2023|12:46:46|   105472|E        |
|  1581|Typ|EWAOBJH_GENERIC_FREQUENCY               |     0|25.02.2015|23:58:18|     5120|         |
|  1582|Prg|SAPLEMIG_CONTROL                        |  1582|26.02.2015|06:57:10|    43008|E        |
|  1583|Prg|SAPLDLOG                                |  1583|06.06.2015|16:46:08|     8192|E        |
|  1584|Typ|NRIV                                    |     0|08.09.2011|14:27:06|     4096|         |
|  1585|Prg|ISU_WA_CONT_INSTALL===========CP        |  1585|17.06.2026|07:11:43|    15360|E        |
|  1586|Prg|SAPLEEWA_DB_ROBJ                        |  1586|17.06.2026|07:11:44|   676864|E        |
|  1587|Typ|ISUWA_ROBJ                              |     0|05.05.2026|18:04:11|    23552|         |
|  1588|Typ|EWAOBJH_DATES                           |     0|26.02.2015|00:20:54|     4096|         |
|  1589|Typ|EWAOBJH_DIFFCULT                        |     0|26.02.2015|00:20:54|     3072|         |
|  1590|Typ|EWAOBJH_UPOS                            |     0|05.05.2026|18:05:13|     4096|         |
|  1591|Typ|EWAOBJH_SDSHARE                         |     0|26.02.2015|00:20:54|     4096|         |
|  1592|Typ|ISUWA_EWAOBJH                           |     0|05.05.2026|18:03:53|    18432|         |
|  1593|Prg|/WATP/CL_IM_CS_BADICONTINS====CP        |  1593|17.06.2026|07:11:41|   463872|E        |
|  1594|Typ|ISUWA_SERVLOC                           |     0|17.06.2026|07:11:12|   860160|         |
|  1595|Typ|EGPL                                    |     0|25.02.2015|23:58:01|     8192|         |
|  1596|Prg|SAPLEEWA_DEV_SERVICE                    |  1596|17.06.2026|07:11:44|   669696|E        |
|  1597|Prg|SAPLEEWA_RCI_PROCESSES                  |  1597|17.06.2026|07:11:44|   320512|E        |
|  1598|Prg|SAPLEEWA_MD_EQUI                        |  1598|10.11.2025|17:51:35|   238592|E        |
|  1599|Prg|IF_EX_ISU_WA_CONT_INSTALL=====IP        |  1025|17.06.2026|07:11:43|   151552|E        |
|  1600|Typ|EWAELOCSD                               |     0|05.06.2026|18:01:57|    15360|         |
|  1601|Typ|EWAEQUI                                 |     0|25.02.2015|23:58:18|    10240|         |
|  1602|Typ|EHAU                                    |     0|17.06.2024|13:24:24|     9216|         |
|  1603|Prg|SAPLEHAU                                |  1603|17.06.2024|13:32:17|    90112|E        |
|  1604|Typ|ADDR1_SEL                               |     0|31.05.2013|07:56:55|     3072|         |
|  1605|Prg|SAPLSZA0                                |  1605|17.06.2024|13:21:41|  3220480|E        |
|  1606|Typ|AD1_FLAGS                               |     0|08.09.2011|14:17:07|     4096|         |
|  1607|Typ|ADDR1_TEXT                              |     0|08.09.2011|14:25:39|     3072|         |
|  1608|Typ|ADDR1_VAL                               |     0|17.06.2024|13:15:42|    20480|         |
|  1609|Typ|ADDR_ERROR                              |     0|31.05.2013|07:56:57|     4096|         |
|  1610|Prg|SAPLSZA12                               |  1610|17.06.2024|12:56:02|    25600|E        |
|  1611|Typ|ADCOMTYPES                              |     0|08.09.2011|14:25:29|     3072|         |
|  1612|Prg|CL_ADDR_TSADRV_MNGR===========CP        |  1612|07.04.2016|14:03:16|     8192|E        |
|  1613|Prg|CL_ADDR_TSADRV================CP        |  1613|08.06.2020|09:46:51|    28672|E        |
|  1614|Typ|ADRC                                    |     0|17.06.2024|12:54:39|    24576|         |
|  1615|Typ|ADRCT                                   |     0|07.04.2016|14:03:02|     3072|         |
|  1616|Typ|ADRP                                    |     0|17.05.2022|14:43:52|    10240|         |
|  1617|Typ|ADCP                                    |     0|07.04.2016|14:02:56|     9216|         |
|  1618|Typ|ADR2                                    |     0|07.04.2016|14:02:57|     5120|         |
|  1619|Typ|ADR3                                    |     0|07.04.2016|14:02:57|     5120|         |
|  1620|Typ|ADR4                                    |     0|07.04.2016|14:02:57|     5120|         |
|  1621|Typ|ADR5                                    |     0|07.04.2016|14:02:57|     5120|         |
|  1622|Typ|ADR6                                    |     0|07.04.2016|14:02:58|     6144|         |
|  1623|Typ|ADR7                                    |     0|07.04.2016|14:02:58|     5120|         |
|  1624|Typ|ADR8                                    |     0|07.04.2016|14:02:58|    10240|         |
|  1625|Typ|ADR9                                    |     0|07.04.2016|14:02:58|     5120|         |
|  1626|Typ|ADR10                                   |     0|07.04.2016|14:02:56|     4096|         |
|  1627|Typ|ADR11                                   |     0|07.04.2016|14:02:56|     9216|         |
|  1628|Typ|ADR12                                   |     0|07.04.2016|14:02:56|     9216|         |
|  1629|Typ|ADR13                                   |     0|07.04.2016|14:02:56|     5120|         |
|  1630|Typ|ADRT                                    |     0|07.04.2016|14:03:04|     4096|         |
|  1631|Typ|ADRU                                    |     0|07.04.2016|14:03:04|     4096|         |
|  1632|Typ|ADRCOMC                                 |     0|07.04.2016|14:03:02|     3072|         |
|  1633|Typ|ADRV                                    |     0|07.04.2016|14:03:04|     4096|         |
|  1634|Typ|ADRVP                                   |     0|07.04.2016|14:03:05|     4096|         |
|  1635|Prg|SAPLSZAU                                |  1635|17.06.2024|13:22:16|    35840|E        |
|  1636|Typ|ADDR_ADDR_PERS_LINE                     |     0|08.09.2011|14:17:04|     3072|         |
|  1637|Typ|VADRC                                   |     0|17.06.2024|13:12:09|    24576|         |
|  1638|Typ|ADDR_ADDRNUMBER_LINE                    |     0|08.09.2011|14:25:39|     2048|         |
|  1639|Prg|SAPLSZA13                               |  1639|17.06.2024|13:12:09|   560128|E        |
|  1640|Prg|SAPLSZAD                                |  1640|17.06.2024|13:17:03|   107520|E        |
|  1641|Typ|VADRCT                                  |     0|07.04.2016|14:02:39|     4096|         |
|  1642|Prg|SAPLESAD                                |  1642|17.06.2026|07:12:09|   217088|E        |
|  1643|Typ|ISU02_ADDRESS                           |     0|08.08.2024|08:36:33|    46080|         |
|  1644|Typ|ISU_REGA                                |     0|26.02.2015|00:03:07|     3072|         |
|  1645|Typ|ISU_REGG                                |     0|24.02.2005|13:45:22|     3072|         |
|  1646|Typ|ISU_REGK                                |     0|26.02.2015|00:03:07|     2048|         |
|  1647|Typ|ISU_REGR                                |     0|26.02.2015|00:03:07|     2048|         |
|  1648|Typ|ISU_REGS                                |     0|24.02.2005|13:45:22|     3072|         |
|  1649|Typ|ADDR_VERS                               |     0|31.05.2013|07:56:58|     2048|         |
|  1650|Typ|ADAGROUPS                               |     0|08.09.2011|14:17:07|     2048|         |
|  1651|Typ|EADRDAT                                 |     0|17.06.2024|13:24:06|    22528|         |
|  1652|Typ|EADRLN                                  |     0|21.01.2014|10:05:08|     4096|         |
|  1653|Prg|SAPLSZAQ                                |  1653|01.06.2015|12:30:31|    16384|E        |
|  1654|Typ|TSAD9                                   |     0|08.09.2011|14:24:50|     2048|         |
|  1655|Typ|ISU_ROBJ_TABC                           |     0|15.06.2018|15:15:40|    29696|         |
|  1656|Typ|EWASCONTAINERASSGN_KEY                  |     0|25.02.2015|23:58:19|     3072|         |
|  1657|Prg|CL_EEWA_RCI_ELOC==============CP        |  1657|17.06.2026|07:11:42|   775168|E        |
|  1658|Prg|SAPLEEWA_MD_LOC                         |  1658|17.06.2026|21:16:45|  2197504|E        |
|  1659|Typ|FKGPD                                   |     0|08.09.2011|14:19:18|     4096|         |
|  1660|Prg|SAPLFKGP                                |  1660|17.06.2026|07:12:09|    51200|E        |
|  1661|Typ|FKKVKP                                  |     0|17.06.2024|13:00:55|    34816|         |
|  1662|Typ|BUS000_EXT                              |     0|17.06.2026|07:09:12|    46080|         |
|  1663|Prg|SAPLBUPA_SELECT_ABA                     |  1663|17.06.2026|07:12:14|   615424|E        |
|  1664|Prg|SAPLBUPA_INIT_SELECT                    |  1664|29.03.2018|15:54:08|    10240|E        |
|  1665|Prg|SAPLBUPA_SELECT                         |  1665|17.06.2026|07:12:14|  1807360|E        |
|  1666|Typ|BUS000___I                              |     0|17.06.2026|07:09:19|    39936|         |
|  1667|Typ|BUT000                                  |     0|17.06.2026|07:11:50|    37888|         |
|  1668|Typ|VBUT000_TD                              |     0|17.06.2026|07:11:36|    31744|         |
|  1669|Prg|SAPLBUD1                                |  1669|17.06.2026|07:12:14|  1889280|E        |
|  1670|Typ|BUS000                                  |     0|17.06.2026|07:07:47|    27648|         |
|  1671|Typ|BUS000_GLB                              |     0|17.06.2026|07:09:19|    29696|         |
|  1672|Typ|BUS000_INT                              |     0|08.09.2011|14:19:00|     5120|         |
|  1673|Typ|BUS000_VALIDITY                         |     0|08.09.2011|14:25:47|     3072|         |
|  1674|Typ|BUS001                                  |     0|13.06.2017|12:40:20|     5120|         |
|  1675|Typ|ADFAX                                   |     0|31.05.2013|07:56:58|     5120|         |
|  1676|Typ|ADPAG                                   |     0|31.05.2013|07:56:58|     5120|         |
|  1677|Typ|ADPRT                                   |     0|31.05.2013|07:56:59|     4096|         |
|  1678|Typ|ADRFC                                   |     0|31.05.2013|07:56:59|     4096|         |
|  1679|Typ|ADRML                                   |     0|31.05.2013|07:56:59|     4096|         |
|  1680|Typ|ADSMTP                                  |     0|31.05.2013|07:57:00|     5120|         |
|  1681|Typ|ADSSF                                   |     0|31.05.2013|07:57:00|     9216|         |
|  1682|Typ|ADTEL                                   |     0|31.05.2013|07:57:01|     5120|         |
|  1683|Typ|ADTLX                                   |     0|31.05.2013|07:57:01|     4096|         |
|  1684|Typ|ADTTX                                   |     0|31.05.2013|07:57:01|     4096|         |
|  1685|Typ|ADURI                                   |     0|31.05.2013|07:57:01|     8192|         |
|  1686|Typ|ADX400                                  |     0|31.05.2013|07:57:01|    10240|         |
|  1687|Typ|BUS000_TD                               |     0|17.06.2026|07:09:19|    28672|         |
|  1688|Typ|BUS000_INT_TD                           |     0|08.09.2011|14:19:01|     5120|         |
|  1689|Prg|BUPA_GENERAL_IMPORT===========CP        |  1689|17.06.2026|07:11:54|    17408|E        |
|  1690|Prg|SAPLBUPA_DIALOG_JOEL                    |  1690|17.06.2026|07:12:07|  1686528|E        |
|  1691|Prg|SAPLBUBA_SERV                           |  1691|17.06.2026|07:12:07|   116736|E        |
|  1692|Prg|BUPA_EXTERN_FLAG==============CP        |  1692|21.08.2023|09:59:08|    15360|E        |
|  1693|Prg|FSBP_SPL_IM_CHECKMODE_ONLY====CP        |  1693|16.06.2015|10:06:37|     8192|E        |
|  1694|Prg|IF_EX_BUPA_EXTERN_FLAG========IP        |  1691|08.09.2011|16:05:39|     5120|E        |
|  1695|Prg|SAPLBUDA_ABA                            |  1695|17.06.2026|07:12:14|   441344|E        |
|  1696|Typ|TB038                                   |     0|08.09.2011|14:23:55|     3072|         |
|  1697|Typ|BUP_PARTNERGUID_S                       |     0|28.05.2013|10:37:14|     2048|         |
|  1698|Prg|SAPLOM_UTILITIES                        |  1698|10.11.2022|19:49:53|   185344|E        |
|  1699|Prg|SAPLBP_CENTRAL_PERSON                   |  1699|17.06.2026|07:12:07|   159744|E        |
|  1700|Prg|CL_HR_ALEOX_CUSTOMIZING=======CP        |  1700|21.08.2023|09:19:25|    54272|E        |
|  1701|Prg|HRALX_BP_EEWA=================CP        |  1701|21.08.2023|09:19:26|    14336|E        |
|  1702|Prg|CX_SY_CREATE_DATA_ERROR=======CP        |  1702|10.11.2022|19:49:48|    12288|E        |
|  1703|Prg|CX_SY_CREATE_ERROR============CP        |  1703|10.11.2022|19:49:48|    10240|E        |
|  1704|Prg|CL_HR_T77S0===================CP        |  1704|10.10.2014|18:27:31|    14336|E        |
|  1705|Typ|T77S0                                   |     0|08.03.1992|17:36:54|     3072|         |
|  1706|Typ|TB056                                   |     0|08.09.2011|14:24:29|     3072|         |
|  1707|Prg|SAPLBUDA                                |  1707|20.12.2021|08:33:04|   377856|E        |
|  1708|Prg|SAPLSEOK                                |  1708|16.05.2023|13:25:43|  1223680|E        |
|  1709|Typ|VSEOCLASS                               |     0|27.11.2013|14:30:56|     9216|         |
|  1710|Typ|SEOCLSREF                               |     0|08.09.2011|14:23:23|     2048|         |
|  1711|Prg|SAPLSEOC                                |  1711|18.03.2023|11:02:54|   173056|E        |
|  1712|Typ|SEOCLSKEY                               |     0|08.09.2011|14:23:23|     2048|         |
|  1713|Prg|CL_WFD_FC_UTILITY=============CP        |  1713|17.06.2026|07:14:34|    48128|E        |
|  1714|Prg|IF_WFD_FC_UTILITY=============IP        |  1699|20.11.2020|09:33:23|     6144|E        |
|  1715|Prg|CL_WFD_FC_SWITCH_CHECK========CP        |  1715|20.11.2020|10:45:31|    10240|E        |
|  1716|Prg|SAPLHRALX00_API                         |  1716|17.06.2026|07:11:44|    76800|E        |
|  1717|Prg|SAPLHRIPA01CENTRAL_PERSON               |  1717|27.01.2017|15:04:41|   114688|E        |
|  1718|Prg|SAPLHRIPA00OBJECT_BUPA                  |  1718|13.11.2000|14:25:54|    29696|E        |
|  1719|Typ|HROBJBUPAS                              |     0|14.10.1999|21:56:57|     3072|         |
|  1720|Prg|SAPLBUBA_4                              |  1720|17.06.2026|07:12:14|  2002944|E        |
|  1721|Typ|BAPIBUS1006_KEY                         |     0|08.09.2011|14:18:34|     3072|         |
|  1722|Typ|BUPA_PARTNER                            |     0|08.09.2011|14:18:59|     2048|         |
|  1723|Prg|SAPLBUD_MEM                             |  1723|17.06.2026|07:12:14|   367616|E        |
|  1724|Typ|BUT000_TD                               |     0|17.06.2026|07:12:14|    30720|         |
|  1725|Typ|HRI1001                                 |     0|09.11.2000|14:08:01|     7168|         |
|  1726|Typ|HRSOBID                                 |     0|04.04.1995|15:54:02|     3072|         |
|  1727|Prg|SAPLRHDB                                |  1727|10.11.2022|19:49:53|   434176|E        |
|  1728|Prg|MSTT77S0                                |  1727|01.06.2015|12:30:31|    13312|E        |
|  1729|Typ|HRP1001                                 |     0|07.05.1997|12:54:03|     6144|         |
|  1730|Prg|CL_HRBAS_EXT_RELAT_FACTORY====CP        |  1730|03.11.2021|19:19:36|    37888|E        |
|  1731|Prg|CL_HR_T77EV===================CP        |  1731|10.10.2014|18:48:17|    28672|E        |
|  1732|Typ|BUT0IS                                  |     0|08.09.2011|14:25:50|     3072|         |
|  1733|Prg|SAPLBUPA_TD_SWITCH                      |  1733|17.06.2026|07:12:14|   499712|E        |
|  1734|Typ|BUT020                                  |     0|03.05.2025|05:20:06|     5120|         |
|  1735|Typ|BUT021                                  |     0|08.09.2011|14:19:01|     3072|         |
|  1736|Typ|BUT021_FS                               |     0|08.09.2011|14:25:46|     4096|         |
|  1737|Typ|BUT0BK                                  |     0|14.05.2021|06:31:01|     8192|         |
|  1738|Typ|BUT0CC                                  |     0|29.04.2022|14:52:28|     4096|         |
|  1739|Typ|BUT100                                  |     0|08.09.2011|14:19:00|     4096|         |
|  1740|Prg|BUP_READ_SHORTCUT=============CP        |  1740|21.08.2023|09:19:07|    14336|E        |
|  1741|Typ|BUT022                                  |     0|22.01.2021|08:56:03|     3072|         |
|  1742|Prg|SAPLBUA1                                |  1742|17.06.2026|07:12:14|   377856|E        |
|  1743|Typ|BUS020_EXT                              |     0|03.05.2025|05:23:26|    20480|         |
|  1744|Prg|BUPA_ADDR_IMPORT==============CP        |  1744|17.06.2026|07:11:54|    17408|E        |
|  1745|Prg|CL_BUP_PFIELD_STATUS_SET======CP        |  1745|17.12.2018|10:00:33|    22528|E        |
|  1746|Prg|CL_SACF=======================CP        |  1746|26.01.2023|17:38:13|   100352|E        |
|  1747|Typ|TOBJ_CHK_CTRL_RH                        |     0|10.10.2014|18:27:35|     3072|         |
|  1748|Prg|CL_BUP_TD_REFERENCE_TIMEZONE==CP        |  1748|10.11.2022|19:49:29|     8192|E        |
|  1749|Prg|SAPLBUA_CHECK                           |  1749|31.05.2013|08:28:45|   141312|E        |
|  1750|Typ|BUS_ADDRNUMBER                          |     0|08.09.2011|14:25:47|     2048|         |
|  1751|Prg|SAPLBUBA_5                              |  1751|17.06.2026|07:12:07|   685056|E        |
|  1752|Prg|CL_MD_BP_MAINTAIN=============CP        |  1752|17.06.2026|07:12:01|   378880|E        |
|  1753|Typ|ADSUSE_UUID                             |     0|31.05.2013|07:57:01|     3072|         |
|  1754|Typ|VADR2                                   |     0|07.04.2016|14:02:38|     6144|         |
|  1755|Typ|ADRU_UPD                                |     0|07.04.2016|13:57:06|     4096|         |
|  1756|Typ|VADRT                                   |     0|07.04.2016|14:02:39|     4096|         |
|  1757|Typ|ADSUSE                                  |     0|31.05.2013|07:57:00|     3072|         |
|  1758|Prg|%_CSZADR                                |  1660|17.06.2024|13:05:27|   123904|E        |
|  1759|Typ|VADRP                                   |     0|17.05.2022|14:43:29|    11264|         |
|  1760|Typ|VADCP                                   |     0|07.04.2016|14:02:38|    10240|         |
|  1761|Typ|VADR3                                   |     0|07.04.2016|14:02:38|     6144|         |
|  1762|Typ|VADR4                                   |     0|07.04.2016|14:02:39|     5120|         |
|  1763|Typ|VADR5                                   |     0|07.04.2016|14:02:39|     5120|         |
|  1764|Typ|VADR6                                   |     0|07.04.2016|14:02:39|     6144|         |
|  1765|Typ|VADR7                                   |     0|07.04.2016|14:02:39|     5120|         |
|  1766|Typ|VADR8                                   |     0|07.04.2016|14:02:39|    11264|         |
|  1767|Typ|VADR9                                   |     0|07.04.2016|14:02:39|     5120|         |
|  1768|Typ|VADR10                                  |     0|07.04.2016|14:02:38|     5120|         |
|  1769|Typ|VADR11                                  |     0|07.04.2016|14:02:38|    10240|         |
|  1770|Typ|VADR12                                  |     0|07.04.2016|14:02:38|     9216|         |
|  1771|Typ|VADR13                                  |     0|07.04.2016|14:02:38|     6144|         |
|  1772|Typ|ADDRS_ADDRESS_KEY                       |     0|08.09.2011|14:18:18|     4096|         |
|  1773|Prg|SAPLSADR                                |  1773|17.06.2024|13:17:11|   445440|E        |
|  1774|Prg|CL_ADDR_MAP===================CP        |  1774|17.06.2024|13:17:10|    76800|E        |
|  1775|Typ|ADDRS_VERSION                           |     0|08.08.2024|08:35:50|    17408|         |
|  1776|Typ|ADDRS_PERSON_NAME                       |     0|21.01.2014|10:04:39|     6144|         |
|  1777|Typ|ADDRS_WORKPLACE                         |     0|08.09.2011|14:25:36|     3072|         |
|  1778|Typ|ADDRS_ORGANIZATION_NAME                 |     0|08.09.2011|14:25:39|     4096|         |
|  1779|Typ|ADDRS_LOCATION                          |     0|10.10.2014|18:26:21|    10240|         |
|  1780|Typ|ADDRS_COMMUNICATION_PREFERENCE          |     0|31.05.2013|07:56:56|     3072|         |
|  1781|Prg|CL_ADDR_PRINTFORM_MNGR========CP        |  1781|17.06.2024|13:05:43|     8192|E        |
|  1782|Prg|CL_ADDR_PRINTFORM=============CP        |  1782|17.06.2024|13:05:43|   422912|K        |
|  1783|Prg|CL_ADDRESS_PRINTFORM_CONTEXT==CP        |  1783|03.05.2022|08:42:37|     7168|E        |
|  1784|Typ|T005                                    |     0|20.11.2020|06:48:20|    13312|         |
|  1785|Typ|T005T                                   |     0|27.11.2013|14:27:42|     4096|         |
|  1786|Prg|IF_ADDR_TYPES=================IP        |  1782|08.06.2020|09:46:51|    45056|E        |
|  1787|Typ|TSAD3                                   |     0|16.03.2020|10:25:07|     3072|         |
|  1788|Typ|ISUWA_ALLOC_SHARE                       |     0|28.04.2023|11:38:05|     8192|         |
|  1789|Typ|EWA_DATA_FINDER                         |     0|25.02.2015|23:58:23|     4096|         |
|  1790|Typ|ISUWA_OBJ_LINES                         |     0|05.05.2026|18:04:05|    45056|         |
|  1791|Prg|CL_ISU_WA_CLASSIFICATION======CP        |  1791|17.06.2024|14:33:37|    20480|E        |
|  1792|Typ|ISUWA_SERVICE_CLASS                     |     0|26.02.2015|00:02:54|     3072|         |
|  1793|Typ|V_EORDER                                |     0|03.05.2025|05:26:29|    24576|         |
|  1794|Prg|CL_ISU_WA_EXITHANDLER=========CP        |  1794|26.02.2015|05:42:40|    13312|E        |
|  1795|Prg|CL_EX_ISU_WA_BILLING==========CP        |  1795|17.06.2026|07:11:42|   225280|E        |
|  1796|Prg|IF_EX_ISU_WA_BILLING==========IP        |   634|17.06.2026|07:11:43|   199680|E        |
|  1797|Prg|CL_EX_ISU_WA_ADAPT_SERVICE====CP        |  1797|05.09.2025|15:09:27|    20480|E        |
|  1798|Prg|IF_EX_ISU_WA_ADAPT_SERVICE====IP        |   634|05.09.2025|15:09:28|     8192|E        |
|  1799|Prg|CL_EX_ISU_WA_ELOC=============CP        |  1799|17.06.2026|07:11:42|   177152|E        |
|  1800|Prg|IF_EX_ISU_WA_ELOC=============IP        |   634|17.06.2026|07:11:43|   154624|E        |
|  1801|Prg|CL_EX_ISU_WA_PROP=============CP        |  1801|17.06.2026|07:11:42|   111616|E        |
|  1802|Prg|IF_EX_ISU_WA_PROP=============IP        |   634|17.06.2026|07:11:43|    90112|E        |
|  1803|Prg|CL_EX_ISU_WA_SF_CMPL_CHECK====CP        |  1803|17.06.2026|07:11:42|   150528|E        |
|  1804|Prg|IF_EX_ISU_WA_SF_CMPL_CHECK====IP        |   634|17.06.2026|07:11:43|   129024|E        |
|  1805|Prg|CL_EX_ISU_WA_ROUTE_CHECK======CP        |  1805|17.06.2026|07:11:42|   150528|E        |
|  1806|Prg|IF_EX_ISU_WA_ROUTE_CHECK======IP        |   634|17.06.2026|07:11:43|   132096|E        |
|  1807|Prg|CL_EX_ISU_WA_GOODSMOVEMENT====CP        |  1807|09.04.2026|00:02:55|    17408|E        |
|  1808|Prg|IF_EX_ISU_WA_GOODSMOVEMENT====IP        |   634|09.04.2026|00:02:56|     5120|E        |
|  1809|Prg|CL_EX_ISU_WA_GUARCONTRACT=====CP        |  1809|31.07.2015|14:15:01|    32768|E        |
|  1810|Prg|IF_EX_ISU_WA_GUARCONTRACT=====IP        |   634|26.02.2015|02:57:17|    14336|E        |
|  1811|Prg|CL_EX_ISU_WA_ORDER_CONFIRM====CP        |  1811|17.06.2026|07:11:42|   200704|E        |
|  1812|Prg|IF_EX_ISU_WA_ORDER_CONFIRM====IP        |   634|17.06.2026|07:11:43|   177152|E        |
|  1813|Prg|CL_EX_ISU_WA_ORDER_CREATE=====CP        |  1813|17.06.2026|07:11:42|   175104|E        |
|  1814|Prg|IF_EX_ISU_WA_ORDER_CREATE=====IP        |   634|17.06.2026|07:11:43|   155648|E        |
|  1815|Prg|CL_EX_ISU_WA_ORDER_SELECT=====CP        |  1815|09.04.2026|00:02:55|    57344|E        |
|  1816|Prg|IF_EX_ISU_WA_ORDER_SELECT=====IP        |   634|09.04.2026|00:02:56|    36864|E        |
|  1817|Prg|CL_EX_ISU_WA_ROB_ADR_CHECK====CP        |  1817|17.06.2026|07:11:42|   101376|E        |
|  1818|Prg|IF_EX_ISU_WA_ROB_ADR_CHECK====IP        |   634|17.06.2026|07:11:43|    90112|E        |
|  1819|Prg|/WATP/CL_IM_MNB_WA_BILLING====CP        |  1819|17.06.2026|07:11:41|   294912|E        |
|  1820|Typ|SALES_KEY                               |     0|13.04.2016|15:03:44|     2048|         |
|  1821|Prg|/WATP/CL_MNB_MISC=============CP        |  1821|08.08.2024|10:53:30|    32768|E        |
|  1822|Prg|SAPLEMSG                                |  1822|08.09.2011|16:00:35|   110592|E        |
|  1823|Prg|%_CEEMSG                                |  1822|08.09.2011|15:55:17|    29696|E        |
|  1824|Prg|CX_SABP_DD_CATALOG_IO_NOT_FND=CP        |  1824|10.11.2022|19:49:48|    13312|E        |
|  1825|Prg|CX_SABP_DD_CATALOG_IO_ERROR===CP        |  1825|10.11.2022|19:49:48|    12288|E        |
|  1826|Prg|CX_STATIC_CHECK===============CP        |  1826|10.11.2022|19:49:48|    10240|E        |
|  1827|Prg|CL_DD_RTOBJ_FACTORY===========CP        |  1827|16.05.2023|13:23:57|    86016|E        |
|  1828|Prg|IF_DD_RTOBJ_SPECIAL===========IP        |  1827|17.04.2020|19:46:06|     8192|E        |
|  1829|Prg|CL_DD_DIST_UTILS==============CP        |  1829|16.05.2023|13:23:57|    38912|E        |
|  1830|Prg|CX_DD_RTOBJ_NOT_FOUND=========CP        |  1830|16.05.2023|13:23:55|    56320|E        |
|  1831|Prg|CX_DD_RTOBJ===================CP        |  1831|16.05.2023|13:23:55|    60416|E        |
|  1832|Prg|CX_DD_STATIC_CHECK============CP        |  1832|03.03.2023|16:55:26|    20480|E        |
|  1833|Prg|CX_SY_ASSIGN_CAST_ILLEGAL_CASTCP        |  1833|10.11.2022|19:49:48|    10240|E        |
|  1834|Prg|CX_SY_ASSIGN_CAST_ERROR=======CP        |  1834|10.11.2022|19:49:48|    10240|E        |
|  1835|Prg|CX_SY_ASSIGN_ERROR============CP        |  1835|10.11.2022|19:49:48|    10240|E        |
|  1836|Typ|TEWACHECKSERVICE                        |     0|26.02.2015|00:12:10|     3072|         |
|  1837|Typ|TEWASERVICEBILL                         |     0|26.02.2015|00:12:10|     3072|         |
|  1838|Prg|ISU_WA_SERVICE_FREQ===========CP        |  1838|17.06.2026|07:11:43|    15360|E        |
|  1839|Prg|/WATP/CL_IM_CS_BADISERVFRE====CP        |  1839|17.06.2026|07:11:41|   171008|E        |
|  1840|Prg|IF_EX_ISU_WA_SERVICE_FREQ=====IP        |  1566|17.06.2026|07:11:43|   156672|E        |
|  1841|Prg|/WATP/CL_IMPX_CS_BADISERVRE===CP        |  1841|17.06.2026|07:11:41|   277504|E        |
|  1842|Prg|CL_DEF_IM_ISU_WA_SERVICE_FREQ=CP        |  1842|17.06.2026|07:11:42|   354304|E        |
|  1843|Prg|/WATP/SAPLCS_SDWASTEINTERFACE           |  1843|17.06.2026|07:11:53|  1596416|E        |
|  1844|Prg|/WATP/CL_BO_ASSIGN_VBAP_SFREQ=CP        |  1844|11.12.2025|08:34:41|   272384|E        |
|  1845|Typ|/WATP/VVBAPEWAOB                        |     0|08.08.2024|08:35:53|     4096|         |
|  1846|Prg|/WATP/CL_CONVERT==============CP        |  1846|12.02.2025|20:55:25|    89088|E        |
|  1847|Typ|EWASSERVFREQITEM_KEY                    |     0|25.02.2015|23:58:21|     3072|         |
|  1848|Typ|EWASSERVFREQITEM                        |     0|25.02.2015|23:58:21|     4096|         |
|  1849|Typ|EWASSERVFREQITEM_DATA                   |     0|25.02.2015|23:58:21|     3072|         |
|  1850|Typ|EWASSERVFREQHEAD_KEY                    |     0|25.02.2015|23:58:21|     2048|         |
|  1851|Prg|CL_EEWA_BO_SERVFREQTIMESLICE==CP        |  1851|17.06.2026|07:11:42|   331776|E        |
|  1852|Typ|EWASSERVFREQTIMESLICE_DET               |     0|05.09.2025|15:07:44|    13312|         |
|  1853|Prg|EEWA_BA_SERVFREQITEM==========CP        |  1853|21.08.2023|09:18:45|    14336|E        |
|  1854|Typ|EWASSERVFREQTIMESLICE_KEY               |     0|25.02.2015|23:58:21|     3072|         |
|  1855|Typ|EWASSERVFREQTIMESLICE                   |     0|05.09.2025|15:07:44|    13312|         |
|  1856|Typ|EWASSERVFREQTIMESLICE_DATA              |     0|25.02.2015|23:58:21|     9216|         |
|  1857|Typ|/WATP/ASFTS_ORDERTXT                    |     0|24.10.2024|08:59:52|     3072|         |
|  1858|Typ|ZWR_AEWMD_SERVFREQTIMESLICE             |     0|05.09.2025|15:09:15|     4096|         |
|  1859|Typ|EWASROUTE_SEQUENCE                      |     0|25.02.2015|23:58:20|     3072|         |
|  1860|Typ|ZWR_SEEWA_EWAOBJH_EXT                   |     0|05.09.2025|15:09:16|     3072|         |
|  1861|Typ|EWASEWAOBJH_DATES                       |     0|25.02.2015|23:58:20|     5120|         |
|  1862|Typ|EWASEWAOBJH_DIFFCULT                    |     0|25.02.2015|23:58:20|     4096|         |
|  1863|Typ|EWASEWAOBJH_SDSHARE                     |     0|25.02.2015|23:58:20|     4096|         |
|  1864|Typ|EWASEWAOBJH_UPOS                        |     0|05.05.2026|18:03:43|     5120|         |
|  1865|Typ|EWASEWAOBJH_SEQUENCE                    |     0|25.02.2015|23:58:20|     5120|         |
|  1866|Prg|EEWA_BA_SERVFREQTIMESLICE=====CP        |  1866|21.08.2023|09:18:45|    14336|E        |
|  1867|Typ|EWAOBJH                                 |     0|05.09.2025|15:09:26|    14336|         |
|  1868|Prg|/WATP/CL_BO_HW_SR=============CP        |  1868|07.11.2025|23:02:35|   115712|E        |
|  1869|Prg|/WATP/CL_BO_HW_SR_BASE========CP        |  1869|12.02.2025|20:57:09|    98304|E        |
|  1870|Prg|SAPLEEWA_MD_ROUTE                       |  1870|17.06.2026|07:11:44|  1043456|E        |
|  1871|Prg|CL_EEWA_OR_DIFFCULT===========CP        |  1871|26.02.2015|07:25:08|    53248|E        |
|  1872|Prg|CL_EEWA_OR_DIFFCULT_SF========CP        |  1872|08.05.2026|09:28:15|    38912|E        |
|  1873|Prg|CL_EEWA_OR_DIFFCULT_SFG=======CP        |  1873|08.05.2026|09:28:15|    38912|E        |
|  1874|Typ|EWASDIFFCULTINFO                        |     0|25.02.2015|23:58:19|     3072|         |
|  1875|Prg|%_CVRM                                  |  1566|01.06.2015|12:30:30|     8192|E        |
|  1876|Typ|EWASSERVICEFREQSDSHARE_KEY              |     0|25.02.2015|23:58:21|     3072|         |
|  1877|Typ|ISUWA_VBELN                             |     0|24.02.2005|13:45:13|     2048|         |
|  1878|Typ|EWASSERVICEFREQUENCYUPOS_KEY            |     0|16.06.2015|08:28:24|     2048|         |
|  1879|Prg|SAPLSCON                                |  1879|28.05.2018|11:38:51|    27648|E        |
|  1880|Prg|CL_EEWA_RCI_RO================CP        |  1880|17.06.2026|07:11:42|   186368|E        |
|  1881|Prg|SAPLEEWA_ROBJ_INT                       |  1881|17.06.2026|07:11:44|   617472|E        |
|  1882|Prg|CL_DEF_IM_ISU_WA_ROUTE_CHECK==CP        |  1882|17.06.2026|07:11:42|   176128|E        |
|  1883|Typ|V_SERVTYPES_AREA                        |     0|01.04.2022|19:45:14|     3072|         |
|  1884|Typ|EMSG_MSG                                |     0|31.05.2013|08:01:32|     4096|         |
|  1885|Prg|SAPLET01                                |  1885|17.06.2024|12:59:20|    17408|E        |
|  1886|Prg|CL_DEF_IM_ISU_WA_SF_CMPL_CHECKCP        |  1886|17.06.2026|07:11:42|   164864|E        |
|  1887|Prg|SAPLSDTO                                |  1887|04.04.2023|11:22:51|    66560|E        |
|  1888|Prg|SAPLSCA1                                |  1888|10.11.2022|19:49:54|    48128|E        |
|  1889|Typ|ADRC_REGIO                              |     0|08.09.2011|14:18:23|     4096|         |
|  1890|Prg|SAPLSZRH                                |  1890|17.06.2024|12:56:02|    15360|E        |
|  1891|Typ|ISU_REG0                                |     0|26.02.2015|00:03:07|     5120|         |
|  1892|Prg|SAPLEEWA_ROBJ_UPD                       |  1892|17.06.2026|07:11:44|   465920|E        |
|  1893|Typ|EWAOBJH_SEQUENCE                        |     0|26.02.2015|00:20:55|     4096|         |
|  1894|Typ|ISUWA_EWAOBJH_AAT                       |     0|26.02.2015|00:02:54|     6144|         |
|  1895|Typ|EWAOBJH_AAT                             |     0|26.02.2015|00:20:53|     3072|         |
|  1896|Typ|EWAOBJH_AAT_HIST                        |     0|26.02.2015|00:20:53|     8192|         |
|  1897|Typ|VEWAOBJ                                 |     0|03.05.2025|05:26:31|    20480|         |
|  1898|Typ|VEWAOBJH                                |     0|05.09.2025|15:09:10|    15360|         |
|  1899|Prg|/1BCDWBEN/SAPLIEN0000                   |  1899|17.06.2024|13:03:13|   315392|E        |
|  1900|Prg|CL_ADDR_MAIN_PORT_SZA0========CP        |  1900|10.11.2022|19:49:27|     9216|E        |
|  1901|Prg|CL_ADDR_MAIN==================CP        |  1901|17.06.2026|07:11:54|   466944|E        |
|  1902|Prg|CL_ADDR_PERSISTENCY_SZA0======CP        |  1902|17.06.2024|13:17:10|   146432|E        |
|  1903|Prg|CL_ADDR_ENCODE================CP        |  1903|10.11.2022|19:49:27|    62464|E        |
|  1904|Prg|CL_ADDR_TD_CFG_TSADVC_ALL=====CP        |  1904|01.06.2015|12:30:30|    17408|E        |
|  1905|Typ|TSADVC_CLIDEP                           |     0|31.05.2013|08:15:53|     3072|         |
|  1906|Prg|CL_ADDR_CHECK=================CP        |  1906|17.06.2024|13:17:10|   156672|E        |
|  1907|Prg|CL_ADDR_METADATA_MNGR=========CP        |  1907|08.06.2020|09:46:51|     8192|E        |
|  1908|Prg|CL_ADDR_METADATA==============CP        |  1908|03.11.2021|19:19:31|    76800|E        |
|  1909|Prg|CL_ADDR_COMPARE_MNGR==========CP        |  1909|19.05.2020|14:16:46|     8192|E        |
|  1910|Prg|CL_ADDR_COMPARE===============CP        |  1910|03.11.2021|19:19:31|   103424|E        |
|  1911|Prg|CL_CMD_APPLLOG================CP        |  1911|26.01.2023|17:38:04|   276480|E        |
|  1912|Prg|IF_CMD_APPLLOG_CONV===========IP        |  1911|03.11.2021|19:19:43|    11264|E        |
|  1913|Prg|SAPLBDLS                                |  1913|16.05.2023|13:23:32|   513024|E        |
|  1914|Prg|IF_CMD_APPLLOG_CONST==========IP        |  1911|01.06.2015|12:30:31|     8192|E        |
|  1915|Typ|CMD_S_LOGS                              |     0|08.09.2011|14:19:17|     3072|         |
|  1916|Prg|CL_ADDR_MAIN_BEFORE_IMAGE=====CP        |  1916|10.11.2022|19:49:27|    92160|E        |
|  1917|Prg|CL_ADDR_RETRIEVE_DB===========CP        |  1917|17.06.2024|13:17:10|   180224|E        |
|  1918|Prg|/PLCE/CL_APPLLOG_HELPER=======CP        |  1918|07.05.2025|00:32:07|    49152|E        |
|  1919|Prg|CL_BALI_LOG_DB================CP        |  1919|16.05.2023|13:23:23|    60416|E        |
|  1920|Prg|CL_BAL_DB_SEARCH==============CP        |  1920|10.11.2022|19:49:28|    57344|E        |
|  1921|Prg|CL_DB_SYS=====================CP        |  1921|26.01.2023|17:38:08|     9216|E        |
|  1922|Prg|CL_DATA_AGING_STATE===========CP        |  1922|26.01.2023|17:38:10|    10240|E        |
|  1923|Typ|BAL_S_DTTM                              |     0|08.09.2011|14:17:15|     3072|         |
|  1924|Prg|CX_BALI_NOT_FOUND=============CP        |  1924|17.05.2020|15:53:50|    12288|E        |
|  1925|Prg|CX_BALI_RUNTIME===============CP        |  1925|17.05.2020|15:53:50|    12288|E        |
|  1926|Prg|/PLCE/CX_BASEEXCEPTION========CP        |  1926|07.05.2025|00:31:44|    31744|E        |
|  1927|Prg|CX_SY_NO_HANDLER==============CP        |  1927|10.11.2022|19:49:48|    10240|E        |
|  1928|Prg|IF_ABAP_BEHV_MESSAGE==========IP        |  1926|26.11.2018|11:15:13|     7168|E        |
|  1929|Prg|IF_CX_DEV_OBJECT_INFO=========IP        |  1918|08.11.2019|10:24:35|     5120|E        |
|  1930|Prg|/PLCE/IF_NOTIFICATION_API=====IP        |   554|07.05.2025|00:31:44|    13312|E        |
|  1931|Prg|IF_BALI_CONSTANTS=============IP        |   554|19.05.2020|14:45:39|     7168|E        |
|  1932|Prg|%_CRSDS                                 |     0|01.06.2015|12:30:30|    11264|E        |
|  1933|Prg|%_CSYLDB                                |     0|01.06.2015|12:30:30|     9216|E        |
|  1934|Typ|CAT_SVARS                               |     0|08.09.2011|14:17:48|     3072|         |
|  1935|Typ|MATCH_RESULT                            |     0|15.06.2005|11:38:22|     3072|         |
|  1936|Typ|TEST_FUNC                               |     0|31.05.2013|08:14:53|     3072|         |
|  1937|Typ|RSTXP                                   |     0|31.07.2015|12:07:20|    19456|         |
|  1938|Typ|DDXTT                                   |     0|23.04.2018|15:43:57|     8192|         |
|  1939|Typ|BAL_S_MDEF                              |     0|08.09.2011|14:25:43|     6144|         |
|  1940|Typ|BAL_S_CONF                              |     0|07.04.2016|13:57:08|    29696|         |
|  1941|Typ|BAL_S_COLL                              |     0|07.04.2016|13:57:08|    15360|         |
|  1942|Typ|BAL_S_MFIL                              |     0|07.04.2016|13:57:08|    15360|         |
|  1943|Typ|BAL_S_DISP                              |     0|07.04.2016|13:57:08|    16384|         |
|  1944|Typ|BAL_S_NTAB                              |     0|31.05.2013|07:57:43|     5120|         |
|  1945|Prg|%_CISU2B                                |   730|24.02.2005|17:20:59|     7168|E        |
|  1946|Prg|%_CVIMTY                                |   781|09.06.2015|09:09:17|    36864|E        |
|  1947|Typ|TDTY4_SD_VBAP_ARRAY_READ_VBELN          |     0|11.12.2025|08:33:15|   101376|         |
|  1948|Prg|%_CISU21                                |  1018|17.06.2026|07:11:41|   416768|E        |
|  1949|Typ|EPRINTPARAMS                            |     0|19.11.2020|09:40:00|    20480|         |
|  1950|Typ|TOA_DARA                                |     0|30.11.1993|16:53:37|     4096|         |
|  1951|Typ|EWA_ORDER_ALV                           |     0|09.04.2026|00:02:05|    55296|         |
|  1952|Typ|ISUWA_GP_DATA                           |     0|17.06.2026|07:11:02|   280576|         |
|  1953|Typ|ISUWA_ALL_OBJECTS                       |     0|17.06.2026|07:11:02|  2373632|         |
|  1954|Typ|LVC_S_MSG                               |     0|28.05.2018|11:34:04|     7168|         |
|  1955|Typ|RFCCBWHITELIST_S                        |     0|28.05.2018|11:35:28|     4096|         |
|  1956|Typ|SHLP_DESCR                              |     0|26.02.2015|00:11:10|    15360|         |
|  1957|Typ|ALRLRSNODE                              |     0|31.05.2013|07:57:14|     7168|         |
|  1958|Prg|%_CISU2A                                |  1568|05.05.2026|18:05:13|   445440|E        |
|  1959|Typ|ICMS_DATA                               |     0|17.06.2024|13:32:59|    27648|         |
|  1960|Typ|ERCH                                    |     0|17.06.2024|12:59:50|    21504|         |
|  1961|Typ|ETRG                                    |     0|24.05.2017|07:40:01|     9216|         |
|  1962|Typ|ISU2A_SCHEDULING_DATA                   |     0|26.02.2015|00:02:53|     5120|         |
|  1963|Typ|ISU2A_UPDATE_DATA                       |     0|09.04.2026|00:02:07|   247808|         |
|  1964|Typ|ISU2A_BILL_DOC                          |     0|17.06.2024|13:25:49|    54272|         |
|  1965|Typ|EVER                                    |     0|03.05.2025|05:24:18|    25600|         |
|  1966|Typ|EITR                                    |     0|20.05.2019|13:39:53|     7168|         |
|  1967|Typ|ISU2A_EVENT_CONTROL                     |     0|26.02.2015|00:02:53|     5120|         |
|  1968|Typ|ISU2A_ABORT_MSG                         |     0|31.05.2013|08:04:50|     3072|         |
|  1969|Typ|ISUZ4_CUSTOMER_UPDATE_DATA              |     0|17.06.2024|13:01:24|     6144|         |
|  1970|Typ|ETTA                                    |     0|13.11.2019|22:21:02|     7168|         |
|  1971|Typ|ISUZ4_PROJECT_UPDATE_DATA               |     0|03.05.2025|05:24:37|     6144|         |
|  1972|Typ|PRJU_BRAZIL_MICROGEN                    |     0|03.05.2025|05:25:36|     5120|         |
|  1973|Typ|BIPARAM                                 |     0|06.03.2023|02:06:04|     5120|         |
|  1974|Typ|ISU2A_FL                                |     0|26.02.2015|00:02:53|     4096|         |
|  1975|Typ|ISU2A_VAR                               |     0|24.02.2005|13:45:11|     2048|         |
|  1976|Typ|ISUZ2_CUSTOMER_BILLING_DATA             |     0|17.06.2024|13:01:21|     7168|         |
|  1977|Typ|ETTIFN                                  |     0|24.04.2017|16:28:58|     7168|         |
|  1978|Typ|ISUZ2_PROJECT_BILLING_DATA              |     0|03.05.2025|05:24:37|     7168|         |
|  1979|Typ|PRJI_BRAZIL_MICROGEN                    |     0|03.05.2025|05:25:36|     7168|         |
|  1980|Typ|ISU_ROUNDING                            |     0|26.02.2015|00:03:07|     2048|         |
|  1981|Typ|ISU2A_BILLRUN                           |     0|24.02.2005|13:45:10|     3072|         |
|  1982|Typ|ERUN                                    |     0|24.02.2005|13:43:34|     3072|         |
|  1983|Typ|ISU_INSTGROUP                           |     0|24.02.2005|13:45:20|     3072|         |
|  1984|Typ|EMDUSPBINST                             |     0|24.05.2017|07:35:09|     8192|         |
|  1985|Prg|%_CEENOT                                |  1596|08.09.2011|15:55:17|    27648|E        |
|  1986|Prg|%_CISU20                                |  1658|17.06.2026|07:11:41|   947200|E        |
|  1987|Typ|REBEZUG                                 |     0|16.06.2015|08:32:04|     9216|         |
|  1988|Typ|RESOHZG                                 |     0|24.02.2005|13:47:45|     3072|         |
|  1989|Typ|ISU02_ADDRESS_AUTO                      |     0|17.06.2024|13:19:55|    18432|         |
|  1990|Typ|ISUWA_OBJ_AUTO_CONTR                    |     0|26.02.2015|00:02:54|     3072|         |
|  1991|Typ|ISW_USE                                 |     0|26.02.2015|00:03:10|     3072|         |
|  1992|Prg|%_CISU01                                |  1658|17.06.2026|07:11:50|   848896|E        |
|  1993|Typ|EANLHKEY                                |     0|24.02.2005|13:43:15|     3072|         |
|  1994|Typ|ISU_AITTYP                              |     0|24.02.2005|13:45:13|     3072|         |
|  1995|Typ|EUI_EXT_OBJ_AUTO                        |     0|24.02.2005|13:43:35|     4096|         |
|  1996|Typ|SEORELKEY                               |     0|08.09.2011|14:24:00|     3072|         |
|  1997|Prg|%_CSEOC                                 |  1711|18.03.2023|11:02:51|    20480|E        |
|  1998|Typ|ISUWA_ROUTE                             |     0|17.06.2026|07:11:12|   818176|         |
|  1999|Typ|ISUWA_ROUTE_LINES                       |     0|03.05.2025|05:24:37|    31744|         |
|  2000|Typ|BDI_LSPAR                               |     0|31.05.2013|07:58:13|     5120|         |
|  2001|Typ|SCREEN                                  |     0|18.05.2004|14:07:14|     5120|         |
|  2002|Typ|VARI                                    |     0|08.06.2015|13:45:41|     7168|         |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Directory of Application Tables                                                                   |
----------------------------------------------------------------------------------------------------
|Name                                     Date       Time       Lngth                              |
|    Val.                                                                                          |
----------------------------------------------------------------------------------------------------
|Program  20260618061533                                                                           |
----------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------
|Program  RS_TESTFRAME_CALL                                                                        |
----------------------------------------------------------------------------------------------------
|SYST                                     10.10.2014 18:27:32   00004612                           |
|    \x0001\0\0\0\x0003\0\x0001\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0                                    |
|SY                                         .  .       :  :     00004612                           |
|    \x0001\0\0\0\x0003\0\x0001\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0                                    |
|VARI                                       .  .       :  :     00003052                           |
|                                                                                                  |
|SSCRFIELDS                               31.05.2013 08:13:14   00001708                           |
|                                                                                                  |
|SSCRTEXTS                                  .  .       :  :     00000742                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPMSSY0                                                                                 |
----------------------------------------------------------------------------------------------------
|TFDIR                                      .  .       :  :     00000290                           |
|    ISU_EWA_CACHE_UPDATETASK      SAPLEEWA_EL_BASE                                                |
----------------------------------------------------------------------------------------------------
|Program  RSDBRUNT                                                                                 |
----------------------------------------------------------------------------------------------------
|VARID                                      .  .       :  :     00000252                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLICON                                                                                 |
----------------------------------------------------------------------------------------------------
|ICONT                                      .  .       :  :     00000130                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSABE                                                                                 |
----------------------------------------------------------------------------------------------------
|TRDIR                                      .  .       :  :     00000340                           |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC==FT         X SD$       1SS                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPFSDS1                                                                                 |
----------------------------------------------------------------------------------------------------
|TDCLD                                      .  .       :  :     00000098                           |
|    RE  DOKU  40R XX XXLIMUDOCUX (XR3TRPROGX        X|                                            |
----------------------------------------------------------------------------------------------------
|Program  SAPLSDOD                                                                                 |
----------------------------------------------------------------------------------------------------
|DOKIL                                      .  .       :  :     00000152                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLLANG                                                                                 |
----------------------------------------------------------------------------------------------------
|T002                                       .  .       :  :     00000010                           |
|    ES1EN|                                                                                        |
----------------------------------------------------------------------------------------------------
|Program  SAPFS_SECLOG                                                                             |
----------------------------------------------------------------------------------------------------
|INDX                                       .  .       :  :     00003104                           |
|    110%L%_SeclogExcludedCaller\0\0\0  00000000                                                   |
----------------------------------------------------------------------------------------------------
|Program  SAPLOLEA                                                                                 |
----------------------------------------------------------------------------------------------------
|TOLE                                       .  .       :  :     00000394                           |
|    SAPGUI.InfoCtrl.1                     SAPGUI.InfoCtrl.1                                       |
----------------------------------------------------------------------------------------------------
|Program  SAPLSEUJ                                                                                 |
----------------------------------------------------------------------------------------------------
|EUFUNC                                     .  .       :  :     00004056                           |
|    FL/PLCP/EEWA_CONFIRMATION       /PLCP/EEWA_ORDER_CONFIRM_R                                    |
|TFDIR                                      .  .       :  :     00000290                           |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC  /PLCP/SAPLEEWA_CONFIRMATION                                     |
|TRDIR                                      .  .       :  :     00000340                           |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC==FT         X SD$       1SS                                      |
|*TRDIR                                     .  .       :  :     00000340                           |
|    /PLCP/LEEWA_CONFIRMATION$01              XXSD$        IS                                      |
|RS38L                                      .  .       :  :     00004994                           |
|                                                                                                  |
|RFCDES                                     .  .       :  :     00007566                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSUNI                                                                                 |
----------------------------------------------------------------------------------------------------
|RS38L                                      .  .       :  :     00004994                           |
|                                                                                                  |
|TFTIT                                      .  .       :  :     00000210                           |
|    E/PLCP/EEWA_ORDER_CONFIRM_RFC  EEWA Order Confirmation                                        |
|TFDIR                                      .  .       :  :     00000290                           |
|    BUP_TB056_SELECT_SINGLE       SAPLBUDA                                                        |
|ENLFDIR                                    .  .       :  :     00000146                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  CONTEXT_X_TR_SYS_PARAMS                                                                  |
----------------------------------------------------------------------------------------------------
|CONTEXTB                                   .  .       :  :     00002992                           |
|    110EBTR_SYS_PARAMS       000006000100000000000000\0\0\0\0                                     |
|TCONT                                      .  .       :  :     00000068                           |
|    TR_SYS_PARAMS       20260615143315|                                                           |
----------------------------------------------------------------------------------------------------
|Program  SAPLSEUF                                                                                 |
----------------------------------------------------------------------------------------------------
|TFDIR                                      .  .       :  :     00000290                           |
|    /PLCP/EEWA_ORDER_CONFIRM_RFC                                                                  |
|RS38L                                      .  .       :  :     00004994                           |
|                                                                                                  |
|ENLFDIR                                    .  .       :  :     00000146                           |
|    SUSR_USER_AUTH_FOR_OBJ_GET    SUSE                      X                                     |
|RSINFDIR                                   .  .       :  :     00000174                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLWB_CUSTOMIZING                                                                       |
----------------------------------------------------------------------------------------------------
|RSEUMOD                                    .  .       :  :     00007544                           |
|    PROLOGAEFO  X4G                                        OC                                     |
|*RSEUMOD                                   .  .       :  :     00007544                           |
|    PROLOGAEFO  X4G                                        OC                                     |
----------------------------------------------------------------------------------------------------
|Program  CL_GUI_PROPS_CONSUMER=========CP                                                         |
----------------------------------------------------------------------------------------------------
|EUDB                                       .  .       :  :     00004068                           |
|    %CCB057860B5A71FE19AD83F82387916C0         \0\0\0                                             |
----------------------------------------------------------------------------------------------------
|Program  /PERSONAS/CL_SESSION_MEMORY===CP                                                         |
----------------------------------------------------------------------------------------------------
|/PERSONAS/T_MEMO                           .  .       :  :     00002976                           |
|    ST01814002CB057860B5A71FE19AD85B0DC2F5D8BC\0\0\0\0\0\0\0\0                                    |
----------------------------------------------------------------------------------------------------
|Program  CL_DD_DFIES_CACHE=============CP                                                         |
----------------------------------------------------------------------------------------------------
|DDDFIESCACHE                               .  .       :  :     00000076                           |
|    DFEWAOBJH                        \0\x0014\0\x0001\0|                                          |
----------------------------------------------------------------------------------------------------
|Program  SAPLBSVT                                                                                 |
----------------------------------------------------------------------------------------------------
|TJ03T                                      .  .       :  :     00000068                           |
|    WOHEWaste Disposal Order Header   |                                                           |
|TJ05                                       .  .       :  :     00000014                           |
|    WOHWA08|                                                                                      |
|INDX                                       .  .       :  :     00003104                           |
|    11000BSV_SWITCH_BUFFER     \0\0\0  00000000                                                   |
----------------------------------------------------------------------------------------------------
|Program  SAPLBSVA                                                                                 |
----------------------------------------------------------------------------------------------------
|JSTO                                       .  .       :  :     00000096                           |
|    110RO0000005427          ROU        X00100000000|                                             |
|TJ01                                       .  .       :  :     00000074                           |
|    WA15XX                               |                                                        |
|TJ02                                       .  .       :  :     00000014                           |
|    IWA11  |                                                                                      |
|TJ03                                       .  .       :  :     00000230                           |
|    WOP                                        0000  000000                                       |
|TJ05                                       .  .       :  :     00000014                           |
|    WOPWA15|                                                                                      |
|IONR                                       .  .       :  :     00000044                           |
|    WE00000000000000006613|                                                                       |
----------------------------------------------------------------------------------------------------
|Program  SAPLRSAN                                                                                 |
----------------------------------------------------------------------------------------------------
|T000                                       .  .       :  :     00000214                           |
|    110SAP SE                   Walldorf                 DKK                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPLMESS                                                                                 |
----------------------------------------------------------------------------------------------------
|T100C                                      .  .       :  :     00000080                           |
|                                            |                                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPLRHFE                                                                                 |
----------------------------------------------------------------------------------------------------
|TFDIR                                      .  .       :  :     00000290                           |
|    ECATT_SAVE_BALMSG             SAPLSETRM                                                       |
----------------------------------------------------------------------------------------------------
|Program  SAPLSBAL_DB                                                                              |
----------------------------------------------------------------------------------------------------
|BALDAT                                     .  .       :  :     00000604                           |
|    110ALomLuOBMd7z6QsP1FRmtEk0000001\0\x0002\000000000M蛇怜ʺ龼목祂                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLE10Z                                                                                 |
----------------------------------------------------------------------------------------------------
|ETYP                                       .  .       :  :     00001624                           |
|    110MINI_240L                               B                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLEGPL                                                                                 |
----------------------------------------------------------------------------------------------------
|EGPLTX                                     .  .       :  :     00000128                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSWOR                                                                                 |
----------------------------------------------------------------------------------------------------
|TOJTB                                      .  .       :  :     00000966                           |
|    WASTEODITMXE          RESOWASTEORDERITEM                                                      |
|SWOTLV                                     .  .       :  :     00001402                           |
|    WASTEORDERORDERALLCREATED                 E                                                   |
----------------------------------------------------------------------------------------------------
|Program  SAPLSWOD                                                                                 |
----------------------------------------------------------------------------------------------------
|TOJTB                                      .  .       :  :     00000966                           |
|    WASTEORDERXE          RESOWASTE                                                               |
----------------------------------------------------------------------------------------------------
|Program  SAPLSWOL                                                                                 |
----------------------------------------------------------------------------------------------------
|TOJTT                                      .  .       :  :     00000222                           |
|    WASTEORDEREOrder for Waste Disposal                                                           |
----------------------------------------------------------------------------------------------------
|Program  CL_SWF_TRC_MANAGER============CP                                                         |
----------------------------------------------------------------------------------------------------
|SWWINDX                                    .  .       :  :     00003104                           |
|    110TRBC-BMT-WFM            \0\0\0  00000000                                                   |
----------------------------------------------------------------------------------------------------
|Program  CL_PAK_INACTIVE_EXT_PACKAGES==CP                                                         |
----------------------------------------------------------------------------------------------------
|PAKDATBUF                                  .  .       :  :     00002968                           |
|    XXINACTIVE_PACKAGES     \0\000000000000000\0\0\0\0\0\0\0\0                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLSCP1                                                                                 |
----------------------------------------------------------------------------------------------------
|TBDA1                                      .  .       :  :     00000008                           |
|    110X|                                                                                         |
----------------------------------------------------------------------------------------------------
|Program  SAPLSCV0                                                                                 |
----------------------------------------------------------------------------------------------------
|T006D                                      .  .       :  :     00000042                           |
|    110VOLUME\x0003\0\0\0\0\0\0M3 XX|                                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPLOMCV                                                                                 |
----------------------------------------------------------------------------------------------------
|TMCNV                                      .  .       :  :     00000206                           |
|    110MATCONV \x0012\0\0\0\x0012                                                                 |
----------------------------------------------------------------------------------------------------
|Program  SAPLSCVU                                                                                 |
----------------------------------------------------------------------------------------------------
|T006A                                      .  .       :  :     00000112                           |
|    110EL  L  l     l         Liter                         |                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPLBSVY                                                                                 |
----------------------------------------------------------------------------------------------------
|BSVWCOUP5                                  .  .       :  :     00000234                           |
|    110COHCRMQUOTEBUS2000115RELEASED                                                              |
----------------------------------------------------------------------------------------------------
|Program  SAPLARFC                                                                                 |
----------------------------------------------------------------------------------------------------
|ARFCTID                                    .  .       :  :     00000048                           |
|                            |                                                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPFSSO0                                                                                 |
----------------------------------------------------------------------------------------------------
|SOPR                                       .  .       :  :     00001338                           |
|    110        X                                         \0                                       |
|SOUC                                       .  .       :  :     00000090                           |
|    110PROLOGAEFO  USR50000000000058PROLOGAEFO   |                                                |
|V_SOUDADCP                                 .  .       :  :     00001296                           |
|    110USR500000000000581100000001302000000137900010101 USR500                                    |
|SOADRBUF                                   .  .       :  :     00002964                           |
|    110SOSOADDRBUF                     \0\0\0\0\0\0\0\0\0\0\0                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPFSSO3                                                                                 |
----------------------------------------------------------------------------------------------------
|TSOUX                                      .  .       :  :     00000030                           |
|                   |                                                                              |
|T002                                       .  .       :  :     00000010                           |
|         |                                                                                        |
|TFDIR                                      .  .       :  :     00000290                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSX04                                                                                 |
----------------------------------------------------------------------------------------------------
|SXADDRTYPE                                 .  .       :  :     00000042                           |
|    110PRTSAPCONNECTXPRT |                                                                        |
----------------------------------------------------------------------------------------------------
|Program  SAPLSOC8                                                                                 |
----------------------------------------------------------------------------------------------------
|TSOTD                                      .  .       :  :     00000020                           |
|    EXT    XX |                                                                                   |
|TSOTT                                      .  .       :  :     00000060                           |
|    EEXTPC document               |                                                               |
----------------------------------------------------------------------------------------------------
|Program  SAPFSSO6                                                                                 |
----------------------------------------------------------------------------------------------------
|SOFD                                       .  .       :  :     00000570                           |
|    110FOL50000000000004                 E#         #                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPLSDCL                                                                                 |
----------------------------------------------------------------------------------------------------
|SDOKIOCL                                   .  .       :  :     00000600                           |
|                                                                                                  |
|SDOKLOCL                                   .  .       :  :     00000754                           |
|                          00000000000000            0000000000                                    |
|SDOKLOTAB                                  .  .       :  :     00000362                           |
|                                                                                                  |
|SDOKPHTAB                                  .  .       :  :     00000662                           |
|                                                                                                  |
|SDOKRECL                                   .  .       :  :     00000188                           |
|                          00000000000000            0000000000                                    |
|SDOKREPOS                                  .  .       :  :     00000112                           |
|                                              00000000000000|                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPLSCMS_URL                                                                             |
----------------------------------------------------------------------------------------------------
|CREP_HTTP                                  .  .       :  :     00000730                           |
|    Z1                            0047                                                            |
----------------------------------------------------------------------------------------------------
|Program  SAPLSSFG                                                                                 |
----------------------------------------------------------------------------------------------------
|SSFARGS                                    .  .       :  :     00001734                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSFTP                                                                                 |
----------------------------------------------------------------------------------------------------
|THTTP                                      .  .       :  :     00000426                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLCRFC                                                                                 |
----------------------------------------------------------------------------------------------------
|RFCDISPLAZ                                 .  .       :  :     00001036                           |
|                                                                                                  |
|ST4QRFC                                    .  .       :  :     00000006                           |
|    0  |                                                                                          |
----------------------------------------------------------------------------------------------------
|Program  SAPLSALI                                                                                 |
----------------------------------------------------------------------------------------------------
|T100                                       .  .       :  :     00000194                           |
|    ECMS                 025HTTP error: &1 &2 &3&4                                                |
----------------------------------------------------------------------------------------------------
|Program  SAPLCLSE                                                                                 |
----------------------------------------------------------------------------------------------------
|USR01                                      .  .       :  :     00000230                           |
|    110PROLOGAEFO                           HK1                                                   |
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_MD_RO                                                                           |
----------------------------------------------------------------------------------------------------
|EWAOBJD                                    .  .       :  :     00008232                           |
|                                                                                                  |
|EWA_DATA_FINDER                            .  .       :  :     00000264                           |
|                                                                                                  |
|ISUWA_SELDAT                               .  .       :  :     00000124                           |
|    2026051920270618  XXX                                                                         |
----------------------------------------------------------------------------------------------------
|Program  SAPLSZA0                                                                                 |
----------------------------------------------------------------------------------------------------
|ADRC                                       .  .       :  :     00003176                           |
|                 00000000 00000000                                                                |
|ADRP                                       .  .       :  :     00001238                           |
|                 00000000 00000000                                                                |
|ADCP                                       .  .       :  :     00000732                           |
|                           00000000 00000000                                                      |
|ADRCOMC                                    .  .       :  :     00000090                           |
|                              00000000   00000000|                                                |
|ADRCT                                      .  .       :  :     00000162                           |
|                 00000000                                                                         |
|ADRT                                       .  .       :  :     00000192                           |
|                              00000000000                                                         |
|ADRV                                       .  .       :  :     00000352                           |
|                 0000000000                                                                       |
|ADRVP                                      .  .       :  :     00000374                           |
|                 0000000000                                                                       |
|ADRU                                       .  .       :  :     00000168                           |
|                              000                                                                 |
|ADR2                                       .  .       :  :     00000356                           |
|                           00000000000                                                            |
|ADR3                                       .  .       :  :     00000358                           |
|                           00000000000                                                            |
|ADR4                                       .  .       :  :     00000210                           |
|                           00000000000                                                            |
|ADR5                                       .  .       :  :     00000210                           |
|                           00000000000                                                            |
|ADR6                                       .  .       :  :     00000676                           |
|                           00000000000                                                            |
|ADR7                                       .  .       :  :     00000202                           |
|                           00000000000                                                            |
|ADR8                                       .  .       :  :     00001968                           |
|                           00000000000                                                            |
|ADR9                                       .  .       :  :     00000214                           |
|                           00000000000                                                            |
|ADR10                                      .  .       :  :     00000156                           |
|                           00000000000                                                            |
|ADR11                                      .  .       :  :     00004580                           |
|                           00000000000                                                            |
|ADR12                                      .  .       :  :     00004296                           |
|                           00000000000                                                            |
|ADR13                                      .  .       :  :     00000344                           |
|                           00000000000                                                            |
----------------------------------------------------------------------------------------------------
|Program  SAPLSZAQ                                                                                 |
----------------------------------------------------------------------------------------------------
|TSAD9                                      .  .       :  :     00000012                           |
|          |                                                                                       |
----------------------------------------------------------------------------------------------------
|Program  SAPLFKGP                                                                                 |
----------------------------------------------------------------------------------------------------
|FKGPD                                      .  .       :  :     00000942                           |
|    0002156591Rob Ejendomme / Kingstrupvej 152B / 5591 Gelsted                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLBUD1                                                                                 |
----------------------------------------------------------------------------------------------------
|BUT000                                     .  .       :  :     00003818                           |
|    11000021565912    BP01                    ROB EJENDOMME                                       |
----------------------------------------------------------------------------------------------------
|Program  MSTT77S0                                                                                 |
----------------------------------------------------------------------------------------------------
|T77S0                                      .  .       :  :     00000046                           |
|    110DBSELDBPCH100       |                                                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPLBDLS                                                                                 |
----------------------------------------------------------------------------------------------------
|T000                                       .  .       :  :     00000214                           |
|    110SAP SE                   Walldorf                 DKK                                      |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|ABAP Control Blocks (CONT)                                                                        |
----------------------------------------------------------------------------------------------------
|Index|Name|Fl|PAR0|PAR1|PAR2|PAR3|PAR4|PAR5|PAR6|Source Code                             |Line    |
----------------------------------------------------------------------------------------------------
|  486|clom|00|000A|C012|0001|0000|0000|0000|0000|/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  490|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  491|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  492|PAR1|01|C011|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  493|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  494|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  495|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  496|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  497|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  498|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  499|PAR1|00|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  500|PAR2|00|0000|0001|0000|    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|>>>>>|EXCP|05|C012|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      59|
|  503|BRAX|00|0001|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      62|
|  504|EXCP|0B|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      62|
|  505|CMPS|20|0390|C005|C005|    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      64|
|  507|BRAF|02|0021|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      64|
|  508|BREL|80|0000|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      64|
|  509|EXCP|06|0002|    |    |    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      65|
|  510|ABOO|00|000A|C006|0002|    |    |    |    |/PLCE/CL_APPLLOG_HELPER=======CM004     |      65|
----------------------------------------------------------------------------------------------------
                                                                                                                                                                
