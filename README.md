----------------------------------------------------------------------------------------------------
Category               ABAP programming error                                                       
Runtime Errors         GEN_PROGRAM_NOT_EXISTS                                                       
ABAP: Program          SAPL/PLCP/BGPROCESSING                                                       
Application Component  Not assigned                                                                 
Date and Time          19.02.2026 08:36:25 (CET)                                                    
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Short Text                                                                                        |
|    Program "SAPL/PLCP/BGPROCESSING" does not exist.                                              |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|What happened?                                                                                    |
|    Generation of ABAP program "SAPL/PLCP/BGPROCESSING" had to be interrupted                     |
|     because it                                                                                   |
|    contains a statement that cannot be generated.                                                |
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
|    During processing, program "SAPL/PLCP/BGPROCESSING" was addressed. This                       |
|     program does not                                                                             |
|    exist however.                                                                                |
|                                                                                                  |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|How to correct the error                                                                          |
|    If the error occurs in one of your own ABAP programs or an SAP program                        |
|    that you have modified, try to correct the error yourself.                                    |
|    If the error occurs in a non-modfied SAP program, you might be able to                        |
|    find a solution in the SAP Notes system. If you have access to the SAP                        |
|    Notes system, check there first using the following keywords:                                 |
|                                                                                                  |
|    "GEN_PROGRAM_NOT_EXISTS"                                                                      |
|    "SAPL/PLCP/BGPROCESSING" bzw. ????????????????????????????????????????                        |
|    "SCAN_PROGRAM"                                                                                |
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
|    Work process number... 48                                                                     |
|    Work process ID....... 3161215                                                                |
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
|    Created on....... Oct 24 2025 21:10:20                                                        |
|    Created at....... Linux GNU SLES-15 x86_64  cc10.3.0 use-pr251024                             |
|    Database version SQLDBC 2.26.018.1753711400                                                   |
|    Patch level....... 334                                                                        |
|    Patch text.......                                                                             |
|    Hash............. a3c68d0981f09f3b67ee5d6fda8af9fd17a7e83b                                    |
|    Version.......... v7.93-7387-ga3c68d09                                                        |
|                                                                                                  |
|    Database............ HANA 1.0, HANA 2.0                                                       |
|    SAP database version. 793                                                                     |
|    Operating system... Linux,   GitVers: v7.93-7387-ga3c68d09,   GitHash:                        |
|     a3c68d0981f09f3b67ee5d6fda8af9fd17a7e83b                                                     |
|                                                                                                  |
|    Memory consumption                                                                            |
|    EM...... 34810944 Bytes                                                                       |
|    Heap........................ 0 Bytes                                                          |
|    Page........................ 155648 Bytes                                                     |
|    MM used memory..... 9737280 Bytes                                                             |
|    MM free memory.......... 6133840 Bytes                                                        |
|    MM largest free block... 1458080 Bytes                                                        |
|    MM used blocks....... 11781                                                                   |
|    MM free blocks............ 87                                                                 |
|    MM maximum memory used... 14412984 Bytes                                                      |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|User and Transaction                                                                              |
|    Client................. 110                                                                   |
|    User.................. PROLOGAEFO                                                             |
|    Language key.......... D                                                                      |
|    Transaction......... SBGRFCMON                                                                |
|    Transaction ID...... 713C1E3CC4680300E0069968C43299CC                                         |
|                                                                                                  |
|    EPP Root Context ID.... CB057860B5A71FD183ACAD0DE6CB63E8                                      |
|    EPP Connection ID...... 00000000000000000000000000000000                                      |
|    EPP Connection Counter. 0                                                                     |
|    EPP Component Name..... S4Q/vhmuzs4qci_S4Q_00                                                 |
|                                                                                                  |
|    Program....................... SAPL/PLCP/BGPROCESSING                                         |
|    Screen......................... SAPLSFUNCTION_BUILDER                   3000                  |
|    Screen line.................... 17                                                            |
|    Debugger interpretor loop... "none"                                                           |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Information on where terminated                                                                   |
|    The termination occurred during generation of ABAP/4 program                                  |
|     "SAPL/PLCP/BGPROCESSING".                                                                    |
|                                                                                                  |
|    In the source code, the termination point is in line 0 of (Include)                           |
|    program "????????????????????????????????????????".                                           |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Contents of system fields                                                                         |
----------------------------------------------------------------------------------------------------
|Name    |Val.                                                                                     |
----------------------------------------------------------------------------------------------------
|SY-SUBRC|8                                                                                        |
|SY-INDEX|0                                                                                        |
|SY-TABIX|1                                                                                        |
|SY-DBCNT|1                                                                                        |
|SY-FDPOS|4                                                                                        |
|SY-LSIND|0                                                                                        |
|SY-PAGNO|0                                                                                        |
|SY-LINNO|1                                                                                        |
|SY-COLNO|1                                                                                        |
|SY-PFKEY|WB_WITH_TOOL_MODI                                                                        |
|SY-UCOMM|WB_SELECT_OBJECT                                                                         |
|SY-TITLE|Function Builder: /PLCP/BGPROCESSUNIT anzeigen                                           |
|SY-MSGTY|S                                                                                        |
|SY-MSGID|E2                                                                                       |
|SY-MSGNO|012                                                                                      |
|SY-MSGV1|BAPIRET                                                                                  |
|SY-MSGV2|                                                                                         |
|SY-MSGV3|                                                                                         |
|SY-MSGV4|                                                                                         |
|SY-MODNO|2                                                                                        |
|SY-DATUM|20260219                                                                                 |
|SY-UZEIT|083624                                                                                   |
|SY-XPROG|SAPMSSY1                                                                                 |
|SY-XFORM|XAB_WFLUSH                                                                               |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Active Calls/Events                                                                               |
----------------------------------------------------------------------------------------------------
|No.   Ty.          Program                             Include                             Line   |
|      Name                                                                                        |
----------------------------------------------------------------------------------------------------
|   30 METHOD       CL_ABAP_COMPILER==============CP    CL_ABAP_COMPILER==============CM043    97  |
|      CL_ABAP_COMPILER=>SCAN_PROGRAM                                                              |
|   29 METHOD       CL_ABAP_COMPILER==============CP    CL_ABAP_COMPILER==============CM017    89  |
|      CL_ABAP_COMPILER=>SCAN                                                                      |
|   28 METHOD       CL_ABAP_COMPILER==============CP    CL_ABAP_COMPILER==============CM00W    73  |
|      CL_ABAP_COMPILER=>GET_SINGLE_REF                                                            |
|   27 METHOD       CL_SEDI_ABAP_COMPILER=========CP    CL_SEDI_ABAP_COMPILER=========CM00A    18  |
|      CL_SEDI_ABAP_COMPILER=>GET_SINGLE_REF                                                       |
|   26 FORM         SAPLSEDA                            LSEDAF04                              145  |
|      COMPILER_COMPONENT_NAVIGATION                                                               |
|   25 FUNCTION     SAPLSEDA                            LSEDAU34                              253  |
|      RS_NAVIGATION_PREPARE_NEW                                                                   |
|   24 FORM         SAPLSEDA                            LSEDAF01                             2607  |
|      CALL_ABAP_COMPILER                                                                          |
|   23 FUNCTION     SAPLSEDA                            LSEDAU15                             3624  |
|      RS_NAVIGATION_PREPARE                                                                       |
|   22 METHOD       CL_WB_SOURCE_NAVIGATOR========CP    CL_WB_SOURCE_NAVIGATOR========CM002    42  |
|      CL_WB_SOURCE_NAVIGATOR=>PREPARE_NAVIGATION                                                  |
|   21 METHOD       CL_WB_SOURCE_NAVIGATOR========CP    CL_WB_SOURCE_NAVIGATOR========CM006    14  |
|      CL_WB_SOURCE_NAVIGATOR=>GET_NAVIGATION_OBJECT                                               |
|   20 METHOD       CL_WB_EDITOR==================CP    CL_WB_EDITOR==================CM00G    45  |
|      CL_WB_EDITOR=>GET_NAVIGATION_OBJECT                                                         |
|   19 METHOD       CL_FB_FUNCTION_NAVIGATION=====CP    CL_FB_FUNCTION_NAVIGATION=====CM002    26  |
|      CL_FB_FUNCTION_NAVIGATION=>EDITOR_NAVIGATION                                                |
|   18 METHOD       CL_FB_FUNCTION_NAVIGATION=====CP    CL_FB_FUNCTION_NAVIGATION=====CM001    63  |
|      CL_FB_FUNCTION_NAVIGATION=>NAVIGATE                                                         |
|   17 METHOD       CL_FUNCTION_BUILDER===========CP    CL_FUNCTION_BUILDER===========CM01A     4  |
|      CL_FUNCTION_BUILDER=>NAVIGATE                                                               |
|   16 FORM         SAPLSFUNCTION_BUILDER               LSFUNCTION_BUILDERFDI                 650  |
|      DI_GENERAL_OK_CODE                                                                          |
|   15 FORM         SAPLSFUNCTION_BUILDER               LSFUNCTION_BUILDERF01                 232  |
|      OK_CODE_TABSTRIP                                                                            |
|   14 MODULE (PAI) SAPLSFUNCTION_BUILDER               LSFUNCTION_BUILDERI01                  48  |
|      OK_CODE_TABSTRIP                                                                            |
|   13 FUNCTION     SAPLSFUNCTION_BUILDER               LSFUNCTION_BUILDERU01                  43  |
|      RS_FUNCTION_DISPLAY                                                                         |
|   12 METHOD       CL_FUNCTION_BUILDER===========CP    CL_FUNCTION_BUILDER===========CM001   463  |
|      CL_FUNCTION_BUILDER=>CHANGE                                                                 |
|   11 METHOD       CL_FUNCTION_BUILDER===========CP    CL_FUNCTION_BUILDER===========CM00T   132  |
|      CL_FUNCTION_BUILDER=>IF_WB_PROGRAM~PROCESS_WB_REQUEST                                       |
|   10 METHOD       CL_WB_NAVIGATOR===============CP    CL_WB_NAVIGATOR===============CM006   116  |
|      CL_WB_NAVIGATOR=>DO_THE_NAVIGATION                                                          |
|    9 METHOD       CL_WB_NAVIGATOR_VIS_AS_DYNPRO=CP    CL_WB_NAVIGATOR_VIS_AS_DYNPRO=CM006    13  |
|      CL_WB_NAVIGATOR_VIS_AS_DYNPRO=>DO_THE_NAVIGATION                                            |
|    8 METHOD       CL_WB_MANAGER=================CP    CL_WB_MANAGER=================CM001   102  |
|      CL_WB_MANAGER=>PROCESS_WB_REQUEST                                                           |
|    7 METHOD       CL_WB_MANAGER=================CP    CL_WB_MANAGER=================CM006    34  |
|      CL_WB_MANAGER=>PROCESS_REQUEST_QUEUE                                                        |
|    6 METHOD       CL_WB_MANAGER=================CP    CL_WB_MANAGER=================CM00P    29  |
|      CL_WB_MANAGER=>IF_WB_MANAGER~SET_WORKSPACE                                                  |
|    5 METHOD       CL_WB_STARTUP=================CP    CL_WB_STARTUP=================CM003    51  |
|      CL_WB_STARTUP=>START_INTERNAL                                                               |
|    4 MODULE (PAI) SAPLWB_MANAGER                      LWB_MANAGERU03                         31  |
|      MANAGER_START                                                                               |
|    3 FUNCTION     SAPLWB_MANAGER                      LWB_MANAGERU03                         17  |
|      WB_MANAGER_START                                                                            |
|    2 METHOD       CL_WB_STARTUP=================CP    CL_WB_STARTUP=================CM001     5  |
|      CL_WB_STARTUP=>START                                                                        |
|    1 EVENT        RS_EDTR_START_NEW_MODE              RS_EDTR_START_NEW_MODE                 74  |
|      START-OF-SELECTION                                                                          |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Selected Variables                                                                                |
----------------------------------------------------------------------------------------------------
|Name                                                                                              |
|    Val.                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      30 Ty.          METHOD                                                                   |
|Name  CL_ABAP_COMPILER=>SCAN_PROGRAM                                                              |
----------------------------------------------------------------------------------------------------
|SYNT_ID-GLOBDATA                                                                                  |
|    GLDT                                                                                          |
|    4445                                                                                          |
|    7C44                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    47004C0044005400                                                                              |
|ME->GLOBAL_DATAS                                                                                  |
|    Table IT_3433[0x12]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=GLOBAL_DATAS                                             |
|    Table reference: 2593                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000030020000                                        |
|    TABH+40(20) = 00000000210A0000690D0000000000000C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200303D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 4     (ItAccHashed)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2593  (0x210A0000)                                                  |
|    label                   = 3433  (0x690D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 12    (0x0C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000087                                             |
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
|SYNT_ID-FORMS                                                                                     |
|    FORM                                                                                          |
|    4454                                                                                          |
|    6F2D                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    46004F0052004D00                                                                              |
|ME->FORMS                                                                                         |
|    Table IT_3434[0x12]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=FORMS                                                    |
|    Table reference: 2594                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000030020000                                        |
|    TABH+40(20) = 00000000220A00006A0D0000000000000C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200303D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 4     (ItAccHashed)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2594  (0x220A0000)                                                  |
|    label                   = 3434  (0x6A0D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 12    (0x0C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000087                                             |
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
|SYNT_ID-FUNCTIONS                                                                                 |
|    FUNC                                                                                          |
|    4544                                                                                          |
|    65E3                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    460055004E004300                                                                              |
|ME->FUNCTIONS                                                                                     |
|    Table IT_3435[0x12]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=FUNCTIONS                                                |
|    Table reference: 2595                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000030020000                                        |
|    TABH+40(20) = 00000000230A00006B0D0000000000000C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200303D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 4     (ItAccHashed)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2595  (0x230A0000)                                                  |
|    label                   = 3435  (0x6B0D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 12    (0x0C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000087                                             |
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
|SYNT_ID-CLASSES                                                                                   |
|    CLAS                                                                                          |
|    4445                                                                                          |
|    3C13                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    43004C0041005300                                                                              |
|ME->CLASSES                                                                                       |
|    Table IT_3436[0x12]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=CLASSES                                                  |
|    Table reference: 2596                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000030020000                                        |
|    TABH+40(20) = 00000000240A00006C0D0000000000000C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200303D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 4     (ItAccHashed)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2596  (0x240A0000)                                                  |
|    label                   = 3436  (0x6C0D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 12    (0x0C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000087                                             |
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
|SYNT_ID-GLOBCLASSES                                                                               |
|    GLCL                                                                                          |
|    4444                                                                                          |
|    7C3C                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    47004C0043004C00                                                                              |
|ME->GLOBAL_CLASSES                                                                                |
|    Table IT_3437[0x12]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=GLOBAL_CLASSES                                           |
|    Table reference: 2597                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000030020000                                        |
|    TABH+40(20) = 00000000250A00006D0D0000000000000C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200303D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 4     (ItAccHashed)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2597  (0x250A0000)                                                  |
|    label                   = 3437  (0x6D0D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 12    (0x0C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000087                                             |
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
|SYNT_ID-INTERFACES                                                                                |
|    INTF                                                                                          |
|    4454                                                                                          |
|    9E46                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    49004E0054004600                                                                              |
|ME->INTFS                                                                                         |
|    Table IT_3438[0x12]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=INTFS                                                    |
|    Table reference: 2598                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000030020000                                        |
|    TABH+40(20) = 00000000260A00006E0D0000000000000C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200303D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 4     (ItAccHashed)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2598  (0x260A0000)                                                  |
|    label                   = 3438  (0x6E0D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 12    (0x0C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000087                                             |
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
|SYNT_ID-GLOBINTF                                                                                  |
|    GLIF                                                                                          |
|    4444                                                                                          |
|    7C96                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    47004C0049004600                                                                              |
|ME->GLOBAL_INTFS                                                                                  |
|    Table IT_3439[0x12]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=GLOBAL_INTFS                                             |
|    Table reference: 2599                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000030020000                                        |
|    TABH+40(20) = 00000000270A00006F0D0000000000000C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200303D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 4     (ItAccHashed)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2599  (0x270A0000)                                                  |
|    label                   = 3439  (0x6F0D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 12    (0x0C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000087                                             |
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
|SYNT_ID-TYPEPOOLS                                                                                 |
|    TYPO                                                                                          |
|    5554                                                                                          |
|    490F                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    5400590050004F00                                                                              |
|ME->TYPEPOOLS                                                                                     |
|    Table IT_3440[0x12]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=TYPEPOOLS                                                |
|    Table reference: 2600                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000030020000                                        |
|    TABH+40(20) = 00000000280A0000700D0000000000000C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200303D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 4     (ItAccHashed)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2600  (0x280A0000)                                                  |
|    label                   = 3440  (0x700D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 12    (0x0C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000087                                             |
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
|SYNT_ID-PCOMMENT                                                                                  |
|    PCOM                                                                                          |
|    5444                                                                                          |
|    03FD                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    500043004F004D00                                                                              |
|ME->PCOMMENTS                                                                                     |
|    Table IT_3441[0x96]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=PCOMMENTS                                                |
|    Table reference: 2601                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044040000                                        |
|    TABH+40(20) = 00000000290A0000710D00000000000060000000                                        |
|    TABH+60(20) = FFFFFFFF04190200104524000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 2     (table_line)                                                  |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2601  (0x290A0000)                                                  |
|    label                   = 3441  (0x710D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 96    (0x60000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000129                                             |
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
|SYNT_ID-COMMENT                                                                                   |
|    SCOM                                                                                          |
|    5444                                                                                          |
|    33FD                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    530043004F004D00                                                                              |
|ME->COMMENTS                                                                                      |
|    Table IT_3442[0x108]                                                                          |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=COMMENTS                                                 |
|    Table reference: 2602                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044010000                                        |
|    TABH+40(20) = 000000002A0A0000720D0000000000006C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200903D24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 2     (table_line)                                                  |
|      cmpMode               = 2     (cmpSingleMcmpR)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2602  (0x2A0A0000)                                                  |
|    label                   = 3442  (0x720D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 108   (0x6C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000089                                             |
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
|ME->USAGES                                                                                        |
|    Table IT_3443[0x16]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=USAGES                                                   |
|    Table reference: 2603                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044040000                                        |
|    TABH+40(20) = 000000002B0A0000730D00000000000010000000                                        |
|    TABH+60(20) = FFFFFFFF04190200F04C24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2603  (0x2B0A0000)                                                  |
|    label                   = 3443  (0x730D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 16    (0x10000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000171                                             |
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
|ME->OBJECTS                                                                                       |
|    Table IT_3444[0x368]                                                                          |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=OBJECTS                                                  |
|    Table reference: 2604                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044010000                                        |
|    TABH+40(20) = 000000002C0A0000740D00000000000070010000                                        |
|    TABH+60(20) = FFFFFFFF04190200204424000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
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
|    id                      = 2604  (0x2C0A0000)                                                  |
|    label                   = 3444  (0x740D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 368   (0x70010000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000124                                             |
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
|L_ID_OBJ_USE_1                                                                                    |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|L_DUMMY                                                                                           |
|    Table IT_3459[0x4]                                                                            |
|    \CLASS=CL_ABAP_COMPILER\METHOD=SCAN_PROGRAM\DATA=L_DUMMY                                      |
|    Table reference: 2613                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044010000                                        |
|    TABH+40(20) = 00000000350A0000830D00000000000004000000                                        |
|    TABH+60(20) = FFFFFFFF04190200306A24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
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
|    id                      = 2613  (0x350A0000)                                                  |
|    label                   = 3459  (0x830D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 4     (0x04000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000327                                             |
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
|ME->ID_LONGTEXT                                                                                   |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ME->LONGTEXT                                                                                      |
|    Table IT_3445[0x36]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=LONGTEXT                                                 |
|    Table reference: 2605                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044040000                                        |
|    TABH+40(20) = 000000002D0A0000750D00000000000024000000                                        |
|    TABH+60(20) = FFFFFFFF04190200604324000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2605  (0x2D0A0000)                                                  |
|    label                   = 3445  (0x750D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 36    (0x24000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000120                                             |
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
|ME->ID_INFOS                                                                                      |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ME->INFOS                                                                                         |
|    Table IT_3446[0x632]                                                                          |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=INFOS                                                    |
|    Table reference: 2606                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044040000                                        |
|    TABH+40(20) = 000000002E0A0000760D00000000000078020000                                        |
|    TABH+60(20) = FFFFFFFF04190200F04024000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2606  (0x2E0A0000)                                                  |
|    label                   = 3446  (0x760D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 632   (0x78020000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000107                                             |
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
|ME->ID_WARNINGS                                                                                   |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ME->WARNINGS                                                                                      |
|    Table IT_3447[0x632]                                                                          |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=WARNINGS                                                 |
|    Table reference: 2607                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044040000                                        |
|    TABH+40(20) = 000000002F0A0000770D00000000000078020000                                        |
|    TABH+60(20) = FFFFFFFF04190200F04024000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2607  (0x2F0A0000)                                                  |
|    label                   = 3447  (0x770D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 632   (0x78020000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000107                                             |
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
|ME->ID_ENHANCEMENT_IMPLS                                                                          |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ME->ENHANCEMENT_IMPLS                                                                             |
|    Table IT_3448[0x60]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=ENHANCEMENT_IMPLS                                        |
|    Table reference: 2608                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044010000                                        |
|    TABH+40(20) = 00000000300A0000780D0000000000003C000000                                        |
|    TABH+60(20) = FFFFFFFF04190200703F24000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
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
|    id                      = 2608  (0x300A0000)                                                  |
|    label                   = 3448  (0x780D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 60    (0x3C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000099                                             |
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
|%_%_ACTN                                                                                          |
|    Table IT_1416[0x20]                                                                           |
|    \CLASS-POOL=CL_ABAP_COMPILER\DATA=%_%_ACTN                                                    |
|    Table reference: 587                                                                          |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 00000000000000000000000000000000A8010000                                        |
|    TABH+40(20) = 000000004B020000880500000000000014000000                                        |
|    TABH+60(20) = FFFFFFFF04190200C06724000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 2     (ItAccSorted)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 3     (cmpSingleMcmpU)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 587   (0x4B020000)                                                  |
|    label                   = 1416  (0x88050000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 20    (0x14000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000314                                             |
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
|%_%_RDIR                                                                                          |
|    Table IT_1417[0x82]                                                                           |
|    \CLASS-POOL=CL_ABAP_COMPILER\DATA=%_%_RDIR                                                    |
|    Table reference: 588                                                                          |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044010000                                        |
|    TABH+40(20) = 000000004C020000890500000000000052000000                                        |
|    TABH+60(20) = FFFFFFFF04190200206824000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
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
|    id                      = 588   (0x4C020000)                                                  |
|    label                   = 1417  (0x89050000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 82    (0x52000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000316                                             |
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
|%_%_BDLO                                                                                          |
|    Table IT_1418[0x152]                                                                          |
|    \CLASS-POOL=CL_ABAP_COMPILER\DATA=%_%_BDLO                                                    |
|    Table reference: 589                                                                          |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044040000                                        |
|    TABH+40(20) = 000000004D0200008A0500000000000098000000                                        |
|    TABH+60(20) = FFFFFFFF04190200806824000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 589   (0x4D020000)                                                  |
|    label                   = 1418  (0x8A050000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 152   (0x98000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000318                                             |
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
|SYST                                                                                              |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
|SY-SUBRC                                                                                          |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
|SYST-REPID                                                                                        |
|    CL_ABAP_COMPILER==============CP                                                              |
|    4454445544454445333333333333334522222222                                                      |
|    3CF1210F3FD09C52DDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0041004200410050005F0043004F004D00500049004C00450052003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
----------------------------------------------------------------------------------------------------
|No.      29 Ty.          METHOD                                                                   |
|Name  CL_ABAP_COMPILER=>SCAN                                                                      |
----------------------------------------------------------------------------------------------------
|ME->SCAN_GRADES                                                                                   |
|    Table[initial]                                                                                |
|                                                                                                  |
|ME->GRADES                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|ME->INCLUDE_NAME                                                                                  |
|                                                                                                  |
|    2222222222222222222222222222222222222222                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|L_INCLUDE_CHECKSUM                                                                                |
|    {0;0}                                                                                         |
|    00000000                                                                                      |
|    00000000                                                                                      |
|    0000000000000000                                                                              |
|SCREEN                                                                                            |
|    {SOURCE;;;;;0;1;0;0;0;00;1;0;0;0;0;0}                                                         |
|    504050504040202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    30F050203050000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|ME                                                                                                |
|    {O:259*\CLASS=CL_ABAP_COMPILER}                                                               |
|    F0000000                                                                                      |
|    C0003100                                                                                      |
|    FC00000003010000                                                                              |
|ME->REF_SOURCE                                                                                    |
|    {A:initial}                                                                                   |
|    C0000000                                                                                      |
|    F0000000                                                                                      |
|    CF00000000000000                                                                              |
|ME->REF_TRDIR                                                                                     |
|    {A:initial}                                                                                   |
|    C0000000                                                                                      |
|    F0000000                                                                                      |
|    CF00000000000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|ME->INCLUDE_CHECKSUM                                                                              |
|    {0;0}                                                                                         |
|    00000000                                                                                      |
|    00000000                                                                                      |
|    0000000000000000                                                                              |
|ME->IS_FOR_PROGRAM                                                                                |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|ME->PROGRAM                                                                                       |
|    SAPL/PLCP/BGPROCESSING                                                                        |
|    5454254452445544455444222222222222222222                                                      |
|    310CF0C30F2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C002F0050004C00430050002F0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|L_TRDIR                                                                                           |
|    {;;;;;;;;;;;;;00000000;;00000000;;;;;;;;00000000;;00000000;;;}                                |
|    202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202030303030303030302020202020202020202020\|
|20303030303030303020202020202020202020202020202020203030303030303                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200030003000300030003000300030003000200020002000200020002000200020002000200020002000\|
|3000300030003000300030003000300020002000200020002000200020002000200020002000200020002000200020002\|
|00030003000300030003000300030                                                                     |
|L_TRDIR-SUBC                                                                                      |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      28 Ty.          METHOD                                                                   |
|Name  CL_ABAP_COMPILER=>GET_SINGLE_REF                                                            |
----------------------------------------------------------------------------------------------------
|P_FULL_NAME                                                                                       |
|    \FU:/PLCP/BGPROCESSUNIT\DA:BAPIRET                                                            |
|    5453254452445544455544554434454545                                                            |
|    C65AF0C30F2702F35335E94C41A2109254                                                            |
|    0000000000000000000000000000000000                                                            |
|    0000000000000000000000000000000000                                                            |
|    5C00460055003A002F0050004C00430050002F0042004700500052004F00430045005300530055004E00490054005\|
|C00440041003A004200410050004900520045005400                                                       |
|P_GRADE                                                                                           |
|    0                                                                                             |
|    3                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    3000                                                                                          |
|P_ONLY_FIRST                                                                                      |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|P_FULL_NAMES                                                                                      |
|    Table[initial]                                                                                |
|                                                                                                  |
|P_EXTENDED                                                                                        |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|P_RESULT                                                                                          |
|    Table[initial]                                                                                |
|                                                                                                  |
|P_ERRORS                                                                                          |
|    Table[initial]                                                                                |
|                                                                                                  |
|P_ERROR                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|P_ABORT                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|P_INDEX_FULL_NAME                                                                                 |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|SY                                                                                                |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
|ME->DTEL_OBJ_STMNT                                                                                |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|ME->ID_ERRORS                                                                                     |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SYNT_ID-ERR_MSG                                                                                   |
|    ERR                                                                                           |
|    4552                                                                                          |
|    5220                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    4500520052002000                                                                              |
|ME                                                                                                |
|    {O:259*\CLASS=CL_ABAP_COMPILER}                                                               |
|    F0000000                                                                                      |
|    C0003100                                                                                      |
|    FC00000003010000                                                                              |
|ME->SUBRC                                                                                         |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
|%_FSREG_001                                                                                       |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|ME->TYPES                                                                                         |
|    Table IT_3418[0x64]                                                                           |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=TYPES                                                    |
|    Table reference: 2578                                                                         |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044040000                                        |
|    TABH+40(20) = 00000000120A00005A0D00000000000040000000                                        |
|    TABH+60(20) = FFFFFFFF04190200904924000400010602000000                                        |
|    store                   = 0x0000000000000000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2578  (0x120A0000)                                                  |
|    label                   = 3418  (0x5A0D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 64    (0x40000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000153                                             |
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
|ME->ERRORS                                                                                        |
|    Table IT_3417[0x632]                                                                          |
|    {O:259*\CLASS=CL_ABAP_COMPILER}\DATA=ERRORS                                                   |
|    Table reference: 2576                                                                         |
|    TABH+ 0(20) = 102DB3D3067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044040000                                        |
|    TABH+40(20) = 00000000100A0000590D00000000000078020000                                        |
|    TABH+60(20) = FFFFFFFF04190200F04024000400010602000000                                        |
|    store                   = 0x102DB3D3067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2576  (0x100A0000)                                                  |
|    label                   = 3417  (0x590D0000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 632   (0x78020000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000537/TYPE#000107                                             |
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
|    tabi                    = 0xE80FBDD3067F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 1788  (0xFC060000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
----------------------------------------------------------------------------------------------------
|No.      27 Ty.          METHOD                                                                   |
|Name  CL_SEDI_ABAP_COMPILER=>GET_SINGLE_REF                                                       |
----------------------------------------------------------------------------------------------------
|I_FULLNAME                                                                                        |
|    \FU:/PLCP/BGPROCESSUNIT\DA:BAPIRET                                                            |
|    5453254452445544455544554434454545                                                            |
|    C65AF0C30F2702F35335E94C41A2109254                                                            |
|    0000000000000000000000000000000000                                                            |
|    0000000000000000000000000000000000                                                            |
|    5C00460055003A002F0050004C00430050002F0042004700500052004F00430045005300530055004E00490054005\|
|C00440041003A004200410050004900520045005400                                                       |
|I_GRADE                                                                                           |
|    0                                                                                             |
|    3                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    3000                                                                                          |
|E_SINGLE_REFS                                                                                     |
|    Table[initial]                                                                                |
|                                                                                                  |
|SY-SUBRC                                                                                          |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
|SINGLE_REF-SINGLE_REFS                                                                            |
|    Table[initial]                                                                                |
|                                                                                                  |
|SINGLE_REF-FULLNAME                                                                               |
|    \FU:/PLCP/BGPROCESSUNIT\DA:BAPIRET                                                            |
|    5453254452445544455544554434454545                                                            |
|    C65AF0C30F2702F35335E94C41A2109254                                                            |
|    0000000000000000000000000000000000                                                            |
|    0000000000000000000000000000000000                                                            |
|    5C00460055003A002F0050004C00430050002F0042004700500052004F00430045005300530055004E00490054005\|
|C00440041003A004200410050004900520045005400                                                       |
|SY                                                                                                |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
|ME->ABAP_COMPILER                                                                                 |
|    {O:259*\CLASS=CL_ABAP_COMPILER}                                                               |
|    F0000000                                                                                      |
|    C0003100                                                                                      |
|    FC00000003010000                                                                              |
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
|ABAP_TRUE                                                                                         |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SYST-REPID                                                                                        |
|    CL_SEDI_ABAP_COMPILER=========CP                                                              |
|    4455444544455444544453333333334522222222                                                      |
|    3CF3549F1210F3FD09C52DDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0053004500440049005F0041004200410050005F0043004F004D00500049004C00450052003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SY-REPID                                                                                          |
|    CL_SEDI_ABAP_COMPILER=========CP                                                              |
|    4455444544455444544453333333334522222222                                                      |
|    3CF3549F1210F3FD09C52DDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0053004500440049005F0041004200410050005F0043004F004D00500049004C00450052003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ME                                                                                                |
|    {O:255*\CLASS=CL_SEDI_ABAP_COMPILER}                                                          |
|    0000F000                                                                                      |
|    0000F000                                                                                      |
|    00000000FF000000                                                                              |
----------------------------------------------------------------------------------------------------
|No.      26 Ty.          FORM                                                                     |
|Name  COMPILER_COMPONENT_NAVIGATION                                                               |
----------------------------------------------------------------------------------------------------
|SYST-REPID                                                                                        |
|    SAPLSEDA                                                                                      |
|    5454544422222222222222222222222222222222                                                      |
|    310C354100000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C0053004500440041002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|CO_TYPE_GROUP_PREFIX                                                                              |
|    %_C                                                                                           |
|    254                                                                                           |
|    5F3                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    25005F004300                                                                                  |
|P_PROGRAM                                                                                         |
|    SAPL/PLCP/BGPROCESSING                                                                        |
|    5454254452445544455444222222222222222222                                                      |
|    310CF0C30F2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C002F0050004C00430050002F0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|OLD_PROGRAM                                                                                       |
|    SAPL/PLCP/BGPROCESSING                                                                        |
|    5454254452445544455444222222222222222222                                                      |
|    310CF0C30F2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C002F0050004C00430050002F0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|COMPILER                                                                                          |
|    {O:255*\CLASS=CL_SEDI_ABAP_COMPILER}                                                          |
|    0000F000                                                                                      |
|    0000F000                                                                                      |
|    00000000FF000000                                                                              |
|SY                                                                                                |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|I_INCL                                                                                            |
|    /PLCP/LBGPROCESSINGU01                                                                        |
|    2544524445544455444533222222222222222222                                                      |
|    F0C30FC2702F35339E7501000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F004C0042004700500052004F00430045005300530049004E00470055003000310020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|I_SOURCE[]                                                                                        |
|    Table IT_1199[89x8]                                                                           |
|    {O:243*\CLASS=CL_WB_SOURCE_NAVIGATOR}\DATA=CTXT_SOURCE                                        |
|    Table reference: 521                                                                          |
|    TABH+ 0(20) = E02C52D7067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000005900000044020000                                        |
|    TABH+40(20) = 0000000009020000AF0400005900000008000000                                        |
|    TABH+60(20) = FFFFFFFF04E10100104B02000400410602000000                                        |
|    store                   = 0xE02C52D7067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 521   (0x09020000)                                                  |
|    label                   = 1199  (0xAF040000)                                                  |
|    fill                    = 89    (0x59000000)                                                  |
|    leng                    = 8     (0x08000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000481/TYPE#000018                                             |
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
|    tabi                    = 0xA82C52D7067F0000                                                  |
|    pgHook                  = 0x184C52D7067F0000                                                  |
|    uniqueNumber            = 430   (0xAE010000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 16    (0x10000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 1020  (0xFC030000)                                                  |
|    lineAlloc               = 124   (0x7C000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 17    (0x11000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|                                                                                                  |
|    function /PLCP/BGPROCESSUNIT.                                                                 |
|    67667666225445244554445554452                                                                 |
|    65E349FE0F0C30F2702F35335E94E                                                                 |
|    00000000000000000000000000000                                                                 |
|    00000000000000000000000000000                                                                 |
|    660075006E006300740069006F006E0020002F0050004C00430050002F0042004700500052004F004300450053005\|
|30055004E00490054002E00                                                                           |
|                                                                                                  |
|    *"----------------------------------------------------------------------                      |
|    222222222222222222222222222222222222222222222222222222222222222222222222                      |
|    A2DDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDD                      |
|    000000000000000000000000000000000000000000000000000000000000000000000000                      |
|    000000000000000000000000000000000000000000000000000000000000000000000000                      |
|    2A0022002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002\|
|D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D\|
|002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D0\|
|0                                                                                                 |
|                                                                                                  |
|    *"*"Local Interface:                                                                          |
|    22224666624676766663                                                                          |
|    A2A2CF31C09E4526135A                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    2A0022002A0022004C006F00630061006C00200049006E0074006500720066006100630065003A00              |
|                                                                                                  |
|    *"  IMPORTING                                                                                 |
|    2222445455444                                                                                 |
|    A2009D0F249E7                                                                                 |
|    0000000000000                                                                                 |
|    0000000000000                                                                                 |
|    2A0022002000200049004D0050004F005200540049004E004700                                          |
|                                                                                                  |
|    *"     VALUE(I_BGPUNIT) TYPE  /PLCE/UUID                                                      |
|    2222222544542454455445225554222544425544                                                      |
|    A20000061C5589F2705E9490490500F0C35F5594                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2A00220020002000200020002000560041004C0055004500280049005F0042004700500055004E004900540029002\|
|0005400590050004500200020002F0050004C00430045002F005500550049004400                               |
|G_ENH_HANDLER                                                                                     |
|    {O:26*\CLASS=CL_WB_ED_ENHANCEMENT_HANDLER}                                                    |
|    E0001000                                                                                      |
|    5000A000                                                                                      |
|    E50000001A000000                                                                              |
|P_FULLNAME                                                                                        |
|    \FU:/PLCP/BGPROCESSUNIT\DA:BAPIRET                                                            |
|    5453254452445544455544554434454545                                                            |
|    C65AF0C30F2702F35335E94C41A2109254                                                            |
|    0000000000000000000000000000000000                                                            |
|    0000000000000000000000000000000000                                                            |
|    5C00460055003A002F0050004C00430050002F0042004700500052004F00430045005300530055004E00490054005\|
|C00440041003A004200410050004900520045005400                                                       |
|SINGLE_REFS                                                                                       |
|    Table[initial]                                                                                |
|                                                                                                  |
|SCREEN                                                                                            |
|    {SOURCE;;;;;0;1;0;0;0;00;1;0;0;0;0;0}                                                         |
|    504050504040202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    30F050203050000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SINGLE_REF                                                                                        |
|    {{O:initial};{O:initial};;;0;0;;0;;;0;}                                                       |
|    F0000000F00000001000000010000000000000002020302020002000                                      |
|    F0000000F00000004000000040000000000000000000000000000000                                      |
|    FF00000000000000FF000000000000001400000000000000140000000000000000000000000000002000200030002\|
|0002000000020000000                                                                               |
|SY-REPID                                                                                          |
|    SAPLSEDA                                                                                      |
|    5454544422222222222222222222222222222222                                                      |
|    310C354100000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C0053004500440041002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|SANA_TOK_ENTITY_ACTION                                                                            |
|    <                                                                                             |
|    3                                                                                             |
|    C                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    3C00                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      25 Ty.          FUNCTION                                                                 |
|Name  RS_NAVIGATION_PREPARE_NEW                                                                   |
----------------------------------------------------------------------------------------------------
|ERROR_NAVIGATION                                                                                  |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|G_ENH_HANDLER                                                                                     |
|    {O:26*\CLASS=CL_WB_ED_ENHANCEMENT_HANDLER}                                                    |
|    E0001000                                                                                      |
|    5000A000                                                                                      |
|    E50000001A000000                                                                              |
|I_CHANGED                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|I_COL                                                                                             |
|    65                                                                                            |
|    4000                                                                                          |
|    1000                                                                                          |
|    41000000                                                                                      |
|I_CONTEXT                                                                                         |
|    {O:242*\CLASS=CL_WB_CONTEXT}                                                                  |
|    0000F000                                                                                      |
|    D0002000                                                                                      |
|    0D000000F2000000                                                                              |
|I_EDITOR                                                                                          |
|    EDTR                                                                                          |
|    4455                                                                                          |
|    5442                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    4500440054005200                                                                              |
|I_INCL                                                                                            |
|    /PLCP/LBGPROCESSINGU01                                                                        |
|    2544524445544455444533222222222222222222                                                      |
|    F0C30FC2702F35339E7501000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F004C0042004700500052004F00430045005300530049004E00470055003000310020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|I_MAINPROGRAM                                                                                     |
|    /PLCP/SAPLBGPROCESSING                                                                        |
|    2544525454445544455444222222222222222222                                                      |
|    F0C30F310C2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F005300410050004C0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|I_OPERATION                                                                                       |
|    DEFINITION                                                                                    |
|    4444445444                                                                                    |
|    4569E949FE                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004500460049004E004900540049004F004E00                                                      |
|I_ROW                                                                                             |
|    47                                                                                            |
|    2000                                                                                          |
|    F000                                                                                          |
|    2F000000                                                                                      |
|I_SEARCH_LOCAL                                                                                    |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|I_TOKEN_STRING                                                                                    |
|    BAPIRET-MESSAGE_V1                                                                            |
|    445454524455444553222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    2109254DD533175F61000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    42004100500049005200450054002D004D004500530053004100470045005F0056003100200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|NO_DIALOG                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|P_ADT_POPUP                                                                                       |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|O_CLASS                                                                                           |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222222222222222                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000                                        |
|O_CLASSTYPE                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|O_COL                                                                                             |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|O_DYNNR                                                                                           |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|O_EVENT                                                                                           |
|                                                                                                  |
|    222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|O_EVENTTYPE                                                                                       |
|                                                                                                  |
|    22                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    20002000                                                                                      |
|O_FULLNAME                                                                                        |
|    \FU:/PLCP/BGPROCESSUNIT\DA:BAPIRET\DA:MESSAGE_V1                                              |
|    545325445244554445554455443445454554434455444553                                              |
|    C65AF0C30F2702F35335E94C41A2109254C41AD533175F61                                              |
|    000000000000000000000000000000000000000000000000                                              |
|    000000000000000000000000000000000000000000000000                                              |
|    5C00460055003A002F0050004C00430050002F0042004700500052004F00430045005300530055004E00490054005\|
|C00440041003A0042004100500049005200450054005C00440041003A004D004500530053004100470045005F00560031\|
|00                                                                                                |
|O_INCL                                                                                            |
|                                                                                                  |
|    2222222222222222222222222222222222222222                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|O_LOCAL                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|O_OBJECTNAME                                                                                      |
|                                                                                                  |
|    222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|O_OBJECTTYPE                                                                                      |
|                                                                                                  |
|    22                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    20002000                                                                                      |
|O_OPERATION                                                                                       |
|                                                                                                  |
|    2222222222                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    2000200020002000200020002000200020002000                                                      |
|O_PROGRAM                                                                                         |
|    SAPL/PLCP/BGPROCESSING                                                                        |
|    5454254452445544455444222222222222222222                                                      |
|    310CF0C30F2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C002F0050004C00430050002F0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|O_ROW                                                                                             |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|O_STATE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|I_SOURCE[]                                                                                        |
|    Table IT_1199[89x8]                                                                           |
|FULLNAME                                                                                          |
|    \FU:/PLCP/BGPROCESSUNIT\DA:BAPIRET                                                            |
|    5453254452445544455544554434454545                                                            |
|    C65AF0C30F2702F35335E94C41A2109254                                                            |
|    0000000000000000000000000000000000                                                            |
|    0000000000000000000000000000000000                                                            |
|    5C00460055003A002F0050004C00430050002F0042004700500052004F00430045005300530055004E00490054005\|
|C00440041003A004200410050004900520045005400                                                       |
|COMPONENT                                                                                         |
|    MESSAGE_V1                                                                                    |
|    4455444553                                                                                    |
|    D533175F61                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    4D004500530053004100470045005F0056003100                                                      |
|USE_SYMBOL_FOR_CMP_NAVIGATION                                                                     |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SOURCE                                                                                            |
|    <empty string>                                                                                |
|    10000000                                                                                      |
|    40000000                                                                                      |
|    1400000000000000                                                                              |
|G_TO_OBJECT_NAME                                                                                  |
|    <empty string>                                                                                |
|    10000000                                                                                      |
|    40000000                                                                                      |
|    1400000000000000                                                                              |
|T_CLASS                                                                                           |
|    {;0;;;00000000000000000000000000000000;00;0;0;0;;00000000;;00000000;;00000000;;;;;;;;;;;;;;;;\|
|00;;;;;;;0;0;}                                                                                    |
|    202020202020202020202020202020202020202020202020202020202020302020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020000000\|
|00000000003030303030202020202020202020202020303030303030303020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020003000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020000000000000000000\|
|0000000000000000300030003000300030002000200020002000200020002000200020002000200020003000300030003\|
|00030003000300030002000200020                                                                     |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|FULLNAME_TYPE_TABLE[]                                                                             |
|    Table IT_3401[2x16]                                                                           |
|    \FUNCTION=RS_NAVIGATION_PREPARE_NEW\DATA=FULLNAME_TYPE_TABLE[]                                |
|    Table reference: 2571                                                                         |
|    TABH+ 0(20) = 302EB3D3067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000200000044040000                                        |
|    TABH+40(20) = 000000000B0A0000490D00000200000010000000                                        |
|    TABH+60(20) = FFFFFFFF04E6010030F306000400010602000000                                        |
|    store                   = 0x302EB3D3067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 2571  (0x0B0A0000)                                                  |
|    label                   = 3401  (0x490D0000)                                                  |
|    fill                    = 2     (0x02000000)                                                  |
|    leng                    = 16    (0x10000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000486/TYPE#000191                                             |
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
|    tabi                    = 0xD82DB3D3067F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 1783  (0xF7060000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|                                                                                                  |
|    {FU;/PLCP/BGPROCESSUNIT}                                                                      |
|    100090001000F008                                                                              |
|    4000C50040008500                                                                              |
|    140000009C05000014000000F8050080                                                              |
|                                                                                                  |
|    {DA;BAPIRET}                                                                                  |
|    1000900010009000                                                                              |
|    4000B5004000A500                                                                              |
|    140000009B050000140000009A050000                                                              |
|REF_FOUND                                                                                         |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SY-XPROG                                                                                          |
|    SAPMSSY1                                                                                      |
|    5454555322222222222222222222222222222222                                                      |
|    310D339100000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004D0053005300590031002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|CL_WB_SOURCE_NAVIGATOR=>C_OPERATION_DEFINITION                                                    |
|    DEFINITION                                                                                    |
|    4444445444                                                                                    |
|    4569E949FE                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004500460049004E004900540049004F004E00                                                      |
|SWBM_C_TYPE_PRG_INCLUDE                                                                           |
|    I                                                                                             |
|    422                                                                                           |
|    900                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    490020002000                                                                                  |
|SANA_TOK_TYPE                                                                                     |
|    l                                                                                             |
|    6                                                                                             |
|    C                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    6C00                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      24 Ty.          FORM                                                                     |
|Name  CALL_ABAP_COMPILER                                                                          |
----------------------------------------------------------------------------------------------------
|SANA_TOK_MACRO                                                                                    |
|    k                                                                                             |
|    6                                                                                             |
|    B                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    6B00                                                                                          |
|SY-TABIX                                                                                          |
|    1                                                                                             |
|    0000                                                                                          |
|    1000                                                                                          |
|    01000000                                                                                      |
|SAVE_CLASS                                                                                        |
|                                                                                                  |
|    2222222222222222222222222222222222222222                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|O_CLASS                                                                                           |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222222222222222                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000                                        |
|SAVE_CLASS_TYPE                                                                                   |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|O_CLASSTYPE                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SAVE_EVENTNAME                                                                                    |
|    /PLCP/BGPROCESSUNIT                                                                           |
|    25445244554445554452222222222222222222222222222222222222222222                                |
|    F0C30F2702F35335E940000000000000000000000000000000000000000000                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000                                        |
|O_EVENT                                                                                           |
|    /PLCP/BGPROCESSUNIT                                                                           |
|    254452445544455544522222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    F0C30F2702F35335E9400000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|SAVE_EVENTTYPE                                                                                    |
|    F                                                                                             |
|    4                                                                                             |
|    6                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    4600                                                                                          |
|O_EVENTTYPE                                                                                       |
|    F                                                                                             |
|    42                                                                                            |
|    60                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    46002000                                                                                      |
|CO_EVENTTYPE_CLASS                                                                                |
|    L                                                                                             |
|    4                                                                                             |
|    C                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    4C00                                                                                          |
|I_CHANGED                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
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
|I_COL                                                                                             |
|    65                                                                                            |
|    4000                                                                                          |
|    1000                                                                                          |
|    41000000                                                                                      |
|CO_EVENTTYPE_END                                                                                  |
|    I                                                                                             |
|    4                                                                                             |
|    9                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    4900                                                                                          |
|I_CONTEXT                                                                                         |
|    {O:242*\CLASS=CL_WB_CONTEXT}                                                                  |
|    0000F000                                                                                      |
|    D0002000                                                                                      |
|    0D000000F2000000                                                                              |
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
|I_EDITOR                                                                                          |
|    EDTR                                                                                          |
|    4455                                                                                          |
|    5442                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    4500440054005200                                                                              |
|I_MAINPROGRAM                                                                                     |
|    /PLCP/SAPLBGPROCESSING                                                                        |
|    2544525454445544455444222222222222222222                                                      |
|    F0C30F310C2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F005300410050004C0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|SY-XFORM                                                                                          |
|    XAB_WFLUSH                                                                                    |
|    544554455422222222222222222222                                                                |
|    812F76C53800000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    5800410042005F00570046004C0055005300480020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|I_OPERATION                                                                                       |
|    DEFINITION                                                                                    |
|    4444445444                                                                                    |
|    4569E949FE                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004500460049004E004900540049004F004E00                                                      |
|I_ROW                                                                                             |
|    47                                                                                            |
|    2000                                                                                          |
|    F000                                                                                          |
|    2F000000                                                                                      |
|SOURCE1                                                                                           |
|              <unit_update>-SystemMessageVariable1 = bapiret-message_v1.                          |
|    22222222223766757766763257776646776665676666632326676767266776665732                          |
|    0000000000C5E94F504145ED39345DD533175612912C510D02109254DD533175F61E                          |
|    00000000000000000000000000000000000000000000000000000000000000000000                          |
|    00000000000000000000000000000000000000000000000000000000000000000000                          |
|    20002000200020002000200020002000200020003C0075006E00690074005F007500700064006100740065003E002\|
|D00530079007300740065006D004D006500730073006100670065005600610072006900610062006C006500310020003D\|
|00200062006100700069007200650074002D006D006500730073006100670065005F00760031002E00                |
|ABAP_TRUE                                                                                         |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SOURCE1[]                                                                                         |
|    Table IT_1205[89x8]                                                                           |
|    \FUNCTION-POOL=SEDA\DATA=SOURCE1[]                                                            |
|    Table reference: 525                                                                          |
|    TABH+ 0(20) = E02C52D7067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000005900000044020000                                        |
|    TABH+40(20) = 000000000D020000B50400005900000008000000                                        |
|    TABH+60(20) = FFFFFFFF04E6010040D106000400410602000000                                        |
|    store                   = 0xE02C52D7067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 525   (0x0D020000)                                                  |
|    label                   = 1205  (0xB5040000)                                                  |
|    fill                    = 89    (0x59000000)                                                  |
|    leng                    = 8     (0x08000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000486/TYPE#000010                                             |
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
|    tabi                    = 0xA82C52D7067F0000                                                  |
|    pgHook                  = 0x184C52D7067F0000                                                  |
|    uniqueNumber            = 430   (0xAE010000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 16    (0x10000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 1020  (0xFC030000)                                                  |
|    lineAlloc               = 124   (0x7C000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 17    (0x11000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|                                                                                                  |
|    function /PLCP/BGPROCESSUNIT.                                                                 |
|    67667666225445244554445554452                                                                 |
|    65E349FE0F0C30F2702F35335E94E                                                                 |
|    00000000000000000000000000000                                                                 |
|    00000000000000000000000000000                                                                 |
|    660075006E006300740069006F006E0020002F0050004C00430050002F0042004700500052004F004300450053005\|
|30055004E00490054002E00                                                                           |
|                                                                                                  |
|    *"----------------------------------------------------------------------                      |
|    222222222222222222222222222222222222222222222222222222222222222222222222                      |
|    A2DDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDD                      |
|    000000000000000000000000000000000000000000000000000000000000000000000000                      |
|    000000000000000000000000000000000000000000000000000000000000000000000000                      |
|    2A0022002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002\|
|D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D\|
|002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D002D0\|
|0                                                                                                 |
|                                                                                                  |
|    *"*"Local Interface:                                                                          |
|    22224666624676766663                                                                          |
|    A2A2CF31C09E4526135A                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    2A0022002A0022004C006F00630061006C00200049006E0074006500720066006100630065003A00              |
|                                                                                                  |
|    *"  IMPORTING                                                                                 |
|    2222445455444                                                                                 |
|    A2009D0F249E7                                                                                 |
|    0000000000000                                                                                 |
|    0000000000000                                                                                 |
|    2A0022002000200049004D0050004F005200540049004E004700                                          |
|                                                                                                  |
|    *"     VALUE(I_BGPUNIT) TYPE  /PLCE/UUID                                                      |
|    2222222544542454455445225554222544425544                                                      |
|    A20000061C5589F2705E9490490500F0C35F5594                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2A00220020002000200020002000560041004C0055004500280049005F0042004700500055004E004900540029002\|
|0005400590050004500200020002F0050004C00430045002F005500550049004400                               |
|O_OBJECTNAME                                                                                      |
|    BAPIRET-MESSAGE_V1                                                                            |
|    445454524455444553222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    2109254DD533175F61000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    42004100500049005200450054002D004D004500530053004100470045005F0056003100200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|CO_EVENTTYPE_EVENT                                                                                |
|    E                                                                                             |
|    4                                                                                             |
|    5                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    4500                                                                                          |
|P_ADT_POPUP                                                                                       |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|TDCT                                                                                              |
|    {;;0000;;}                                                                                    |
|    202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202030303030202020202020202020                         |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000                         |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020003000300\|
|030003000200020002000200020002000200020002000                                                     |
|D020S                                                                                             |
|    {;;;;;0;0;0;0;0;0;0;0;0;;;;;00000000;000000}                                                  |
|    202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020000000000020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000000000000000000000002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|CO_EVENTTYPE_PROCESS                                                                              |
|    P                                                                                             |
|    5                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5000                                                                                          |
|O_COL                                                                                             |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|CO_EVENTTYPE_MODULE                                                                               |
|    M                                                                                             |
|    4                                                                                             |
|    D                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    4D00                                                                                          |
|O_DYNNR                                                                                           |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|TRDIR                                                                                             |
|    {DBD$SSEL;X;;;S;;;;;I;S;;SAP;00000000;SAP;20001113;000008;;;000;;X;;20001113;151932;20001113;\|
|151933;;}                                                                                         |
|    404040205050404020202020202020202020202020202020202020202020202020202020202020205020205020202\|
|0202020202020204050202020202020202050405020202020202020202030303030303030305040502020202020202020\|
|20303030303030303030303030303020202020203030302050203030303030303                                 |
|    40204040303050C000000000000000000000000000000000000000000000000000000000000000008000003000000\|
|0000000000000009030000000000000000030100000000000000000000000000000000000003010000000000000000000\|
|00200000001010103000000000008000000000000000000080002000000010101                                 |
|    44004200440024005300530045004C002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000580020002000530020002000200020\|
|0020002000200020002000200049005300200020002000200020002000200020005300410050002000200020002000200\|
|0200020002000200030003000300030003000300030003000530041005000200020002000200020002000200020002000\|
|3200300030003000310031003100330030003000300030003000380020002000200020002000300030003000200058002\|
|00032003000300030003100310031                                                                     |
|O_FULLNAME                                                                                        |
|    <empty string>                                                                                |
|    10000000                                                                                      |
|    40000000                                                                                      |
|    1400000000000000                                                                              |
|TSTC                                                                                              |
|    {;;0000;;00;}                                                                                 |
|    202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|020202020202020202020202020303030302020202020002020202020202020202020202020202020202020           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200030003000300030002000200020002000200000002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000                         |
|O_INCL                                                                                            |
|                                                                                                  |
|    2222222222222222222222222222222222222222                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|RSMPTEXTS                                                                                         |
|    {;;;;;;;}                                                                                     |
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
|O_LOCAL                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      23 Ty.          FUNCTION                                                                 |
|Name  RS_NAVIGATION_PREPARE                                                                       |
----------------------------------------------------------------------------------------------------
|KEYWORD                                                                                           |
|    <UNIT_UPDATE>-SYSTEMMESSAGEVAR                                                                |
|    354455554454325555444455444545                                                                |
|    C5E94F504145ED39345DD533175612                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    3C0055004E00490054005F005500500044004100540045003E002D00530059005300540045004D004D00450053005\|
|300410047004500560041005200                                                                       |
|O_OPERATION                                                                                       |
|    ADD                                                                                           |
|    4442222222                                                                                    |
|    1440000000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    4100440044002000200020002000200020002000                                                      |
|CL_WB_SOURCE_NAVIGATOR=>C_OPERATION_ADD                                                           |
|    ADD                                                                                           |
|    444                                                                                           |
|    144                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    410044004400                                                                                  |
|DIALOG                                                                                            |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|TOKEN_TABIX                                                                                       |
|    146                                                                                           |
|    9000                                                                                          |
|    2000                                                                                          |
|    92000000                                                                                      |
|IN_METHOD                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|EXT_CLASS                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|O_OBJECTTYPE                                                                                      |
|    D                                                                                             |
|    42                                                                                            |
|    40                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    44002000                                                                                      |
----------------------------------------------------------------------------------------------------
|No.      22 Ty.          METHOD                                                                   |
|Name  CL_WB_SOURCE_NAVIGATOR=>PREPARE_NAVIGATION                                                  |
----------------------------------------------------------------------------------------------------
|P_OPERATION                                                                                       |
|    DISPLAY                                                                                       |
|    4455445222                                                                                    |
|    4930C19000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004900530050004C0041005900200020002000                                                      |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|L_OPERATION                                                                                       |
|                                                                                                  |
|    2222222222                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    2000200020002000200020002000200020002000                                                      |
|ME->M_ENH_HANDLER                                                                                 |
|    {O:26*\CLASS=CL_WB_ED_ENHANCEMENT_HANDLER}                                                    |
|    E0001000                                                                                      |
|    5000A000                                                                                      |
|    E50000001A000000                                                                              |
|ME->CTXT_IS_CHANGED                                                                               |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|ME->CTXT_COL                                                                                      |
|    65                                                                                            |
|    4000                                                                                          |
|    1000                                                                                          |
|    41000000                                                                                      |
|ME->CTXT_OBJECT                                                                                   |
|    {O:242*\CLASS=CL_WB_CONTEXT}                                                                  |
|    0000F000                                                                                      |
|    D0002000                                                                                      |
|    0D000000F2000000                                                                              |
|ME->CTXT_EDITOR                                                                                   |
|    EDTR                                                                                          |
|    4455                                                                                          |
|    5442                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    4500440054005200                                                                              |
|ME->CTXT_INCLUDE                                                                                  |
|    /PLCP/LBGPROCESSINGU01                                                                        |
|    2544524445544455444533222222222222222222                                                      |
|    F0C30FC2702F35339E7501000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F004C0042004700500052004F00430045005300530049004E00470055003000310020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|ME->CTXT_MAINPROG                                                                                 |
|    /PLCP/SAPLBGPROCESSING                                                                        |
|    2544525454445544455444222222222222222222                                                      |
|    F0C30F310C2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F005300410050004C0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|SWBM_C_TYPE_CLS_ATTRIBUTE                                                                         |
|    OA                                                                                            |
|    442                                                                                           |
|    F10                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    4F0041002000                                                                                  |
|L_IOPERATION                                                                                      |
|    DEFINITION                                                                                    |
|    4444445444                                                                                    |
|    4569E949FE                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004500460049004E004900540049004F004E00                                                      |
|ME->CTXT_LINE                                                                                     |
|    47                                                                                            |
|    2000                                                                                          |
|    F000                                                                                          |
|    2F000000                                                                                      |
|L_NO_DIALOG                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SWBM_C_TYPE_CLS_TYPE                                                                              |
|    OT                                                                                            |
|    452                                                                                           |
|    F40                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    4F0054002000                                                                                  |
|ME->ADT_POPUP                                                                                     |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|L_CLASS                                                                                           |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222222222222222                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    00000000000000000000000000000000000000000000000000000000000000                                |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000                                        |
|L_CLASSTYPE                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SWBM_C_TYPE_PRG_INTF_ATTR                                                                         |
|    PNA                                                                                           |
|    544                                                                                           |
|    0E1                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    50004E004100                                                                                  |
|L_COL                                                                                             |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|L_DYNNR                                                                                           |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|L_EVENT                                                                                           |
|                                                                                                  |
|    222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|SWBM_C_TYPE_SF_SWITCH                                                                             |
|    6S                                                                                            |
|    352                                                                                           |
|    630                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    360053002000                                                                                  |
|L_EVENTTYPE                                                                                       |
|                                                                                                  |
|    22                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    00                                                                                            |
|    20002000                                                                                      |
|SWBM_C_TYPE_DDIC_DOMAIN                                                                           |
|    DD                                                                                            |
|    442                                                                                           |
|    440                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    440044002000                                                                                  |
|ME->TRGT_FULLNAME                                                                                 |
|    <empty string>                                                                                |
|    10000000                                                                                      |
|    40000000                                                                                      |
|    1400000000000000                                                                              |
|L_INCL                                                                                            |
|                                                                                                  |
|    2222222222222222222222222222222222222222                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|L_LOCAL                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      21 Ty.          METHOD                                                                   |
|Name  CL_WB_SOURCE_NAVIGATOR=>GET_NAVIGATION_OBJECT                                               |
----------------------------------------------------------------------------------------------------
|P_SOURCE_EDITOR                                                                                   |
|    {O:60*\CLASS=CL_WB_EDITOR}                                                                    |
|    C0003000                                                                                      |
|    3000C000                                                                                      |
|    C30000003C000000                                                                              |
|P_CONTEXT                                                                                         |
|    {O:242*\CLASS=CL_WB_CONTEXT}                                                                  |
|    0000F000                                                                                      |
|    D0002000                                                                                      |
|    0D000000F2000000                                                                              |
|P_OPTIONS                                                                                         |
|    {;;;;;;;;0;}                                                                                  |
|    202020202020202000002000                                                                      |
|    000000000000000000000000                                                                      |
|    200020002000200020002000200020000000000020000000                                              |
|P_OPERATION                                                                                       |
|    DISPLAY                                                                                       |
|    4455445222                                                                                    |
|    4930C19000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004900530050004C0041005900200020002000                                                      |
|P_WB_REQUEST                                                                                      |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|P_CURSOR_LINE                                                                                     |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|P_CURSOR_OFFSET                                                                                   |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|P_FIRST_LINE                                                                                      |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|P_LOCAL_CONTEXT                                                                                   |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|P_NAV_OBJECT                                                                                      |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|SY-REPID                                                                                          |
|    CL_WB_SOURCE_NAVIGATOR========CP                                                              |
|    4455455455445445444545333333334522222222                                                      |
|    3CF72F3F5235FE169714F2DDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F0053004F0055005200430045005F004E004100560049004700410054004F0052003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME->CTXT_MODE_DISPLAY                                                                             |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|P_SOURCE_EDITOR->EDIT-APP_DISP                                                                    |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|ME->CTXT_IS_CHANGED                                                                               |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|P_SOURCE_EDITOR->EDIT-BUF_VARIED                                                                  |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|ME->OPTIONS                                                                                       |
|    {;;;;;;;;0;}                                                                                  |
|    202020202020202000002000                                                                      |
|    000000000000000000000000                                                                      |
|    200020002000200020002000200020000000000020000000                                              |
|ME                                                                                                |
|    {O:243*\CLASS=CL_WB_SOURCE_NAVIGATOR}                                                         |
|    0000F000                                                                                      |
|    C0003000                                                                                      |
|    0C000000F3000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ME->NAVIGATION_REQUEST                                                                            |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|SCREEN                                                                                            |
|    {SOURCE;;;;;0;1;0;0;0;00;1;0;0;0;0;0}                                                         |
|    504050504040202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    30F050203050000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|ME->NAVIGATION_REQUEST->OPERATION                                                                 |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|SWBM_C_OP_CREATE                                                                                  |
|    CREATE                                                                                        |
|    4544542222                                                                                    |
|    3251450000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    4300520045004100540045002000200020002000                                                      |
|SYST-REPID                                                                                        |
|    CL_WB_SOURCE_NAVIGATOR========CP                                                              |
|    4455455455445445444545333333334522222222                                                      |
|    3CF72F3F5235FE169714F2DDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F0053004F0055005200430045005F004E004100560049004700410054004F0052003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
----------------------------------------------------------------------------------------------------
|No.      20 Ty.          METHOD                                                                   |
|Name  CL_WB_EDITOR=>GET_NAVIGATION_OBJECT                                                         |
----------------------------------------------------------------------------------------------------
|P_CONTEXT                                                                                         |
|    {O:242*\CLASS=CL_WB_CONTEXT}                                                                  |
|    0000F000                                                                                      |
|    D0002000                                                                                      |
|    0D000000F2000000                                                                              |
|P_OPERATION                                                                                       |
|    DISPLAY                                                                                       |
|    4455445222                                                                                    |
|    4930C19000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004900530050004C0041005900200020002000                                                      |
|P_WB_REQUEST                                                                                      |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY                                                                                                |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
|L_NAVIGATOR                                                                                       |
|    {O:243*\CLASS=CL_WB_SOURCE_NAVIGATOR}                                                         |
|    0000F000                                                                                      |
|    C0003000                                                                                      |
|    0C000000F3000000                                                                              |
|SY-REPID                                                                                          |
|    CL_WB_EDITOR==================CP                                                              |
|    4455454445453333333333333333334522222222                                                      |
|    3CF72F5494F2DDDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F0045004400490054004F0052003D003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME                                                                                                |
|    {O:60*\CLASS=CL_WB_EDITOR}                                                                    |
|    C0003000                                                                                      |
|    3000C000                                                                                      |
|    C30000003C000000                                                                              |
|L_LOCAL_CONTEXT                                                                                   |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SWBM_C_FC_EDIT_ENHANCEMENTS                                                                       |
|    WB_EDIT_ENH                                                                                   |
|    5454445544422222222222222222222222222222222222222222222222222222222222                        |
|    72F5494F5E800000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F0045004400490054005F0045004E0048002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|ME->CURSOR-INDEX                                                                                  |
|    47                                                                                            |
|    2000                                                                                          |
|    F000                                                                                          |
|    2F000000                                                                                      |
|SYST-REPID                                                                                        |
|    CL_WB_EDITOR==================CP                                                              |
|    4455454445453333333333333333334522222222                                                      |
|    3CF72F5494F2DDDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F0045004400490054004F0052003D003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME->CONTROL_HANDLE2_I                                                                             |
|    {O:85*\CLASS=CL_GUI_ABAPEDIT}                                                                 |
|    A0005000                                                                                      |
|    A0005000                                                                                      |
|    AA00000055000000                                                                              |
|ME->VISUALIZED                                                                                    |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SYST                                                                                              |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.      19 Ty.          METHOD                                                                   |
|Name  CL_FB_FUNCTION_NAVIGATION=>EDITOR_NAVIGATION                                                |
----------------------------------------------------------------------------------------------------
|EDITOR                                                                                            |
|    {O:60*\CLASS=CL_WB_EDITOR}                                                                    |
|    C0003000                                                                                      |
|    3000C000                                                                                      |
|    C30000003C000000                                                                              |
|MANAGER                                                                                           |
|    {O:17*\CLASS=CL_WB_MANAGER}                                                                   |
|    E0001000                                                                                      |
|    E0001000                                                                                      |
|    EE00000011000000                                                                              |
|HEADER                                                                                            |
|    {/PLCP/BGPROCESSUNIT;;;process background unit;D;/PLCP/BGPROCESSING;BGPROCESSING;SAP;;E;/PLCP\|
|/LBGPROCESSINGU01;/PLCP/;/PLCP/SAPLBGPROCESSING;CUSTOMER;EXTEND;T;{{/PLCP/BGPROCESSUNIT;/PLCP/SAP\|
|LBGPROCESSING;01;00000000;;;R;;;/PLCP/SAPLBGPROCESSING;;};;;;{;;;                                 |
|    205040405020404050504040405050504040502020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202070706060\|
|60707020606060606070607060602070606070202020202020202020202020202                                 |
|    F000C03000F020700020F03050303050E090400000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020F030\|
|50303000201030B07020F050E0400050E09040000000000000000000000000000                                 |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|02000200020002000200020002000200020002000200020002000200020002000200020002000700072006F0063006500\|
|7300730020006200610063006B00670072006F0075006E006400200075006E00690074002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|INTERFACE                                                                                         |
|    {IT_426[1x790];Table[initial];Table[initial];Table[initial];Table[initial];IT_428[1x354];IT_4\|
|96[89x8];Table[initial];IT_425[1x460];Table[initial];Table[initial];Table[initial];Table[initial]\|
|;Table[initial];Table[initial];Table[initial];Table[initial]}                                     |
|    500080000000FFFF0000FFFF0000FFFF0000FFFF500080005000A0000000FFFF500080000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    A00051000000FFFF0000FFFF0000FFFF0000FFFF80007100E00011000000FFFFB00041000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    5A0000008501000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF5800000087010\|
|0005E000000A101000000000000FFFFFFFF5B0000008401000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFF\|
|FF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF                |
|L_WB_FUNCTION_CONTEXT                                                                             |
|    {O:241*\CLASS=CL_WB_FUNCTION_CONTEXT}                                                         |
|    0000F000                                                                                      |
|    E0001000                                                                                      |
|    0E000000F1000000                                                                              |
|SCREEN                                                                                            |
|    {SOURCE;;;;;0;1;0;0;0;00;1;0;0;0;0;0}                                                         |
|    504050504040202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    30F050203050000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|L_WB_FUNCTION_CONTEXT->RAHMEN                                                                     |
|    /PLCP/SAPLBGPROCESSING                                                                        |
|    2544525454445544455444222222222222222222                                                      |
|    F0C30F310C2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F005300410050004C0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|HEADER-PROGNAME                                                                                   |
|    /PLCP/SAPLBGPROCESSING                                                                        |
|    2544525454445544455444222222222222222222                                                      |
|    F0C30F310C2702F35339E7000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F005300410050004C0042004700500052004F00430045005300530049004E00470020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|L_WB_FUNCTION_CONTEXT->INCLUDE                                                                    |
|    /PLCP/LBGPROCESSINGU01                                                                        |
|    2544524445544455444533222222222222222222                                                      |
|    F0C30FC2702F35339E7501000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F004C0042004700500052004F00430045005300530049004E00470055003000310020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|HEADER-INCLUDE                                                                                    |
|    /PLCP/LBGPROCESSINGU01                                                                        |
|    2544524445544455444533222222222222222222                                                      |
|    F0C30FC2702F35339E7501000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    2F0050004C00430050002F004C0042004700500052004F00430045005300530049004E00470055003000310020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|L_WB_FUNCTION_CONTEXT->OBJ_TYPE                                                                   |
|    FF                                                                                            |
|    442                                                                                           |
|    660                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    460046002000                                                                                  |
|SWBM_C_TYPE_FUNCTION                                                                              |
|    FF                                                                                            |
|    442                                                                                           |
|    660                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    460046002000                                                                                  |
|SY-REPID                                                                                          |
|    CL_FB_FUNCTION_NAVIGATION=====CP                                                              |
|    4454454544544454454445444333334522222222                                                      |
|    3CF62F65E349FEFE1697149FEDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00460042005F00460055004E004300540049004F004E005F004E0041005600490047004100540049004\|
|F004E003D003D003D003D003D004300500020002000200020002000200020002000                               |
|L_WB_FUNCTION_CONTEXT->OBJ_NAME                                                                   |
|    /PLCP/BGPROCESSUNIT                                                                           |
|    254452445544455544522222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    F0C30F2702F35335E9400000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|HEADER-NAME                                                                                       |
|    /PLCP/BGPROCESSUNIT                                                                           |
|    254452445544455544522222222222                                                                |
|    F0C30F2702F35335E9400000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|000200020002000200020002000                                                                       |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|L_WB_CONTEXT                                                                                      |
|    {O:242*\CLASS=CL_WB_CONTEXT}                                                                  |
|    0000F000                                                                                      |
|    D0002000                                                                                      |
|    0D000000F2000000                                                                              |
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
|HEADER-XCODE                                                                                      |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SWBM_C_FC_SELECT_OBJECT                                                                           |
|    WB_SELECT_OBJECT                                                                              |
|    5455444455444445222222222222222222222222222222222222222222222222222222                        |
|    72F35C534FF2A534000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F00530045004C004500430054005F004F0042004A0045004300540020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
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
|SYST-REPID                                                                                        |
|    CL_FB_FUNCTION_NAVIGATION=====CP                                                              |
|    4454454544544454454445444333334522222222                                                      |
|    3CF62F65E349FEFE1697149FEDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00460042005F00460055004E004300540049004F004E005F004E0041005600490047004100540049004\|
|F004E003D003D003D003D003D004300500020002000200020002000200020002000                               |
|????+0(8)#ECYCLE_MNGR_ENUM=>AMEONON_ADDINITIONETE#################################################|
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|L_WB_REQUEST                                                                                      |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|SUNI_C_TAB_EXCEPT                                                                                 |
|    EXCEPT                                                                                        |
|    4544552222                                                                                    |
|    5835040000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    4500580043004500500054002000200020002000                                                      |
|SWBM_C_FC_SHOW_CUE_CARD                                                                           |
|    WB_CUE_CARD                                                                                   |
|    5454545445422222222222222222222222222222222222222222222222222222222222                        |
|    72F355F312400000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F004300550045005F0043004100520044002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
----------------------------------------------------------------------------------------------------
|No.      18 Ty.          METHOD                                                                   |
|Name  CL_FB_FUNCTION_NAVIGATION=>NAVIGATE                                                         |
----------------------------------------------------------------------------------------------------
|EDITOR                                                                                            |
|    {O:60*\CLASS=CL_WB_EDITOR}                                                                    |
|    C0003000                                                                                      |
|    3000C000                                                                                      |
|    C30000003C000000                                                                              |
|MANAGER                                                                                           |
|    {O:17*\CLASS=CL_WB_MANAGER}                                                                   |
|    E0001000                                                                                      |
|    E0001000                                                                                      |
|    EE00000011000000                                                                              |
|FB_INSTANCE                                                                                       |
|    {O:39*\CLASS=CL_FUNCTION_BUILDER}                                                             |
|    D0002000                                                                                      |
|    80007000                                                                                      |
|    D800000027000000                                                                              |
|NAVIGATION_REQUEST                                                                                |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|TEXT_TYPE                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|HEADER                                                                                            |
|    {/PLCP/BGPROCESSUNIT;;;process background unit;D;/PLCP/BGPROCESSING;BGPROCESSING;SAP;;E;/PLCP\|
|/LBGPROCESSINGU01;/PLCP/;/PLCP/SAPLBGPROCESSING;CUSTOMER;EXTEND;T;{{/PLCP/BGPROCESSUNIT;/PLCP/SAP\|
|LBGPROCESSING;01;00000000;;;R;;;/PLCP/SAPLBGPROCESSING;;};;;;{;;;                                 |
|    205040405020404050504040405050504040502020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202070706060\|
|60707020606060606070607060602070606070202020202020202020202020202                                 |
|    F000C03000F020700020F03050303050E090400000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020F030\|
|50303000201030B07020F050E0400050E09040000000000000000000000000000                                 |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|02000200020002000200020002000200020002000200020002000200020002000200020002000700072006F0063006500\|
|7300730020006200610063006B00670072006F0075006E006400200075006E00690074002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|INTERFACE                                                                                         |
|    {IT_426[1x790];Table[initial];Table[initial];Table[initial];Table[initial];IT_428[1x354];IT_4\|
|96[89x8];Table[initial];IT_425[1x460];Table[initial];Table[initial];Table[initial];Table[initial]\|
|;Table[initial];Table[initial];Table[initial];Table[initial]}                                     |
|    500080000000FFFF0000FFFF0000FFFF0000FFFF500080005000A0000000FFFF500080000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    A00051000000FFFF0000FFFF0000FFFF0000FFFF80007100E00011000000FFFFB00041000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    5A0000008501000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF5800000087010\|
|0005E000000A101000000000000FFFFFFFF5B0000008401000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFF\|
|FF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF                |
|HEADER-XCODE                                                                                      |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SWBM_C_FC_WHERE_USED_LIST                                                                         |
|    WB_WHERE_USED_LIST                                                                            |
|    5455445455544544552222222222222222222222222222222222222222222222222222                        |
|    72F78525F5354FC9340000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F00570048004500520045005F0055005300450044005F004C00490053005400200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|HEADER-FBSTATE-CURSORFLD                                                                          |
|                                                                                                  |
|    222222222222222222222222222222                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|HEADER-FBSTATE-ACTIVETAB                                                                          |
|    SOURCE                                                                                        |
|    5455442222                                                                                    |
|    3F52350000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    53004F0055005200430045002000200020002000                                                      |
|SUNI_C_TAB_SOURCE                                                                                 |
|    SOURCE                                                                                        |
|    5455442222                                                                                    |
|    3F52350000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    53004F0055005200430045002000200020002000                                                      |
|SY-XFORM                                                                                          |
|    XAB_WFLUSH                                                                                    |
|    544554455422222222222222222222                                                                |
|    812F76C53800000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    5800410042005F00570046004C0055005300480020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SUNI_C_TAB_IMPORT                                                                                 |
|    IMPORT                                                                                        |
|    4454552222                                                                                    |
|    9D0F240000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    49004D0050004F00520054002000200020002000                                                      |
|INTERFACE-IMPORT                                                                                  |
|    Table IT_426[1x790]                                                                           |
|    {O:39*\CLASS=CL_FUNCTION_BUILDER}\DATA=INTERFACE-IMPORT                                       |
|    Table reference: 389                                                                          |
|    TABH+ 0(20) = 70EB1AD7067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000100000044010000                                        |
|    TABH+40(20) = 0000000085010000AA0100000100000016030000                                        |
|    TABH+60(20) = FFFFFFFF04C70000B01402000400410602000000                                        |
|    store                   = 0x70EB1AD7067F0000                                                  |
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
|    id                      = 389   (0x85010000)                                                  |
|    label                   = 426   (0xAA010000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 790   (0x16030000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000199/TYPE#000029                                             |
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
|    tabi                    = 0x801143D7067F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 414   (0x9E010000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    {I_BGPUNIT;TYPE;/PLCE/UUID;;X;;;X;;;;I;;;;}                                                   |
|    405040405050404050202020202020202020202020202020202020202020505050402020202020202020202020202\|
|0202020202020202020202020202050404040205050404020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    90F020700050E09040000000000000000000000000000000000000000000409000500000000000000000000000000\|
|000000000000000000000000000F000C03050F05050904000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    49005F0042004700500055004E0049005400200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020005400590050004500200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002F0050004C00430045002F0055005500490044002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|L_PARAM                                                                                           |
|    {;;;;;;;;;;;;;;;}                                                                             |
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
|HEADER-FBSTATE-POS_CUROW                                                                          |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|L_PARAM-PARAMETER                                                                                 |
|                                                                                                  |
|    222222222222222222222222222222                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
----------------------------------------------------------------------------------------------------
|No.      17 Ty.          METHOD                                                                   |
|Name  CL_FUNCTION_BUILDER=>NAVIGATE                                                               |
----------------------------------------------------------------------------------------------------
|NAVIGATION_REQUEST                                                                                |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|TEXT_TYPE                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|HEADER                                                                                            |
|    {/PLCP/BGPROCESSUNIT;;;process background unit;D;/PLCP/BGPROCESSING;BGPROCESSING;SAP;;E;/PLCP\|
|/LBGPROCESSINGU01;/PLCP/;/PLCP/SAPLBGPROCESSING;CUSTOMER;EXTEND;T;{{/PLCP/BGPROCESSUNIT;/PLCP/SAP\|
|LBGPROCESSING;01;00000000;;;R;;;/PLCP/SAPLBGPROCESSING;;};;;;{;;;                                 |
|    205040405020404050504040405050504040502020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202070706060\|
|60707020606060606070607060602070606070202020202020202020202020202                                 |
|    F000C03000F020700020F03050303050E090400000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020F030\|
|50303000201030B07020F050E0400050E09040000000000000000000000000000                                 |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|02000200020002000200020002000200020002000200020002000200020002000200020002000700072006F0063006500\|
|7300730020006200610063006B00670072006F0075006E006400200075006E00690074002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|INTERFACE                                                                                         |
|    {IT_426[1x790];Table[initial];Table[initial];Table[initial];Table[initial];IT_428[1x354];IT_4\|
|96[89x8];Table[initial];IT_425[1x460];Table[initial];Table[initial];Table[initial];Table[initial]\|
|;Table[initial];Table[initial];Table[initial];Table[initial]}                                     |
|    500080000000FFFF0000FFFF0000FFFF0000FFFF500080005000A0000000FFFF500080000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    A00051000000FFFF0000FFFF0000FFFF0000FFFF80007100E00011000000FFFFB00041000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    5A0000008501000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF5800000087010\|
|0005E000000A101000000000000FFFFFFFF5B0000008401000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFF\|
|FF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF                |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SY-REPID                                                                                          |
|    CL_FUNCTION_BUILDER===========CP                                                              |
|    4454544544454544445333333333334522222222                                                      |
|    3CF65E349FEF259C452DDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00460055004E004300540049004F004E005F004200550049004C004400450052003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME->EDITOR                                                                                        |
|    {O:60*\CLASS=CL_WB_EDITOR}                                                                    |
|    C0003000                                                                                      |
|    3000C000                                                                                      |
|    C30000003C000000                                                                              |
|ME->IF_WB_PROGRAM~WB_MANAGER                                                                      |
|    {O:17*\CLASS=CL_WB_MANAGER}                                                                   |
|    E0001000                                                                                      |
|    E0001000                                                                                      |
|    EE00000011000000                                                                              |
|ME                                                                                                |
|    {O:39*\CLASS=CL_FUNCTION_BUILDER}                                                             |
|    D0002000                                                                                      |
|    80007000                                                                                      |
|    D800000027000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
----------------------------------------------------------------------------------------------------
|No.      16 Ty.          FORM                                                                     |
|Name  DI_GENERAL_OK_CODE                                                                          |
----------------------------------------------------------------------------------------------------
|EXCLUDING_TAB                                                                                     |
|    {RSCL}                                                                                        |
|    505040402020202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|02020202020202020202020202020202020202020202020                                                   |
|    203030C00000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000                                                   |
|    5200530043004C0020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|XCODE                                                                                             |
|    WB_SELECT_OBJECT                                                                              |
|    5455444455444445222222222222222222222222222222222222222222222222222222                        |
|    72F35C534FF2A534000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F00530045004C004500430054005F004F0042004A0045004300540020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SWBM_C_FC_WHERE_USED_LIST                                                                         |
|    WB_WHERE_USED_LIST                                                                            |
|    5455445455544544552222222222222222222222222222222222222222222222222222                        |
|    72F78525F5354FC9340000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F00570048004500520045005F0055005300450044005F004C00490053005400200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SUNI_C_FC_MESSAGES                                                                                |
|    WB_MESSAGE                                                                                    |
|    5454455444222222222222222222222222222222222222222222222222222222222222                        |
|    72FD533175000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F004D0045005300530041004700450020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SY-XPROG                                                                                          |
|    SAPMSSY1                                                                                      |
|    5454555322222222222222222222222222222222                                                      |
|    310D339100000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004D0053005300590031002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|SY-REPID                                                                                          |
|    SAPLSFUNCTION_BUILDER                                                                         |
|    5454545445444545444452222222222222222222                                                      |
|    310C365E349FEF259C4520000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C005300460055004E004300540049004F004E005F004200550049004C00440045005200200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|RS38L-REMOTE                                                                                      |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SCREEN                                                                                            |
|    {SOURCE;;;;;0;1;0;0;0;00;1;0;0;0;0;0}                                                         |
|    504050504040202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    30F050203050000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SUNI_C_FC_BREAKPOINTS                                                                             |
|    FL_SHOW_BREAK                                                                                 |
|    4455445545444222222222222222222222222222222222222222222222222222222222                        |
|    6CF38F7F2251B000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    46004C005F00530048004F0057005F0042005200450041004B0020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SUNI_C_FC_EXT_BREAK                                                                               |
|    FL_SHOW_EXT_BREAK                                                                             |
|    4455445545554544422222222222222222222222222222222222222222222222222222                        |
|    6CF38F7F584F2251B00000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    46004C005F00530048004F0057005F004500580054005F0042005200450041004B002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|RS38L-BASXML_ENABLED                                                                              |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|EXCLUDING_TAB[]                                                                                   |
|    Table IT_882[138x140]                                                                         |
|    \FUNCTION-POOL=SFUNCTION_BUILDER\DATA=EXCLUDING_TAB[]                                         |
|    Table reference: 463                                                                          |
|    TABH+ 0(20) = 901BD1DD067F0000000000000000000000000000                                        |
|    TABH+20(20) = 00000000C026D5DD067F00008A00000045010000                                        |
|    TABH+40(20) = 00000000CF010000720300008A0000008C000000                                        |
|    TABH+60(20) = FFFFFFFF043B010090E402000A00000602000000                                        |
|    store                   = 0x901BD1DD067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0xC026D5DD067F0000                                                  |
|      idxKind               = 1     (ItIndexLinear)                                               |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 2     (cmpSingleMcmpR)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 463   (0xCF010000)                                                  |
|    label                   = 882   (0x72030000)                                                  |
|    fill                    = 138   (0x8A000000)                                                  |
|    leng                    = 140   (0x8C000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000315/TYPE#000005                                             |
|    occu                    = 10    (0x0A000000)                                                  |
|    occu0                   = 0                                                                   |
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
|    tabi                    = 0x0016D1DD067F0000                                                  |
|    pgHook                  = 0x9827D1DD067F0000                                                  |
|    uniqueNumber            = 846   (0x4E030000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 570   (0x3A020000)                                                  |
|    lineAlloc               = 186   (0xBA000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|                                                                                                  |
|    {WB_ACT_INACT_TOGGLE}                                                                         |
|    504050404050504040404050505040404040402020202020202020202020202020202020202020202020202020202\|
|02020202020202020202020202020202020202020202020                                                   |
|    7020F0103040F090E0103040F040F07070C0500000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000                                                   |
|    570042005F004100430054005F0049004E004100430054005F0054004F00470047004C00450020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|                                                                                                  |
|    {WB_ACTIVE_VERSION}                                                                           |
|    504050404050405040505040505040404020202020202020202020202020202020202020202020202020202020202\|
|02020202020202020202020202020202020202020202020                                                   |
|    7020F0103040906050F06050203090F0E000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000                                                   |
|    570042005F004100430054004900560045005F00560045005200530049004F004E002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|                                                                                                  |
|    {WB_MULTI_OBJECT_PREV}                                                                        |
|    504050405040504050404040404050505050405020202020202020202020202020202020202020202020202020202\|
|02020202020202020202020202020202020202020202020                                                   |
|    7020F0D050C04090F0F020A0503040F00020506000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000                                                   |
|    570042005F004D0055004C00540049005F004F0042004A004500430054005F0050005200450056002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|                                                                                                  |
|    {WB_MULTI_OBJECT_NEXT}                                                                        |
|    504050405040504050404040404050504040505020202020202020202020202020202020202020202020202020202\|
|02020202020202020202020202020202020202020202020                                                   |
|    7020F0D050C04090F0F020A0503040F0E050804000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000                                                   |
|    570042005F004D0055004C00540049005F004F0042004A004500430054005F004E004500580054002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|                                                                                                  |
|    {TABG}                                                                                        |
|    504040402020202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|02020202020202020202020202020202020202020202020                                                   |
|    401020700000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000                                                   |
|    540041004200470020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|FB_INSTANCE                                                                                       |
|    {O:39*\CLASS=CL_FUNCTION_BUILDER}                                                             |
|    D0002000                                                                                      |
|    80007000                                                                                      |
|    D800000027000000                                                                              |
|HEADER                                                                                            |
|    {/PLCP/BGPROCESSUNIT;;;process background unit;D;/PLCP/BGPROCESSING;BGPROCESSING;SAP;;E;/PLCP\|
|/LBGPROCESSINGU01;/PLCP/;/PLCP/SAPLBGPROCESSING;CUSTOMER;EXTEND;T;{{/PLCP/BGPROCESSUNIT;/PLCP/SAP\|
|LBGPROCESSING;01;00000000;;;R;;;/PLCP/SAPLBGPROCESSING;;};;;;{;;;                                 |
|    205040405020404050504040405050504040502020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202070706060\|
|60707020606060606070607060602070606070202020202020202020202020202                                 |
|    F000C03000F020700020F03050303050E090400000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020F030\|
|50303000201030B07020F050E0400050E09040000000000000000000000000000                                 |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|02000200020002000200020002000200020002000200020002000200020002000200020002000700072006F0063006500\|
|7300730020006200610063006B00670072006F0075006E006400200075006E00690074002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|INTERFACE                                                                                         |
|    {IT_426[1x790];Table[initial];Table[initial];Table[initial];Table[initial];IT_428[1x354];IT_4\|
|96[89x8];Table[initial];IT_425[1x460];Table[initial];Table[initial];Table[initial];Table[initial]\|
|;Table[initial];Table[initial];Table[initial];Table[initial]}                                     |
|    500080000000FFFF0000FFFF0000FFFF0000FFFF500080005000A0000000FFFF500080000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    A00051000000FFFF0000FFFF0000FFFF0000FFFF80007100E00011000000FFFFB00041000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    5A0000008501000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF5800000087010\|
|0005E000000A101000000000000FFFFFFFF5B0000008401000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFF\|
|FF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF                |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|FB_EDITOR                                                                                         |
|    {O:59*\CLASS=CL_FB_FUNCTION_EDITOR}                                                           |
|    C0003000                                                                                      |
|    4000B000                                                                                      |
|    C40000003B000000                                                                              |
|DUMMYBUTTON                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|RS38L-RFCSCOPE                                                                                    |
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
----------------------------------------------------------------------------------------------------
|No.      15 Ty.          FORM                                                                     |
|Name  OK_CODE_TABSTRIP                                                                            |
----------------------------------------------------------------------------------------------------
|SY-REPID                                                                                          |
|    SAPLSFUNCTION_BUILDER                                                                         |
|    5454545445444545444452222222222222222222                                                      |
|    310C365E349FEF259C4520000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C005300460055004E004300540049004F004E005F004200550049004C00440045005200200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|FUNC_TAB_STRIP-ACTIVETAB                                                                          |
|    SOURCE                                                                                        |
|    545544222222222222222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222                                                           |
|    3F5235000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000                                                           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000                                                           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000                                                           |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020002000200020002000                                                   |
|XCODE                                                                                             |
|    WB_SELECT_OBJECT                                                                              |
|    5455444455444445222222222222222222222222222222222222222222222222222222                        |
|    72F35C534FF2A534000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F00530045004C004500430054005F004F0042004A0045004300540020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|RS38L-NORMAL                                                                                      |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SUNI_C_FC_FB_DOCUMENTATION                                                                        |
|    FB_DOCU                                                                                       |
|    4454445222222222222222222222222222222222222222222222222222222222222222                        |
|    62F4F35000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    460042005F0044004F004300550020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SAVE_OK                                                                                           |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|FB_EDITOR                                                                                         |
|    {O:59*\CLASS=CL_FB_FUNCTION_EDITOR}                                                           |
|    C0003000                                                                                      |
|    4000B000                                                                                      |
|    C40000003B000000                                                                              |
|%_CX_TABSTRIP_VERSION                                                                             |
|    0                                                                                             |
|    3                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    3000                                                                                          |
|HEADER-FBDOCU_FOUND                                                                               |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SOURCE-*CX*                                                                                       |
|    <initial>                                                                                     |
|    <initial>                                                                                     |
|    <initial>                                                                                     |
|    <<iinniittiiaall>>                                                                            |
----------------------------------------------------------------------------------------------------
|No.      14 Ty.          MODULE (PAI)                                                             |
|Name  OK_CODE_TABSTRIP                                                                            |
----------------------------------------------------------------------------------------------------
|%_FL2*EXCEPTCL                                                                                    |
|    1                                                                                             |
|    0000                                                                                          |
|    1000                                                                                          |
|    01000000                                                                                      |
|XCODE                                                                                             |
|    WB_SELECT_OBJECT                                                                              |
|    5455444455444445222222222222222222222222222222222222222222222222222222                        |
|    72F35C534FF2A534000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    570042005F00530045004C004500430054005F004F0042004A0045004300540020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|OK_CODE                                                                                           |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
----------------------------------------------------------------------------------------------------
|No.      13 Ty.          FUNCTION                                                                 |
|Name  RS_FUNCTION_DISPLAY                                                                         |
----------------------------------------------------------------------------------------------------
|TADIR                                                                                             |
|    {;;;;;;;;;;;;;;;;;;;00000000;00000000;}                                                       |
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
|'Langtext vorhanden'                                                                              |
|    Langtext vorhanden                                                                            |
|    466676772767666666                                                                            |
|    C1E7458406F281E45E                                                                            |
|    000000000000000000                                                                            |
|    000000000000000000                                                                            |
|    4C0061006E0067007400650078007400200076006F007200680061006E00640065006E00                      |
|CX_TRUE                                                                                           |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|ICON_LED_GREEN                                                                                    |
|    @5B@                                                                                          |
|    4344                                                                                          |
|    0520                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    4000350042004000                                                                              |
|CX_FALSE                                                                                          |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|G_ICON_OK                                                                                         |
|    @5B\QLangtext vorhanden@                                                                      |
|    4345546667677276766666642222222222222222222222222222222222222222                              |
|    052C1C1E7458406F281E45E00000000000000000000000000000000000000000                              |
|    0000000000000000000000000000000000000000000000000000000000000000                              |
|    0000000000000000000000000000000000000000000000000000000000000000                              |
|    4000350042005C0051004C0061006E0067007400650078007400200076006F007200680061006E00640065006E004\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000                                |
|%_CX_GRID_ID                                                                                      |
|    1                                                                                             |
|    0000                                                                                          |
|    1000                                                                                          |
|    01000000                                                                                      |
|SY-REPID                                                                                          |
|    SAPLSFUNCTION_BUILDER                                                                         |
|    5454545445444545444452222222222222222222                                                      |
|    310C365E349FEF259C4520000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C005300460055004E004300540049004F004E005F004200550049004C00440045005200200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|FUNC_TAB_STRIP-ACTIVETAB                                                                          |
|    SOURCE                                                                                        |
|    545544222222222222222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222                                                           |
|    3F5235000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000                                                           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000                                                           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000                                                           |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020002000200020002000                                                   |
|HEADER-FBSTATE-ACTIVETAB                                                                          |
|    SOURCE                                                                                        |
|    5455442222                                                                                    |
|    3F52350000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    53004F0055005200430045002000200020002000                                                      |
|SUNI_C_TAB_SOURCE                                                                                 |
|    SOURCE                                                                                        |
|    5455442222                                                                                    |
|    3F52350000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    53004F0055005200430045002000200020002000                                                      |
|TAB_PROGRAM                                                                                       |
|    SAPLEDITOR_START                                                                              |
|    5454444545555455222222222222222222222222                                                      |
|    310C5494F2F34124000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C0045004400490054004F0052005F005300540041005200540020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|%_CX_TABLEVIEW_ID                                                                                 |
|    2                                                                                             |
|    0000                                                                                          |
|    2000                                                                                          |
|    02000000                                                                                      |
|%_CX_TABSTRIP_VERSION                                                                             |
|    0                                                                                             |
|    3                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    3000                                                                                          |
|%_CX_TABLEVIEW_VERSION                                                                            |
|    0                                                                                             |
|    3                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    3000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      12 Ty.          METHOD                                                                   |
|Name  CL_FUNCTION_BUILDER=>CHANGE                                                                 |
----------------------------------------------------------------------------------------------------
|MODE                                                                                              |
|    SHOW                                                                                          |
|    5445222222                                                                                    |
|    38F7000000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    530048004F005700200020002000200020002000                                                      |
|ACTIVETAB                                                                                         |
|    SOURCE                                                                                        |
|    5455442222                                                                                    |
|    3F52350000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    53004F0055005200430045002000200020002000                                                      |
|DYNNR                                                                                             |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|CURSOR                                                                                            |
|    {;0;0;0;0}                                                                                    |
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
|SUB_TYPE                                                                                          |
|                                                                                                  |
|    222                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    200020002000                                                                                  |
|SUB_OBJECT                                                                                        |
|                                                                                                  |
|    222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|PRETTY_PRINTED                                                                                    |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|LOADSTATE                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|TEXT_ELEMENT                                                                                      |
|    {;;;0}                                                                                        |
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
|CONTROL_STATE                                                                                     |
|    {{;;0;;};0;0;;;;;0;;;0;0;0}                                                                   |
|    202020202020202000002020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202000000000202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020000000000020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200000000000000000002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|IM_CREATE_RFAC_OBJECT                                                                             |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|SY-XFORM                                                                                          |
|    XAB_WFLUSH                                                                                    |
|    544554455422222222222222222222                                                                |
|    812F76C53800000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    5800410042005F00570046004C0055005300480020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|ENH_ERR                                                                                           |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_MESS_GET_TEXT                                                                                   |
|    <empty string>                                                                                |
|    10000000                                                                                      |
|    40000000                                                                                      |
|    1400000000000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SUNI_C_TAB_TABLES                                                                                 |
|    TABLES                                                                                        |
|    5444452222                                                                                    |
|    412C530000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    5400410042004C00450053002000200020002000                                                      |
|ME->NO_VISUALIZATION                                                                              |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|CL_ABAP_UNIT_ASSERT=>RTOL_DEFAULT                                                                 |
|    1e-14                                                                                         |
|    92A89803                                                                                      |
|    BB16B46D                                                                                      |
|    9B2BA1869B84063D                                                                              |
|ME->FB_VISUALIZATION                                                                              |
|    {O:136*\CLASS=CL_FB_FUNCTION_VISUALIZATION}                                                   |
|    70008000                                                                                      |
|    70008000                                                                                      |
|    7700000088000000                                                                              |
|SWBCN_C_CTY_REQUEST_ID                                                                            |
|    TRKORR                                                                                        |
|    554455222222222222222222222222                                                                |
|    42BF22000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    540052004B004F0052005200200020002000200020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|ME->FUNCTION_EDITOR                                                                               |
|    {O:59*\CLASS=CL_FB_FUNCTION_EDITOR}                                                           |
|    C0003000                                                                                      |
|    4000B000                                                                                      |
|    C40000003B000000                                                                              |
|SWBM_C_OP_SHIFT                                                                                   |
|    SHIFT                                                                                         |
|    5444522222                                                                                    |
|    3896400000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    5300480049004600540020002000200020002000                                                      |
|ME                                                                                                |
|    {O:39*\CLASS=CL_FUNCTION_BUILDER}                                                             |
|    D0002000                                                                                      |
|    80007000                                                                                      |
|    D800000027000000                                                                              |
|SWBM_C_OP_RENAME                                                                                  |
|    RENAME                                                                                        |
|    5444442222                                                                                    |
|    25E1D50000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    520045004E0041004D0045002000200020002000                                                      |
|SWBM_C_OP_COPY                                                                                    |
|    COPY                                                                                          |
|    4455222222                                                                                    |
|    3F09000000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    43004F0050005900200020002000200020002000                                                      |
|ME->HEADER                                                                                        |
|    {/PLCP/BGPROCESSUNIT;;;process background unit;D;/PLCP/BGPROCESSING;BGPROCESSING;SAP;;E;/PLCP\|
|/LBGPROCESSINGU01;/PLCP/;/PLCP/SAPLBGPROCESSING;CUSTOMER;EXTEND;T;{{/PLCP/BGPROCESSUNIT;/PLCP/SAP\|
|LBGPROCESSING;01;00000000;;;R;;;/PLCP/SAPLBGPROCESSING;;};;;;{;;;                                 |
|    205040405020404050504040405050504040502020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202070706060\|
|60707020606060606070607060602070606070202020202020202020202020202                                 |
|    F000C03000F020700020F03050303050E090400000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020F030\|
|50303000201030B07020F050E0400050E09040000000000000000000000000000                                 |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|02000200020002000200020002000200020002000200020002000200020002000200020002000700072006F0063006500\|
|7300730020006200610063006B00670072006F0075006E006400200075006E00690074002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SWBM_C_OP_ACTIVATE                                                                                |
|    ACTIVATE                                                                                      |
|    4454545422                                                                                    |
|    1349614500                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    4100430054004900560041005400450020002000                                                      |
|ME->INTERFACE                                                                                     |
|    {IT_426[1x790];Table[initial];Table[initial];Table[initial];Table[initial];IT_428[1x354];IT_4\|
|96[89x8];Table[initial];IT_425[1x460];Table[initial];Table[initial];Table[initial];Table[initial]\|
|;Table[initial];Table[initial];Table[initial];Table[initial]}                                     |
|    500080000000FFFF0000FFFF0000FFFF0000FFFF500080005000A0000000FFFF500080000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    A00051000000FFFF0000FFFF0000FFFF0000FFFF80007100E00011000000FFFFB00041000000FFFF0000FFFF0000F\|
|FFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF                                                       |
|    5A0000008501000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF5800000087010\|
|0005E000000A101000000000000FFFFFFFF5B0000008401000000000000FFFFFFFF00000000FFFFFFFF00000000FFFFFF\|
|FF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF                |
|ME->HEADER-NAME                                                                                   |
|    /PLCP/BGPROCESSUNIT                                                                           |
|    254452445544455544522222222222                                                                |
|    F0C30F2702F35335E9400000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    2F0050004C00430050002F0042004700500052004F00430045005300530055004E004900540020002000200020002\|
|000200020002000200020002000                                                                       |
----------------------------------------------------------------------------------------------------
|No.      11 Ty.          METHOD                                                                   |
|Name  CL_FUNCTION_BUILDER=>IF_WB_PROGRAM~PROCESS_WB_REQUEST                                       |
----------------------------------------------------------------------------------------------------
|P_WB_REQUEST                                                                                      |
|    {O:33*\CLASS=CL_WB_REQUEST}                                                                   |
|    D0002000                                                                                      |
|    E0001000                                                                                      |
|    DE00000021000000                                                                              |
|P_WB_PROGRAM_STATE                                                                                |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|P_WINDOW_ID                                                                                       |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|P_WB_DATA_CONTAINER                                                                               |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|L_CONTROL_STATE                                                                                   |
|    {{;;0;;};0;0;;;;;0;;;0;0;0}                                                                   |
|    202020202020202000002020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202000000000202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020000000000020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200000000000000000002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|L_OBJECT_STATE->CONTROL_STATE                                                                     |
|    {{;;0;;};0;0;;;;;0;;;0;0;0}                                                                   |
|    202020202020202000002020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202000000000202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020000000000020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200000000000000000002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|P_WB_REQUEST->OPERATION                                                                           |
|    DISPLAY                                                                                       |
|    4455445222                                                                                    |
|    4930C19000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004900530050004C0041005900200020002000                                                      |
|SWBM_C_OP_DISPLAY                                                                                 |
|    DISPLAY                                                                                       |
|    4455445222                                                                                    |
|    4930C19000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004900530050004C0041005900200020002000                                                      |
|SCREEN                                                                                            |
|    {SOURCE;;;;;0;1;0;0;0;00;1;0;0;0;0;0}                                                         |
|    504050504040202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    30F050203050000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|ME                                                                                                |
|    {O:39*\CLASS=CL_FUNCTION_BUILDER}                                                             |
|    D0002000                                                                                      |
|    80007000                                                                                      |
|    D800000027000000                                                                              |
|IF_ABAP_UNIT_CONSTANT=>SEVERITY-MEDIUM                                                            |
|    1                                                                                             |
|    0                                                                                             |
|    1                                                                                             |
|    01                                                                                            |
|L_ACTIVETAB                                                                                       |
|    SOURCE                                                                                        |
|    5455442222                                                                                    |
|    3F52350000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    53004F0055005200430045002000200020002000                                                      |
|L_DYNNR                                                                                           |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|L_CURSOR                                                                                          |
|    {;0;0;0;0}                                                                                    |
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
|L_TYPE                                                                                            |
|                                                                                                  |
|    222                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    200020002000                                                                                  |
|L_OBJECT                                                                                          |
|                                                                                                  |
|    222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|L_PRETTY_PRINTED                                                                                  |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|L_STATE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|L_NEW_TEXTELEMENT                                                                                 |
|    {;;;0}                                                                                        |
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
|SYST-REPID                                                                                        |
|    CL_FUNCTION_BUILDER===========CP                                                              |
|    4454544544454544445333333333334522222222                                                      |
|    3CF65E349FEF259C452DDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00460055004E004300540049004F004E005F004200550049004C004400450052003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SUNI_C_TAB_TABLES                                                                                 |
|    TABLES                                                                                        |
|    5444452222                                                                                    |
|    412C530000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    5400410042004C00450053002000200020002000                                                      |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY-SUBRC                                                                                          |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
|SY-MSGV2                                                                                          |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|SYST                                                                                              |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.      10 Ty.          METHOD                                                                   |
|Name  CL_WB_NAVIGATOR=>DO_THE_NAVIGATION                                                          |
----------------------------------------------------------------------------------------------------
|P_REQUEST_ENTRY                                                                                   |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|P_ERROR                                                                                           |
|    {0;;;000;;;;}                                                                                 |
|    000020202020202020202020202020202020202020202030303020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000002000200020002000200020002000200020002000200020002000200020002000200020002000200020003\|
|0003000300020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|ME->REQUEST                                                                                       |
|    {O:33*\CLASS=CL_WB_REQUEST}                                                                   |
|    D0002000                                                                                      |
|    E0001000                                                                                      |
|    DE00000021000000                                                                              |
|SCREEN                                                                                            |
|    {SOURCE;;;;;0;1;0;0;0;00;1;0;0;0;0;0}                                                         |
|    504050504040202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    30F050203050000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SWBM_C_OP_DISPLAY                                                                                 |
|    DISPLAY                                                                                       |
|    4455445222                                                                                    |
|    4930C19000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004900530050004C0041005900200020002000                                                      |
|ME->NEW_TOOL_WINDOW                                                                               |
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
|SY-REPID                                                                                          |
|    CL_WB_NAVIGATOR===============CP                                                              |
|    4455454454445453333333333333334522222222                                                      |
|    3CF72FE169714F2DDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004E004100560049004700410054004F0052003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ABAP_TRUE                                                                                         |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SYST-REPID                                                                                        |
|    CL_WB_NAVIGATOR===============CP                                                              |
|    4455454454445453333333333333334522222222                                                      |
|    3CF72FE169714F2DDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004E004100560049004700410054004F0052003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME->PROGRAM_STATE                                                                                 |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|ME->DATA_CONTAINER                                                                                |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SWBM_C_ER_NO_ERROR                                                                                |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|ME                                                                                                |
|    {O:35*\CLASS=CL_WB_NAVIGATOR_VIS_AS_DYNPRO}                                                   |
|    D0002000                                                                                      |
|    C0003000                                                                                      |
|    DC00000023000000                                                                              |
|SYST                                                                                              |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       9 Ty.          METHOD                                                                   |
|Name  CL_WB_NAVIGATOR_VIS_AS_DYNPRO=>DO_THE_NAVIGATION                                            |
----------------------------------------------------------------------------------------------------
|P_REQUEST_ENTRY                                                                                   |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|P_ERROR                                                                                           |
|    {0;;;000;;;;}                                                                                 |
|    000020202020202020202020202020202020202020202030303020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000002000200020002000200020002000200020002000200020002000200020002000200020002000200020003\|
|0003000300020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SY-REPID                                                                                          |
|    CL_WB_NAVIGATOR_VIS_AS_DYNPRO=CP                                                              |
|    4455454454445455545545545455434522222222                                                      |
|    3CF72FE169714F2F693F13F49E02FD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004E004100560049004700410054004F0052005F005600490053005F00410053005F004\|
|40059004E00500052004F003D004300500020002000200020002000200020002000                               |
|ME->NEW_NAVIGATION_CONTEXT                                                                        |
|    PG_/PLCP/SAPLBGPROCESSING                                                                     |
|    545254452545444554445544422222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    07FF0C30F310C2702F35339E700000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    500047005F002F0050004C00430050002F005300410050004C0042004700500052004F00430045005300530049004\|
|E004700200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|SCREEN                                                                                            |
|    {SOURCE;;;;;0;1;0;0;0;00;1;0;0;0;0;0}                                                         |
|    504050504040202020202020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    30F050203050000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    53004F005500520043004500200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SYST-REPID                                                                                        |
|    CL_WB_NAVIGATOR_VIS_AS_DYNPRO=CP                                                              |
|    4455454454445455545545545455434522222222                                                      |
|    3CF72FE169714F2F693F13F49E02FD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004E004100560049004700410054004F0052005F005600490053005F00410053005F004\|
|40059004E00500052004F003D004300500020002000200020002000200020002000                               |
|ME                                                                                                |
|    {O:35*\CLASS=CL_WB_NAVIGATOR_VIS_AS_DYNPRO}                                                   |
|    D0002000                                                                                      |
|    C0003000                                                                                      |
|    DC00000023000000                                                                              |
|ME->REQUEST->OBJECT_TYPE                                                                          |
|    FF                                                                                            |
|    442                                                                                           |
|    660                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    460046002000                                                                                  |
|ME->REQUEST->OBJECT_NAME                                                                          |
|    /PLCP/SAPLBGPROCESSING                  /PLCP/BGPROCESSUNIT                                   |
|    254452545444554445544422222222222222222225445244554445554452222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    F0C30F310C2702F35339E7000000000000000000F0C30F2702F35335E940000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    2F0050004C00430050002F005300410050004C0042004700500052004F00430045005300530049004E00470020002\|
|00020002000200020002000200020002000200020002000200020002000200020002F0050004C00430050002F00420047\|
|00500052004F00430045005300530055004E0049005400200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SYST                                                                                              |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SWBM_C_OP_ADJUST                                                                                  |
|    ADJUST                                                                                        |
|    4445552222                                                                                    |
|    14A5340000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    410044004A005500530054002000200020002000                                                      |
|SWBM_C_OP_DISPLAY                                                                                 |
|    DISPLAY                                                                                       |
|    4455445222                                                                                    |
|    4930C19000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004900530050004C0041005900200020002000                                                      |
|SY-SUBRC                                                                                          |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
----------------------------------------------------------------------------------------------------
|No.       8 Ty.          METHOD                                                                   |
|Name  CL_WB_MANAGER=>PROCESS_WB_REQUEST                                                           |
----------------------------------------------------------------------------------------------------
|P_WB_REQUEST                                                                                      |
|    {O:10*\CLASS=CL_WB_REQUEST}                                                                   |
|    F0000000                                                                                      |
|    5000A000                                                                                      |
|    F50000000A000000                                                                              |
|P_WB_PROGRAM_STATE                                                                                |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|P_ACTIVE_NAVIGATOR                                                                                |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|P_ERROR                                                                                           |
|    {0;;;000;;;;}                                                                                 |
|    000020202020202020202020202020202020202020202030303020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000002000200020002000200020002000200020002000200020002000200020002000200020002000200020003\|
|0003000300020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|P_WB_DATA_CONTAINER                                                                               |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|SY-REPID                                                                                          |
|    CL_WB_MANAGER=================CP                                                              |
|    4455454444445333333333333333334522222222                                                      |
|    3CF72FD1E1752DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004D0041004E0041004700450052003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME                                                                                                |
|    {O:17*\CLASS=CL_WB_MANAGER}                                                                   |
|    E0001000                                                                                      |
|    E0001000                                                                                      |
|    EE00000011000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|L_WB_REQUEST->OPERATION                                                                           |
|    DISPLAY                                                                                       |
|    4455445222                                                                                    |
|    4930C19000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    44004900530050004C0041005900200020002000                                                      |
|SWBM_C_OP_INITIAL_SCREEN                                                                          |
|    INITIAL                                                                                       |
|    4445444222                                                                                    |
|    9E9491C000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    49004E0049005400490041004C00200020002000                                                      |
|SYST-REPID                                                                                        |
|    CL_WB_MANAGER=================CP                                                              |
|    4455454444445333333333333333334522222222                                                      |
|    3CF72FD1E1752DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004D0041004E0041004700450052003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME->REQUEST_QUEUE                                                                                 |
|    Table IT_39[0x16]                                                                             |
|    {O:17*\CLASS=CL_WB_MANAGER}\DATA=REQUEST_QUEUE                                                |
|    Table reference: 18                                                                           |
|    TABH+ 0(20) = F0BC29D7067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044060000                                        |
|    TABH+40(20) = 0000000012000000270000000000000010000000                                        |
|    TABH+60(20) = 0000000004600000908B01000400010601000000                                        |
|    store                   = 0xF0BC29D7067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 12    (ILLEGAL)                                                     |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 18    (0x12000000)                                                  |
|    label                   = 39    (0x27000000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 16    (0x10000000)                                                  |
|    loop                    = 0     (0x00000000)                                                  |
|    xtyp                    = PROG#000096/TYPE#000014                                             |
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
|    primKeyIsEmpty          = ItPrimKeyIsEmptyYes                                                 |
|    rtmSelectNoOrderBy      = 0                                                                   |
|    tstForceExtSort         = 0                                                                   |
|    ----- Shareable Table Header Data (Store) -----                                               |
|    tabi                    = 0x98BC29D7067F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 32    (0x20000000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|%_EVNTSENDER                                                                                      |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|%_EVNTSLOT                                                                                        |
|    1073741823                                                                                    |
|    FFF3                                                                                          |
|    FFFF                                                                                          |
|    FFFFFF3F                                                                                      |
|L_NAVIGATOR->REQUEST                                                                              |
|    {O:33*\CLASS=CL_WB_REQUEST}                                                                   |
|    D0002000                                                                                      |
|    E0001000                                                                                      |
|    DE00000021000000                                                                              |
|L_NAVIGATOR                                                                                       |
|    {O:35*\CLASS=CL_WB_NAVIGATOR_VIS_AS_DYNPRO}                                                   |
|    D0002000                                                                                      |
|    C0003000                                                                                      |
|    DC00000023000000                                                                              |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|L_REQUEST_ENTRY                                                                                   |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|IF_ARS_OBJECT_CHANGE_CHECK=>GC_CHANGE_MODE-DELETE                                                 |
|    D                                                                                             |
|    4                                                                                             |
|    4                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    4400                                                                                          |
|SY-SUBRC                                                                                          |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
|SWBM_C_TYPE_NO_OBJECT                                                                             |
|    INI                                                                                           |
|    444                                                                                           |
|    9E9                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    49004E004900                                                                                  |
|L_NAVIGATOR->NEW_NAVIGATION_CONTEXT                                                               |
|    PG_/PLCP/SAPLBGPROCESSING                                                                     |
|    545254452545444554445544422222222222222222222222222222222222222222222222222222222222222222222\|
|222222222222222222222222222222222222222222222222222222222222222222222222222222222222222           |
|    07FF0C30F310C2702F35339E700000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000000000000000000000000000000000000000000000000000000000000000000000000000           |
|    500047005F002F0050004C00430050002F005300410050004C0042004700500052004F00430045005300530049004\|
|E004700200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|020002000200020002000200020002000200020002000                                                     |
----------------------------------------------------------------------------------------------------
|No.       7 Ty.          METHOD                                                                   |
|Name  CL_WB_MANAGER=>PROCESS_REQUEST_QUEUE                                                        |
----------------------------------------------------------------------------------------------------
|P_ERROR                                                                                           |
|    {0;;;000;;;;}                                                                                 |
|    000020202020202020202020202020202020202020202030303020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000002000200020002000200020002000200020002000200020002000200020002000200020002000200020003\|
|0003000300020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|SWBCN_C_CID_PREVIOUS_ERROR                                                                        |
|    PREV_ERROR                                                                                    |
|    554554554522222222222222222222                                                                |
|    0256F522F200000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    50005200450056005F004500520052004F00520020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|SY-REPID                                                                                          |
|    CL_WB_MANAGER=================CP                                                              |
|    4455454444445333333333333333334522222222                                                      |
|    3CF72FD1E1752DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004D0041004E0041004700450052003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SYST-MSGNO                                                                                        |
|    0#1#2#B#A#P#I#R#E#T# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # \|
|# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #\|
| # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #                                  |
|    303030404050405040502020202020202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    001020201000902050400000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    300031003200420041005000490052004500540020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|P_ERROR-EXCEPTION                                                                                 |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
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
|SYST-REPID                                                                                        |
|    CL_WB_MANAGER=================CP                                                              |
|    4455454444445333333333333333334522222222                                                      |
|    3CF72FD1E1752DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004D0041004E0041004700450052003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|L_REQUEST_ENTRY->WB_DATA_CONTAINER                                                                |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|SWBCN_C_CTY_PREVIOUS_ERROR                                                                        |
|    WBERROR                                                                                       |
|    544554522222222222222222222222                                                                |
|    72522F200000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    570042004500520052004F00520020002000200020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|SWBM_C_ER_PERMISSION_FAILURE                                                                      |
|    4                                                                                             |
|    0000                                                                                          |
|    4000                                                                                          |
|    04000000                                                                                      |
|ME                                                                                                |
|    {O:17*\CLASS=CL_WB_MANAGER}                                                                   |
|    E0001000                                                                                      |
|    E0001000                                                                                      |
|    EE00000011000000                                                                              |
|L_REQUEST_ENTRY->WB_REQUEST                                                                       |
|    {O:10*\CLASS=CL_WB_REQUEST}                                                                   |
|    F0000000                                                                                      |
|    5000A000                                                                                      |
|    F50000000A000000                                                                              |
|L_REQUEST_ENTRY->WB_PROGRAM_STATE                                                                 |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|L_REQUEST_QUEUE_ENTRY-NAVIGATOR                                                                   |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY-SUBRC                                                                                          |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
|SY                                                                                                |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
|SWBM_C_OP_RENAME                                                                                  |
|    RENAME                                                                                        |
|    5444442222                                                                                    |
|    25E1D50000                                                                                    |
|    0000000000                                                                                    |
|    0000000000                                                                                    |
|    520045004E0041004D0045002000200020002000                                                      |
|%_MESS_DISPLAY_VAR                                                                                |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.       6 Ty.          METHOD                                                                   |
|Name  CL_WB_MANAGER=>IF_WB_MANAGER~SET_WORKSPACE                                                  |
----------------------------------------------------------------------------------------------------
|P_WORKSPACE                                                                                       |
|    {O:23*\CLASS=CL_WB_WORKSPACE}                                                                 |
|    E0001000                                                                                      |
|    80007000                                                                                      |
|    E800000017000000                                                                              |
|P_WB_ERROR                                                                                        |
|    {0;;;000;;;;}                                                                                 |
|    000020202020202020202020202020202020202020202030303020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000002000200020002000200020002000200020002000200020002000200020002000200020002000200020003\|
|0003000300020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|L_WB_REQUEST_ENTRY                                                                                |
|    {O:24*\CLASS=CL_WB_REQUEST_ENTRY}                                                             |
|    E0001000                                                                                      |
|    70008000                                                                                      |
|    E700000018000000                                                                              |
|SY-REPID                                                                                          |
|    CL_WB_MANAGER=================CP                                                              |
|    4455454444445333333333333333334522222222                                                      |
|    3CF72FD1E1752DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F004D0041004E0041004700450052003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY-SUBRC                                                                                          |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
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
|ME                                                                                                |
|    {O:17*\CLASS=CL_WB_MANAGER}                                                                   |
|    E0001000                                                                                      |
|    E0001000                                                                                      |
|    EE00000011000000                                                                              |
|L_WB_REQUEST_ENTRY->WB_REQUEST                                                                    |
|    {O:10*\CLASS=CL_WB_REQUEST}                                                                   |
|    F0000000                                                                                      |
|    5000A000                                                                                      |
|    F50000000A000000                                                                              |
|L_WB_REQUEST_ENTRY->WB_PROGRAM_STATE                                                              |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|L_WB_REQUEST_ENTRY->WB_DATA_CONTAINER                                                             |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.       5 Ty.          METHOD                                                                   |
|Name  CL_WB_STARTUP=>START_INTERNAL                                                               |
----------------------------------------------------------------------------------------------------
|P_WB_REQUEST_SET                                                                                  |
|    Table IT_32[1x8]                                                                              |
|    \FUNCTION-POOL=WB_MANAGER\DATA=G_REQUEST_SET                                                  |
|    Table reference: 16                                                                           |
|    TABH+ 0(20) = A00B21D7067F00000000000000000000B0B529D7                                        |
|    TABH+20(20) = 067F000000000000000000000100000044020000                                        |
|    TABH+40(20) = 0000000010000000200000000100000008000000                                        |
|    TABH+60(20) = FFFFFFFF045C0000507100000400410602000000                                        |
|    store                   = 0xA00B21D7067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0xB0B529D7067F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 16    (0x10000000)                                                  |
|    label                   = 32    (0x20000000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 8     (0x08000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000092/TYPE#000008                                             |
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
|    tabi                    = 0x680B21D7067F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 22    (0x16000000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0x40B429D7067F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|    {O:10*\CLASS=CL_WB_REQUEST}                                                                   |
|    F0000000                                                                                      |
|    5000A000                                                                                      |
|    F50000000A000000                                                                              |
|FORCE_NEW_WBMANAGER_INSTANCE                                                                      |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|P_WB_DATA_CONTAINER                                                                               |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|L_WB_WINDOW_MANAGER                                                                               |
|    {O:16*\CLASS=CL_WB_WINDOW_MANAGER}                                                            |
|    E0001000                                                                                      |
|    F0000000                                                                                      |
|    EF00000010000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|L_IF_WB_MANAGER                                                                                   |
|    {O:17*\CLASS=CL_WB_MANAGER}                                                                   |
|    E0001000                                                                                      |
|    E0001000                                                                                      |
|    EE00000011000000                                                                              |
|L_WORKSPACE                                                                                       |
|    {O:23*\CLASS=CL_WB_WORKSPACE}                                                                 |
|    E0001000                                                                                      |
|    80007000                                                                                      |
|    E800000017000000                                                                              |
|<WB_REQUEST>                                                                                      |
|    {O:10*\CLASS=CL_WB_REQUEST}                                                                   |
|    F0000000                                                                                      |
|    5000A000                                                                                      |
|    F50000000A000000                                                                              |
|L_WB_REQUEST_ENTRY                                                                                |
|    {O:24*\CLASS=CL_WB_REQUEST_ENTRY}                                                             |
|    E0001000                                                                                      |
|    70008000                                                                                      |
|    E700000018000000                                                                              |
|SYST                                                                                              |
|    {0;0;1;0;0;0;0;0;0;0;1;4;1;0;1;0;0;0;0;24;0;0;0;8;120;0;0;0;0;0;0;0;0;0;0;0;0;0;26;121;0;120;\|
|0;3600;;C;0000;4;0;0;;;;D;2;;;X;3000;;;;;;0;__S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;\|
|;;S4Q;Linux;WB_WITH_TOOL_MODI;758;SBGRFCMON;WB_SELECT_OBJECT;;;00                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000\|
|0007000000000000000000000000000000000000000000000000000000010007000000070000000100020400000000000\|
|00002020204000000020205030303030202020202020202000000050505020202                                 |
|    000000001000000000000000000000000000000010004000100000001000000000000000000080000000000000008\|
|00080000000000000000000000000000000000000000000000000000000A00090000000800000000E0000300040000C00\|
|00C000000040002000000080300000000000000000000000000000F0F03000000                                 |
|    000000000000000001000000000000000000000000000000000000000000000000000000000000000100000004000\|
|0000100000000000000010000000000000000000000000000000000000018000000000000000000000000000000080000\|
|0078000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|000000000000000001A00000079000000000000007800000000000000100E000020004300000004000000000C00000000\|
|0C00200020002000440000000200000020002000580033003000300030002000200020002000200020002000200000000\|
|00000005F005F0053002000200020                                                                     |
|L_WB_REQUEST_ENTRY->WB_REQUEST                                                                    |
|    {O:10*\CLASS=CL_WB_REQUEST}                                                                   |
|    F0000000                                                                                      |
|    5000A000                                                                                      |
|    F50000000A000000                                                                              |
|L_WB_REQUEST_ENTRY->WB_DATA_CONTAINER                                                             |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|SYST-REPID                                                                                        |
|    CL_WB_STARTUP=================CP                                                              |
|    4455455545555333333333333333334522222222                                                      |
|    3CF72F3412450DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F0053005400410052005400550050003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SY-REPID                                                                                          |
|    CL_WB_STARTUP=================CP                                                              |
|    4455455545555333333333333333334522222222                                                      |
|    3CF72F3412450DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F0053005400410052005400550050003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|ME->ERROR                                                                                         |
|    {0;;;000;;;;}                                                                                 |
|    000020202020202020202020202020202020202020202030303020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000002000200020002000200020002000200020002000200020002000200020002000200020002000200020003\|
|0003000300020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       4 Ty.          MODULE (PAI)                                                             |
|Name  MANAGER_START                                                                               |
----------------------------------------------------------------------------------------------------
|SYST-REPID                                                                                        |
|    SAPLWB_MANAGER                                                                                |
|    5454545444444522222222222222222222222222                                                      |
|    310C72FD1E175200000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C00570042005F004D0041004E0041004700450052002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|G_STARTUP                                                                                         |
|    {O:13*\CLASS=CL_WB_STARTUP}                                                                   |
|    F0000000                                                                                      |
|    2000D000                                                                                      |
|    F20000000D000000                                                                              |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|G_REQUEST_SET                                                                                     |
|    Table IT_32[1x8]                                                                              |
|G_FORCE_NEW_WBMANAGER_INSTANCE                                                                    |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|G_DATA_CONTAINER                                                                                  |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
----------------------------------------------------------------------------------------------------
|No.       3 Ty.          FUNCTION                                                                 |
|Name  WB_MANAGER_START                                                                            |
----------------------------------------------------------------------------------------------------
|FORCE_NEW_WBMANAGER_INSTANCE                                                                      |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|REQUEST_SET                                                                                       |
|    Table IT_28[1x8]                                                                              |
|    \PROGRAM=RS_EDTR_START_NEW_MODE\DATA=L_REQUEST_SET                                            |
|    Table reference: 12                                                                           |
|    TABH+ 0(20) = A00B21D7067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000100000044020000                                        |
|    TABH+40(20) = 000000000C0000001C0000000100000008000000                                        |
|    TABH+60(20) = FFFFFFFF04000000007900000400410602000000                                        |
|    store                   = 0xA00B21D7067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 12    (0x0C000000)                                                  |
|    label                   = 28    (0x1C000000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 8     (0x08000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000000/TYPE#000019                                             |
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
|    tabi                    = 0x680B21D7067F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 22    (0x16000000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    {O:10*\CLASS=CL_WB_REQUEST}                                                                   |
|    F0000000                                                                                      |
|    5000A000                                                                                      |
|    F50000000A000000                                                                              |
|STARTUP                                                                                           |
|    {O:13*\CLASS=CL_WB_STARTUP}                                                                   |
|    F0000000                                                                                      |
|    2000D000                                                                                      |
|    F20000000D000000                                                                              |
|DATA_CONTAINER                                                                                    |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|IE_OBJTYPE_SCOPE=>C_WB_BROWSERTOOL                                                                |
|    WB_BROWSERTOOL                                                                                |
|    545454554554442222222222222222                                                                |
|    72F22F73524FFC0000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    570042005F00420052004F00570053004500520054004F004F004C002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|SWBM_C_TYPE_AUNIT_TEST_BROWSER                                                                    |
|    AUN                                                                                           |
|    454                                                                                           |
|    15E                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    410055004E00                                                                                  |
|SWBM_C_TYPE_WDY_TEXT_BROWSER                                                                      |
|    WTX                                                                                           |
|    555                                                                                           |
|    748                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    570054005800                                                                                  |
|G_STARTUP                                                                                         |
|    {O:13*\CLASS=CL_WB_STARTUP}                                                                   |
|    F0000000                                                                                      |
|    2000D000                                                                                      |
|    F20000000D000000                                                                              |
|G_REQUEST_SET                                                                                     |
|    Table IT_32[1x8]                                                                              |
|G_DATA_CONTAINER                                                                                  |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|SY-DYNNR                                                                                          |
|    3000                                                                                          |
|    3333                                                                                          |
|    3000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    3300300030003000                                                                              |
----------------------------------------------------------------------------------------------------
|No.       2 Ty.          METHOD                                                                   |
|Name  CL_WB_STARTUP=>START                                                                        |
----------------------------------------------------------------------------------------------------
|P_WB_REQUEST_SET                                                                                  |
|    Table IT_28[1x8]                                                                              |
|FORCE_NEW_WBMANAGER_INSTANCE                                                                      |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|P_WB_ERROR                                                                                        |
|    {0;;;000;;;;}                                                                                 |
|    000020202020202020202020202020202020202020202030303020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000002000200020002000200020002000200020002000200020002000200020002000200020002000200020003\|
|0003000300020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|P_WB_DATA_CONTAINER                                                                               |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SY-REPID                                                                                          |
|    CL_WB_STARTUP=================CP                                                              |
|    4455455545555333333333333333334522222222                                                      |
|    3CF72F3412450DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F0053005400410052005400550050003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|%_##TVREG_001                                                                                     |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|STARTUP                                                                                           |
|    {O:13*\CLASS=CL_WB_STARTUP}                                                                   |
|    F0000000                                                                                      |
|    2000D000                                                                                      |
|    F20000000D000000                                                                              |
|SYST-REPID                                                                                        |
|    CL_WB_STARTUP=================CP                                                              |
|    4455455545555333333333333333334522222222                                                      |
|    3CF72F3412450DDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F00570042005F0053005400410052005400550050003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SY-SUBRC                                                                                          |
|    8                                                                                             |
|    0000                                                                                          |
|    8000                                                                                          |
|    08000000                                                                                      |
|STARTUP->ERROR                                                                                    |
|    {0;;;000;;;;}                                                                                 |
|    000020202020202020202020202020202020202020202030303020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    000000002000200020002000200020002000200020002000200020002000200020002000200020002000200020003\|
|0003000300020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       1 Ty.          EVENT                                                                    |
|Name  START-OF-SELECTION                                                                          |
----------------------------------------------------------------------------------------------------
|SY-REPID                                                                                          |
|    RS_EDTR_START_NEW_MODE                                                                        |
|    5554455555455544554444222222222222222222                                                      |
|    23F5442F34124FE57FDF45000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    520053005F0045004400540052005F00530054004100520054005F004E00450057005F004D004F004400450020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|L_EDITOR_STATE-EDIT-BUF_NAME                                                                      |
|                                                                                                  |
|    222222222222222222222222222222222222222222222222222222222222                                  |
|    000000000000000000000000000000000000000000000000000000000000                                  |
|    000000000000000000000000000000000000000000000000000000000000                                  |
|    000000000000000000000000000000000000000000000000000000000000                                  |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|00200020002000200020002000200020002000200020002000                                                |
|BUF_NAME                                                                                          |
|                                                                                                  |
|    2222222222222222222222                                                                        |
|    0000000000000000000000                                                                        |
|    0000000000000000000000                                                                        |
|    0000000000000000000000                                                                        |
|    2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000      |
|L_EDITOR_STATE-EDIT-BUF_CUOFFS                                                                    |
|    000                                                                                           |
|    333                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    300030003000                                                                                  |
|OFFSET                                                                                            |
|    000000                                                                                        |
|    333333                                                                                        |
|    000000                                                                                        |
|    000000                                                                                        |
|    000000                                                                                        |
|    300030003000300030003000                                                                      |
|L_EDITOR_STATE-CONTENT                                                                            |
|    Table IT_29[1x8]                                                                              |
|    \PROGRAM=RS_EDTR_START_NEW_MODE\DATA=L_EDITOR_STATE-CONTENT                                   |
|    Table reference: 13                                                                           |
|    TABH+ 0(20) = 300D21D7067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000100000044020000                                        |
|    TABH+40(20) = 000000000D0000001D0000000100000008000000                                        |
|    TABH+60(20) = FFFFFFFF04000000F07900000400410602000000                                        |
|    store                   = 0x300D21D7067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 4     (cmpSingleEq)                                                 |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 13    (0x0D000000)                                                  |
|    label                   = 29    (0x1D000000)                                                  |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 8     (0x08000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000000/TYPE#000024                                             |
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
|    tabi                    = 0xF80C21D7067F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 23    (0x17000000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 1     (0x01000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 2     (0x02000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
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
|SYST-REPID                                                                                        |
|    RS_EDTR_START_NEW_MODE                                                                        |
|    5554455555455544554444222222222222222222                                                      |
|    23F5442F34124FE57FDF45000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    520053005F0045004400540052005F00530054004100520054005F004E00450057005F004D004F004400450020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|L_CONTAINER                                                                                       |
|    {O:12*\CLASS=CL_WB_DATA_CONTAINER}                                                            |
|    F0000000                                                                                      |
|    3000C000                                                                                      |
|    F30000000C000000                                                                              |
|SWBCN_C_CID_NEW_STATE                                                                             |
|    EDITOR_STATE                                                                                  |
|    444545555454222222222222222222                                                                |
|    5494F2F34145000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    45004400490054004F0052005F0053005400410054004500200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|L_EDITOR_STATE                                                                                    |
|    {{;;;;;;;;;;;;;;000000;000000;000;;;000;;;;;;;;;};IT_29[1x8];;SAPLS38E;SET_CALL_BACK;;;}      |
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
|SWBCN_C_CTY_NEW_STATE                                                                             |
|    EDCONT                                                                                        |
|    444445222222222222222222222222                                                                |
|    543FE4000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    4500440043004F004E005400200020002000200020002000200020002000200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
|%_SSCR[]                                                                                          |
|    Table IT_1[9x492]                                                                             |
|    \PROGRAM=RS_EDTR_START_NEW_MODE\DATA=%_SSCR[]                                                 |
|    Table reference: 0                                                                            |
|    TABH+ 0(20) = 00E51AD7067F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000900000044040000                                        |
|    TABH+40(20) = 00000000000000000100000009000000EC010000                                        |
|    TABH+60(20) = FFFFFFFF04000000607600000900400602000000                                        |
|    store                   = 0x00E51AD7067F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0000000000000000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 0     (0x00000000)                                                  |
|    label                   = 1     (0x01000000)                                                  |
|    fill                    = 9     (0x09000000)                                                  |
|    leng                    = 492   (0xEC010000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000000/TYPE#000005                                             |
|    occu                    = 9     (0x09000000)                                                  |
|    occu0                   = 0                                                                   |
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
|    tabi                    = 0xE08822D7067F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 2     (0x02000000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 2     (0x02000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 9     (0x09000000)                                                  |
|    lineAlloc               = 9     (0x09000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 3     (0x03000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|                                                                                                  |
|    {OBJNAME;1000000;P;180;C;CHAR;SEU_OBJKEY;;;;0;360;;;;;;01;00;00;00}                           |
|    404040404040402044005020B00040404040505040505040404040405020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    F020A0E010D0500002F0000040003030801020305050F0F020A0B0509000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    4F0042004A004E0041004D004500200040420F0050002000B4000000430043004800410052005300450055005F004\|
|F0042004A004B004500590020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|                                                                                                  |
|    {OBJTYPE;1000001;P;3;C;CHAR;SEU_OBJTYP;;;;0;6;;;;;;01;00;00;00}                               |
|    404040505050402044005020000040404040505040505040404050505020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    F020A0409000500012F0000030003030801020305050F0F020A040900000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    4F0042004A005400590050004500200041420F005000200003000000430043004800410052005300450055005F004\|
|F0042004A0054005900500020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|                                                                                                  |
|    {OP;1000002;P;10;C;CHAR;SEU_ACTION;;;;0;20;;;;;;01;00;00;00}                                  |
|    405020202020202044005020000040404040505040505040405040404020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    F00000000000000022F00000A0003030801020305050F010304090F0E000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    4F00500020002000200020002000200042420F00500020000A000000430043004800410052005300450055005F004\|
|1004300540049004F004E0020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|                                                                                                  |
|    {LINE;1000003;P;6;N;NUMC;;;;;0;12;;;;;;01;00;00;00}                                           |
|    404040402020202044005020000040405040402020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    C090E0500000000032F000006000E0E050D0300000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    4C0049004E004500200020002000200043420F0050002000060000004E004E0055004D00430020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|                                                                                                  |
|    {TOPLINE;1000004;P;6;N;NUMC;;;;;0;12;;;;;;01;00;00;00}                                        |
|    504050404040402044005020000040405040402020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    40F000C090E0500042F000006000E0E050D0300000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    54004F0050004C0049004E004500200044420F0050002000060000004E004E0055004D00430020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|L_REQUEST_SET                                                                                     |
|    Table IT_28[1x8]                                                                              |
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
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Information About Memory Usage                                                                    |
----------------------------------------------------------------------------------------------------
|Memory Consumption                                                                                |
----------------------------------------------------------------------------------------------------
|Memory Area                                                      Bytes                            |
----------------------------------------------------------------------------------------------------
|Extended Memory (EM)                                        34.810.944                            |
|PRIV Memory (Heap)                                                   0                            |
|SAP Paging                                                     155.648                            |
----------------------------------------------------------------------------------------------------
|MM Memory Management                                    Bytes / Number                            |
----------------------------------------------------------------------------------------------------
|Used Memory                                                  9.737.280                            |
|Free Memory                                                  6.133.840                            |
|Largest Free Block                                           1.458.080                            |
|Used Blocks                                                     11.781                            |
|Free Blocks                                                         87                            |
|Maximum Used Memory                                         14.412.984                            |
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
|     27028               503498 CL_GUI_FRONTEND_SERVICES======CM01Z               6 20260219083516|
|         3 CL_GUI_RESOURCE_MEMORY                                                                 |
|$DEFAULT_INSTANCE$                                                               110 R            |
|      8013               503500 CL_GUI_RESOURCES==============CM00G             168 20260219083517|
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Internal notes                                                                                    |
|    The termination was triggered in function "cg_generate" of the SAP kernel, in                 |
|    line 1059 of module "//bas/GIT/krn/abap/gen/cgmain2.c#0".                                     |
|    The internal operation just processed is "mvqw".                                              |
|    Internal mode started at 20260219083516.                                                      |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Active Calls in SAP Kernel                                                                        |
----------------------------------------------------------------------------------------------------
|Lines of C stack in kernel (structure different on each platform)                                 |
----------------------------------------------------------------------------------------------------
|# buildID: 25c0d917084e38b8580fb288faf763c6c79f622e                                               |
|dw.sapS4Q_D00;0x9712b6;[S](LinStackBacktrace(void**, int, int)+0xa8)[0x5608ea3712b6]              |
|dw.sapS4Q_D00;0x9758ba;[S](LinStack(_IO_FILE*)+0x48)[0x5608ea3758ba]                              |
|dw.sapS4Q_D00;0x970d36;[S](CTrcStack2+0x4b)[0x5608ea370d36]                                       |
|dw.sapS4Q_D00;0xc58211;[S](rabax_CStackSave()+0x9e)[0x5608ea658211]                               |
|dw.sapS4Q_D00;0xc536f3;[S](rabax(char16_t const*, char16_t const*, int, char16_t const*, void con\|
|st*) [clone .cold]+0x102b)[0x5608ea6536f3]                                                        |
|dw.sapS4Q_D00;0x134ac44;[S](cg_generate+0x14a7)[0x5608ead4ac44]                                   |
|dw.sapS4Q_D00;0xc19221;[S](ab_jsych()+0x10e3)[0x5608ea619221]                                     |
|dw.sapS4Q_D00;0x2d5c5d3;[S](ab_extri()+0x1b3)[0x5608ec75c5d3]                                     |
|dw.sapS4Q_D00;0x2d8e311;[S](ab_xevent(char16_t const*)+0x31)[0x5608ec78e311]                      |
|dw.sapS4Q_D00;0x2d5a607;[S](ab_dstep+0x57)[0x5608ec75a607]                                        |
|dw.sapS4Q_D00;0x2d240b4;[S](dynpmcal(DINFDUMY*, STPDUMMY*)+0x5e4)[0x5608ec7240b4]                 |
|dw.sapS4Q_D00;0x2d24a8a;[S](dynppai0(DINFDUMY*)+0x29a)[0x5608ec724a8a]                            |
|dw.sapS4Q_D00;0x2d22ef9;[S](dynprctl(DINFDUMY*)+0x129)[0x5608ec722ef9]                            |
|dw.sapS4Q_D00;0x2d2211c;[S](dynpen00(int)+0x3bc)[0x5608ec72211c]                                  |
|dw.sapS4Q_D00;0x9598c7;[S](GuiHandler::handleRequest(REQUEST_BUF*, bool)+0x8ed)[0x5608ea3598c7]   |
|dw.sapS4Q_D00;0x30bf333;[S](ThStart()+0x7f3)[0x5608ecabf333]                                      |
|dw.sapS4Q_D00;0x30875f4;[S](DpMain+0x3d4)[0x5608eca875f4]                                         |
|dw.sapS4Q_D00;0x2ca37fe;[S](main+0x2e)[0x5608ec6a37fe]                                            |
|# buildID: 2c8359b67579ed1cba5cce7875abfd60fa954ca7                                               |
|libc.so.6;0x351fd;[S](__libc_start_main+0xef)[0x7f0fa0dea1fd]                                     |
|# buildID: 25c0d917084e38b8580fb288faf763c6c79f622e                                               |
|dw.sapS4Q_D00;0x308715a;[S](_start+0x2a)[0x5608eca8715a]                                          |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|List of ABAP programs affected                                                                    |
----------------------------------------------------------------------------------------------------
|Index |Ty.|Program                                 |Group |Date      |Time    |Size     |Lang.    |
----------------------------------------------------------------------------------------------------
|     0|Prg|RS_EDTR_START_NEW_MODE                  |     0|29.04.2016|13:31:28|    39936|D        |
|     1|Prg|SAPMSSY0                                |     1|31.03.2023|15:25:25|   120832|D        |
|     2|Prg|SAPMSSYD                                |     1|03.05.2025|07:02:38|    34816|D        |
|     3|Prg|SAPFSYSCALLS                            |     1|01.06.2015|12:30:31|     7168|D        |
|     4|Prg|CL_DYNP_CUA_DEFAULT===========CP        |     4|22.01.2021|09:24:38|    10240|D        |
|     5|Prg|RSDBRUNT                                |     0|08.08.2024|10:13:36|   323584|D        |
|     6|Typ|RSSCR                                   |     0|31.05.2013|08:11:11|     6144|         |
|     7|Prg|RSDBSPBL                                |     0|01.04.2022|13:52:52|    82944|D        |
|     8|Prg|SAPDB__S                                |     0|15.03.2020|19:32:16|    21504|D        |
|     9|Typ|VARID                                   |     0|12.05.1997|16:51:30|     5120|         |
|    10|Typ|SSCRFIELDS                              |     0|31.05.2013|08:13:14|     5120|         |
|    11|Prg|RSDBSPVA                                |     0|03.05.2025|08:07:31|   221184|D        |
|    12|Prg|%_CSYDB0                                |     0|01.04.2022|13:52:51|    43008|D        |
|    13|Typ|RSVAMEMKEY                              |     0|07.05.1997|13:07:49|     3072|         |
|    14|Prg|RSDBSPMC                                |     0|01.04.2022|13:52:52|    94208|D        |
|    15|Typ|DDSHDESCR                               |     0|03.09.1997|03:05:16|     4096|         |
|    16|Typ|SPPARAMS                                |     0|07.05.1997|13:10:38|     3072|         |
|    17|Prg|SAPLICON                                |    17|01.06.2015|12:30:31|    54272|D        |
|    18|Prg|%_CICON                                 |    17|16.04.2021|19:43:34|   117760|D        |
|    19|Typ|ICONT                                   |     0|31.05.2013|08:04:28|     3072|         |
|    20|Prg|SAPLSABE                                |    20|07.04.2016|14:27:01|    15360|D        |
|    21|Prg|SAPLSECU                                |    21|15.06.2023|07:05:35|   156672|D        |
|    22|Typ|RSSUBINFO                               |     0|31.05.2013|08:11:14|     4096|         |
|    23|Typ|RSEXFCODE                               |     0|13.08.1997|12:52:57|     2048|         |
|    24|Prg|SAPLDSYA                                |    24|15.06.2023|07:05:19|    91136|D        |
|    25|Prg|SAPFSDS1                                |    24|19.11.2020|11:23:15|    76800|D        |
|    26|Typ|TDCLD                                   |     0|03.08.2015|16:12:59|     6144|         |
|    27|Prg|SAPLSDOD                                |    27|18.03.2023|11:02:31|   216064|D        |
|    28|Typ|DOKIL                                   |     0|12.05.1997|16:46:17|     4096|         |
|    29|Prg|SAPLSDOC                                |    29|15.06.2023|07:05:19|  1815552|D        |
|    30|Prg|SAPCNVE                                 |    30|26.01.2018|14:57:35|     8192|D        |
|    31|Prg|SAPLLANG                                |    31|26.01.2023|17:38:15|    18432|D        |
|    32|Typ|T002                                    |     0|21.01.2014|10:06:21|     3072|         |
|    33|Typ|RSPARINT                                |     0|10.04.1995|09:58:38|     2048|         |
|    34|Prg|SAPLSCNT                                |    34|28.05.2018|11:34:27|    32768|D        |
|    35|Typ|DYCBOX                                  |     0|20.08.1998|11:16:53|     3072|         |
|    36|Prg|SAPLSVSM                                |    36|10.11.2022|19:49:55|    31744|D        |
|    37|Prg|SAPLSGUI                                |    37|31.03.2023|15:25:25|   103424|D        |
|    38|Prg|SAPLSTTM                                |    38|10.11.2022|19:49:55|    80896|D        |
|    39|Prg|SAPLSBDC                                |    39|18.03.2023|11:02:33|    71680|D        |
|    40|Prg|CL_DATAPROVIDER===============CP        |    40|28.05.2018|11:34:26|    53248|D        |
|    41|Prg|SAPSHDTV                                |    34|31.03.2023|15:25:24|    91136|D        |
|    42|Prg|SAPFS_SECLOG                            |     1|10.11.2022|19:49:52|    25600|D        |
|    43|Typ|INDX                                    |     0|29.07.1998|18:38:15|     7168|         |
|    44|Prg|SDYNPRAL                                |     1|02.05.2016|13:52:39|     8192|D        |
|    45|Prg|SAPLSDYNPRAL                            |    45|10.11.2022|19:49:54|    16384|D        |
|    46|Typ|DYNP_RAL_ID                             |     0|27.11.2013|14:25:51|     3072|         |
|    47|Typ|DYNP_RAL_SCREEN                         |     0|31.05.2013|08:00:52|     3072|         |
|    48|Prg|CL_DYNP_RAL_API===============CP        |    48|26.01.2023|17:37:52|   119808|D        |
|    49|Prg|CL_SRAL_RECORDER_FACTORY_DYNP=CP        |    49|26.01.2023|17:38:14|    21504|D        |
|    50|Prg|SRAL_DYNP_ADAPTER                       |    48|10.11.2022|19:49:55|    27648|D        |
|    51|Prg|CL_SRAL_FACTORY_DYNP==========CP        |    51|26.01.2023|17:38:14|    45056|D        |
|    52|Prg|CL_SRAL_SWITCH_CLIENT=========CP        |    52|10.11.2022|19:49:43|    10240|D        |
|    53|Prg|CL_SRAL_DB_PARAMS=============CP        |    53|10.11.2022|19:49:43|    23552|D        |
|    54|Prg|RADSHPBOPREFLIGHT                       |     1|10.11.2022|19:49:51|    18432|D        |
|    55|Prg|RADSHPBOSTATUS                          |     1|10.11.2022|19:49:51|    20480|D        |
|    56|Prg|RADSHPBOSWITCH                          |     1|10.11.2022|19:49:51|    21504|D        |
|    57|Prg|RADSHUPGRADE                            |     1|10.11.2022|19:49:51|    21504|D        |
|    58|Prg|WPBHELP_REGISTRATION                    |     1|06.04.2021|14:01:49|    11264|D        |
|    59|Prg|SAPFGUICNTL                             |     1|17.12.2018|10:04:08|    28672|D        |
|    60|Prg|SAPLOLEA                                |    60|10.11.2022|19:49:53|   120832|D        |
|    61|Prg|SAPLSFES                                |    61|16.05.2023|13:23:16|   323584|D        |
|    62|Prg|CL_SYSTEM_TRANSACTION_STATE===CP        |    62|31.03.2023|15:25:24|   107520|D        |
|    63|Prg|SAPLTHFB                                |    63|15.06.2023|07:05:24|   518144|D        |
|    64|Prg|SAPLSPLUGIN                             |    64|03.11.2021|19:19:46|     8192|D        |
|    65|Typ|ARFCRDATA                               |     0|08.09.2011|14:25:38|     7168|         |
|    66|Prg|SAPLGRFC                                |    66|01.06.2015|12:30:31|    15360|D        |
|    67|Typ|SWCBCONT                                |     0|08.09.2011|14:24:16|     4096|         |
|    68|Typ|OLE_VERBS                               |     0|04.04.1995|16:02:20|     3072|         |
|    69|Typ|OLE_PA                                  |     0|04.04.1995|16:02:19|     3072|         |
|    70|Prg|SAPFS_DYNPRO_HOOKS                      |     1|26.01.2018|15:16:09|     7168|D        |
|    71|Prg|SAPLSALV_IDA_IMPL_UI_SCREEN             |    71|18.03.2023|11:03:29|    16384|D        |
|    72|Prg|CL_SALV_GUI_ROUNDTRIP_BROKER==CP        |    72|18.03.2023|11:03:29|    53248|D        |
|    73|Prg|SAPLSPOF                                |    73|18.07.2023|12:08:17|   220160|D        |
|    74|Typ|SSCRTEXTS                               |     0|03.09.1997|03:12:33|     4096|         |
|    75|Prg|SAPLSEWORKINGAREA                       |    75|26.05.2023|05:06:55|   660480|D        |
|    76|Typ|SABAP_INACT_OBJ                         |     0|09.09.2004|14:17:47|     3072|         |
|    77|Typ|DWWASYNC                                |     0|31.05.2013|08:00:52|     2048|         |
|    78|Typ|DWINACTIV                               |     0|31.05.2013|08:00:52|     3072|         |
|    79|Typ|ACT_WORK_ITEM                           |     0|31.05.2013|07:56:54|     5120|         |
|    80|Prg|SAPLSABAPWORKINGAREA                    |    80|16.05.2023|13:23:18|    41984|D        |
|    81|Prg|CL_WB_WORKING_AREA============CP        |    81|16.05.2023|13:23:18|    93184|D        |
|    82|Typ|SABAP_INACTIVE                          |     0|31.05.2013|08:00:35|     4096|         |
|    83|Prg|CL_WB_PROGRAM_STATE===========CP        |    83|16.05.2023|13:23:29|    19456|D        |
|    84|Prg|%_CCNTL                                 |    83|28.05.2018|11:34:26|    16384|D        |
|    85|Prg|CL_WB_REQUEST=================CP        |    85|16.05.2023|13:23:20|    50176|D        |
|    86|Typ|WBOBJTYPE                               |     0|08.09.2011|14:25:26|     2048|         |
|    87|Prg|CL_WB_OPERATION===============CP        |    87|08.11.2019|10:12:31|    29696|D        |
|    88|Prg|CL_WB_DATA_CONTAINER==========CP        |    88|17.12.2018|10:03:59|    14336|D        |
|    89|Typ|EDCONT                                  |     0|31.07.2015|11:59:53|    10240|         |
|    90|Prg|CL_WB_STARTUP=================CP        |    90|16.05.2023|13:23:19|    18432|D        |
|    91|Typ|WBERROR                                 |     0|31.05.2013|08:18:21|     4096|         |
|    92|Prg|SAPLWB_MANAGER                          |    92|16.05.2023|13:23:20|    33792|D        |
|    93|Prg|CL_WB_REGISTRY================CP        |    93|28.04.2023|07:29:37|    63488|D        |
|    94|Prg|CL_WB_LAYOUT_MANAGER==========CP        |    94|07.02.2022|10:20:08|    43008|D        |
|    95|Prg|CL_WB_WINDOW_MANAGER==========CP        |    95|16.12.2022|13:58:33|    25600|D        |
|    96|Prg|CL_WB_MANAGER=================CP        |    96|16.05.2023|13:23:29|   111616|D        |
|    97|Prg|CL_WB_TOOL_MANAGER============CP        |    97|10.11.2022|19:49:46|    34816|D        |
|    98|Prg|CL_WB_MANAGER_FCODE_HANDLER===CP        |    98|16.05.2023|13:23:19|    57344|D        |
|    99|Prg|CL_WB_WINDOW_EVENTS===========CP        |    99|01.06.2015|12:30:30|     8192|D        |
|   100|Prg|IF_WB_CONTROL_TOOL============IP        |    96|08.11.2019|10:12:32|     6144|D        |
|   101|Prg|CL_WB_OBJECT_EVENTS===========CP        |   101|01.06.2015|12:30:30|    12288|D        |
|   102|Prg|CL_WB_SETTINGS================CP        |   102|01.06.2015|12:30:30|     8192|D        |
|   103|Prg|CL_WB_WORKSPACE===============CP        |   103|21.04.2017|12:35:14|    11264|D        |
|   104|Prg|CL_WB_REQUEST_ENTRY===========CP        |   104|08.11.2019|10:12:32|     9216|D        |
|   105|Prg|%_CWMNGR                                |    96|01.06.2015|12:30:30|    18432|D        |
|   106|Prg|CL_WB_REGISTRY_SHM_AREA=======CP        |   106|10.11.2022|19:49:46|    47104|D        |
|   107|Prg|CL_SHM_AREA===================CP        |   107|10.11.2022|19:49:42|    63488|D        |
|   108|Prg|CL_ABAP_MEMORY_AREA===========CP        |   108|10.11.2022|19:49:27|    12288|D        |
|   109|Prg|CL_SHM_SERVICE================CP        |   109|10.11.2022|19:49:42|    55296|D        |
|   110|Prg|CL_WB_REGISTRY_SHM_METADATA===CP        |   110|15.06.2023|07:05:36|    63488|D        |
|   111|Typ|WBOBJTYPE_DISC_RES_T                    |     0|26.01.2018|14:09:04|     3072|         |
|   112|Typ|WBOBJTYPE_RESOURCE                      |     0|28.04.2023|07:25:18|     5120|         |
|   113|Typ|WBOBJECTTYPE_RT                         |     0|28.04.2023|07:25:17|    10240|         |
|   114|Typ|WBOBJTYPE_SCOPE_RT                      |     0|08.09.2011|14:25:28|     3072|         |
|   115|Typ|WVOBJECTTYPE_RT                         |     0|08.09.2011|14:28:48|     3072|         |
|   116|Prg|CL_WB_REGISTRY_SHM_ROOT=======CP        |   116|01.06.2015|12:30:30|     8192|D        |
|   117|Typ|WBREGISTRY                              |     0|08.09.2011|14:28:45|     3072|         |
|   118|Prg|CL_WB_SOURCE_REQ_DISPATCHER===CP        |   118|16.05.2023|13:23:20|   100352|D        |
|   119|Prg|IF_WB_DECISION_CLASS==========IP        |    93|10.11.2022|19:49:51|     6144|D        |
|   120|Prg|CL_WB_OBJECT_TYPE=============CP        |   120|16.05.2023|13:23:43|   122880|D        |
|   121|Prg|IF_WB_OBJTYPE_PROVIDER========IP        |    93|16.12.2022|13:58:33|     8192|D        |
|   122|Typ|WBACCESS_TOOL                           |     0|01.06.2015|10:55:50|     7168|         |
|   123|Prg|IE_OBJTYPE_SCOPE==============IP        |    93|25.02.2022|16:23:31|     8192|D        |
|   124|Prg|SAPLSUNI                                |   124|28.04.2023|07:29:53|   290816|D        |
|   125|Typ|TFDIR                                   |     0|29.03.2018|15:55:42|     4096|         |
|   126|Prg|CL_FB_FUNCTION_STATE==========CP        |   126|10.11.2022|19:49:35|    17408|D        |
|   127|Prg|CL_ADT_GUI_EVENT_DISPATCHER===CP        |   127|31.03.2023|15:25:31|    60416|D        |
|   128|Typ|WBTOOLTYPE                              |     0|31.05.2013|08:18:21|     6144|         |
|   129|Prg|CL_WB_NAVIGATOR_VIS_AS_DYNPRO=CP        |   129|10.11.2022|19:49:46|    27648|D        |
|   130|Prg|CL_WB_NAVIGATOR===============CP        |   130|10.11.2022|19:49:46|    30720|D        |
|   131|Prg|SAPLSEWB_CONTROL                        |   131|16.05.2023|13:23:19|   717824|D        |
|   132|Prg|SAPLSEWB                                |   132|16.05.2023|13:23:44|   573440|D        |
|   133|Prg|CL_WB_OBJECT==================CP        |   133|15.06.2023|07:05:22|   103424|D        |
|   134|Prg|SAPLSEQ0                                |   134|15.06.2023|07:05:23|   202752|D        |
|   135|Prg|SAPLSLDB                                |   135|16.05.2023|13:25:04|   119808|D        |
|   136|Typ|TADIR                                   |     0|26.01.2018|14:08:30|     6144|         |
|   137|Prg|CL_WB_REQUEST_STACK===========CP        |   137|16.05.2023|13:23:29|    22528|D        |
|   138|Prg|CL_WB_TOOL_STACK==============CP        |   138|10.11.2022|19:49:46|    10240|D        |
|   139|Prg|CL_FUNCTION_BUILDER===========CP        |   139|16.05.2023|13:23:19|   396288|D        |
|   140|Prg|IF_WB_PROGRAM=================IP        |    97|10.11.2022|19:49:51|     9216|D        |
|   141|Typ|HEADER_FB                               |     0|08.11.2019|08:53:50|    38912|         |
|   142|Typ|FBINFO1                                 |     0|09.01.2003|17:10:24|     3072|         |
|   143|Prg|SAPLSEUQ                                |   143|16.05.2023|13:23:21|   359424|D        |
|   144|Typ|TRKEY                                   |     0|31.05.2013|08:15:50|     3072|         |
|   145|Typ|DDERR                                   |     0|31.05.2013|08:00:31|     4096|         |
|   146|Typ|TRDIR                                   |     0|31.05.2013|08:19:17|     7168|         |
|   147|Typ|E071                                    |     0|31.05.2013|08:00:54|     4096|         |
|   148|Prg|SAPLSTRD                                |   148|15.06.2023|07:05:19|  1218560|D        |
|   149|Prg|SAPLSCP3                                |   149|01.06.2015|12:30:31|     9216|D        |
|   150|Prg|SAPLSCTS_UNICODE                        |   150|10.11.2022|19:49:54|    11264|D        |
|   151|Prg|CL_ABAP_CHAR_UTILITIES========CP        |   151|10.11.2022|19:49:27|    13312|D        |
|   152|Prg|IF_ABAP_LANGUAGE_VERSION======IP        |   143|10.11.2022|19:49:49|    11264|D        |
|   153|Typ|PROGDIR                                 |     0|31.05.2013|08:19:12|     7168|         |
|   154|Prg|%_CSMODI                                |   143|01.06.2015|12:30:30|    36864|D        |
|   155|Prg|SAPLSEQ1                                |   155|16.12.2022|11:36:13|    22528|D        |
|   156|Prg|CL_EXITHANDLER================CP        |   156|31.03.2023|15:25:35|    51200|D        |
|   157|Prg|CL_ABAP_TYPEDESCR=============CP        |   157|16.05.2023|13:23:14|    29696|D        |
|   158|Prg|CL_ABAP_REFDESCR==============CP        |   158|16.05.2023|13:23:14|    30720|D        |
|   159|Prg|CL_ABAP_DATADESCR=============CP        |   159|16.05.2023|13:23:14|    26624|D        |
|   160|Prg|CL_ABAP_SOFT_REFERENCE========CP        |   160|26.11.2018|11:01:11|     9216|D        |
|   161|Prg|CL_ABAP_REFERENCE=============CP        |   161|01.06.2015|12:30:30|     7168|D        |
|   162|Prg|IF_EX_WB_MODASS===============IP        |   158|31.05.2013|12:09:15|     6144|D        |
|   163|Prg|CL_ABAP_INTFDESCR=============CP        |   163|10.11.2022|19:49:27|    24576|D        |
|   164|Prg|CL_ABAP_OBJECTDESCR===========CP        |   164|31.03.2023|15:25:34|    32768|D        |
|   165|Prg|%_CABAP                                 |   163|10.11.2022|19:49:06|    36864|D        |
|   166|Prg|SAPLSEXV                                |   166|16.05.2023|13:23:36|   163840|D        |
|   167|Prg|CL_EX_WB_MODASS===============CP        |   167|28.05.2018|11:38:05|    16384|D        |
|   168|Prg|CL_BADI_FLT_DATA_TRANS_AND_DB=CP        |   168|15.06.2023|07:05:34|    47104|D        |
|   169|Typ|SXC_EXIT                                |     0|09.11.2000|14:23:43|     3072|         |
|   170|Prg|CL_EXIT_MASTER================CP        |   170|16.12.2022|11:36:12|    39936|D        |
|   171|Prg|CL_EX_BADI_LAYER==============CP        |   171|16.12.2022|11:36:12|    30720|D        |
|   172|Prg|SAPLSEXE                                |   172|15.06.2023|07:05:35|   137216|D        |
|   173|Prg|IF_EX_BADI_LAYER==============IP        |   156|16.12.2022|11:36:13|     7168|D        |
|   174|Prg|%_CSXRT                                 |   170|01.06.2015|12:30:30|    16384|D        |
|   175|Prg|CL_DEF_IM_WB_MODASS===========CP        |   175|13.02.2005|18:21:10|     8192|D        |
|   176|Prg|SAPLASTAT_TRIG                          |   176|01.06.2015|12:30:31|    17408|D        |
|   177|Prg|SAPLSETX                                |   177|25.02.2022|14:55:09|   219136|D        |
|   178|Prg|SAPLSTR4                                |   178|10.11.2022|19:49:55|    93184|D        |
|   179|Typ|SMODILOG                                |     0|31.05.2013|08:12:42|     7168|         |
|   180|Prg|CL_ABAP_LANGUAGE_VERSION======CP        |   180|04.04.2023|11:22:51|    53248|D        |
|   181|Prg|CL_ABAP_LANGUAGE_VERSION_CFG==CP        |   181|26.01.2023|17:38:09|    56320|D        |
|   182|Prg|CL_ARS_LANG_VRS_OBJTYP_FACTORYCP        |   182|26.01.2023|17:38:09|    32768|D        |
|   183|Prg|IF_ARS_LANG_VRS_OBJTYPE=======IP        |   182|10.11.2022|19:49:49|     9216|D        |
|   184|Prg|CL_ARS_LANG_VRS_OBJTYPE_SRC===CP        |   184|26.01.2023|17:38:09|    29696|D        |
|   185|Prg|CL_ARS_LANG_VRS_OBJTYPE=======CP        |   185|10.11.2022|19:49:28|    14336|D        |
|   186|Prg|CL_SEDI_ABAP_LANG_VERS_UTIL===CP        |   186|26.01.2023|17:38:14|    30720|D        |
|   187|Prg|SAPMS38L                                |   139|16.05.2023|13:23:24|  1150976|D        |
|   188|Prg|SAPLPA_PACKAGE_SERVICES                 |   188|28.04.2023|07:29:40|   295936|D        |
|   189|Prg|CL_FUNCTION_BUILDER_DATA======CP        |   189|03.03.2023|16:55:56|   211968|D        |
|   190|Prg|SAPLWB_CUSTOMIZING                      |   190|16.05.2023|13:25:05|   315392|D        |
|   191|Typ|RSEUMOD                                 |     0|10.10.2014|18:27:11|    55296|         |
|   192|Prg|SAPLSTR9                                |   192|26.01.2023|17:38:07|    63488|D        |
|   193|Prg|CONTEXT_X_TR_SYS_PARAMS                 |   192|25.02.2022|14:55:08|    47104|D        |
|   194|Typ|TCONT                                   |     0|31.05.2013|08:14:44|     3072|         |
|   195|Typ|CONTEXTB                                |     0|31.05.2013|07:59:43|     7168|         |
|   196|Typ|SCXTAB_TABSTRIP                         |     0|06.11.2003|20:31:00|     3072|         |
|   197|Prg|CL_FUNCTION_BUILDER_POOL======CP        |   197|16.05.2023|13:23:19|    73728|D        |
|   198|Typ|RSFBINTFV                               |     0|31.05.2013|08:10:42|    31744|         |
|   199|Prg|CL_FB_PARAMETER_DB============CP        |   199|03.05.2025|07:02:00|   216064|D        |
|   200|Typ|FUPARAREF                               |     0|31.05.2013|07:57:17|     5120|         |
|   201|Prg|CL_CLM_TOOL_LOG===============CP        |   201|23.04.2019|09:17:18|   227328|D        |
|   202|Prg|%_CSMM                                  |   201|23.04.2019|09:17:18|    55296|D        |
|   203|Typ|SMODIUSER                               |     0|08.09.2011|14:23:28|     5120|         |
|   204|Prg|CL_FB_PARAMETER_CONVERSION====CP        |   204|26.01.2023|17:38:05|    37888|D        |
|   205|Typ|RSFBPARA                                |     0|31.05.2013|08:10:42|     5120|         |
|   206|Typ|RSFB_DOCU                               |     0|02.11.1998|09:48:09|     4096|         |
|   207|Prg|CL_R3STANDARD_PERSISTENCE=====CP        |   207|28.04.2023|07:26:24|   974848|D        |
|   208|Typ|ENHNAME_STRUC                           |     0|30.09.2004|13:14:49|     2048|         |
|   209|Prg|SAPLSWOC                                |   209|10.11.2022|19:49:55|   131072|D        |
|   210|Typ|RSFBCURSOR                              |     0|31.05.2013|08:10:42|     3072|         |
|   211|Prg|CL_FB_FUNCTION_EDITOR=========CP        |   211|16.05.2023|13:23:19|   128000|D        |
|   212|Prg|CL_WB_EDITOR==================CP        |   212|16.05.2023|13:23:20|   331776|D        |
|   213|Prg|CL_SEDI_SHARED================CP        |   213|28.04.2023|07:29:47|    45056|D        |
|   214|Typ|EDLINEINDX                              |     0|02.05.2016|13:41:42|     4096|         |
|   215|Typ|EDLINENUM                               |     0|31.05.2013|08:01:21|     2048|         |
|   216|Prg|SAPLEDITOR_START                        |   216|28.04.2023|07:29:40|   223232|D        |
|   217|Typ|EDITORMODE                              |     0|27.08.1999|18:05:41|     4096|         |
|   218|Typ|D020S                                   |     0|31.05.2013|08:00:15|     5120|         |
|   219|Typ|EDITORSTAT                              |     0|31.05.2013|08:01:21|     5120|         |
|   220|Typ|RSTXP                                   |     0|31.07.2015|12:07:20|    19456|         |
|   221|Typ|SCXTAB_CONTROL                          |     0|18.05.2004|14:07:29|     6144|         |
|   222|Typ|D022S                                   |     0|07.03.1992|23:46:40|     2048|         |
|   223|Typ|D021S                                   |     0|20.08.1998|11:14:50|     9216|         |
|   224|Typ|D023S                                   |     0|29.10.1998|08:11:06|     3072|         |
|   225|Prg|SAPLSEUR                                |   225|16.05.2023|13:23:20|   900096|D        |
|   226|Prg|%_CSEDI                                 |   216|03.04.2018|18:41:35|    49152|D        |
|   227|Prg|%_CCXTAB                                |   216|01.06.2015|12:30:30|     7168|D        |
|   228|Typ|STOKESX                                 |     0|26.02.2001|12:01:13|     3072|         |
|   229|Prg|SAPLLOCAL_EDT1                          |   229|16.05.2023|13:23:57|  1214464|D        |
|   230|Typ|SRFC_SMETH                              |     0|06.11.2003|20:31:19|     3072|         |
|   231|Typ|SYCH_TABLES_ENTRY                       |     0|17.06.2009|13:37:47|     2048|         |
|   232|Prg|CL_SEDI_EDITOR_MARKER_SERVICE=CP        |   232|28.04.2023|07:29:40|    28672|D        |
|   233|Typ|EDLINECOMS                              |     0|14.02.1998|07:13:34|     4096|         |
|   234|Prg|CL_WB_FUNCTION_CONTEXT========CP        |   234|18.06.2009|09:16:04|     8192|D        |
|   235|Prg|CL_WB_CONTEXT=================CP        |   235|01.06.2015|12:30:30|     8192|D        |
|   236|Typ|SEOCPDKEY                               |     0|08.09.2011|14:24:00|     3072|         |
|   237|Prg|CL_WB_SOURCE==================CP        |   237|28.04.2023|07:29:53|   171008|D        |
|   238|Prg|CL_SEDI_TEMPORARY_STORAGE=====CP        |   238|28.04.2023|07:29:47|    24576|D        |
|   239|Prg|SAPLSABC                                |   239|10.11.2022|19:49:54|    15360|D        |
|   240|Prg|CL_FS_PATH====================CP        |   240|26.01.2023|17:38:05|    62464|D        |
|   241|Prg|CL_FS_UNIX_PATH===============CP        |   241|10.11.2022|19:49:36|    16384|D        |
|   242|Prg|SAPLSYST                                |   242|10.11.2022|19:49:55|    52224|D        |
|   243|Prg|CL_WB_ED_ENHANCEMENT_HANDLER==CP        |   243|16.05.2023|13:23:16|   332800|D        |
|   244|Prg|CL_WB_ED_CONSTANTS============CP        |   244|01.06.2015|12:30:30|    19456|D        |
|   245|Prg|SAPLSCTSDLVU                            |   245|10.11.2022|19:49:28|    36864|D        |
|   246|Prg|CX_ENH_IS_NOT_ENHANCEABLE=====CP        |   246|10.11.2022|19:49:48|    18432|D        |
|   247|Prg|CX_ENH_ROOT===================CP        |   247|10.11.2022|19:49:48|    25600|D        |
|   248|Prg|CX_STATIC_CHECK===============CP        |   248|10.11.2022|19:49:48|    10240|D        |
|   249|Prg|CX_ROOT=======================CP        |   249|10.11.2022|19:49:48|    14336|D        |
|   250|Prg|CX_NO_CHECK===================CP        |   250|10.11.2022|19:49:48|    10240|D        |
|   251|Prg|CL_WB_ED_SCP_INFO=============CP        |   251|16.12.2022|13:58:45|    24576|D        |
|   252|Prg|CL_ENH_SCP_CONTAINER==========CP        |   252|28.04.2023|07:26:23|   200704|D        |
|   253|Prg|CL_WB_PROGRAM_CONTEXT=========CP        |   253|26.01.2018|14:50:41|     8192|D        |
|   254|Typ|SMODISRC                                |     0|02.11.1998|09:48:57|     8192|         |
|   255|Typ|SSTMNT                                  |     0|15.06.2005|11:39:27|     4096|         |
|   256|Prg|CL_GUI_ABAPEDIT===============CP        |   256|28.04.2023|07:29:36|   111616|D        |
|   257|Prg|CL_GUI_SOURCEEDIT=============CP        |   257|28.04.2023|07:29:36|   156672|D        |
|   258|Prg|CL_GUI_CONTROL================CP        |   258|10.11.2022|19:49:36|   121856|D        |
|   259|Prg|CL_GUI_OBJECT=================CP        |   259|10.11.2022|19:49:36|   446464|D        |
|   260|Prg|CL_GUI_CFW====================CP        |   260|17.12.2018|10:03:42|   185344|D        |
|   261|Prg|CL_GUI_PROPS_CONSUMER=========CP        |   261|28.05.2018|11:34:26|    33792|D        |
|   262|Typ|EUDB                                    |     0|31.05.2013|08:02:19|     9216|         |
|   263|Prg|CL_DYNAMIC_GUI_EXTENSIONS=====CP        |   263|01.06.2015|12:30:30|    41984|D        |
|   264|Typ|OBJ_RECORD                              |     0|28.05.2018|11:34:27|     3072|         |
|   265|Typ|TOLE                                    |     0|29.07.1998|19:56:01|     4096|         |
|   266|Prg|%_COLE2                                 |    60|28.05.2018|11:34:26|    12288|D        |
|   267|Prg|SAPLSCP2                                |   267|15.06.2023|07:05:35|   197632|D        |
|   268|Typ|SWCONT                                  |     0|08.09.2011|14:24:14|     3072|         |
|   269|Prg|CL_GUI_FRONTEND_SERVICES======CP        |   269|28.04.2023|07:29:38|   165888|D        |
|   270|Prg|CL_GUI_MEMORY=================CP        |   270|10.11.2022|19:49:36|    47104|D        |
|   271|Prg|CL_GUI_MEMORY_ROOT============CP        |   271|10.11.2022|19:49:36|    15360|D        |
|   272|Typ|MATCH_RESULT                            |     0|15.06.2005|11:38:22|     3072|         |
|   273|Typ|THLINES                                 |     0|31.05.2013|08:15:00|     2048|         |
|   274|Prg|CL_STSK_UTIL==================CP        |   274|10.11.2022|19:49:44|     9216|D        |
|   275|Prg|CL_RFC========================CP        |   275|26.01.2023|17:38:13|    18432|D        |
|   276|Prg|CL_ABAP_DEBUGGER==============CP        |   276|26.01.2023|17:37:48|   130048|D        |
|   277|Prg|SAPLSDBG                                |   277|26.01.2023|17:38:07|    60416|D        |
|   278|Typ|BREAKPOINT                              |     0|08.09.2011|14:18:52|     3072|         |
|   279|Typ|BREAKPOINT_COMPLETE                     |     0|31.05.2013|07:58:26|     3072|         |
|   280|Typ|TEXTPOOL                                |     0|10.07.1997|00:11:35|     3072|         |
|   281|Prg|CL_GUI_CUSTOM_CONTAINER=======CP        |   281|28.05.2018|11:34:26|    45056|D        |
|   282|Prg|CL_GUI_CONTAINER==============CP        |   282|28.05.2018|11:34:26|    60416|D        |
|   283|Prg|SAPLCNTH                                |   283|28.05.2018|11:34:27|    30720|D        |
|   284|Prg|CL_ABAP_ELEMDESCR=============CP        |   284|16.05.2023|13:24:01|    47104|D        |
|   285|Prg|SAPLCNDP                                |   285|16.05.2023|13:23:14|   231424|D        |
|   286|Prg|SAPLCNTL                                |   286|25.03.2019|14:14:58|   356352|D        |
|   287|Typ|CODEPAGES                               |     0|09.07.1997|23:51:03|     2048|         |
|   288|Prg|CL_ABAP_CONV_OBJ==============CP        |   288|16.05.2023|13:23:09|    31744|D        |
|   289|Prg|SAPLCNT0                                |   289|28.05.2018|11:34:27|    43008|D        |
|   290|Typ|RFC_FIELDS                              |     0|20.08.1998|10:56:34|     3072|         |
|   291|Typ|DPPROPS                                 |     0|09.07.1997|23:53:23|     3072|         |
|   292|Prg|SAPLURFC                                |   292|15.06.2023|07:05:35|    54272|D        |
|   293|Prg|SAPLSRFC                                |   293|18.03.2023|11:02:29|    52224|D        |
|   294|Prg|CL_SYSTEM_UUID================CP        |   294|26.01.2023|17:38:07|    52224|D        |
|   295|Prg|SAPMSSY1                                |   285|26.11.2018|11:04:02|    34816|D        |
|   296|Prg|CL_GUI_RESOURCES==============CP        |   296|10.11.2022|19:49:36|    64512|D        |
|   297|Typ|ARFCSDATA                               |     0|08.09.2011|14:17:09|     7168|         |
|   298|Typ|ARFCCALLID                              |     0|08.09.2011|14:18:31|     3072|         |
|   299|Typ|ARFCTID                                 |     0|04.04.1995|15:40:33|     3072|         |
|   300|Typ|ARFCDATA                                |     0|04.04.1995|15:40:32|     5120|         |
|   301|Prg|CL_ABAP_STRUCTDESCR===========CP        |   301|16.05.2023|13:24:02|    51200|D        |
|   302|Prg|CL_ABAP_COMPLEXDESCR==========CP        |   302|16.05.2023|13:23:14|    16384|D        |
|   303|Prg|CL_GUI_DATAMANAGER============CP        |   303|10.11.2022|19:49:36|    87040|D        |
|   304|Prg|IF_CACHED_PROP================IP        |   303|01.06.2015|12:30:31|     5120|D        |
|   305|Prg|CL_DATAPROVIDER_DATAOBJECT====CP        |   305|28.05.2018|11:34:26|     9216|D        |
|   306|Prg|SAPFGUICNTL_CFW                         |     1|28.05.2018|11:34:27|    15360|D        |
|   307|Prg|CL_GUI_RESOURCE_MEMORY========CP        |   307|10.11.2022|19:49:36|    47104|D        |
|   308|Prg|CL_GUI_RESOURCE_MEMORY_ROOT===CP        |   308|28.05.2018|12:09:30|    10240|D        |
|   309|Prg|CL_ABAP_PARSER================CP        |   309|16.05.2023|13:23:58|   507904|D        |
|   310|Typ|SCC_ICON_MAPPING                        |     0|08.09.2011|14:23:19|     3072|         |
|   311|Prg|CL_CLS_CHAR_SOURCE_TYPE=======CP        |   311|10.11.2022|19:49:30|    19456|D        |
|   312|Prg|CL_DRAGDROP===================CP        |   312|28.05.2018|11:34:26|    40960|D        |
|   313|Prg|IF_WB_MANAGER=================IP        |   212|08.11.2019|10:12:32|    10240|D        |
|   314|Prg|CL_FB_FUNCTION_VISUALIZATION==CP        |   314|28.04.2023|07:26:24|    61440|D        |
|   315|Prg|SAPLSFUNCTION_BUILDER                   |   315|16.05.2023|13:23:25|   226304|D        |
|   316|Typ|ENLFDIR                                 |     0|01.06.2015|10:51:41|     4096|         |
|   317|Prg|CL_SATC_WB_INTEGRATION========CP        |   317|16.05.2023|13:23:49|    22528|D        |
|   318|Prg|CL_SATC_ACCESS_CONTROL_FACTORYCP        |   318|16.05.2023|13:23:14|    17408|D        |
|   319|Prg|CL_SATC_RESULT_ACCESS_CONTROL=CP        |   319|16.05.2023|13:23:49|    35840|D        |
|   320|Prg|CL_SATC_RUN_ACCESS_CONTROL====CP        |   320|10.11.2022|19:49:41|    16384|D        |
|   321|Prg|CL_AUNIT_PERMISSION_CONTROL===CP        |   321|21.08.2023|09:32:02|    23552|D        |
|   322|Prg|CL_AUNIT_CUST_ACCESS==========CP        |   322|16.05.2023|13:23:28|    18432|D        |
|   323|Prg|SAPLWB_INITIAL_TOOL                     |   323|15.06.2023|07:05:19|    37888|D        |
|   324|Typ|RSMPE_FUNL                              |     0|09.11.2000|14:11:27|     4096|         |
|   325|Prg|SAPLSEU1                                |   325|16.12.2022|13:57:52|   349184|D        |
|   326|Typ|RSEUL_FUN                               |     0|02.10.1997|18:27:32|     3072|         |
|   327|Prg|SAPLSMPI                                |   327|26.01.2023|17:38:15|   779264|D        |
|   328|Typ|RSMPE_STA                               |     0|29.07.1998|19:08:23|     3072|         |
|   329|Typ|RSMPE_CTX                               |     0|18.11.1998|16:23:59|     3072|         |
|   330|Typ|RSMPE_FUN                               |     0|07.04.2016|14:01:49|     5120|         |
|   331|Typ|RSMPE_MNL                               |     0|10.10.2014|18:27:12|     4096|         |
|   332|Typ|RSMPE_ATR                               |     0|15.11.2000|17:51:55|     3072|         |
|   333|Typ|RSMPE_TIT                               |     0|07.05.1997|13:07:03|     2048|         |
|   334|Typ|RSMPE_MEN                               |     0|15.11.2000|17:51:56|     3072|         |
|   335|Typ|RSMPE_ACT                               |     0|15.11.2000|17:51:54|     3072|         |
|   336|Typ|RSMPE_BUT                               |     0|15.11.2000|17:51:55|     3072|         |
|   337|Typ|RSMPE_PFK                               |     0|15.11.2000|17:51:56|     3072|         |
|   338|Typ|RSMPE_ADM                               |     0|12.05.1997|16:48:02|     3072|         |
|   339|Typ|RSMPE_STAF                              |     0|07.04.2016|14:01:49|     3072|         |
|   340|Typ|RSMPE_BUTS                              |     0|15.11.2000|17:51:55|     3072|         |
|   341|Typ|RSMPTEXTS                               |     0|31.05.2013|08:10:51|     4096|         |
|   342|Prg|CL_ABAP_FILE_UTILITIES========CP        |   342|10.11.2022|19:49:27|    26624|D        |
|   343|Typ|RSMPE_FUNT                              |     0|07.04.2016|14:01:49|     5120|         |
|   344|Typ|RSMPE_MNLT                              |     0|10.10.2014|18:27:12|     5120|         |
|   345|Typ|RSMPE_STAT                              |     0|15.11.2000|17:51:57|     4096|         |
|   346|Typ|RSMPE_ATRT                              |     0|15.11.2000|17:51:55|     4096|         |
|   347|Typ|RSMPE_TITT                              |     0|15.11.2000|17:51:57|     3072|         |
|   348|Typ|SMP_DYNTXT                              |     0|25.01.1999|13:03:11|     3072|         |
|   349|Prg|IF_SEDI_APPLICATION===========IP        |   216|01.06.2015|12:27:09|     5120|D        |
|   350|Typ|ABAPSOURCE                              |     0|04.04.1995|15:38:30|     2048|         |
|   351|Typ|EDSTEP                                  |     0|31.05.2013|08:01:21|     2048|         |
|   352|Prg|SAPLNAVI                                |   352|10.11.2022|19:48:43|   129024|D        |
|   353|Typ|NAVTAB                                  |     0|13.05.1997|12:51:57|     4096|         |
|   354|Prg|RADSHPBO                                |     1|18.03.2023|11:02:30|    62464|D        |
|   355|Prg|CL_DSH_SCREEN_ASSIGNMENT======CP        |   355|16.05.2023|13:23:10|    43008|D        |
|   356|Prg|CL_ABAP_MESSAGE_DIGEST========CP        |   356|16.05.2023|13:24:01|    46080|D        |
|   357|Prg|/PERSONAS/DYNP_INTEGRATION              |     1|12.02.2025|20:56:50|    24576|D        |
|   358|Prg|/PERSONAS/CL_API==============CP        |   358|12.02.2025|20:56:50|    12288|D        |
|   359|Prg|/PERSONAS/CL_HANDLER_FACTORY==CP        |   359|12.02.2025|20:56:50|    35840|D        |
|   360|Prg|/PERSONAS/IF_CONTEXT_TYPES====IP        |   358|12.02.2025|18:36:08|    64512|D        |
|   361|Prg|/PERSONAS/CL_SETTINGS_DATA====CP        |   361|12.02.2025|20:56:50|   178176|D        |
|   362|Prg|/PERSONAS/IF_SETTINGS_TYPES===IP        |   361|12.02.2025|18:35:57|    16384|D        |
|   363|Prg|/PERSONAS/CL_AUTH_UTILS=======CP        |   363|12.02.2025|18:36:08|    50176|D        |
|   364|Prg|/PERSONAS/IF_AUTH_TYPES=======IP        |   363|12.02.2025|18:36:08|    19456|D        |
|   365|Prg|/PERSONAS/CL_AUTHENTICATOR====CP        |   365|12.02.2025|18:35:43|    15360|D        |
|   366|Typ|RSEXC                                   |     0|17.06.2009|13:36:19|     2048|         |
|   367|Typ|RSEXP                                   |     0|31.05.2013|08:10:41|     4096|         |
|   368|Typ|RSIMP                                   |     0|31.05.2013|08:10:43|     4096|         |
|   369|Typ|RSTBL                                   |     0|31.05.2013|08:11:14|     4096|         |
|   370|Typ|RSCHA                                   |     0|31.05.2013|08:10:18|     4096|         |
|   371|Typ|RSFDO                                   |     0|13.08.1997|12:52:58|     3072|         |
|   372|Typ|RS38L                                   |     0|29.03.2018|15:54:09|    33792|         |
|   373|Typ|USVALUES                                |     0|31.05.2013|08:16:39|     3072|         |
|   374|Prg|SAPLSUSE                                |   374|17.06.2024|13:21:25|   271360|D        |
|   375|Typ|USER12                                  |     0|08.09.2011|14:28:36|     3072|         |
|   376|Prg|CL_ABAP_SYST==================CP        |   376|24.03.2016|13:14:20|    13312|D        |
|   377|Prg|CL_SUSR_AUTHORIZATIONS========CP        |   377|13.02.2026|18:19:35|   125952|D        |
|   378|Prg|CL_SUID_TOOLS=================CP        |   378|13.02.2026|18:19:35|   311296|D        |
|   379|Typ|SUID_ST_BNAME                           |     0|08.09.2011|14:23:36|     2048|         |
|   380|Prg|CL_SUSR_AUTH_TOOLS============CP        |   380|26.01.2023|17:38:07|   174080|D        |
|   381|Prg|CL_RBAMRT=====================CP        |   381|10.11.2022|19:49:39|     8192|D        |
|   382|Prg|/PERSONAS/CL_SESSION_MEMORY===CP        |   382|12.02.2025|20:56:50|    68608|D        |
|   383|Typ|/PERSONAS/T_MEMO                        |     0|12.02.2025|18:29:06|     6144|         |
|   384|Typ|DYNPRO_ID                               |     0|09.11.2000|14:07:21|     3072|         |
|   385|Prg|/PERSONAS/CL_DYNP_CONTEXT=====CP        |   385|12.02.2025|20:56:50|    83968|D        |
|   386|Prg|/PERSONAS/CL_CONTEXT==========CP        |   386|12.02.2025|20:56:50|    29696|D        |
|   387|Prg|/PERSONAS/CL_EWM_RF_DATA======CP        |   387|12.02.2025|18:35:42|    13312|D        |
|   388|Prg|CL_DYNPRO=====================CP        |   388|10.11.2022|19:49:34|    38912|D        |
|   389|Prg|/PERSONAS/IF_EWM_RF_DATA======IP        |   387|12.02.2025|18:35:41|    10240|D        |
|   390|Typ|DYNPVALUE                               |     0|26.02.2001|12:00:36|     2048|         |
|   391|Typ|DYNP_EL_ID                              |     0|18.05.2004|14:07:47|     3072|         |
|   392|Prg|/PERSONAS/CL_CONTEXT_DATA=====CP        |   392|12.02.2025|20:56:50|    90112|D        |
|   393|Prg|IF_SYSTEM_UUID================IP        |   392|03.11.2021|19:19:43|     6144|D        |
|   394|Prg|IF_SYSTEM_UUID_STATIC=========IP        |   392|10.11.2022|19:49:51|     7168|D        |
|   395|Prg|/PERSONAS/CL_OVERWRITE_DATA===CP        |   395|12.02.2025|20:56:50|   398336|D        |
|   396|Prg|/PERSONAS/CL_DYNP_AUTH_HNDL===CP        |   396|12.02.2025|18:33:49|    32768|D        |
|   397|Prg|/PERSONAS/CL_AUTH_HNDL========CP        |   397|12.02.2025|20:56:50|    51200|D        |
|   398|Prg|/PERSONAS/CL_GUI_XML_HNDL=====CP        |   398|12.02.2025|20:56:52|   268288|D        |
|   399|Prg|/PERSONAS/CL_DYNP_XML_HNDL====CP        |   399|12.02.2025|20:56:52|   353280|D        |
|   400|Prg|/PERSONAS/CL_XML_HNDL=========CP        |   400|12.02.2025|20:56:52|   353280|D        |
|   401|Prg|/PERSONAS/CL_LOG_MANAGER======CP        |   401|12.02.2025|18:32:43|    20480|D        |
|   402|Prg|/PERSONAS/CL_LOGGER===========CP        |   402|12.02.2025|18:32:43|    58368|D        |
|   403|Prg|/PERSONAS/IF_MESSAGE_TYPES====IP        |   402|12.02.2025|18:35:57|   620544|D        |
|   404|Prg|/PERSONAS/CL_APP_LOGGER=======CP        |   404|12.02.2025|18:32:38|    21504|D        |
|   405|Prg|/PERSONAS/CL_COMMAND_LOGGER===CP        |   405|12.02.2025|20:56:50|    19456|D        |
|   406|Prg|/PERSONAS/CL_PERSONAS_LOGGER==CP        |   406|12.02.2025|20:56:50|    21504|D        |
|   407|Prg|/PERSONAS/CL_MSG_HNDL=========CP        |   407|12.02.2025|20:56:52|    47104|D        |
|   408|Prg|/PERSONAS/CL_DYNP_FLV_FACTORY=CP        |   408|12.02.2025|20:56:52|    80896|D        |
|   409|Prg|/PERSONAS/CL_FLAVOR_FACTORY===CP        |   409|12.02.2025|20:56:52|   203776|D        |
|   410|Prg|/PERSONAS/CL_TRANSPORT_FACTORYCP        |   410|12.02.2025|20:56:52|    13312|D        |
|   411|Prg|/PERSONAS/CL_NO_AUTH_TR_CLIENTCP        |   411|12.02.2025|20:56:52|    26624|D        |
|   412|Prg|/PERSONAS/CL_DYNP_THM_FACTORY=CP        |   412|12.02.2025|20:56:50|    61440|D        |
|   413|Prg|/PERSONAS/CL_THEME_FACTORY====CP        |   413|12.02.2025|20:56:52|   172032|D        |
|   414|Prg|/PERSONAS/CL_USER_PROFILE=====CP        |   414|12.02.2025|20:56:50|   201728|D        |
|   415|Prg|/PERSONAS/CL_GUI_RUNTIME_MODE=CP        |   415|12.02.2025|20:56:52|   166912|D        |
|   416|Prg|/PERSONAS/CL_DYNP_RUNTIME_MODECP        |   416|12.02.2025|20:56:52|   243712|D        |
|   417|Prg|/PERSONAS/CL_RUNTIME_MODE=====CP        |   417|12.02.2025|20:56:52|   315392|D        |
|   418|Prg|/PERSONAS/CL_MODE=============CP        |   418|12.02.2025|20:56:52|   248832|D        |
|   419|Prg|/PERSONAS/CL_FLAVOR_DATA======CP        |   419|12.02.2025|20:56:52|  1162240|D        |
|   420|Prg|/PERSONAS/IF_CONSTANTS========IP        |   419|12.02.2025|18:35:43|    76800|D        |
|   421|Prg|/PERSONAS/CL_ROLE_DATA========CP        |   421|12.02.2025|20:56:52|   194560|D        |
|   422|Typ|AGR_USERS                               |     0|31.05.2013|07:57:04|     4096|         |
|   423|Prg|/PERSONAS/INIT_ROLE_ASSIGNMENTCP        |   423|12.02.2025|20:56:50|    15360|D        |
|   424|Prg|CL_BADI_BASE==================CP        |   424|16.12.2022|13:58:24|    32768|D        |
|   425|Prg|CL_ABAP_BADI_TRACER===========CP        |   425|10.11.2022|19:49:27|    18432|D        |
|   426|Prg|CL_SXT_TRACE_OPTIONS==========CP        |   426|10.11.2022|19:49:45|    11264|D        |
|   427|Prg|IF_SXT_TYPES==================IP        |   426|03.11.2021|19:22:04|    10240|D        |
|   428|Prg|CL_SXT_TRACER_FACTORY=========CP        |   428|10.11.2022|19:49:45|    20480|D        |
|   429|Prg|CL_SXT_TRACER=================CP        |   429|16.05.2023|13:23:10|    68608|D        |
|   430|Prg|CL_SXT_RT_DB_TRACE_WRITER=====CP        |   430|26.01.2023|17:38:15|    56320|D        |
|   431|Prg|CL_SXT_ACTIVATION_HANDLER=====CP        |   431|26.01.2023|17:38:15|    27648|D        |
|   432|Prg|CL_SXT_MEMORY_LIMITS==========CP        |   432|26.01.2023|17:38:15|    29696|D        |
|   433|Prg|CL_REQUEST_ENTRY_POINT========CP        |   433|26.01.2023|17:38:13|    16384|D        |
|   434|Typ|SXT_ACT                                 |     0|19.11.2020|09:54:38|     5120|         |
|   435|Prg|CL_SXT_E2E_ACTIVATION=========CP        |   435|10.11.2022|19:50:11|    18432|D        |
|   436|Prg|CL_EPP_GLOBAL_FACTORY=========CP        |   436|16.05.2023|13:23:53|    12288|D        |
|   437|Prg|CL_EPP_GLOBAL=================CP        |   437|26.01.2023|17:37:53|    33792|D        |
|   438|Prg|CL_EPP_CORE===================CP        |   438|16.05.2023|13:23:16|   105472|D        |
|   439|Prg|IF_EPP_GLOBAL_SECTION=========IP        |   437|10.11.2022|19:50:11|     9216|D        |
|   440|Prg|CX_BADI_NOT_IMPLEMENTED=======CP        |   440|10.11.2022|19:49:47|    10240|D        |
|   441|Prg|CX_BADI_NOT_SINGLE_USE========CP        |   441|10.11.2022|19:49:47|    10240|D        |
|   442|Prg|CX_BADI=======================CP        |   442|10.11.2022|19:49:47|    10240|D        |
|   443|Prg|/PERSONAS/CL_TRANSLATION_UTILSCP        |   443|12.02.2025|18:32:43|    24576|D        |
|   444|Prg|SAPLSOTR_DB_READ                        |   444|03.11.2021|19:19:46|    98304|D        |
|   445|Prg|/PERSONAS/CL_DYNP_FLAVOR======CP        |   445|12.02.2025|20:56:50|   152576|D        |
|   446|Prg|/PERSONAS/CL_FLAVOR===========CP        |   446|12.02.2025|20:56:52|   406528|D        |
|   447|Prg|/PERSONAS/IF_FLAVOR_TYPES=====IP        |   446|12.02.2025|18:35:43|   190464|D        |
|   448|Prg|/PERSONAS/CL_DYNP_FLV_VERSION=CP        |   448|12.02.2025|20:56:50|    79872|D        |
|   449|Prg|/PERSONAS/CL_FLAVOR_VERSION===CP        |   449|12.02.2025|20:56:50|   108544|D        |
|   450|Prg|/PERSONAS/CL_BASIC_UTILS======CP        |   450|12.02.2025|20:56:49|    73728|D        |
|   451|Prg|/PERSONAS/CL_USAGE_DATA=======CP        |   451|12.02.2025|20:56:50|   275456|D        |
|   452|Prg|/PERSONAS/CL_RESOURCE_DATA====CP        |   452|12.02.2025|20:56:52|   390144|D        |
|   453|Prg|/PERSONAS/CL_THEME_DATA=======CP        |   453|12.02.2025|20:56:52|   467968|D        |
|   454|Prg|/PERSONAS/CL_DYNP_THEME=======CP        |   454|12.02.2025|20:56:50|    49152|D        |
|   455|Prg|/PERSONAS/CL_THEME============CP        |   455|12.02.2025|20:56:52|    99328|D        |
|   456|Prg|/PERSONAS/CL_THEMES_UTILS=====CP        |   456|12.02.2025|20:56:50|    47104|D        |
|   457|Prg|/PERSONAS/IF_THEME_TYPES======IP        |   455|12.02.2025|20:56:51|   120832|D        |
|   458|Prg|/PERSONAS/CL_SETTINGS=========CP        |   458|12.02.2025|20:56:52|    30720|D        |
|   459|Typ|/PERSONAS/STTNGS                        |     0|12.02.2025|18:29:05|     5120|         |
|   460|Prg|/PERSONAS/CL_CONFIG_DATA======CP        |   460|12.02.2025|18:35:38|    56320|D        |
|   461|Typ|/PERSONAS/C_MDUI                        |     0|12.02.2025|18:29:01|     5120|         |
|   462|Typ|/PERSONAS/FLVSCR                        |     0|12.02.2025|18:29:02|     3072|         |
|   463|Prg|/PERSONAS/CL_WIN_FLV_SCREEN===CP        |   463|12.02.2025|20:56:50|    86016|D        |
|   464|Prg|/PERSONAS/CL_DYNP_FLV_SCREEN==CP        |   464|12.02.2025|20:56:50|   220160|D        |
|   465|Prg|/PERSONAS/CL_FLAVOR_SCREEN====CP        |   465|12.02.2025|20:56:50|   194560|D        |
|   466|Prg|/PERSONAS/CL_NO_FLV_CHNG_STRATCP        |   466|12.02.2025|18:34:42|    13312|D        |
|   467|Prg|CL_ABAP_GET_CALL_STACK========CP        |   467|26.01.2023|17:38:04|    17408|D        |
|   468|Typ|CALL_STACK_ENTRY                        |     0|17.06.2009|13:31:55|     3072|         |
|   469|Prg|/PERSONAS/CL_CHANGE_DATA======CP        |   469|12.02.2025|20:56:50|   571392|D        |
|   470|Prg|CL_ABAP_TABLEDESCR============CP        |   470|16.05.2023|13:23:14|    36864|D        |
|   471|Prg|CL_IXML_MINI==================CP        |   471|28.05.2018|11:32:46|     9216|D        |
|   472|Prg|CL_IXML_MINI_DOM==============CP        |   472|18.02.2005|14:13:24|    18432|D        |
|   473|Prg|IF_IXML_MINI_NODE=============IP        |   472|09.09.2004|14:18:21|     8192|D        |
|   474|Typ|IXMLTDOM                                |     0|18.05.2004|14:07:00|     3072|         |
|   475|Prg|CL_IXML_MINI_NODE=============CP        |   475|18.02.2005|14:13:24|    22528|D        |
|   476|Prg|CL_IXML_MINI_RENDERER=========CP        |   476|18.02.2005|14:13:24|    12288|D        |
|   477|Prg|CL_GUI_EVENT==================CP        |   477|28.05.2018|11:34:26|    11264|D        |
|   478|Prg|CX_PROGRAM_NOT_FOUND==========CP        |   478|18.06.2009|09:20:38|    11264|D        |
|   479|Prg|CL_EX_WB_PROCESS_FCODE========CP        |   479|15.07.2009|10:04:32|    15360|D        |
|   480|Prg|CL_FB_FUNCTION_NAVIGATION=====CP        |   480|28.04.2023|07:29:40|    84992|D        |
|   481|Prg|CL_WB_SOURCE_NAVIGATOR========CP        |   481|16.05.2023|13:23:20|   156672|D        |
|   482|Prg|CL_WB_DYNPRO_CONTEXT==========CP        |   482|26.01.2023|17:38:15|     9216|D        |
|   483|Prg|CL_SEDI_NAVIGATION_FACTORY====CP        |   483|16.05.2023|13:23:57|    63488|D        |
|   484|Prg|SAPLSANA                                |   484|28.04.2023|07:29:36|   121856|D        |
|   485|Prg|IF_SEDI_NAVIGATION_FACTORY====IP        |   483|08.01.2019|12:37:13|     7168|D        |
|   486|Prg|SAPLSEDA                                |   486|18.04.2025|12:30:24|   553984|D        |
|   487|Typ|SEOCLSKEY                               |     0|08.09.2011|14:23:23|     2048|         |
|   488|Prg|SAPLSEUF                                |   488|15.06.2023|07:05:35|   843776|D        |
|   489|Typ|FUPARAREF_ENHA                          |     0|31.05.2013|08:03:33|     6144|         |
|   490|Typ|SNODE                                   |     0|08.11.2019|08:56:18|     7168|         |
|   491|Typ|DIRTREE                                 |     0|31.05.2013|08:00:42|     3072|         |
|   492|Typ|DWTREE                                  |     0|07.05.1997|13:34:29|     7168|         |
|   493|Prg|/1BCDWBEN/SAPLSENQ10                    |   493|03.11.2021|18:10:31|    12288|D        |
|   494|Prg|SAPLSENA                                |   494|16.05.2023|13:23:57|    60416|D        |
|   495|Prg|CL_RLFW_SERVER_GROUP==========CP        |   495|03.03.2023|16:55:44|    32768|D        |
|   496|Prg|SAPLSUGS                                |   496|02.05.2025|11:07:09|   173056|D        |
|   497|Typ|TBTCO                                   |     0|31.05.2013|08:14:35|    12288|         |
|   498|Typ|TBTCSTEP                                |     0|10.10.2014|18:27:34|    14336|         |
|   499|Prg|SAPLBTCH                                |   499|12.02.2025|20:54:39|  3769344|D        |
|   500|Typ|BTC_S_EMAIL                             |     0|30.01.2017|09:51:29|     3072|         |
|   501|Typ|BTCJOBEPP                               |     0|10.08.2011|11:34:55|     5120|         |
|   502|Typ|BAPIXMSPOOLID                           |     0|16.03.2020|10:24:08|     9216|         |
|   503|Typ|TBTCJOB                                 |     0|31.05.2013|08:14:35|    12288|         |
|   504|Typ|TBTCP                                   |     0|26.02.2015|00:12:02|    16384|         |
|   505|Prg|CL_BATCH_SCHEDULER============CP        |   505|12.02.2025|20:53:49|   138240|D        |
|   506|Prg|SAPLSPRI                                |   506|17.06.2024|13:06:43|   498688|D        |
|   507|Typ|TPRI_PAR                                |     0|28.05.2018|11:37:58|     8192|         |
|   508|Typ|PRIPAR_EXT                              |     0|31.05.2013|08:08:31|     4096|         |
|   509|Typ|PRIPAR_EX2                              |     0|21.01.2014|10:05:43|     3072|         |
|   510|Typ|PRIPAR_EX3                              |     0|08.09.2011|14:21:28|     3072|         |
|   511|Typ|PRI_LAYOUT                              |     0|06.11.2003|20:30:04|     4096|         |
|   512|Typ|SEUTEXPAND                              |     0|04.04.1995|16:16:35|     2048|         |
|   513|Prg|%_CSTREE                                |   132|19.11.2020|09:53:19|    34816|D        |
|   514|Typ|CROSS                                   |     0|08.06.2015|12:38:30|     4096|         |
|   515|Prg|SAPLSEUEXIST                            |   515|28.04.2023|07:25:19|   241664|D        |
|   516|Typ|D010INC                                 |     0|21.01.2014|10:04:44|     3072|         |
|   517|Prg|SAPLSEDA_INDEX                          |   517|16.05.2023|13:23:57|   135168|D        |
|   518|Typ|TRDIRE                                  |     0|31.05.2013|08:15:47|     3072|         |
|   519|Typ|SCOMPO                                  |     0|08.11.2019|08:56:11|     5120|         |
|   520|Prg|SAPLSEDA_INC                            |   520|30.05.2023|13:35:55|   131072|D        |
|   521|Prg|SAPLSEU_BUFFER_MANAGEMENT               |   521|16.03.2020|10:27:27|    12288|D        |
|   522|Prg|SAPLSYDB                                |   522|03.05.2025|07:10:17|    71680|D        |
|   523|Prg|CL_DBI_TRANSACTION_STATE======CP        |   523|01.06.2015|12:30:30|     8192|D        |
|   524|Prg|SAPLSEUTABL                             |   524|16.05.2023|13:23:16|    99328|D        |
|   525|Prg|CL_SEDI_NAV_LCL_DB============CP        |   525|16.05.2023|13:23:57|    48128|D        |
|   526|Prg|SAPLTR_OBJECTS                          |   526|10.11.2022|16:53:56|    57344|D        |
|   527|Typ|KO105                                   |     0|02.11.1998|09:45:33|     2048|         |
|   528|Typ|KO100                                   |     0|02.11.1998|09:45:32|     3072|         |
|   529|Typ|ENHSPOTNAME_STRUC                       |     0|08.09.2011|14:18:45|     2048|         |
|   530|Typ|RLSEU0_DOK                              |     0|29.07.1998|19:04:50|     3072|         |
|   531|Prg|SAPLSED1                                |   531|10.11.2022|19:49:54|   107520|D        |
|   532|Prg|SAPLSDIF                                |   532|31.03.2023|13:52:11|   475136|D        |
|   533|Prg|CL_SEDI_ABAP_COMPILER=========CP        |   533|26.05.2023|05:06:56|    57344|D        |
|   534|Prg|CL_FB_FUNC_SOURCE_CONVERTER===CP        |   534|16.05.2023|13:23:18|   134144|D        |
|   535|Prg|CL_FB_SYNTAX_CHECK_HELPER=====CP        |   535|04.04.2023|11:24:27|    41984|D        |
|   536|Typ|SREPTABLN                               |     0|31.05.2013|08:13:02|     3072|         |
|   537|Prg|CL_ABAP_COMPILER==============CP        |   537|18.04.2025|12:30:37|  2452480|D        |
|   538|Prg|SAPLSEUA                                |   538|16.05.2023|13:23:23|   772096|D        |
|   539|Typ|SCR_RGRADE                              |     0|31.05.2013|08:12:16|     3072|         |
|   540|Typ|RSLINLMSG                               |     0|22.01.2021|08:55:12|     4096|         |
|   541|Prg|%_CSYNT                                 |   537|03.03.2023|16:56:28|   117760|D        |
|   542|Typ|SYCH_EXT_OBJ_USE_ENTRY                  |     0|01.06.2015|10:55:29|     3072|         |
|   543|Typ|SYCH_OBJECT_ENTRY                       |     0|01.06.2015|10:55:29|     3072|         |
|   544|Typ|RSLINLTXT                               |     0|28.05.2018|11:36:02|     3072|         |
|   545|Typ|SYCH_ACTION                             |     0|21.01.2014|10:06:14|     2048|         |
|   546|Typ|SYCH_RDIR_ENTRY                         |     0|30.03.2016|07:31:39|     3072|         |
|   547|Typ|SYCH_BDL_OBJ_ENTRY                      |     0|29.03.2018|15:54:51|     3072|         |
|   548|Prg|CL_ABAP_PRECOMP_HEADER_ADMIN==CP        |   548|10.11.2022|19:49:27|    60416|D        |
|   549|Prg|%_CRSDS                                 |     0|01.06.2015|12:30:30|    11264|D        |
|   550|Prg|%_CSYLDB                                |     0|01.06.2015|12:30:30|     9216|D        |
|   551|Prg|%_CSLIS                                 |    21|16.12.2022|13:58:46|   102400|D        |
|   552|Typ|DTC_S_LAYO                              |     0|28.05.2018|11:31:36|     7168|         |
|   553|Typ|CAT_SVARS                               |     0|08.09.2011|14:17:48|     3072|         |
|   554|Typ|RSOBJTB_WA                              |     0|28.05.2018|11:36:03|     4096|         |
|   555|Typ|ALRLRSNODE                              |     0|31.05.2013|07:57:14|     7168|         |
|   556|Typ|BTCSELECT                               |     0|08.09.2011|14:17:40|     5120|         |
|   557|Typ|TBTCSTRT                                |     0|23.06.2016|13:26:16|     7168|         |
|   558|Typ|BTCH1140                                |     0|07.02.2022|08:52:24|    15360|         |
|   559|Typ|DEVPAR                                  |     0|08.09.2011|14:26:09|     4096|         |
|   560|Typ|PRI_PARAMS                              |     0|31.05.2013|08:08:32|     6144|         |
|   561|Prg|%_CSINFO                                |   538|08.08.2024|10:14:42|    45056|D        |
|   562|Typ|DISVARIANT                              |     0|29.07.1998|18:25:39|     3072|         |
|   563|Typ|WBCROSSGI                               |     0|07.02.2022|08:59:57|     5120|         |
|   564|Typ|SYST                                    |     0|10.10.2014|18:27:32|    32768|         |
|   565|Typ|SCR_CRC64                               |     0|18.05.2004|14:07:14|     2048|         |
|   566|Typ|SCREEN                                  |     0|18.05.2004|14:07:14|     5120|         |
|   567|Typ|VSEOCLASS                               |     0|27.11.2013|14:30:56|     9216|         |
|   568|Typ|ARC_PARAMS                              |     0|10.10.2014|18:26:25|     6144|         |
|   569|Typ|TDCT                                    |     0|19.11.2001|15:48:25|     3072|         |
|   570|Typ|TSTC                                    |     0|31.05.2013|08:15:55|     3072|         |
|   571|Typ|S38E                                    |     0|31.07.2015|12:07:24|     9216|         |
|   572|Typ|RSFBMODI                                |     0|08.11.2019|08:56:04|    11264|         |
|   573|Typ|SMODIRS38L                              |     0|08.11.2019|08:56:18|     6144|         |
|   574|Typ|TFTIT                                   |     0|31.05.2013|08:14:59|     3072|         |
|   575|Typ|RSINFDIR                                |     0|29.03.2018|15:55:38|     4096|         |
|   576|Typ|RODIR                                   |     0|08.09.2011|14:27:43|     4096|         |
|   577|Typ|FBTRDIR                                 |     0|31.05.2013|08:02:33|     4096|         |
|   578|Typ|FBSTATE                                 |     0|31.05.2013|08:02:33|    11264|         |
|   579|Prg|CL_ADT_LIFECYCLE_MNGR_ENUM====CP        |   579|03.03.2023|16:55:56|     8192|D        |
|   580|Prg|CL_ABAP_UNIT_ASSERT===========CP        |   580|16.05.2023|13:23:12|   138240|D        |
|   581|Prg|IF_ABAP_UNIT_CONSTANT=========IP        |   139|26.01.2023|17:38:15|     6144|D        |
|   582|Prg|IF_ARS_OBJECT_CHANGE_CHECK====IP        |    96|28.02.2019|16:19:31|    12288|D        |
|   583|Typ|SREPTABLN1                              |     0|08.11.2019|08:56:19|     7168|         |
|   584|Typ|OBJINFO                                 |     0|18.05.2004|14:07:07|     3072|         |
|   585|Typ|SCR_REF                                 |     0|10.11.2022|16:52:11|     4096|         |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Directory of Application Tables                                                                   |
----------------------------------------------------------------------------------------------------
|Name                                     Date       Time       Lngth                              |
|    Val.                                                                                          |
----------------------------------------------------------------------------------------------------
|Program  20260219083516                                                                           |
----------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------
|Program  RS_EDTR_START_NEW_MODE                                                                   |
----------------------------------------------------------------------------------------------------
|SYST                                     10.10.2014 18:27:32   00004612                           |
|    \0\0\0\0\x0001\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\x0001\0\x0004                                    |
|SY                                         .  .       :  :     00004612                           |
|    \0\0\0\0\x0001\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\x0001\0\x0004                                    |
|SSCRFIELDS                               31.05.2013 08:13:14   00001708                           |
|                                                                                                  |
|SSCRTEXTS                                  .  .       :  :     00000742                           |
|                                                                                                  |
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
|Program  SAPLSDOC                                                                                 |
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
|    SAP.DATAPROVIDER.1                    {799104C5-04C5-11D0-                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLSEWORKINGAREA                                                                        |
----------------------------------------------------------------------------------------------------
|DWINACTIV                                  .  .       :  :     00000274                           |
|                                                                                                  |
|DWWASYNC                                   .  .       :  :     00000028                           |
|                \0\0|                                                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPLSUNI                                                                                 |
----------------------------------------------------------------------------------------------------
|RS38L                                      .  .       :  :     00004994                           |
|                                                                                                  |
|TFDIR                                      .  .       :  :     00000290                           |
|    /PLCP/BGPROCESSUNIT           /PLCP/SAPLBGPROCESSING                                          |
|ENLFDIR                                    .  .       :  :     00000146                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSEWB                                                                                 |
----------------------------------------------------------------------------------------------------
|DIRTREE                                    .  .       :  :     00000138                           |
|    PG_DBD$SSEL                                  0000000000017                                    |
|DWTREE                                     .  .       :  :     00004104                           |
|    TRPG_DBD$SSEL                                  \0\x0001\0ó                                    |
|TADIR                                      .  .       :  :     00000464                           |
|    R3TRLDBAD$S                                                                                   |
|TRDIR                                      .  .       :  :     00000340                           |
|                                                                                                  |
|TFDIR                                      .  .       :  :     00000290                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSEUQ                                                                                 |
----------------------------------------------------------------------------------------------------
|E071                                       .  .       :  :     00000360                           |
|                        000000LIMUFUNC/PLCP/BGPROCESSUNIT                                         |
|TADIR                                      .  .       :  :     00000464                           |
|    R3TRFUGR/PLCP/BGPROCESSING                                                                    |
|TFDIR                                      .  .       :  :     00000290                           |
|                                  /PLCP/SAPLBGPROCESSING                                          |
|TRDIR                                      .  .       :  :     00000340                           |
|    /PLCP/SAPLBGPROCESSING                     SD$        FS                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPLSTRD                                                                                 |
----------------------------------------------------------------------------------------------------
|TFDIR                                      .  .       :  :     00000290                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSETX                                                                                 |
----------------------------------------------------------------------------------------------------
|TADIR                                      .  .       :  :     00000464                           |
|    R3TRFUGR/PLCP/BGPROCESSING                                                                    |
|SMODILOG                                   .  .       :  :     00000784                           |
|    FUGR/PLCP/BGPROCESSING                      FUNC/PLCP/BGPR                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPMS38L                                                                                 |
----------------------------------------------------------------------------------------------------
|RS38L                                      .  .       :  :     00004994                           |
|                                                                                                  |
|ENLFDIR                                    .  .       :  :     00000146                           |
|    SUSR_USER_AUTH_FOR_OBJ_GET    SUSE                      X                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPLWB_CUSTOMIZING                                                                       |
----------------------------------------------------------------------------------------------------
|RSEUMOD                                    .  .       :  :     00007544                           |
|    PROLOGAEFO  X G                                        K                                      |
|*RSEUMOD                                   .  .       :  :     00007544                           |
|    PROLOGAEFO  X G                                        K                                      |
----------------------------------------------------------------------------------------------------
|Program  CONTEXT_X_TR_SYS_PARAMS                                                                  |
----------------------------------------------------------------------------------------------------
|CONTEXTB                                   .  .       :  :     00002992                           |
|    110EBTR_SYS_PARAMS       000006000100000000000000\0\0\0\0                                     |
|TCONT                                      .  .       :  :     00000068                           |
|    TR_SYS_PARAMS       20260205125155|                                                           |
----------------------------------------------------------------------------------------------------
|Program  SAPLEDITOR_START                                                                         |
----------------------------------------------------------------------------------------------------
|TRDIR                                      .  .       :  :     00000340                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLLOCAL_EDT1                                                                           |
----------------------------------------------------------------------------------------------------
|TRDIR                                      .  .       :  :     00000340                           |
|                                                                                                  |
|RSEUMOD                                    .  .       :  :     00007544                           |
|    PROLOGAEFO  X G                                        K                                      |
----------------------------------------------------------------------------------------------------
|Program  CL_GUI_PROPS_CONSUMER=========CP                                                         |
----------------------------------------------------------------------------------------------------
|EUDB                                       .  .       :  :     00004068                           |
|    %CCB057860B5A71FE183ABCC21F4A1D98E         \0\0\0                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPLSFUNCTION_BUILDER                                                                    |
----------------------------------------------------------------------------------------------------
|ENLFDIR                                  03.05.2025 05:24:15   00000146                           |
|                                                                                                  |
|TADIR                                      .  .       :  :     00000464                           |
|                                                                                                  |
|RS38L                                      .  .       :  :     00004994                           |
|                                                                                                  |
|RSFBPARA                                 31.05.2013 08:10:42   00000790                           |
|                                                                                                  |
|*RSFBPARA                                31.05.2013 08:10:42   00000790                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSEU1                                                                                 |
----------------------------------------------------------------------------------------------------
|TRDIR                                      .  .       :  :     00000340                           |
|    SAPLWB_INITIAL_TOOL                       XSD$        FS                                      |
|EUDB                                       .  .       :  :     00004068                           |
|    CUDBD$SSEL                                D\0\0\0                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPLSMPI                                                                                 |
----------------------------------------------------------------------------------------------------
|EUDB                                       .  .       :  :     00004068                           |
|    CUSAPLWB_INITIAL_TOOL                     D\0\0\0DSAP                                         |
----------------------------------------------------------------------------------------------------
|Program  /PERSONAS/CL_SESSION_MEMORY===CP                                                         |
----------------------------------------------------------------------------------------------------
|/PERSONAS/T_MEMO                           .  .       :  :     00002976                           |
|    ST00011002CB057860B5A71FD183ACAD0DE6CB43E8\0\0\0\0\0\0\0\0                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLSEDA                                                                                 |
----------------------------------------------------------------------------------------------------
|TRDIR                                      .  .       :  :     00000340                           |
|    DBD$SSEL                                X  S          IS                                      |
|D020S                                      .  .       :  :     00000414                           |
|                                                         \0\0                                     |
|TDCT                                       .  .       :  :     00000166                           |
|                                                                                                  |
|TSTC                                       .  .       :  :     00000180                           |
|                                                                                                  |
|RSMPTEXTS                                  .  .       :  :     00000256                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSEUF                                                                                 |
----------------------------------------------------------------------------------------------------
|TRDIR                                      .  .       :  :     00000340                           |
|    /PLCP/LBGPROCESSINGU01                    XSD$        IS                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPLSPRI                                                                                 |
----------------------------------------------------------------------------------------------------
|TPRI_PAR                                   .  .       :  :     00001428                           |
|    5y}UuCi57jwne1ExB20240206     001SAPRSLOG_BAT                                                 |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|ABAP Control Blocks (CONT)                                                                        |
----------------------------------------------------------------------------------------------------
|Index|Name|Fl|PAR0|PAR1|PAR2|PAR3|PAR4|PAR5|PAR6|Source Code                             |Line    |
----------------------------------------------------------------------------------------------------
|21333| PAR| 0| 000| 06B| C04|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21333| PAR| 0| 000| 069| C04|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21333| PAR| 0| 000| 064| C05|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21333| PAR| 0| 000| 064| C05|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21333| PAR| 0| 000| C00| C00|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21334| PAR| 0| 000| C05| C04|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21334| PAR| 0| 000| C05| C04|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21334| PAR| 0| 000| C05| C04|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21334| PAR| 0| 000| C05| C04|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21334| PAR| 0| 000| 064| 064|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21335| PAR| 0| 000| 064| 064|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21335| PAR| 0| 000| 064| 064|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|>>>>>| mvq| 0| 050| 001| 005|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21335| CMP| 2| 038| 064| 064|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21335| BRA| 0| 000|    |    |    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21335| cmp| 0| 00F| 05E| 000|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21336| BRA| 0| 000|    |    |    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21336| CMP| 2| 038| C05| C05|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21336| BRA| 0| 000|    |    |    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
|21336| CMP| 2| 038| 064| 064|    |    |    |    | CL_ABAP_COMPILER==============CM043    |       9|
----------------------------------------------------------------------------------------------------
                                                                                                                                                                                                                           
