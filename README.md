----------------------------------------------------------------------------------------------------
Category               ABAP programming error                                                       
Runtime Errors         DATREF_NOT_ASSIGNED                                                          
ABAP: Program          ZCL_WR_HELPER_TREE============CP                                             
Application Component  Not assigned                                                                 
Date and Time          17.06.2026 14:20:09 (CET)                                                    
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Short Text                                                                                        |
|    Access using a 'NULL' data reference is not possible.                                         |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|What happened?                                                                                    |
|    Error in the ABAP application program.                                                        |
|                                                                                                  |
|    The current ABAP program "ZCL_WR_HELPER_TREE============CP" had to be                         |
|     terminated because it found a                                                                |
|    statement that could not be executed.                                                         |
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
|    An attempt was made to access a data object using a 'NULL' data                               |
|    reference (points to 'nothing').                                                              |
|    A data reference must point to a data object before it can be used to                         |
|    access the referenced data object.                                                            |
|    The reference has either never been set, or it was set to 'NULL' with a                       |
|    CLEAR statement.                                                                              |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|How to correct the error                                                                          |
|    If the error occurs in one of your own ABAP programs or an SAP program                        |
|    that you have modified, try to correct the error yourself.                                    |
|    If the error occurs in a non-modfied SAP program, you might be able to                        |
|    find a solution in the SAP Notes system. If you have access to the SAP                        |
|    Notes system, check there first using the following keywords:                                 |
|                                                                                                  |
|    "DATREF_NOT_ASSIGNED"                                                                         |
|    "ZCL_WR_HELPER_TREE============CP" bzw. ZCL_WR_HELPER_TREE============CM003                   |
|    "ZZ_ADJUST_NODE_WNOTE"                                                                        |
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
|    Work process number... 56                                                                     |
|    Work process ID....... 4149482                                                                |
|    ABAP load version.... 3236                                                                    |
|    Shortdump setting. full                                                                       |
|                                                                                                  |
|    Database server... vhmuzh4qdb01                                                               |
|    Database type..... HDB                                                                        |
|    Database name..... S4Q                                                                        |
|    Database user ID SAPHANADB                                                                    |
|                                                                                                  |
|    Terminal.......... 172.19.21.29                                                               |
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
|    EM...... 78832000 Bytes                                                                       |
|    Heap........................ 0 Bytes                                                          |
|    Page........................ 3825664 Bytes                                                    |
|    MM used memory..... 71204144 Bytes                                                            |
|    MM free memory.......... 7620520 Bytes                                                        |
|    MM largest free block... 4004768 Bytes                                                        |
|    MM used blocks....... 94771                                                                   |
|    MM free blocks............ 53                                                                 |
|    MM maximum memory used... 96089488 Bytes                                                      |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|User and Transaction                                                                              |
|    Client................. 110                                                                   |
|    User.................. FERPC                                                                  |
|    Language key.......... K                                                                      |
|    Transaction......... EWAWDOC                                                                  |
|    Transaction ID...... 70B13F4D03A0482D737B5A44AE000000                                         |
|                                                                                                  |
|    EPP Root Context ID.... 1BB73543777C4339844920367F874FA9                                      |
|    EPP Connection ID...... 7457316AD6EB4862E10000000AC90230                                      |
|    EPP Connection Counter. 347                                                                   |
|    EPP Component Name..... EWAWDOC_SAPLEEWA_WDOCOCKPIT                                           |
|                                                                                                  |
|    Program....................... ZCL_WR_HELPER_TREE============CP                               |
|    Screen......................... SAPLEEWA_WDOCOCKPIT                     0100                  |
|    Screen line.................... 15                                                            |
|    Debugger interpretor loop... "none"                                                           |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Server-Side Connection Information                                                                |
|    Information on caller of Remote Function Call (RFC):                                          |
|    System.............. S4Q                                                                      |
|    Installation number 0021329263                                                                |
|    Database Release.....                                                                         |
|    Kernel Release......                                                                          |
|    Connection type..... " "  (2=R/2, 3=ABAP-System, E=External,                                  |
|    R=Reg.External) call type....... synchronous and non-transactional (emode 0,                  |
|     imode 0)                                                                                     |
|    Inbound TID..........                                                                         |
|    Inbound queue name...                                                                         |
|    Outbound TID.........                                                                         |
|    Outbound queue name..                                                                         |
|                                                                                                  |
|    Client................. 110                                                                   |
|    User.................. FERPC                                                                  |
|    Transaction......... EWAWDOC                                                                  |
|    Call program...........                                                                       |
|    Function module.....                                                                          |
|    Call destination.....                                                                         |
|    Source server......                                                                           |
|    Source IP address..                                                                           |
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
|Information on where terminated                                                                   |
|    The termination occurred in ABAP program or include                                           |
|     "ZCL_WR_HELPER_TREE============CP", in "ZZ_ADJUST_NODE_WNOTE". The                           |
|    main program was "SAPLEEWA_WDOCOCKPIT".                                                       |
|                                                                                                  |
|    In the source code, the termination point is in line 40 of include                            |
|     "ZCL_WR_HELPER_TREE============CM003".                                                       |
|    include "ZCL_WR_HELPER_TREE============CM003".                                                |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Source Code Extract                                                                               |
----------------------------------------------------------------------------------------------------
|Line |Code                                                                                        |
----------------------------------------------------------------------------------------------------
|   10|    LS_ITEM         type EWAEL_TREEV_ITEM,                                                  |
|   11|    LD_PROPVALUEREF type ref to EWAEL_PROPVALUE,                                            |
|   12|    LD_INFOREF      type ref to EWAEL_TREEV_NODE_INFO,                                      |
|   13|    LV_TEXT         type STRING,                                                            |
|   14|    LT_DATA         type  standard table of STY_TWEIGHDATA.                                 |
|   15|                                                                                            |
|   16|  loop at PAR_INFOS reference into LD_INFOREF.                                              |
|   17|    read table LD_INFOREF->NODEREF->FIELDS with key NAME = 'POBJNR' reference into LD_PROPV\|
|ALUEREF.                                                                                          |
|   18|                                                                                            |
|   19|    select                                                                                  |
|   20|      EWA_ORDER_WEIGH~WASTE,                                                                |
|   21|      MAKT~MAKTG                                                                            |
|   22|     into table @LT_DATA                                                                    |
|   23|      from EWA_ORDER_WEIGH                                                                  |
|   24|      inner join MAKT                                                                       |
|   25|              on MAKT~MATNR = EWA_ORDER_WEIGH~WASTE                                         |
|   26|      where EWA_ORDER_WEIGH~POBJNR = @LD_PROPVALUEREF->VALUE                                |
|   27|      and MAKT~SPRAS = @SY-LANGU.                                                           |
|   28|                                                                                            |
|   29|    loop at LT_DATA reference into data(LS_DATA).                                           |
|   30|      LS_DATA->WASTE = |{ LS_DATA->WASTE alpha = out }|.                                    |
|   31|*      concatenate LS_DATA->WASTE            ','                                            |
|   32|*                  LS_DATA->MAT_DESC                                                        |
|   33|*                  into Lv_TEXT separated by ' '.                                           |
|   34|    endloop.                                                                                |
|   35|                                                                                            |
|   36|    loop at PAR_ITEMS into LS_ITEM where NODE_KEY = LD_INFOREF->NODEREF->NODE_KEY and ITEM_\|
|NAME = 21.                                                                                        |
|   37|      data(LTABIX) = SY-TABIX.                                                              |
|   38|*      LS_ITEM-TEXT = LV_TEXT.                                                              |
|   39|      read table LT_DATA with key WASTE = LS_ITEM-TEXT reference into LS_DATA.              |
|>>>>>|      LS_ITEM-TEXT = |{ LS_ITEM-TEXT }, { LS_DATA->MAT_DESC }|.                             |
|   41|      LS_ITEM-CLASS = 2.                                                                    |
|   42|      LS_ITEM-FONT = 2.                                                                     |
|   43|      LS_ITEM-ALIGNMENT = 3 .                                                               |
|   44|      modify PAR_ITEMS from LS_ITEM index LTABIX." SY-TABIX.                                |
|   45|      exit.                                                                                 |
|   46|    endloop.                                                                                |
|   47|                                                                                            |
|   48|  endloop.                                                                                  |
|   49|                                                                                            |
|   50|                                                                                            |
|   51|endmethod.                                                                                  |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Contents of system fields                                                                         |
----------------------------------------------------------------------------------------------------
|Name    |Val.                                                                                     |
----------------------------------------------------------------------------------------------------
|SY-SUBRC|4                                                                                        |
|SY-INDEX|1                                                                                        |
|SY-TABIX|0                                                                                        |
|SY-DBCNT|0                                                                                        |
|SY-FDPOS|1                                                                                        |
|SY-LSIND|0                                                                                        |
|SY-PAGNO|0                                                                                        |
|SY-LINNO|1                                                                                        |
|SY-COLNO|1                                                                                        |
|SY-PFKEY|EDIT                                                                                     |
|SY-UCOMM|                                                                                         |
|SY-TITLE|(Ændr) Bortskaffelsesordreposition 6768                                                  |
|SY-MSGTY|E                                                                                        |
|SY-MSGID|EEWA_EL                                                                                  |
|SY-MSGNO|016                                                                                      |
|SY-MSGV1|                                                                                         |
|SY-MSGV2|                                                                                         |
|SY-MSGV3|                                                                                         |
|SY-MSGV4|                                                                                         |
|SY-MODNO|0                                                                                        |
|SY-DATUM|20260617                                                                                 |
|SY-UZEIT|142009                                                                                   |
|SY-XPROG|SAPCNVE                                                                                  |
|SY-XFORM|CONVERSION_EXIT                                                                          |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Active Calls/Events                                                                               |
----------------------------------------------------------------------------------------------------
|No.   Ty.          Program                             Include                             Line   |
|      Name                                                                                        |
----------------------------------------------------------------------------------------------------
|   19 METHOD       ZCL_WR_HELPER_TREE============CP    ZCL_WR_HELPER_TREE============CM003    40  |
|      ZCL_WR_HELPER_TREE=>ZZ_ADJUST_NODE_WNOTE                                                    |
|   18 METHOD       ZCL_IM_EEWA_WDOC_TREE=========CP    ZCL_IM_EEWA_WDOC_TREE=========CCIMP   266  |
|      LCL_APP=>ADJUST_NODE_WNOTE                                                                  |
|   17 METHOD       ZCL_IM_EEWA_WDOC_TREE=========CP    ZCL_IM_EEWA_WDOC_TREE=========CCIMP   199  |
|      LCL_APP=>IF_EX_EEWAEL_TREE~ADJUST_NODES                                                     |
|   16 METHOD       CL_EEWA_FORM_TREE=============CP    CL_EEWA_FORM_TREE=============CM06T     3  |
|      CL_EEWA_FORM_TREE=>PLUGIN_ADJUST_NODES                                                      |
|   15 METHOD       CL_EEWA_FORM_TREE=============CP    CL_EEWA_FORM_TREE=============CM02S    43  |
|      CL_EEWA_FORM_TREE=>TREE_ADD_NODES_AND_ITEMS                                                 |
|   14 METHOD       CL_EEWA_FORM_TREE=============CP    CL_EEWA_FORM_TREE=============CM056   146  |
|      CL_EEWA_FORM_TREE=>TRANSFERTREEDATA                                                         |
|   13 METHOD       CL_EEWA_FORM_TREE=============CP    CL_EEWA_FORM_TREE=============CM02K     5  |
|      CL_EEWA_FORM_TREE=>ENDUPDATE                                                                |
|   12 METHOD       CL_EEWA_FORM_TREE=============CP    CL_EEWA_FORM_TREE=============CM01H     9  |
|      CL_EEWA_FORM_TREE=>HANDLE_TREE_EXPAND_NO_CHILDREN                                           |
|   11 METHOD       CL_TREE_CONTROL_BASE==========CP    CL_TREE_CONTROL_BASE==========CM01L    35  |
|      CL_TREE_CONTROL_BASE=>HANDLE_NODE_KEY_EVENT                                                 |
|   10 METHOD       CL_TREE_CONTROL_BASE==========CP    CL_TREE_CONTROL_BASE==========CM015    61  |
|      CL_TREE_CONTROL_BASE=>HANDLE_BASE_EVENT                                                     |
|    9 METHOD       CL_TREE_CONTROL_BASE==========CP    CL_TREE_CONTROL_BASE==========CM017    26  |
|      CL_TREE_CONTROL_BASE=>DISPATCH                                                              |
|    8 METHOD       CL_GUI_CFW====================CP    CL_GUI_CFW====================CM001   138  |
|      CL_GUI_CFW=>DISPATCH                                                                        |
|    7 METHOD       CL_EEWA_FORM_BASE=============CP    CL_EEWA_FORM_BASE=============CM05Q    38  |
|      CL_EEWA_FORM_BASE=>HANDLE_DISPATCH                                                          |
|    6 METHOD       CL_EEWA_FORM_BASE=============CP    CL_EEWA_FORM_BASE=============CM09M     5  |
|      CL_EEWA_FORM_BASE=>HANDLE_DISPATCH_SUBAPP                                                   |
|    5 METHOD       CL_EEWA_FORM_BASE=============CP    CL_EEWA_FORM_BASE=============CM05Q    30  |
|      CL_EEWA_FORM_BASE=>HANDLE_DISPATCH                                                          |
|    4 METHOD       SAPLEEWA_WDORDERRESULT              LEEWA_WDORDERRESULTCI1                 13  |
|      LCL_APP=>IF_EEWA_WDOC_APPLICATION~HANDLE_DISPATCH                                           |
|    3 METHOD       SAPLEEWA_WDOCOCKPIT                 LEEWA_WDOCOCKPITCI1                    71  |
|      LCL_APP=>HANDLE_DISPATCH                                                                    |
|    2 METHOD       CL_EEWA_FORM_BASE=============CP    CL_EEWA_FORM_BASE=============CM00C    15  |
|      CL_EEWA_FORM_BASE=>PAI_DISPATCH                                                             |
|    1 MODULE (PAI) SAPLEEWA_WDOCOCKPIT                 LEEWA_WDOCOCKPITI01                    18  |
|      USER_COMMAND_0100                                                                           |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Selected Variables                                                                                |
----------------------------------------------------------------------------------------------------
|Name                                                                                              |
|    Val.                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      19 Ty.          METHOD                                                                   |
|Name  ZCL_WR_HELPER_TREE=>ZZ_ADJUST_NODE_WNOTE                                                    |
----------------------------------------------------------------------------------------------------
|PAR_INFOS                                                                                         |
|    Table IT_186179[1x24]                                                                         |
|    \CLASS-POOL=ZCL_IM_EEWA_WDOC_TREE\CLASS=LCL_APP\METHOD=IF_EX_EEWAEL_TREE~ADJUST_NODES\DATA=LT_|
|    Table reference: 16694                                                                        |
|    TABH+ 0(20) = C04BDE2EC07F00000000000000000000E8362A96                                        |
|    TABH+20(20) = BF7F000000000000000000000100000044060000                                        |
|    TABH+40(20) = 000000003641000043D702000100000018000000                                        |
|    TABH+60(20) = 7001000004440300A0D504000400010601000000                                        |
|    store                   = 0xC04BDE2EC07F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0xE8362A96BF7F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 12    (ILLEGAL)                                                     |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 16694 (0x36410000)                                                  |
|    label                   = 186179 (0x43D70200)                                                 |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 24    (0x18000000)                                                  |
|    loop                    = 368   (0x70010000)                                                  |
|    xtyp                    = PROG#000836/TYPE#000284                                             |
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
|    tabi                    = 0x88E4DF2EC07F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 165815 (0xB7870200)                                                 |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0x00E5DF2EC07F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|    {{A:3743*\TYPE=EWAEL_TREEV_NODE_BASE};{A:3741*\TYPE=TREEV_NODE};{A:initial}}                  |
|    5000900050009000C0000000                                                                      |
|    0000FE002000DE00F0000000                                                                      |
|    500000009F0E0000520000009D0E0000CF00000000000000                                              |
|PAR_LABELS                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|PAR_ITEMS                                                                                         |
|    Table IT_186165[4x424]                                                                        |
|    \CLASS=CL_EEWA_FORM_TREE\METHOD=TRANSFERTREEDATA\DATA=LUPDATEITEMS-ADDED_ITEMS                |
|    Table reference: 13114                                                                        |
|    TABH+ 0(20) = 3090FACEBF7F00000000000000000000803C9823                                        |
|    TABH+20(20) = C07F000000000000000000000400000044040000                                        |
|    TABH+40(20) = 000000003A33000035D7020004000000A8010000                                        |
|    TABH+60(20) = 80020000049B0200305E07000400010602000000                                        |
|    store                   = 0x3090FACEBF7F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x803C9823C07F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 13114 (0x3A330000)                                                  |
|    label                   = 186165 (0x35D70200)                                                 |
|    fill                    = 4     (0x04000000)                                                  |
|    leng                    = 424   (0xA8010000)                                                  |
|    loop                    = 640   (0x80020000)                                                  |
|    xtyp                    = PROG#000667/TYPE#000059                                             |
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
|    tabi                    = 0x50A48690BF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 165802 (0xAA870200)                                                 |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0xB0DFA8A4BF7F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|                                                                                                  |
|    {         74;         20;2;2;;;;3;@5B\Q@;;;0;0;;;;;;}                                         |
|    202020202020202020303020202020202020202020303020000000002020200000004030405050402020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202000000000200020202000202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    00000000000000000070400000000000000000000020000020002000000000003000005020C010000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000370034002000200020002000200020002000200020002000320030002\|
|00002000000020000002000200020000000030000004000350042005C0051004000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200000000000000000002000000020002000200000002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|                                                                                                  |
|    {         74;         21;2;2;;;;3;;;;0;0;;;;;;}                                               |
|    202020202020202020303020202020202020202020303020000000002020200000002020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202000000000200020202000202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000704000000000000000000000201000200020000000000030000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000370034002000200020002000200020002000200020002000320031002\|
|0000200000002000000200020002000000003000000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200000000000000000002000000020002000200000002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|                                                                                                  |
|    {         74;2;2;2;;;;3;@B3\QOprettet@;;;0;0;;;;;;}                                           |
|    202020202020202020303020302020202020202020202020000000002020200000004040305050407070607070607\|
|0402020202020202020202020202020202020202020202020202020202020202020202000000000200020202000202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    00000000000000000070400020000000000000000000000020002000000000003000002030C010F00020504040504\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000370034002000320020002000200020002000200020002000200020002\|
|00002000000020000002000200020000000030000004000420033005C0051004F00700072006500740074006500740040\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200000000000000000002000000020002000200000002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|                                                                                                  |
|    {         74;5;2;2;;;;3;;;;0;0;;;;;0002;}                                                     |
|    202020202020202020303020302020202020202020202020000000002020200000002020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202000000000200020202000303030\|
|30202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000704000500000000000000000000000200020000000000030000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|20000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000370034002000350020002000200020002000200020002000200020002\|
|0000200000002000000200020002000000003000000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200000000000000000002000000020002000200000003000300030003200\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|LS_DATA->WASTE                                                                                    |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|LD_INFOREF->NODEREF->NODE_KEY                                                                     |
|             74                                                                                   |
|    222222222332                                                                                  |
|    000000000740                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    200020002000200020002000200020002000370034002000                                              |
|<%_L007>                                                                                          |
|             74                                                                                   |
|    222222222332                                                                                  |
|    000000000740                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    200020002000200020002000200020002000370034002000                                              |
|SY-REPID                                                                                          |
|    ZCL_WR_HELPER_TREE============CP                                                              |
|    5445555444545555443333333333334522222222                                                      |
|    A3CF72F85C052F4255DDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5A0043004C005F00570052005F00480045004C005000450052005F0054005200450045003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|LS_ITEM                                                                                           |
|    {         74;         21;2;2;;;;3;;;;0;0;;;;;;}                                               |
|    202020202020202020303020202020202020202020303020000000002020200000002020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202000000000200020202000202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000704000000000000000000000201000200020000000000030000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000370034002000200020002000200020002000200020002000320031002\|
|0000200000002000000200020002000000003000000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200000000000000000002000000020002000200000002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|<%_L006>                                                                                          |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|<%_L006>-NODE_KEY                                                                                 |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|SYST-REPID                                                                                        |
|    ZCL_WR_HELPER_TREE============CP                                                              |
|    5445555444545555443333333333334522222222                                                      |
|    A3CF72F85C052F4255DDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5A0043004C005F00570052005F00480045004C005000450052005F0054005200450045003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SY                                                                                                |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
|<%_L006>-ITEM_NAME                                                                                |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|LT_DATA                                                                                           |
|    Table IT_186180[0x160]                                                                        |
|    \CLASS=ZCL_WR_HELPER_TREE\METHOD=ZZ_ADJUST_NODE_WNOTE\DATA=LT_DATA                            |
|    Table reference: 17275                                                                        |
|    TABH+ 0(20) = 0000000000000000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000000000044010000                                        |
|    TABH+40(20) = 000000007B43000044D7020000000000A0000000                                        |
|    TABH+60(20) = FFFFFFFF047C0300509000000400010602000000                                        |
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
|    id                      = 17275 (0x7B430000)                                                  |
|    label                   = 186180 (0x44D70200)                                                 |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 160   (0xA0000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000892/TYPE#000076                                             |
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
|LS_DATA                                                                                           |
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
|%_ITAB_DELETE_LIST                                                                                |
|LS_ITEM-TEXT                                                                                      |
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
|LS_DATA->MAT_DESC                                                                                 |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|CL_DRAGDROP=>USEDEFAULTEFFECT                                                                     |
|    4                                                                                             |
|    0000                                                                                          |
|    4000                                                                                          |
|    04000000                                                                                      |
|LTABIX                                                                                            |
|    2                                                                                             |
|    0000                                                                                          |
|    2000                                                                                          |
|    02000000                                                                                      |
----------------------------------------------------------------------------------------------------
|No.      18 Ty.          METHOD                                                                   |
|Name  LCL_APP=>ADJUST_NODE_WNOTE                                                                  |
----------------------------------------------------------------------------------------------------
|PAR_INFOS                                                                                         |
|    Table IT_186179[1x24]                                                                         |
|PAR_LABELS                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|PAR_ITEMS                                                                                         |
|    Table IT_186165[4x424]                                                                        |
|SYST-REPID                                                                                        |
|    ZCL_IM_EEWA_WDOC_TREE=========CP                                                              |
|    5445445445455444555443333333334522222222                                                      |
|    A3CF9DF5571F74F3F4255DDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5A0043004C005F0049004D005F0045004500570041005F00570044004F0043005F0054005200450045003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY-REPID                                                                                          |
|    ZCL_IM_EEWA_WDOC_TREE=========CP                                                              |
|    5445445445455444555443333333334522222222                                                      |
|    A3CF9DF5571F74F3F4255DDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5A0043004C005F0049004D005F0045004500570041005F00570044004F0043005F0054005200450045003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      17 Ty.          METHOD                                                                   |
|Name  LCL_APP=>IF_EX_EEWAEL_TREE~ADJUST_NODES                                                     |
----------------------------------------------------------------------------------------------------
|PAR_NAME                                                                                          |
|    TREEDEFAULT                                                                                   |
|    5544444454522222222222222222222222222222222222222222222222222222222222                        |
|    425545615C400000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    5400520045004500440045004600410055004C0054002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|PAR_NODES                                                                                         |
|    Table IT_186164[1x280]                                                                        |
|    \CLASS=CL_EEWA_FORM_TREE\METHOD=TRANSFERTREEDATA\DATA=LUPDATEITEMS-ADDED_NODES                |
|    Table reference: 16177                                                                        |
|    TABH+ 0(20) = 884CDE2EC07F000000000000000000000099182F                                        |
|    TABH+20(20) = C07F000000000000000000000100000044040000                                        |
|    TABH+40(20) = 00000000313F000034D702000100000018010000                                        |
|    TABH+60(20) = FFFFFFFF049B0200005E07000100010602000000                                        |
|    store                   = 0x884CDE2EC07F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0x0099182FC07F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 1     (ItAccStandard)                                               |
|      uniKind               = 2     (ItUniNo)                                                     |
|      keyKind (from pbag)   = 1     (default)                                                     |
|      cmpMode               = 8     (cmpManyEq)                                                   |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 16177 (0x313F0000)                                                  |
|    label                   = 186164 (0x34D70200)                                                 |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 280   (0x18010000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000667/TYPE#000058                                             |
|    occu                    = 1     (0x01000000)                                                  |
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
|    tabi                    = 0x008FFACEBF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 165801 (0xA9870200)                                                 |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 1     (0x01000000)                                                  |
|    lineAlloc               = 1     (0x01000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0xC0E6DF2EC07F0000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|    {         74;         66;0;;;;@PG\QVejeseddel@;@PG\QVejeseddel@;0;19;;;0}                     |
|    202020202020202020303020202020202020202020303020000020202040504050505060606070606060606040202\|
|0202020202020202020202020202020202020202020202020202020204050405050506060607060606060604020202020\|
|20202020202020202020202020202020202020202020202020200000003030202                                 |
|    0000000000000000007040000000000000000000006060000000000000000070C0106050A0503050404050C000000\|
|000000000000000000000000000000000000000000000000000000000000070C0106050A0503050404050C00000000000\|
|00000000000000000000000000000000000000000000000000000000001090000                                 |
|    200020002000200020002000200020002000370034002000200020002000200020002000200020002000360036002\|
|000000000002000200020004000500047005C005100560065006A006500730065006400640065006C0040002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|02000200020004000500047005C005100560065006A006500730065006400640065006C00400020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00000000000000031003900200020                                                                     |
|PAR_ITEMS                                                                                         |
|    Table IT_186165[4x424]                                                                        |
|PAR_UNODES                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|PAR_UITEMS                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|PAR_DNODES                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|%_FSREG_001                                                                                       |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|SCREEN                                                                                            |
|    {TB_MSKITEM_BADH;BEI;;;;0;1;0;0;1;00;1;0;0;0;0;0}                                             |
|    504050405040405040405040404040202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    4020F0D030B0904050D0F020104080000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    540042005F004D0053004B004900540045004D005F004200410044004800200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|ME                                                                                                |
|    {O:709*\CLASS-POOL=ZCL_IM_EEWA_WDOC_TREE\CLASS=LCL_APP}                                       |
|    3000C000                                                                                      |
|    A0005200                                                                                      |
|    3A000000C5020000                                                                              |
|LT_INFOS_ITEM                                                                                     |
|    Table[initial]                                                                                |
|                                                                                                  |
|LT_LABELS                                                                                         |
|    Table[initial]                                                                                |
|                                                                                                  |
|LT_INFOS_WNOTE                                                                                    |
|    Table IT_186179[1x24]                                                                         |
----------------------------------------------------------------------------------------------------
|No.      16 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_TREE=>PLUGIN_ADJUST_NODES                                                      |
----------------------------------------------------------------------------------------------------
|PAR_NAME                                                                                          |
|    TREEDEFAULT                                                                                   |
|    5544444454522222222222222222222222222222222222222222222222222222222222                        |
|    425545615C400000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    5400520045004500440045004600410055004C0054002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|PAR_NODES                                                                                         |
|    Table IT_186164[1x280]                                                                        |
|PAR_ITEMS                                                                                         |
|    Table IT_186165[4x424]                                                                        |
|PAR_UNODES                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|PAR_UITEMS                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|PAR_DNODES                                                                                        |
|    Table[initial]                                                                                |
|                                                                                                  |
|LIFTREE                                                                                           |
|    {O:709*\CLASS-POOL=ZCL_IM_EEWA_WDOC_TREE\CLASS=LCL_APP}                                       |
|    3000C000                                                                                      |
|    A0005200                                                                                      |
|    3A000000C5020000                                                                              |
|LIFTREEO                                                                                          |
|    {O:709*\CLASS-POOL=ZCL_IM_EEWA_WDOC_TREE\CLASS=LCL_APP}                                       |
|    3000C000                                                                                      |
|    A0005200                                                                                      |
|    3A000000C5020000                                                                              |
|%_SPACE                                                                                           |
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
|SY-XPROG                                                                                          |
|    SAPCNVE                                                                                       |
|    5454454222222222222222222222222222222222                                                      |
|    3103E65000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    53004100500043004E005600450020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
----------------------------------------------------------------------------------------------------
|No.      15 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_TREE=>TREE_ADD_NODES_AND_ITEMS                                                 |
----------------------------------------------------------------------------------------------------
|NODE_TABLE                                                                                        |
|    Table IT_186164[1x280]                                                                        |
|ITEM_TABLE                                                                                        |
|    Table IT_186165[4x424]                                                                        |
|ME                                                                                                |
|    {O:594*\FUNCTION-POOL=EEWA_WDORDERRESULT\CLASS=LCL_APP}                                       |
|    A0005000                                                                                      |
|    D0002200                                                                                      |
|    AD00000052020000                                                                              |
|LTVITEMREF->NODE_KEY                                                                              |
|             74                                                                                   |
|    222222222332                                                                                  |
|    000000000740                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    200020002000200020002000200020002000370034002000                                              |
|LNODEREF                                                                                          |
|    {A:3761*\TYPE=EWAEL_TREEV_NODE}                                                               |
|    7000B000                                                                                      |
|    E0001E00                                                                                      |
|    7E000000B10E0000                                                                              |
|CL_DRAGDROP=>USEDEFAULTEFFECT                                                                     |
|    4                                                                                             |
|    0000                                                                                          |
|    4000                                                                                          |
|    04000000                                                                                      |
|LNODEKEY                                                                                          |
|             74                                                                                   |
|    222222222332                                                                                  |
|    000000000740                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    200020002000200020002000200020002000370034002000                                              |
|LNODEREF->CTRLSTATE                                                                               |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|CL_ITEM_TREE_CONTROL=>EVENTID_ITEM_DOUBLE_CLICK                                                   |
|    22                                                                                            |
|    1000                                                                                          |
|    6000                                                                                          |
|    16000000                                                                                      |
|SY                                                                                                |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
|ME->FTREECTRLCACHE-ITEMS                                                                          |
|    Table IT_17766[0x424]                                                                         |
|    {O:594*\FUNCTION-POOL=EEWA_WDORDERRESULT\CLASS=LCL_APP}\DATA=FTREECTRLCACHE-ITEMS             |
|    Table reference: 12097                                                                        |
|    TABH+ 0(20) = D042B92BC07F00000000000000000000D05A7321                                        |
|    TABH+20(20) = C07F0000000000000000000000000000A8010000                                        |
|    TABH+40(20) = 00000000412F00006645000000000000A8010000                                        |
|    TABH+60(20) = FFFFFFFF049B0200705D07000400010602000000                                        |
|    store                   = 0xD042B92BC07F0000                                                  |
|    statScndKeyAdmin        = 0x0000000000000000                                                  |
|    ext1                    = 0xD05A7321C07F0000                                                  |
|    ----- Primary Key -----                                                                       |
|    idxPtr                  = 0x0000000000000000                                                  |
|      idxKind               = 0     (ItIndexNone)                                                 |
|      accKind               = 2     (ItAccSorted)                                                 |
|      uniKind               = 1     (ItUniYes)                                                    |
|      keyKind (from pbag)   = 3     (user defined)                                                |
|      cmpMode               = 3     (cmpSingleMcmpU)                                              |
|    shmId                   = 0     (0x00000000)                                                  |
|    id                      = 12097 (0x412F0000)                                                  |
|    label                   = 17766 (0x66450000)                                                  |
|    fill                    = 0     (0x00000000)                                                  |
|    leng                    = 424   (0xA8010000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000667/TYPE#000055                                             |
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
|    tabi                    = 0x9872B92BC07F0000                                                  |
|    pgHook                  = 0xC0CBB42BC07F0000                                                  |
|    uniqueNumber            = 17027 (0x83420000)                                                  |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 284   (0x1C010000)                                                  |
|    lineAlloc               = 12    (0x0C000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|    ----- 1st level extension part -----                                                          |
|    regHook                 = 0x0000000000000000                                                  |
|    collHook                = 0x0000000000000000                                                  |
|    ext2                    = 0x0000000000000000                                                  |
|LTVITEMREF->*                                                                                     |
|    {         74;5;2;2;;;;3;;;;0;0;;;;;0002;}                                                     |
|    202020202020202020303020302020202020202020202020000000002020200000002020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202000000000200020202000303030\|
|30202020202020202020202020202020202020202020202020202020202020202                                 |
|    000000000000000000704000500000000000000000000000200020000000000030000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|20000000000000000000000000000000000000000000000000000000000000000                                 |
|    200020002000200020002000200020002000370034002000350020002000200020002000200020002000200020002\|
|0000200000002000000200020002000000003000000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200000000000000000002000000020002000200000003000300030003200\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|ME->FCURRENTTREE                                                                                  |
|    TREEDEFAULT                                                                                   |
|    5544444454522222222222222222222222222222222222222222222222222222222222                        |
|    425545615C400000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    5400520045004500440045004600410055004C0054002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SY-REPID                                                                                          |
|    CL_EEWA_FORM_TREE=============CP                                                              |
|    4454454544545554433333333333334522222222                                                      |
|    3CF5571F6F2DF4255DDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0045004500570041005F0046004F0052004D005F0054005200450045003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
----------------------------------------------------------------------------------------------------
|No.      14 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_TREE=>TRANSFERTREEDATA                                                         |
----------------------------------------------------------------------------------------------------
|PAR_FLUSH                                                                                         |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|LNEEDED                                                                                           |
|    Table IT_186170[1x24]                                                                         |
|    \CLASS=CL_EEWA_FORM_TREE\METHOD=TRANSFERTREEDATA\DATA=LNEEDED                                 |
|    Table reference: 17323                                                                        |
|    TABH+ 0(20) = 2898182FC07F0000000000000000000000000000                                        |
|    TABH+20(20) = 0000000000000000000000000100000044010000                                        |
|    TABH+40(20) = 00000000AB4300003AD702000100000018000000                                        |
|    TABH+60(20) = FFFFFFFF049B0200205F07000400410602000000                                        |
|    store                   = 0x2898182FC07F0000                                                  |
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
|    id                      = 17323 (0xAB430000)                                                  |
|    label                   = 186170 (0x3AD70200)                                                 |
|    fill                    = 1     (0x01000000)                                                  |
|    leng                    = 24    (0x18000000)                                                  |
|    loop                    = -1    (0xFFFFFFFF)                                                  |
|    xtyp                    = PROG#000667/TYPE#000064                                             |
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
|    tabi                    = 0x38DFA8A4BF7F0000                                                  |
|    pgHook                  = 0x0000000000000000                                                  |
|    uniqueNumber            = 165806 (0xAE870200)                                                 |
|    shmTabhSet              = 0x0000000000000000                                                  |
|    refCount                = 0     (0x00000000)                                                  |
|    tstRefCount             = 0     (0x00000000)                                                  |
|    lineAdmin               = 4     (0x04000000)                                                  |
|    lineAlloc               = 4     (0x04000000)                                                  |
|    shmVersId               = 0     (0x00000000)                                                  |
|    shmRefCount             = 1     (0x01000000)                                                  |
|    rowId                   = 18446744073709551615                                                |
|             74                                                                                   |
|    222222222332                                                                                  |
|    000000000740                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    200020002000200020002000200020002000370034002000                                              |
|LNODEREF->NODE_KEY                                                                                |
|             74                                                                                   |
|    222222222332                                                                                  |
|    000000000740                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    200020002000200020002000200020002000370034002000                                              |
|ME                                                                                                |
|    {O:594*\FUNCTION-POOL=EEWA_WDORDERRESULT\CLASS=LCL_APP}                                       |
|    A0005000                                                                                      |
|    D0002200                                                                                      |
|    AD00000052020000                                                                              |
|LUPDATEITEMS-ADDED_NODES                                                                          |
|    Table IT_186164[1x280]                                                                        |
|LUPDATEITEMS-ADDED_ITEMS                                                                          |
|    Table IT_186165[4x424]                                                                        |
|LTVNODEREF                                                                                        |
|    FREED ITAB:{A:3755*\TYPE=TREEV_NODE}                                                          |
|    6000A000                                                                                      |
|    4000BE00                                                                                      |
|    64000000AB0E0000                                                                              |
|LTVNODEREF->NODE_KEY                                                                              |
|    ???                                                                                           |
|    ??????                                                                                        |
|    ??????                                                                                        |
|    ????????????                                                                                  |
|LNODEREF                                                                                          |
|    {A:3754*\CLASS=CL_EEWA_FORM_TREE\TYPE=TTREENODE}                                              |
|    6000A000                                                                                      |
|    5000AE00                                                                                      |
|    65000000AA0E0000                                                                              |
----------------------------------------------------------------------------------------------------
|No.      13 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_TREE=>ENDUPDATE                                                                |
----------------------------------------------------------------------------------------------------
|ME->FUPDATECOUNT                                                                                  |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|SYST                                                                                              |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
|ME                                                                                                |
|    {O:594*\FUNCTION-POOL=EEWA_WDORDERRESULT\CLASS=LCL_APP}                                       |
|    A0005000                                                                                      |
|    D0002200                                                                                      |
|    AD00000052020000                                                                              |
----------------------------------------------------------------------------------------------------
|No.      12 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_TREE=>HANDLE_TREE_EXPAND_NO_CHILDREN                                           |
----------------------------------------------------------------------------------------------------
|NODE_KEY                                                                                          |
|             66                                                                                   |
|    222222222332                                                                                  |
|    000000000660                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    200020002000200020002000200020002000360036002000                                              |
|ME                                                                                                |
|    {O:594*\FUNCTION-POOL=EEWA_WDORDERRESULT\CLASS=LCL_APP}                                       |
|    A0005000                                                                                      |
|    D0002200                                                                                      |
|    AD00000052020000                                                                              |
|CL_ABAP_OBJECTDESCR=>CHANGING                                                                     |
|    C                                                                                             |
|    4                                                                                             |
|    3                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    4300                                                                                          |
----------------------------------------------------------------------------------------------------
|No.      11 Ty.          METHOD                                                                   |
|Name  CL_TREE_CONTROL_BASE=>HANDLE_NODE_KEY_EVENT                                                 |
----------------------------------------------------------------------------------------------------
|EVENTID                                                                                           |
|    18                                                                                            |
|    1000                                                                                          |
|    2000                                                                                          |
|    12000000                                                                                      |
|SY-SUBRC                                                                                          |
|    4                                                                                             |
|    0000                                                                                          |
|    4000                                                                                          |
|    04000000                                                                                      |
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
|SY-REPID                                                                                          |
|    CL_TREE_CONTROL_BASE==========CP                                                              |
|    4455544544455445445433333333334522222222                                                      |
|    3CF4255F3FE42FCF2135DDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0054005200450045005F0043004F004E00540052004F004C005F0042004100530045003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|SY-MSGID                                                                                          |
|    EEWA_EL                                                                                       |
|    44545442222222222222                                                                          |
|    5571F5C0000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    45004500570041005F0045004C002000200020002000200020002000200020002000200020002000              |
|SPACE                                                                                             |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SY-MSGNO                                                                                          |
|    016                                                                                           |
|    333                                                                                           |
|    016                                                                                           |
|    000                                                                                           |
|    000                                                                                           |
|    300031003600                                                                                  |
|SY-MSGV1                                                                                          |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|SY-MSGV2                                                                                          |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|SY-MSGV3                                                                                          |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|SY-MSGV4                                                                                          |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|ME                                                                                                |
|    {O:413*\CLASS=CL_EEWA_LIST_TREE}                                                              |
|    60009000                                                                                      |
|    2000D100                                                                                      |
|    620000009D010000                                                                              |
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
|NODE_KEY                                                                                          |
|             66                                                                                   |
|    222222222332                                                                                  |
|    000000000660                                                                                  |
|    000000000000                                                                                  |
|    000000000000                                                                                  |
|    200020002000200020002000200020002000360036002000                                              |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SCREEN                                                                                            |
|    {TB_MSKITEM_BADH;BEI;;;;0;1;0;0;1;00;1;0;0;0;0;0}                                             |
|    504050405040405040405040404040202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    4020F0D030B0904050D0F020104080000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    540042005F004D0053004B004900540045004D005F004200410044004800200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.      10 Ty.          METHOD                                                                   |
|Name  CL_TREE_CONTROL_BASE=>HANDLE_BASE_EVENT                                                     |
----------------------------------------------------------------------------------------------------
|EVENTID                                                                                           |
|    18                                                                                            |
|    1000                                                                                          |
|    2000                                                                                          |
|    12000000                                                                                      |
|IS_SHELLEVENT                                                                                     |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|IS_SYSTEMDISPATCH                                                                                 |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|ME                                                                                                |
|    {O:413*\CLASS=CL_EEWA_LIST_TREE}                                                              |
|    60009000                                                                                      |
|    2000D100                                                                                      |
|    620000009D010000                                                                              |
|%_EVNTSENDER                                                                                      |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|%_EVNTSLOT                                                                                        |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|CTMENU                                                                                            |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|SY                                                                                                |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
|SYST-REPID                                                                                        |
|    CL_TREE_CONTROL_BASE==========CP                                                              |
|    4455544544455445445433333333334522222222                                                      |
|    3CF4255F3FE42FCF2135DDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0054005200450045005F0043004F004E00540052004F004C005F0042004100530045003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|CL_GUI_OBJECT=>JAVABEAN                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SYST                                                                                              |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       9 Ty.          METHOD                                                                   |
|Name  CL_TREE_CONTROL_BASE=>DISPATCH                                                              |
----------------------------------------------------------------------------------------------------
|CARGO                                                                                             |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|EVENTID                                                                                           |
|    18                                                                                            |
|    1000                                                                                          |
|    2000                                                                                          |
|    12000000                                                                                      |
|IS_SHELLEVENT                                                                                     |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|IS_SYSTEMDISPATCH                                                                                 |
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
|SY-XPROG                                                                                          |
|    SAPCNVE                                                                                       |
|    5454454222222222222222222222222222222222                                                      |
|    3103E65000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    53004100500043004E005600450020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|BASE_EVENT                                                                                        |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|ME->CTX_EVENT_BASE                                                                                |
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
|ME                                                                                                |
|    {O:413*\CLASS=CL_EEWA_LIST_TREE}                                                              |
|    60009000                                                                                      |
|    2000D100                                                                                      |
|    620000009D010000                                                                              |
----------------------------------------------------------------------------------------------------
|No.       8 Ty.          METHOD                                                                   |
|Name  CL_GUI_CFW=>DISPATCH                                                                        |
----------------------------------------------------------------------------------------------------
|RETURN_CODE                                                                                       |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SY-REPID                                                                                          |
|    CL_GUI_CFW====================CP                                                              |
|    4454545445333333333333333333334522222222                                                      |
|    3CF759F367DDDDDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F004700550049005F004300460057003D003D003D003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|L_EVENT                                                                                           |
|    {0;{O:6241*\CLASS-POOL=CL_GUI_CFW\CLASS=LCL_EVENT}}                                           |
|    000090006100                                                                                  |
|    0000E0001800                                                                                  |
|    000000009E00000061180000                                                                      |
|CL_GUI_CFW=>SYSTEM_STATE                                                                          |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|IS_SHELLEVENT                                                                                     |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SYST-REPID                                                                                        |
|    CL_GUI_CFW====================CP                                                              |
|    4454545445333333333333333333334522222222                                                      |
|    3CF759F367DDDDDDDDDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F004700550049005F004300460057003D003D003D003D003D003D003D003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|EVENTID                                                                                           |
|    18                                                                                            |
|    1000                                                                                          |
|    2000                                                                                          |
|    12000000                                                                                      |
|SY-XFORM                                                                                          |
|    CONVERSION_EXIT                                                                               |
|    444545544454545222222222222222                                                                |
|    3FE65239FEF5894000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    000000000000000000000000000000                                                                |
|    43004F004E00560045005200530049004F004E005F004500580049005400200020002000200020002000200020002\|
|000200020002000200020002000                                                                       |
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
|MY_SHELL                                                                                          |
|    126                                                                                           |
|    7000                                                                                          |
|    E000                                                                                          |
|    7E000000                                                                                      |
|CARGO                                                                                             |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|SCREEN                                                                                            |
|    {TB_MSKITEM_BADH;BEI;;;;0;1;0;0;1;00;1;0;0;0;0;0}                                             |
|    504050405040405040405040404040202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    4020F0D030B0904050D0F020104080000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    540042005F004D0053004B004900540045004D005F004200410044004800200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|CL_GUI_CFW=>CFW_SUBSCRIBER-REF                                                                    |
|    {O:7449*\CLASS=CL_EEWA_GRID}                                                                  |
|    E0001100                                                                                      |
|    60009D00                                                                                      |
|    E6000000191D0000                                                                              |
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
|No.       7 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_BASE=>HANDLE_DISPATCH                                                          |
----------------------------------------------------------------------------------------------------
|PAR_OK_CODE                                                                                       |
|    %_GC 126 18                                                                                   |
|    2544233323322222222222222222222222222222222222222222222222222222222222                        |
|    5F73012601800000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    25005F004700430020003100320036002000310038002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|PAR_ATEXIT                                                                                        |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|PAR_HANDLED                                                                                       |
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
|SYST-REPID                                                                                        |
|    CL_EEWA_FORM_BASE=============CP                                                              |
|    4454454544545445433333333333334522222222                                                      |
|    3CF5571F6F2DF2135DDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0045004500570041005F0046004F0052004D005F0042004100530045003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|LAPP                                                                                              |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|SY-MSGV3                                                                                          |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SYST                                                                                              |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
|ME->C_OKCODE_FAIL                                                                                 |
|    __FAIL__                                                                                      |
|    5544445522222222222222222222222222222222222222222222222222222222222222                        |
|    FF619CFF00000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    5F005F004600410049004C005F005F002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|CX_EEWA_SECURITY=>TA_CALL_NOT_EXISTS                                                              |
|    {S#;343;TEXT;;;}                                                                              |
|    5020202020202020202020202020202020202020303030004B7100004B7120004B7120004B712000              |
|    30300000000000000000000000000000000000003040300041F0000041F0000041F0000041F00000              |
|    530023002000200020002000200020002000200020002000200020002000200020002000200020003300340033000\|
|00044B17F100000000044B17F102000000044B17F102000000044B17F1020000000                               |
|SY-REPID                                                                                          |
|    CL_EEWA_FORM_BASE=============CP                                                              |
|    4454454544545445433333333333334522222222                                                      |
|    3CF5571F6F2DF2135DDDDDDDDDDDDD3000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    43004C005F0045004500570041005F0046004F0052004D005F0042004100530045003D003D003D003D003D003D003\|
|D003D003D003D003D003D003D004300500020002000200020002000200020002000                               |
|CL_DRAGDROP=>MOVE                                                                                 |
|    2                                                                                             |
|    0000                                                                                          |
|    2000                                                                                          |
|    02000000                                                                                      |
|%_SPACE                                                                                           |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|LGUI_RETCODE                                                                                      |
|    0                                                                                             |
|    0000                                                                                          |
|    0000                                                                                          |
|    00000000                                                                                      |
|SCREEN                                                                                            |
|    {TB_MSKITEM_BADH;BEI;;;;0;1;0;0;1;00;1;0;0;0;0;0}                                             |
|    504050405040405040405040404040202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    4020F0D030B0904050D0F020104080000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    540042005F004D0053004B004900540045004D005F004200410044004800200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|ME                                                                                                |
|    {O:734*\FUNCTION-POOL=EEWA_WDOITEM\CLASS=LCL_APP}                                             |
|    2000D000                                                                                      |
|    1000E200                                                                                      |
|    21000000DE020000                                                                              |
|ME->FCURSORFIELD                                                                                  |
|    <empty string>                                                                                |
|    10000000                                                                                      |
|    40000000                                                                                      |
|    1400000000000000                                                                              |
----------------------------------------------------------------------------------------------------
|No.       6 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_BASE=>HANDLE_DISPATCH_SUBAPP                                                   |
----------------------------------------------------------------------------------------------------
|PAR_OK_CODE                                                                                       |
|    %_GC 126 18                                                                                   |
|    2544233323322222222222222222222222222222222222222222222222222222222222                        |
|    5F73012601800000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    25005F004700430020003100320036002000310038002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|PAR_ATEXIT                                                                                        |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|PAR_HANDLED                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|ME->FPBO_INITIALIZED                                                                              |
|    X                                                                                             |
|    5                                                                                             |
|    8                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    5800                                                                                          |
|ME->FPBO_OK_CODE                                                                                  |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|ME                                                                                                |
|    {O:734*\FUNCTION-POOL=EEWA_WDOITEM\CLASS=LCL_APP}                                             |
|    2000D000                                                                                      |
|    1000E200                                                                                      |
|    21000000DE020000                                                                              |
|SY-MSGV2                                                                                          |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
|ME->FDOLEAVESCREEN                                                                                |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SY                                                                                                |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       5 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_BASE=>HANDLE_DISPATCH                                                          |
----------------------------------------------------------------------------------------------------
|PAR_OK_CODE                                                                                       |
|    %_GC 126 18                                                                                   |
|    2544233323322222222222222222222222222222222222222222222222222222222222                        |
|    5F73012601800000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    25005F004700430020003100320036002000310038002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|PAR_ATEXIT                                                                                        |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|PAR_HANDLED                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|ME                                                                                                |
|    {O:594*\FUNCTION-POOL=EEWA_WDORDERRESULT\CLASS=LCL_APP}                                       |
|    A0005000                                                                                      |
|    D0002200                                                                                      |
|    AD00000052020000                                                                              |
|ME->CPAI_AT-HANDLE_DISPATCH_BEGIN                                                                 |
|    B                                                                                             |
|    4                                                                                             |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    4200                                                                                          |
|ME->FUSEOWNOK_CODE                                                                                |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|ME->FOK_CODE                                                                                      |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|LCODE                                                                                             |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|LAPP                                                                                              |
|    {O:734*\FUNCTION-POOL=EEWA_WDOITEM\CLASS=LCL_APP}                                             |
|    2000D000                                                                                      |
|    1000E200                                                                                      |
|    21000000DE020000                                                                              |
|ME->C_OKCODE_FAIL                                                                                 |
|    __FAIL__                                                                                      |
|    5544445522222222222222222222222222222222222222222222222222222222222222                        |
|    FF619CFF00000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    5F005F004600410049004C005F005F002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|CX_EEWA_SECURITY=>TA_CALL_NOT_EXISTS                                                              |
|    {S#;343;TEXT;;;}                                                                              |
|    5020202020202020202020202020202020202020303030004B7100004B7120004B7120004B712000              |
|    30300000000000000000000000000000000000003040300041F0000041F0000041F0000041F00000              |
|    530023002000200020002000200020002000200020002000200020002000200020002000200020003300340033000\|
|00044B17F100000000044B17F102000000044B17F102000000044B17F1020000000                               |
----------------------------------------------------------------------------------------------------
|No.       4 Ty.          METHOD                                                                   |
|Name  LCL_APP=>IF_EEWA_WDOC_APPLICATION~HANDLE_DISPATCH                                           |
----------------------------------------------------------------------------------------------------
|PAR_OKCODE                                                                                        |
|    %_GC 126 18                                                                                   |
|    2544233323322222222222222222222222222222222222222222222222222222222222                        |
|    5F73012601800000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    25005F004700430020003100320036002000310038002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|PAR_HANDLED                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SY-REPID                                                                                          |
|    SAPLEEWA_WDORDERRESULT                                                                        |
|    5454445455445445545545222222222222222222                                                      |
|    310C5571F74F24522535C4000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C0045004500570041005F00570044004F00520044004500520052004500530055004C00540020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SYST-REPID                                                                                        |
|    SAPLEEWA_WDORDERRESULT                                                                        |
|    5454445455445445545545222222222222222222                                                      |
|    310C5571F74F24522535C4000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C0045004500570041005F00570044004F00520044004500520052004500530055004C00540020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|ME                                                                                                |
|    {O:594*\FUNCTION-POOL=EEWA_WDORDERRESULT\CLASS=LCL_APP}                                       |
|    A0005000                                                                                      |
|    D0002200                                                                                      |
|    AD00000052020000                                                                              |
|SY-SUBRC                                                                                          |
|    4                                                                                             |
|    0000                                                                                          |
|    4000                                                                                          |
|    04000000                                                                                      |
|ME->FDOLEAVESCREEN                                                                                |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SYST                                                                                              |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
----------------------------------------------------------------------------------------------------
|No.       3 Ty.          METHOD                                                                   |
|Name  LCL_APP=>HANDLE_DISPATCH                                                                    |
----------------------------------------------------------------------------------------------------
|PAR_OK_CODE                                                                                       |
|    %_GC 126 18                                                                                   |
|    2544233323322222222222222222222222222222222222222222222222222222222222                        |
|    5F73012601800000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    25005F004700430020003100320036002000310038002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|PAR_ATEXIT                                                                                        |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|PAR_HANDLED                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|SYST-REPID                                                                                        |
|    SAPLEEWA_WDOCOCKPIT                                                                           |
|    5454445455444444545222222222222222222222                                                      |
|    310C5571F74F3F3B094000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C0045004500570041005F00570044004F0043004F0043004B0050004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|SY-REPID                                                                                          |
|    SAPLEEWA_WDOCOCKPIT                                                                           |
|    5454445455444444545222222222222222222222                                                      |
|    310C5571F74F3F3B094000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    0000000000000000000000000000000000000000                                                      |
|    5300410050004C0045004500570041005F00570044004F0043004F0043004B0050004900540020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000                               |
|ME->FACTIVEWDOCAPPLICATION                                                                        |
|    {A:223*\FUNCTION-POOL=EEWA_WDOCOCKPIT\CLASS=LCL_APP\TYPE=TWDOCAPPLICATION}                    |
|    1000D000                                                                                      |
|    0000F000                                                                                      |
|    10000000DF000000                                                                              |
|SY                                                                                                |
|    {1;0;0;0;0;0;20;0;0;0;0;1;1;0;1;0;0;0;0;160;0;0;0;4;4;0;0;0;0;0;0;0;0;0;0;0;0;0;24;177;0;11;0\|
|;3600;X;I;0000;3;0;0;;;;K;0;;;;0100;;;;;;0;D$S;110;;;0;0;;;;;;;;;;00;;;;;;;;;;;;000;;;;;;;;;HDB;;\|
|;S4Q;Linux;EDIT;758;EWAWDOC;;;;0000000000;0000000000;;00000000;20                                 |
|    0000000000000000000000001000000000000000000000000000000000000000000000000000A0000000000000000\|
|000000000000000000000000000000000000000000000000000000000001000B000000000000000100050400000000000\|
|00002020204000000020202030303030202020202020202000000040205020202                                 |
|    100000000000000000000000400000000000000000001000100000001000000000000000000000000000000000004\|
|00040000000000000000000000000000000000000000000000000000000800010000000B00000000E0080900030000C00\|
|00C0000000B000000000000000100000000000000000000000000040403000000                                 |
|    010000000000000000000000000000000000000000000000140000000000000000000000000000000000000001000\|
|00001000000000000000100000000000000000000000000000000000000A0000000000000000000000000000000040000\|
|0004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000018000000B1000000000000000B00000000000000100E000058004900000003000000000C00000000\|
|0C002000200020004B0000000000000020002000200030003100300030002000200020002000200020002000200000000\|
|00000004400240053002000200020                                                                     |
|%_DUMMY$$                                                                                         |
|                                                                                                  |
|    2222                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    0000                                                                                          |
|    2000200020002000                                                                              |
|SCREEN                                                                                            |
|    {TB_MSKITEM_BADH;BEI;;;;0;1;0;0;1;00;1;0;0;0;0;0}                                             |
|    504050405040405040405040404040202020202020202020202020202020202020202020202020202020202020202\|
|0202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020202020\|
|20202020202020202020202020202020202020202020202020202020202020202                                 |
|    4020F0D030B0904050D0F020104080000000000000000000000000000000000000000000000000000000000000000\|
|0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\|
|00000000000000000000000000000000000000000000000000000000000000000                                 |
|    540042005F004D0053004B004900540045004D005F004200410044004800200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200\|
|0200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000\|
|2000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|00020002000200020002000200020                                                                     |
|ME                                                                                                |
|    {O:4*\FUNCTION-POOL=EEWA_WDOCOCKPIT\CLASS=LCL_APP}                                            |
|    F0000000                                                                                      |
|    B0004000                                                                                      |
|    FB00000004000000                                                                              |
|SY-MSGV1                                                                                          |
|                                                                                                  |
|    22222222222222222222222222222222222222222222222222                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    00000000000000000000000000000000000000000000000000                                            |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|0020002000                                                                                        |
----------------------------------------------------------------------------------------------------
|No.       2 Ty.          METHOD                                                                   |
|Name  CL_EEWA_FORM_BASE=>PAI_DISPATCH                                                             |
----------------------------------------------------------------------------------------------------
|PAR_OK_CODE                                                                                       |
|                                                                                                  |
|    2222222222222222222222222222222222222222222222222222222222222222222222                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|PAR_HANDLED                                                                                       |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
|LDIALOG                                                                                           |
|    {O:initial}                                                                                   |
|    F0000000                                                                                      |
|    F0000000                                                                                      |
|    FF00000000000000                                                                              |
|SY-MSGID                                                                                          |
|    EEWA_EL                                                                                       |
|    44545442222222222222                                                                          |
|    5571F5C0000000000000                                                                          |
|    00000000000000000000                                                                          |
|    00000000000000000000                                                                          |
|    45004500570041005F0045004C002000200020002000200020002000200020002000200020002000              |
|ME                                                                                                |
|    {O:4*\FUNCTION-POOL=EEWA_WDOCOCKPIT\CLASS=LCL_APP}                                            |
|    F0000000                                                                                      |
|    B0004000                                                                                      |
|    FB00000004000000                                                                              |
|LCODE                                                                                             |
|    %_GC 126 18                                                                                   |
|    2544233323322222222222222222222222222222222222222222222222222222222222                        |
|    5F73012601800000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    0000000000000000000000000000000000000000000000000000000000000000000000                        |
|    25005F004700430020003100320036002000310038002000200020002000200020002000200020002000200020002\|
|0002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000200020\|
|002000200020002000200020002000200020002000200020002000200020002000200020002000200020002000        |
|ME->FDOLEAVESCREEN                                                                                |
|                                                                                                  |
|    2                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    0                                                                                             |
|    2000                                                                                          |
----------------------------------------------------------------------------------------------------
|No.       1 Ty.          MODULE (PAI)                                                             |
|Name  USER_COMMAND_0100                                                                           |
----------------------------------------------------------------------------------------------------
|G_APPLICATION                                                                                     |
|    {O:4*\FUNCTION-POOL=EEWA_WDOCOCKPIT\CLASS=LCL_APP}                                            |
|    F0000000                                                                                      |
|    B0004000                                                                                      |
|    FB00000004000000                                                                              |
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

----------------------------------------------------------------------------------------------------
|Information About Memory Usage                                                                    |
----------------------------------------------------------------------------------------------------
|Memory Consumption                                                                                |
----------------------------------------------------------------------------------------------------
|Memory Area                                                      Bytes                            |
----------------------------------------------------------------------------------------------------
|Extended Memory (EM)                                        78.832.000                            |
|PRIV Memory (Heap)                                                   0                            |
|SAP Paging                                                   3.825.664                            |
----------------------------------------------------------------------------------------------------
|MM Memory Management                                    Bytes / Number                            |
----------------------------------------------------------------------------------------------------
|Used Memory                                                 71.204.144                            |
|Free Memory                                                  7.620.520                            |
|Largest Free Block                                           4.004.768                            |
|Used Blocks                                                     94.771                            |
|Free Blocks                                                         53                            |
|Maximum Used Memory                                         96.089.488                            |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Shared Objects Locks                                                                              |
----------------------------------------------------------------------------------------------------
|ModeLockRe Area Name                                                                              |
|Instance Name                                                                    Clt Lock Type    |
|Version ID Label                Include                                  Line       Time Stamp    |
----------------------------------------------------------------------------------------------------
|         3 CL_GUI_MEMORY                                                                          |
|$DEFAULT_INSTANCE$                                                               110 R            |
|      7344               426750 CL_GUI_FRONTEND_SERVICES======CM01Z              23 20260617132332|
|         4 CL_ESH_RUBU_MEM_MAIN_AREA                                                              |
|110_0                                                                                R            |
|        73                   90 CL_ESH_RUBU_MEM_MAIN==========CM002              37 20260617133509|
|         5 CL_ESH_RUBU_MEM_LANGU_AREA                                                             |
|110_0_K                                                                              R            |
|       242                  351 CL_ESH_RUBU_MEM_LANGU=========CM003              15 20260617133509|
|         6 CL_ESH_RUBU_MEM_LANGU_AREA                                                             |
|110_0_E                                                                              R            |
|        71                   87 CL_ESH_RUBU_MEM_LANGU=========CM003              15 20260617133509|
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Application Calls                                                                                 |
----------------------------------------------------------------------------------------------------
|Index|Calls                                                                                       |
----------------------------------------------------------------------------------------------------
|    1|R=179 P=/sap/bc/gui/sap/its/webgui/batch/json                                               |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Internal notes                                                                                    |
|    The termination was triggered in function "ab_GetPcbAddr" of the SAP kernel, in               |
|    line 2610 of module "//bas/GIT/krn/abap/runt/abpcb.c#0".                                      |
|    The internal operation just processed is "STRG".                                              |
|    Internal mode started at 20260617132332.                                                      |
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
|dw.sapS4Q_D00;0xf50506;[S](ab_GetPcbAddr.cold+0x22)[0x56225c550506]                               |
|dw.sapS4Q_D00;0x2d62abb;[S](ab_DataAddrRead+0x73b)[0x56225e362abb]                                |
|dw.sapS4Q_D00;0x2da8480;[S](ab_jstrg()+0x490)[0x56225e3a8480]                                     |
|dw.sapS4Q_D00;0x2d69113;[S](ab_extri()+0x1b3)[0x56225e369113]                                     |
|dw.sapS4Q_D00;0x2d99981;[S](ab_xevent(char16_t const*)+0x31)[0x56225e399981]                      |
|dw.sapS4Q_D00;0x2d67074;[S](ab_dstep+0x44)[0x56225e367074]                                        |
|dw.sapS4Q_D00;0x2d2e82d;[S](dynpmcal(DINFDUMY*, STPDUMMY*)+0x5dd)[0x56225e32e82d]                 |
|dw.sapS4Q_D00;0x2d2f2bc;[S](dynppai0(DINFDUMY*)+0x34c)[0x56225e32f2bc]                            |
|dw.sapS4Q_D00;0x2d2d669;[S](dynprctl(DINFDUMY*)+0x129)[0x56225e32d669]                            |
|dw.sapS4Q_D00;0x2d2c62e;[S](dynpen00(int)+0x3ce)[0x56225e32c62e]                                  |
|dw.sapS4Q_D00;0x95d50f;[S](GuiHandler::handleRequest(REQUEST_BUF*, bool)+0x8ed)[0x56225bf5d50f]   |
|dw.sapS4Q_D00;0x30a3a3a;[S](ThStart()+0xaca)[0x56225e6a3a3a]                                      |
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
|     0|Prg|SAPLEEWA_WDOCOCKPIT                     |     0|05.09.2025|15:09:32|   267264|K        |
|     1|Prg|SAPMSSYD                                |     0|03.05.2025|07:02:38|    34816|K        |
|     2|Prg|SAPFSYSCALLS                            |     0|01.06.2015|12:30:31|     7168|K        |
|     3|Prg|SAPFSYSCALLSFF                          |     0|18.02.2005|14:13:43|     9216|K        |
|     4|Prg|SAPLSETRAN                              |     4|03.11.2021|19:19:46|    44032|K        |
|     5|Prg|CL_DYNAMIC_GUI_EXTENSIONS=====CP        |     5|01.06.2015|12:30:30|    41984|K        |
|     6|Prg|SAPLSGUI                                |     6|31.03.2023|15:25:25|   103424|K        |
|     7|Prg|SAPLSTTM                                |     7|10.11.2022|19:49:55|    80896|K        |
|     8|Prg|SAPLSBDC                                |     8|18.03.2023|11:02:33|    71680|K        |
|     9|Prg|IF_SIMPLEPROPTREE=============IP        |     5|13.11.2000|16:49:03|     6144|K        |
|    10|Prg|CL_DYNP_CUA_DEFAULT===========CP        |    10|22.01.2021|09:24:38|    10240|K        |
|    11|Prg|CL_EEWA_FORM_DOCK=============CP        |    11|05.09.2025|15:09:31|   228352|K        |
|    12|Prg|CL_EEWA_FORM_BASE=============CP        |    12|07.11.2025|23:02:57|   804864|K        |
|    13|Prg|CL_EEWA_OBJECT================CP        |    13|05.09.2025|15:09:31|    11264|K        |
|    14|Prg|CL_ABAP_TYPEDESCR=============CP        |    14|16.05.2023|13:23:14|    29696|K        |
|    15|Prg|CL_ABAP_CLASSDESCR============CP        |    15|10.11.2022|19:49:27|    30720|K        |
|    16|Prg|CL_ABAP_OBJECTDESCR===========CP        |    16|31.03.2023|15:25:34|    32768|K        |
|    17|Prg|IF_EEWA_STRINGS_SUBSTITUTION==IP        |    15|26.02.2015|05:43:30|     5120|K        |
|    18|Prg|%_CABAP                                 |    15|10.11.2022|19:49:06|    36864|K        |
|    19|Prg|IF_EEWA_CONDITION=============IP        |    15|29.05.2015|12:35:10|     5120|K        |
|    20|Prg|IF_EEWA_WDOC_COCKPIT==========IP        |    15|26.02.2015|05:43:31|     5120|K        |
|    21|Prg|CL_ABAP_SOFT_REFERENCE========CP        |    21|26.11.2018|11:01:11|     9216|K        |
|    22|Prg|CL_ABAP_REFERENCE=============CP        |    22|01.06.2015|12:30:30|     7168|K        |
|    23|Prg|CL_EEWA_BO_RANGE==============CP        |    23|05.09.2025|15:09:31|   168960|K        |
|    24|Prg|CL_EEWA_BO_BASE===============CP        |    24|05.09.2025|15:09:31|   315392|K        |
|    25|Prg|CL_EEWA_PLUGINLIST_BO=========CP        |    25|05.09.2025|15:09:31|    40960|K        |
|    26|Prg|CL_EEWA_DDICINFO==============CP        |    26|06.03.2023|05:37:30|   277504|K        |
|    27|Prg|CL_ABAP_REFDESCR==============CP        |    27|16.05.2023|13:23:14|    30720|K        |
|    28|Prg|CL_ABAP_DATADESCR=============CP        |    28|16.05.2023|13:23:14|    26624|K        |
|    29|Typ|EEWATRANGE                              |     0|28.05.2018|19:39:30|     3072|         |
|    30|Prg|CL_ABAP_STRUCTDESCR===========CP        |    30|16.05.2023|13:24:02|    51200|K        |
|    31|Prg|CL_ABAP_COMPLEXDESCR==========CP        |    31|16.05.2023|13:23:14|    16384|K        |
|    32|Typ|X030L                                   |     0|02.05.2016|13:50:58|     7168|         |
|    33|Prg|CL_ABAP_RELEASE_BLACKLIST=====CP        |    33|10.11.2022|19:49:27|    10240|K        |
|    34|Prg|SAPLSDIFRUNTIME                         |    34|16.05.2023|13:23:52|    43008|K        |
|    35|Typ|DFIES                                   |     0|25.02.2015|23:57:18|    11264|         |
|    36|Typ|X031L                                   |     0|26.02.2015|00:15:45|     7168|         |
|    37|Prg|CL_DD_DFIES_PROVIDER==========CP        |    37|16.05.2023|13:23:52|    41984|K        |
|    38|Prg|SAPLSDNT                                |    38|18.04.2025|12:30:38|   373760|K        |
|    39|Prg|CL_DD_ENV_INFO_INTERNAL=======CP        |    39|03.05.2025|07:02:00|    14336|K        |
|    40|Prg|CL_DD_TESTPROPERTIES_UPG======CP        |    40|03.05.2025|07:02:00|    19456|K        |
|    41|Prg|CL_DD_TESTPROPERTIES_ABS======CP        |    41|03.03.2023|16:56:04|    32768|K        |
|    42|Prg|CL_DD_ENV_INFO================CP        |    42|03.05.2025|08:07:03|    24576|K        |
|    43|Prg|SAPLSUGS                                |    43|02.05.2025|11:07:09|   173056|K        |
|    44|Prg|CL_DD_NAMETAB_INTERNAL========CP        |    44|16.05.2023|13:23:52|   156672|K        |
|    45|Prg|IF_DD_CONSTANTS===============IP        |    44|16.05.2023|13:23:52|    10240|K        |
|    46|Prg|CL_DD_DFIES_PROVIDER_STRU=====CP        |    46|16.05.2023|13:23:52|    45056|K        |
|    47|Prg|CL_ABAP_CHAR_UTILITIES========CP        |    47|10.11.2022|19:49:27|    13312|K        |
|    48|Typ|EWASBO_RELATION                         |     0|16.05.2021|13:14:20|     4096|         |
|    49|Typ|EEWATRANGET                             |     0|14.07.2016|08:01:52|     4096|         |
|    50|Typ|EEWATRANGEV                             |     0|14.07.2016|08:01:52|     5120|         |
|    51|Typ|EWA_EL_BO_INITDATA                      |     0|16.05.2021|13:14:20|     5120|         |
|    52|Prg|EEWA_PLUGIN_BO================CP        |    52|06.03.2025|15:17:15|    15360|K        |
|    53|Prg|CL_BADI_BASE==================CP        |    53|16.12.2022|13:58:24|    32768|K        |
|    54|Prg|CL_BADI_CACHE_HANDLER=========CP        |    54|26.01.2023|17:38:04|    15360|K        |
|    55|Prg|CL_ABAP_BADI_TRACER===========CP        |    55|10.11.2022|19:49:27|    18432|K        |
|    56|Prg|CL_SXT_TRACE_OPTIONS==========CP        |    56|10.11.2022|19:49:45|    11264|K        |
|    57|Prg|IF_SXT_TYPES==================IP        |    56|03.11.2021|19:22:04|    10240|K        |
|    58|Prg|CL_SXT_TRACER_FACTORY=========CP        |    58|10.11.2022|19:49:45|    20480|K        |
|    59|Prg|CL_SXT_TRACER=================CP        |    59|16.05.2023|13:23:10|    68608|K        |
|    60|Prg|CL_SXT_RT_DB_TRACE_WRITER=====CP        |    60|26.01.2023|17:38:15|    56320|K        |
|    61|Prg|CL_SXT_ACTIVATION_HANDLER=====CP        |    61|26.01.2023|17:38:15|    27648|K        |
|    62|Prg|CL_SXT_MEMORY_LIMITS==========CP        |    62|26.01.2023|17:38:15|    29696|K        |
|    63|Prg|CL_REQUEST_ENTRY_POINT========CP        |    63|26.01.2023|17:38:13|    16384|K        |
|    64|Typ|SXT_ACT                                 |     0|19.11.2020|09:54:38|     5120|         |
|    65|Prg|CL_SXT_E2E_ACTIVATION=========CP        |    65|10.11.2022|19:50:11|    18432|K        |
|    66|Prg|CL_EPP_GLOBAL_FACTORY=========CP        |    66|16.05.2023|13:23:53|    12288|K        |
|    67|Prg|CL_EPP_GLOBAL=================CP        |    67|26.01.2023|17:37:53|    33792|K        |
|    68|Prg|CL_EPP_CORE===================CP        |    68|16.05.2023|13:23:16|   105472|K        |
|    69|Prg|IF_EPP_GLOBAL_SECTION=========IP        |    67|10.11.2022|19:50:11|     9216|K        |
|    70|Prg|/WATP/CL_IM_EEWA_PLUGIN_BO====CP        |    70|12.02.2025|20:57:09|    22528|K        |
|    71|Prg|IF_EX_EEWA_PLUGIN_BO==========IP        |    24|29.06.2015|14:08:25|     5120|K        |
|    72|Prg|/WATP/CL_BO_IMPLEMENTATION====CP        |    72|12.02.2025|20:57:09|   690176|K        |
|    73|Prg|/WATP/CL_BO_TRANSPORT_BASE====CP        |    73|12.02.2025|20:57:09|   157696|K        |
|    74|Prg|/WATP/CL_BO_BASE==============CP        |    74|12.02.2025|20:57:08|   462848|K        |
|    75|Prg|/WATP/CL_OBJECT===============CP        |    75|12.02.2025|18:37:16|    10240|K        |
|    76|Typ|/WATP/TTIMPL                            |     0|08.08.2024|08:30:14|     6144|         |
|    77|Typ|/WATP/TTIMPLMAN                         |     0|08.08.2024|08:30:14|     3072|         |
|    78|Typ|EWASRANGE                               |     0|25.02.2015|23:58:20|     3072|         |
|    79|Prg|CX_EEWA_BO_BASE===============CP        |    79|08.08.2024|10:25:27|    29696|K        |
|    80|Prg|CX_EEWA_BASE==================CP        |    80|14.11.2019|00:09:51|    35840|K        |
|    81|Prg|CX_DYNAMIC_CHECK==============CP        |    81|10.11.2022|19:49:48|    10240|K        |
|    82|Prg|CX_ROOT=======================CP        |    82|10.11.2022|19:49:48|    14336|K        |
|    83|Prg|CX_NO_CHECK===================CP        |    83|10.11.2022|19:49:48|    10240|K        |
|    84|Prg|CL_EEWA_BO_APPLICATION========CP        |    84|05.09.2025|15:09:31|    83968|K        |
|    85|Typ|EWASBO_APPL                             |     0|25.02.2015|23:58:19|     3072|         |
|    86|Typ|EWASAPPL_KEY                            |     0|25.02.2015|23:58:19|     2048|         |
|    87|Typ|EWASDDICINCLUDES                        |     0|25.02.2015|23:58:19|     2048|         |
|    88|Prg|CL_ABAP_ELEMDESCR=============CP        |    88|16.05.2023|13:24:01|    47104|K        |
|    89|Prg|CL_EEWA_MISC==================CP        |    89|10.11.2025|17:51:03|   197632|K        |
|    90|Prg|CL_ISU_EHP4_SFW_TOOLS=========CP        |    90|26.02.2015|05:42:51|    15360|K        |
|    91|Prg|SAPLSFW_COMMON                          |    91|15.06.2023|07:05:35|   239616|K        |
|    92|Prg|CL_ISU_EHP5_SFW_TOOLS=========CP        |    92|26.02.2015|05:42:51|    13312|K        |
|    93|Typ|SFW_STATELINE                           |     0|10.10.2014|18:27:23|     3072|         |
|    94|Prg|CL_ABAP_SWITCH================CP        |    94|28.04.2023|07:29:35|    62464|K        |
|    95|Prg|CL_ISU_EHP6_SFW_TOOLS=========CP        |    95|26.02.2015|07:25:56|    12288|K        |
|    96|Prg|CL_EEWA_IMPL_BO_CALLBACK_FORM=CP        |    96|05.09.2025|15:09:31|    18432|K        |
|    97|Prg|CL_EEWA_PROGRESS==============CP        |    97|30.04.2022|12:14:16|    12288|K        |
|    98|Prg|CL_EEWA_SYNCHRONIZE===========CP        |    98|05.09.2025|15:09:31|    20480|K        |
|    99|Prg|CL_EEWA_BEHAVIOR_UI_LIST======CP        |    99|05.09.2025|15:09:31|    28672|K        |
|   100|Prg|CL_EEWA_BO_OBJTYPE============CP        |   100|05.09.2025|15:09:31|   110592|K        |
|   101|Prg|CL_EEWA_CACHE_BO==============CP        |   101|05.09.2025|15:09:31|   124928|K        |
|   102|Prg|CL_EEWA_IMPL_BO_CALLBACK_CACH=CP        |   102|05.09.2025|15:09:31|    21504|K        |
|   103|Prg|CL_EEWA_BULKDATAACCESSLIST====CP        |   103|26.02.2015|07:25:25|    13312|K        |
|   104|Prg|CL_EEWA_OBJECTREADER==========CP        |   104|26.02.2015|07:25:07|    87040|K        |
|   105|Typ|EWA_EL_CLASSNAME                        |     0|25.02.2015|23:58:23|     3072|         |
|   106|Prg|IF_EEWA_BO_CALLBACK_DETAIL====IP        |    24|31.01.2018|17:57:10|     5120|K        |
|   107|Prg|CX_SY_MOVE_CAST_ERROR=========CP        |   107|10.11.2022|19:49:48|    11264|K        |
|   108|Prg|CL_EEWA_BULKDATAACCESS_GEN====CP        |   108|26.02.2015|07:25:25|    31744|K        |
|   109|Prg|CL_EEWA_BULKDATAACCESS_BASE===CP        |   109|26.02.2015|07:45:05|    27648|K        |
|   110|Prg|IF_EEWA_BULKDATAACCESS========IP        |   103|29.05.2015|12:35:10|     7168|K        |
|   111|Typ|EWASOBJTYPE_KEY                         |     0|25.02.2015|23:58:20|     2048|         |
|   112|Typ|EWA_EL_OBJECTTYPE_BO                    |     0|25.02.2015|23:58:23|     9216|         |
|   113|Typ|TEWA_EL_OBJTYPE                         |     0|26.02.2015|01:02:09|     5120|         |
|   114|Typ|TEWA_EL_OBJTYPEM                        |     0|26.02.2015|01:02:10|     4096|         |
|   115|Typ|EWA_EL_OBJECTTYPES_BO                   |     0|25.02.2015|23:58:23|     4096|         |
|   116|Typ|EWASBO_LANGFIELD                        |     0|25.02.2015|23:58:19|     4096|         |
|   117|Typ|TEWA_EL_OBJTYPET                        |     0|26.02.2015|01:02:10|     3072|         |
|   118|Prg|CL_ABAP_DYN_PRG===============CP        |   118|18.03.2023|11:02:27|    71680|K        |
|   119|Prg|SABP_DD_CATALOG_ACCESS========CP        |   119|16.05.2023|13:23:55|    31744|K        |
|   120|Prg|SAPLSABP_DD_CATALOG                     |   120|16.05.2023|13:23:55|   162816|K        |
|   121|Prg|CL_EEWA_OR_OBJBEHAVIOR========CP        |   121|26.02.2015|07:25:09|    63488|K        |
|   122|Prg|CL_EEWA_OR_OBJBEHAVIOR_UIF====CP        |   122|26.02.2015|07:25:09|    36864|K        |
|   123|Prg|IF_EEWA_OBJECTREADER==========IP        |   104|14.11.2019|00:11:53|     9216|K        |
|   124|Prg|CL_EEWA_DYNAMIC_STRUCTURE=====CP        |   124|05.09.2025|15:09:31|    19456|K        |
|   125|Typ|EWASOBJBH                               |     0|25.02.2015|23:58:20|     5120|         |
|   126|Typ|TEWA_EL_OBJBH                           |     0|26.02.2015|01:02:07|     5120|         |
|   127|Rty|type000000000000                        |     0|17.06.2026|13:23:32|        0|         |
|   128|Prg|CL_EEWA_ITERATOR_BOLIST=======CP        |   128|05.09.2025|15:09:31|    17408|K        |
|   129|Rty|type000000000001                        |     0|17.06.2026|13:23:32|        0|         |
|   130|Rty|type000000000002                        |     0|17.06.2026|13:23:32|        0|         |
|   131|Prg|CL_ABAP_TABLEDESCR============CP        |   131|16.05.2023|13:23:14|    36864|K        |
|   132|Prg|CL_EEWA_PLUGINLIST_FORM=======CP        |   132|05.09.2025|15:09:31|    50176|K        |
|   133|Prg|CL_EEWA_MSGLOG================CP        |   133|28.04.2023|12:46:46|   116736|K        |
|   134|Prg|CL_EEWA_MSGLOG_REDIRECTLIST===CP        |   134|26.02.2015|05:40:46|    14336|K        |
|   135|Typ|BAL_S_PROF                              |     0|03.03.2023|14:53:21|    49152|         |
|   136|Prg|SAPLSBAL_PROFILE                        |   136|03.03.2023|14:53:21|    21504|K        |
|   137|Typ|BAL_S_FCAT                              |     0|31.05.2013|07:57:43|     5120|         |
|   138|Prg|SAPLSBAL                                |   138|26.01.2023|17:38:07|   387072|K        |
|   139|Typ|BAL_S_LOG                               |     0|21.01.2014|10:04:42|     8192|         |
|   140|Prg|SAPLSBAL_SERVICE                        |   140|26.01.2023|17:37:48|   218112|K        |
|   141|Typ|BAL_S_PARM                              |     0|08.09.2011|14:18:31|     4096|         |
|   142|Typ|BAL_S_SCNT                              |     0|08.09.2011|14:25:35|     4096|         |
|   143|Typ|BAL_S_SDEF                              |     0|04.12.2000|12:45:15|     3072|         |
|   144|Prg|SAPLSYGU                                |   144|10.11.2022|19:49:55|    24576|K        |
|   145|Prg|CL_SYSTEM_UUID================CP        |   145|26.01.2023|17:38:07|    52224|K        |
|   146|Typ|BAL_S_CONT                              |     0|02.11.1998|09:43:07|     3072|         |
|   147|Typ|BAL_S_CLBK                              |     0|08.09.2011|14:18:31|     3072|         |
|   148|Prg|CX_SY_DYN_CALL_ILLEGAL_CLASS==CP        |   148|10.11.2022|19:49:48|    11264|K        |
|   149|Prg|CX_SY_DYN_CALL_ERROR==========CP        |   149|10.11.2022|19:49:48|    11264|K        |
|   150|Prg|CL_EXITHANDLER================CP        |   150|31.03.2023|15:25:35|    51200|K        |
|   151|Prg|SAPLSEXV                                |   151|16.05.2023|13:23:36|   163840|K        |
|   152|Prg|CL_EX_EEWA_BD_WDOCOCKPIT======CP        |   152|10.03.2025|17:31:15|    14336|K        |
|   153|Prg|CL_BADI_FLT_DATA_TRANS_AND_DB=CP        |   153|15.06.2023|07:05:34|    47104|K        |
|   154|Typ|SXC_EXIT                                |     0|09.11.2000|14:23:43|     3072|         |
|   155|Prg|CL_EEWA_STRINGS===============CP        |   155|08.08.2024|10:25:44|    78848|K        |
|   156|Prg|CL_EEWA_XML_TRANSFORMER=======CP        |   156|26.02.2015|05:41:03|    17408|K        |
|   157|Prg|CL_IXML=======================CP        |   157|10.11.2022|19:49:37|   257024|K        |
|   158|Prg|CL_IXML_UNKNOWN===============CP        |   158|01.06.2015|12:30:30|     8192|K        |
|   159|Prg|IF_EX_EEWA_BD_WDOCOCKPIT======IP        |    12|10.03.2025|17:31:16|     5120|K        |
|   160|Prg|%_CSXRT                                 |   152|01.06.2015|12:30:30|    16384|K        |
|   161|Prg|CL_EXIT_MASTER================CP        |   161|16.12.2022|11:36:12|    39936|K        |
|   162|Prg|CL_EX_BADI_LAYER==============CP        |   162|16.12.2022|11:36:12|    30720|K        |
|   163|Prg|SAPLSEXE                                |   163|15.06.2023|07:05:35|   137216|K        |
|   164|Prg|IF_EX_BADI_LAYER==============IP        |   150|16.12.2022|11:36:13|     7168|K        |
|   165|Typ|EWASFUNCPARAMSIMPLE                     |     0|25.02.2015|23:58:20|     4096|         |
|   166|Prg|CL_EEWA_DRAGOBJECT============CP        |   166|26.02.2015|05:40:38|    10240|K        |
|   167|Prg|EEWA_PLUGIN_FORM==============CP        |   167|07.11.2025|23:02:57|    16384|K        |
|   168|Prg|CL_ABAP_INTFDESCR=============CP        |   168|10.11.2022|19:49:27|    24576|K        |
|   169|Prg|CX_SY_DYN_CALL_ILLEGAL_METHOD=CP        |   169|10.11.2022|19:49:48|    12288|K        |
|   170|Prg|CX_SY_NO_HANDLER==============CP        |   170|10.11.2022|19:49:48|    10240|K        |
|   171|Prg|CL_EEWA_BO_APPLICATION_CSTM===CP        |   171|05.09.2025|15:09:31|    80896|K        |
|   172|Typ|EWASBO_APPLCSTM                         |     0|25.02.2015|23:58:19|     3072|         |
|   173|Typ|EWASAPPLCSTMBOK                         |     0|25.02.2015|23:58:19|    20480|         |
|   174|Typ|SYST                                    |     0|10.10.2014|18:27:32|    32768|         |
|   175|Prg|CL_GUI_CONTAINER==============CP        |   175|28.05.2018|11:34:26|    60416|K        |
|   176|Prg|CL_GUI_CONTROL================CP        |   176|10.11.2022|19:49:36|   121856|K        |
|   177|Prg|CL_GUI_OBJECT=================CP        |   177|10.11.2022|19:49:36|   446464|K        |
|   178|Prg|CL_GUI_CFW====================CP        |   178|17.12.2018|10:03:42|   185344|K        |
|   179|Prg|CL_GUI_PROPS_CONSUMER=========CP        |   179|28.05.2018|11:34:26|    33792|K        |
|   180|Prg|SAPLTHFB                                |   180|15.06.2023|07:05:24|   518144|K        |
|   181|Prg|SAPLSFES                                |   181|16.05.2023|13:23:16|   323584|K        |
|   182|Typ|EUDB                                    |     0|31.05.2013|08:02:19|     9216|         |
|   183|Prg|%_CCNTL                                 |   179|28.05.2018|11:34:26|    16384|K        |
|   184|Prg|CL_SYSTEM_TRANSACTION_STATE===CP        |   184|31.03.2023|15:25:24|   107520|K        |
|   185|Prg|SAPLSPLUGIN                             |   185|03.11.2021|19:19:46|     8192|K        |
|   186|Prg|SAPLOLEA                                |   186|10.11.2022|19:49:53|   120832|K        |
|   187|Typ|OBJ_RECORD                              |     0|28.05.2018|11:34:27|     3072|         |
|   188|Typ|TOLE                                    |     0|29.07.1998|19:56:01|     4096|         |
|   189|Prg|%_COLE2                                 |   186|28.05.2018|11:34:26|    12288|K        |
|   190|Typ|OLE_PA                                  |     0|04.04.1995|16:02:19|     3072|         |
|   191|Prg|SAPLSCP2                                |   191|15.06.2023|07:05:35|   197632|K        |
|   192|Typ|SWCONT                                  |     0|08.09.2011|14:24:14|     3072|         |
|   193|Typ|SWCBCONT                                |     0|08.09.2011|14:24:16|     4096|         |
|   194|Prg|CL_EEWA_GUI_INFO==============CP        |   194|08.08.2024|10:25:43|    43008|K        |
|   195|Prg|CL_GUI_RESOURCES==============CP        |   195|10.11.2022|19:49:36|    64512|K        |
|   196|Prg|SAPLCNTL                                |   196|25.03.2019|14:14:58|   356352|K        |
|   197|Prg|CL_WF_GUI_RESOURCES_4_HTML====CP        |   197|21.01.2014|10:06:18|    36864|K        |
|   198|Typ|SWL_STYLES                              |     0|03.05.1999|14:16:05|     4096|         |
|   199|Prg|CL_GUI_FRONTEND_SERVICES======CP        |   199|28.04.2023|07:29:38|   165888|K        |
|   200|Prg|CL_GUI_MEMORY=================CP        |   200|10.11.2022|19:49:36|    47104|K        |
|   201|Prg|CL_SHM_AREA===================CP        |   201|10.11.2022|19:49:42|    63488|K        |
|   202|Prg|CL_ABAP_MEMORY_AREA===========CP        |   202|10.11.2022|19:49:27|    12288|K        |
|   203|Prg|CL_SHM_SERVICE================CP        |   203|10.11.2022|19:49:42|    55296|K        |
|   204|Prg|CX_SHM_NO_ACTIVE_VERSION======CP        |   204|10.11.2022|19:49:48|    12288|K        |
|   205|Prg|CX_SHM_ATTACH_ERROR===========CP        |   205|10.11.2022|19:49:48|    11264|K        |
|   206|Prg|CX_SHM_ERROR==================CP        |   206|10.11.2022|19:49:48|    11264|K        |
|   207|Prg|CX_STATIC_CHECK===============CP        |   207|10.11.2022|19:49:48|    10240|K        |
|   208|Prg|CL_GUI_MEMORY_ROOT============CP        |   208|10.11.2022|19:49:36|    15360|K        |
|   209|Prg|SAPLSIT1                                |   209|21.04.2017|12:40:00|   428032|K        |
|   210|Prg|SAPLAWRT                                |   210|10.11.2022|19:49:53|   202752|K        |
|   211|Prg|SAPLSITS_KERNEL_INTERFACE               |   211|10.11.2022|19:49:54|    65536|K        |
|   212|Prg|SAPFGUICNTL                             |   186|17.12.2018|10:04:08|    28672|K        |
|   213|Typ|ARFCRDATA                               |     0|08.09.2011|14:25:38|     7168|         |
|   214|Prg|SAPLGRFC                                |   214|01.06.2015|12:30:31|    15360|K        |
|   215|Typ|OLE_VERBS                               |     0|04.04.1995|16:02:20|     3072|         |
|   216|Prg|CL_GUI_DATAMANAGER============CP        |   216|10.11.2022|19:49:36|    87040|K        |
|   217|Typ|EWAEL_DOCK_OPTIONS                      |     0|28.05.2018|11:32:14|     5120|         |
|   218|Prg|CL_HTTP_UTILITY===============CP        |   218|16.05.2023|13:23:53|    78848|K        |
|   219|Typ|CODEPAGES                               |     0|09.07.1997|23:51:03|     2048|         |
|   220|Prg|SAPLSCPA                                |   220|26.01.2023|17:38:15|    65536|K        |
|   221|Prg|CL_TPDA_SERVICES==============CP        |   221|10.11.2022|19:49:46|    18432|K        |
|   222|Typ|FILE_TABLE                              |     0|05.01.1999|10:23:11|     4096|         |
|   223|Prg|CL_GUI_DOCKING_CONTAINER======CP        |   223|28.05.2018|11:34:26|    51200|K        |
|   224|Prg|SAPLCNTH                                |   224|28.05.2018|11:34:27|    30720|K        |
|   225|Prg|CL_GUI_DYNPRO_COMPANION=======CP        |   225|28.05.2018|11:34:26|    18432|K        |
|   226|Prg|CL_GUI_SPLITTER_CONTAINER=====CP        |   226|28.05.2018|11:34:26|    69632|K        |
|   227|Prg|SAPLSBAL_DB_INTERNAL                    |   227|10.11.2022|19:49:54|   343040|K        |
|   228|Prg|SAPLEEWA_WDOCWORKAREA                   |   228|17.06.2026|07:12:08|  1202176|K        |
|   229|Typ|EWASWDOC_WORKAREA_DLG                   |     0|25.02.2015|23:58:21|     6144|         |
|   230|Prg|SAPFGUICNTL_CFW                         |   186|28.05.2018|11:34:27|    15360|K        |
|   231|Prg|CL_EEWA_FORM_MASK_DIALOG======CP        |   231|05.09.2025|15:09:31|   198656|K        |
|   232|Prg|CL_EEWA_FORM_MASK=============CP        |   232|05.09.2025|15:09:31|   197632|K        |
|   233|Prg|CL_EEWA_BO_WORKAREA===========CP        |   233|17.06.2026|07:11:42|   289792|K        |
|   234|Typ|EWASWDOC_WORKAREA                       |     0|25.02.2015|23:58:21|     4096|         |
|   235|Typ|EWA_WDOC_WAREA                          |     0|26.02.2015|00:21:30|     3072|         |
|   236|Typ|EWA_WDOC_WA_BP                          |     0|26.02.2015|00:21:31|     3072|         |
|   237|Typ|EWA_WDOC_WA_PA                          |     0|26.02.2015|00:21:32|     3072|         |
|   238|Typ|EWA_WDOC_WA_PC                          |     0|26.02.2015|00:21:32|     3072|         |
|   239|Typ|EWA_WDOC_WA_RT                          |     0|26.02.2015|00:21:32|     3072|         |
|   240|Typ|EWA_WDOC_WA_SL                          |     0|26.02.2015|00:21:33|     3072|         |
|   241|Typ|EWA_WDOC_WA_ST                          |     0|26.02.2015|00:21:33|     3072|         |
|   242|Typ|EWA_WDOC_WA_SV                          |     0|26.02.2015|00:21:33|     3072|         |
|   243|Typ|EWA_WDOC_WA_WS                          |     0|26.02.2015|00:21:34|     3072|         |
|   244|Typ|EWA_WDOC_WA_JS                          |     0|26.02.2015|00:21:31|     3072|         |
|   245|Typ|EWASAUTHFIELDMAPPING                    |     0|25.02.2015|23:58:19|     3072|         |
|   246|Prg|CL_EX_EEWA_BA_WORKAREA========CP        |   246|26.02.2015|05:41:07|    14336|K        |
|   247|Prg|IF_EX_EEWA_BA_WORKAREA========IP        |    24|26.02.2015|05:43:33|     5120|K        |
|   248|Typ|DATATYPE                                |     0|28.05.2018|11:31:27|     5120|         |
|   249|Typ|DD04L                                   |     0|26.01.2018|14:09:28|     9216|         |
|   250|Prg|SAPLSDDO                                |   250|18.04.2025|12:30:37|   353280|K        |
|   251|Typ|DCDTELGET                               |     0|30.03.1995|13:13:18|     3072|         |
|   252|Typ|DD01L                                   |     0|26.01.2018|14:09:26|     7168|         |
|   253|Typ|DD04T                                   |     0|12.05.1997|16:45:50|     4096|         |
|   254|Typ|DDSETSTATE                              |     0|31.05.2013|08:00:33|     3072|         |
|   255|Typ|TPARA                                   |     0|10.07.1997|00:45:24|     3072|         |
|   256|Prg|RADMASBF                                |   250|31.03.2023|13:47:28|    74752|K        |
|   257|Prg|RADBTDDO                                |   250|19.11.2020|09:53:20|    53248|K        |
|   258|Prg|RADBTDDF                                |   250|16.05.2023|13:06:47|   794624|K        |
|   259|Prg|CL_EEWA_GRID==================CP        |   259|10.03.2025|17:31:15|   301056|K        |
|   260|Prg|CL_GUI_ALV_GRID===============CP        |   260|03.05.2025|08:07:04|   844800|K        |
|   261|Prg|CL_GUI_ALV_GRID_BASE==========CP        |   261|15.06.2023|07:05:34|   258048|K        |
|   262|Typ|EWAEL_GRID_OPTIONS                      |     0|06.03.2023|02:11:21|    28672|         |
|   263|Typ|LVC_S_FCAT                              |     0|28.05.2018|11:34:00|    21504|         |
|   264|Typ|STB_BUTTON                              |     0|25.01.1999|13:03:41|     3072|         |
|   265|Prg|CL_EEWA_GUI_CUSTOM_CONTAINER==CP        |   265|04.05.2016|12:54:01|    45056|K        |
|   266|Prg|CL_GUI_CUSTOM_CONTAINER=======CP        |   266|28.05.2018|11:34:26|    45056|K        |
|   267|Typ|CFW_LINK                                |     0|28.05.2018|11:30:15|     3072|         |
|   268|Prg|CL_SALV_GUI_ENVIRONMENT_INFO==CP        |   268|15.06.2023|07:05:34|    56320|K        |
|   269|Prg|CL_ALV_Z_PARAMS===============CP        |   269|03.05.2025|07:01:56|    83968|K        |
|   270|Prg|CL_SALV_CSQ_PARAMS_DB=========CP        |   270|10.11.2022|19:49:41|    89088|K        |
|   271|Prg|CL_COS_UTILITIES==============CP        |   271|31.03.2023|13:50:55|    32768|K        |
|   272|Prg|CL_GUI_DATAPONDEMAND==========CP        |   272|10.11.2022|19:49:36|    68608|K        |
|   273|Prg|CL_DATAPTABLECACHE============CP        |   273|10.11.2022|19:49:31|    60416|K        |
|   274|Typ|LVC_S_TXTP                              |     0|01.02.2000|09:28:09|     3072|         |
|   275|Prg|SAPLCNDP                                |   275|16.05.2023|13:23:14|   231424|K        |
|   276|Typ|RFC_FIELDS                              |     0|20.08.1998|10:56:34|     3072|         |
|   277|Typ|DPPROPS                                 |     0|09.07.1997|23:53:23|     3072|         |
|   278|Prg|SAPLURFC                                |   278|15.06.2023|07:05:35|    54272|K        |
|   279|Prg|SAPLSRFC                                |   279|18.03.2023|11:02:29|    52224|K        |
|   280|Prg|SAPLSYST                                |   280|10.11.2022|19:49:55|    52224|K        |
|   281|Prg|SAPMSSY1                                |   275|26.11.2018|11:04:02|    34816|K        |
|   282|Prg|SAPLCNT0                                |   282|28.05.2018|11:34:27|    43008|K        |
|   283|Prg|CL_ALV_CUSTOMIZING============CP        |   283|15.06.2023|07:05:19|    12288|K        |
|   284|Prg|CX_SY_CREATE_DATA_ERROR=======CP        |   284|10.11.2022|19:49:48|    12288|K        |
|   285|Prg|CX_SY_CREATE_ERROR============CP        |   285|10.11.2022|19:49:48|    10240|K        |
|   286|Prg|CL_SALV_LOGGER================CP        |   286|10.11.2022|19:49:41|    16384|K        |
|   287|Prg|CL_SALV_CSQ_PARAMS============CP        |   287|10.11.2022|19:49:41|    24576|K        |
|   288|Prg|CL_CTMENU=====================CP        |   288|10.11.2022|19:49:31|    44032|K        |
|   289|Prg|CL_ALV_EVENT_TOOLBAR_SET======CP        |   289|01.06.2015|12:30:30|     9216|K        |
|   290|Prg|CL_AMC_CHANNEL_MANAGER========CP        |   290|31.03.2023|15:25:23|    41984|K        |
|   291|Prg|CX_AMC_ERROR==================CP        |   291|18.03.2023|11:02:38|    17408|K        |
|   292|Prg|CL_MESSAGE_HELPER=============CP        |   292|10.11.2022|19:49:38|    47104|K        |
|   293|Prg|IF_MESSAGE====================IP        |    82|10.11.2022|19:49:50|     5120|K        |
|   294|Typ|SCX_T100KEY                             |     0|31.05.2013|08:12:18|     3072|         |
|   295|Prg|IF_T100_MESSAGE===============IP        |   292|10.11.2022|19:49:51|     7168|K        |
|   296|Prg|SAPLSOTR_DB_READ                        |   296|03.11.2021|19:19:46|    98304|K        |
|   297|Prg|SAPLSOTR_SYSTEM_SETTINGS                |   297|25.02.2022|14:55:09|    28672|K        |
|   298|Typ|ABAPTEXT                                |     0|16.03.1993|18:19:31|     2048|         |
|   299|Prg|IF_SERIALIZABLE_OBJECT========IP        |    15|01.06.2015|12:30:31|     5120|K        |
|   300|Typ|SBTFR_PARAM                             |     0|06.05.2008|14:19:04|     2048|         |
|   301|Prg|CX_SY_DYN_CALL_ILLEGAL_TYPE===CP        |   301|10.11.2022|19:49:48|    13312|K        |
|   302|Prg|CX_SY_DYN_CALL_PARAMETER_ERRORCP        |   302|10.11.2022|19:49:48|    12288|K        |
|   303|Prg|SAPLSOTR_SERVICES                       |   303|23.04.2019|09:17:19|    22528|K        |
|   304|Prg|CL_ABAP_TEXTPOOL==============CP        |   304|26.01.2023|17:38:04|    10240|K        |
|   305|Prg|CL_SALV_GUI_COMMON_HUB========CP        |   305|15.06.2023|07:05:34|    53248|K        |
|   306|Prg|IF_SALV_GUI_COMMON_HUB========IP        |   305|30.05.2023|13:35:51|    12288|K        |
|   307|Prg|IF_SALV_GUI_BROKER============IP        |   305|08.08.2024|10:14:16|     9216|K        |
|   308|Prg|CL_SALV_GUI_BROKER============CP        |   308|16.05.2023|13:26:35|    26624|K        |
|   309|Prg|CL_SALV_GUI_UI_FUNCTION_TOOLS=CP        |   309|03.05.2025|08:07:07|   209920|K        |
|   310|Prg|CL_SALV_GUI_SLIS_FACTORY======CP        |   310|03.05.2025|07:02:07|    69632|K        |
|   311|Prg|CL_SALV_GUI_PROPERTY_BAG======CP        |   311|03.05.2025|07:02:07|    41984|K        |
|   312|Prg|CX_SALV_NOT_FOUND=============CP        |   312|10.11.2022|19:49:48|    15360|K        |
|   313|Prg|CX_SALV_ACCESS_ERROR==========CP        |   313|10.11.2022|19:49:48|    14336|K        |
|   314|Prg|CX_SALV_STATIC_CHECK==========CP        |   314|10.11.2022|19:49:48|    15360|K        |
|   315|Prg|CX_SALV_ERROR=================CP        |   315|10.11.2022|19:49:48|    13312|K        |
|   316|Prg|CL_SALV_GUI_COMMON_FUNDAMENT==CP        |   316|03.05.2025|07:03:12|    37888|K        |
|   317|Prg|CL_SALV_GUI_GRID_TEXTS_IDA====CP        |   317|15.06.2023|07:05:34|    24576|K        |
|   318|Prg|%_CSLIS                                 |   309|16.12.2022|13:58:46|   102400|K        |
|   319|Prg|CL_SALV_GUI_GRID_MODEL_IDA====CP        |   319|15.06.2023|07:05:35|   269312|K        |
|   320|Prg|CL_SALV_GUI_GRID_TABLE_MODEL==CP        |   320|15.06.2023|07:05:34|   200704|K        |
|   321|Prg|IF_SALV_C_FUNCTION============IP        |   309|15.06.2023|07:05:35|    17408|K        |
|   322|Prg|IF_SALV_C_SLIS_FCODE==========IP        |   309|15.06.2023|07:05:35|    17408|K        |
|   323|Prg|CL_SALV_GUI_UI_FUNCTION_OBJ===CP        |   323|03.05.2025|08:07:07|   301056|K        |
|   324|Prg|IF_SALV_C_TABLE_OBJECTS=======IP        |   309|10.11.2022|19:50:31|     5120|K        |
|   325|Prg|CL_SALV_GUI_SUPPLIED_CLNT_FUNCCP        |   325|15.06.2023|07:05:35|    27648|K        |
|   326|Prg|CL_SALV_ATS_PARAMS============CP        |   326|10.11.2022|19:57:43|    19456|K        |
|   327|Prg|CL_SALV_TABLE=================CP        |   327|15.06.2023|07:05:35|    45056|K        |
|   328|Prg|CL_SALV_MODEL=================CP        |   328|15.06.2023|07:05:35|    14336|K        |
|   329|Prg|IF_SALV_C_BOOL_SAP============IP        |   328|10.11.2022|19:49:50|     5120|K        |
|   330|Prg|CL_CLS_UTIL===================CP        |   330|10.11.2022|19:49:30|    18432|K        |
|   331|Prg|CL_SALV_FUNCTIONS=============CP        |   331|03.05.2025|08:07:07|   184320|K        |
|   332|Prg|CL_SALV_METADATA_BASE=========CP        |   332|10.11.2022|19:49:41|    11264|K        |
|   333|Prg|CL_SALV_METADATA==============CP        |   333|03.05.2025|07:02:08|    41984|K        |
|   334|Prg|CL_SALV_GUI_FEATURES==========CP        |   334|03.05.2025|08:07:07|    36864|K        |
|   335|Prg|CL_SALV_GUI_GRID_FACADE=======CP        |   335|03.05.2025|07:02:06|   202752|K        |
|   336|Prg|CL_SALV_GUI_GRID_CONTROLER_IDACP        |   336|15.06.2023|07:06:08|   253952|K        |
|   337|Prg|CL_SALV_MODEL_BASE============CP        |   337|03.05.2025|08:07:07|    69632|K        |
|   338|Prg|CL_SALV_GUI_GRID_FWK_CHANGELOGCP        |   338|03.05.2025|05:55:29|    45056|K        |
|   339|Prg|IF_SALV_GUI_GRID_FWK_CHANGELOGIP        |   316|03.05.2025|05:55:29|     7168|K        |
|   340|Prg|CL_SALV_GUI_ASSIST_GRID=======CP        |   340|03.05.2025|08:07:07|   308224|K        |
|   341|Prg|CL_SALV_GUI_GRID_USER_ACTION==CP        |   341|03.05.2025|07:02:06|    21504|K        |
|   342|Prg|CL_SALV_GUI_DATA_SOURCE=======CP        |   342|15.06.2023|07:05:26|    73728|K        |
|   343|Prg|CL_SALV_GUI_CORE_STRUCTDESCR==CP        |   343|08.08.2024|10:13:21|   155648|K        |
|   344|Prg|IF_SALV_GRM_DATA_SOURCE=======IP        |   335|08.08.2024|10:14:16|    13312|K        |
|   345|Prg|CL_ALV_VARIANT================CP        |   345|08.08.2024|10:13:13|   306176|K        |
|   346|Typ|DISVARIANT                              |     0|29.07.1998|18:25:39|     3072|         |
|   347|Typ|LVC_S_TOTO                              |     0|28.05.2018|11:34:07|     5120|         |
|   348|Typ|LVC_S_LAYO                              |     0|28.05.2018|11:34:03|    16384|         |
|   349|Typ|LVC_S_PRNT                              |     0|10.10.2014|18:26:55|    11264|         |
|   350|Typ|LVC_S_BDS                               |     0|28.05.2018|11:33:57|    27648|         |
|   351|Prg|CL_ALV_BDS====================CP        |   351|15.06.2023|07:05:34|   192512|K        |
|   352|Prg|CL_BDS_DOCUMENT_SET===========CP        |   352|17.04.2020|19:51:54|   125952|K        |
|   353|Prg|CL_ALV_CML_LAYOUT_HANDLER=====CP        |   353|15.06.2023|07:05:34|   113664|K        |
|   354|Prg|CL_SALV_GUI_VARIANT_ATTRIBUTE=CP        |   354|16.05.2023|13:26:35|    38912|K        |
|   355|Prg|IF_SALV_GUI_STRUCT_DESCRIPTOR=IP        |   260|03.05.2025|07:03:05|    48128|K        |
|   356|Prg|CL_SALV_GRZ_BRIDGE============CP        |   356|16.05.2023|13:23:45|    12288|K        |
|   357|Prg|IF_ALV_RM_GRID_BRIDGE=========IP        |   260|10.11.2022|19:49:49|    12288|K        |
|   358|Prg|CL_SALV_GZ_FACTORY============CP        |   358|03.05.2025|07:02:08|    79872|K        |
|   359|Prg|CL_SALV_GRZ_CONTROLLER========CP        |   359|15.06.2023|07:05:34|   186368|K        |
|   360|Prg|IF_SALV_GRZ_CONTROLLER========IP        |   358|10.11.2022|19:49:50|     7168|K        |
|   361|Prg|CL_SALV_CSQ_LOG===============CP        |   361|26.01.2023|17:38:14|    80896|K        |
|   362|Prg|IF_SALV_CSQ===================IP        |   361|25.03.2019|14:10:30|    18432|K        |
|   363|Prg|CL_SALV_CSQ_TRACE_DB==========CP        |   363|26.01.2023|17:37:55|   101376|K        |
|   364|Prg|CL_SALV_GRM_TABLE_MODEL=======CP        |   364|16.05.2023|13:23:47|    44032|K        |
|   365|Prg|CL_SALV_GZ_CHANGE_TICKER======CP        |   365|10.11.2022|19:49:41|    16384|K        |
|   366|Prg|IF_SALV_GZ_CHANGE_TICKER======IP        |   358|18.06.2009|09:24:43|     6144|K        |
|   367|Prg|CL_ALV_CML_CONFIG_CONTROLLER==CP        |   367|15.06.2023|07:05:34|    61440|K        |
|   368|Prg|IF_ALV_CML_CONFIG_CONTROLLER==IP        |   358|16.05.2023|13:23:36|     8192|K        |
|   369|Prg|CL_ALV_CML_CONFIGURATION======CP        |   369|03.05.2025|07:01:56|   171008|K        |
|   370|Prg|SAPLSLVC                                |   370|15.06.2023|07:05:35|   544768|K        |
|   371|Prg|CL_ALV_CML_MODEL_ITEM_FACTORY=CP        |   371|26.01.2023|17:38:18|    20480|K        |
|   372|Prg|CL_ALV_CML_ACCESS_FACTORY=====CP        |   372|26.01.2023|17:38:18|    14336|K        |
|   373|Prg|IF_ALV_CML_MODEL_ITEM_FACTORY=IP        |   372|03.05.2025|07:11:14|    11264|K        |
|   374|Prg|CL_ALV_CML_ACCESS_LOAD========CP        |   374|15.06.2023|07:05:34|    61440|K        |
|   375|Prg|CL_ALV_CML_ACCESS_SAVE========CP        |   375|15.06.2023|07:05:34|    68608|K        |
|   376|Prg|CL_ALV_CML_COLUMN_SETTINGS====CP        |   376|15.06.2023|07:05:34|    59392|K        |
|   377|Prg|CL_ALV_CML_FILTER_CRITERIA====CP        |   377|15.06.2023|07:05:34|    51200|K        |
|   378|Prg|IF_ALV_CML_MODEL_ITEM_EDITOR==IP        |   371|26.01.2023|17:38:19|     9216|K        |
|   379|Prg|CL_ALV_CML_GROUPING_CRITERIA==CP        |   379|15.06.2023|07:05:34|    52224|K        |
|   380|Prg|CL_ALV_CML_VIEWS==============CP        |   380|15.06.2023|07:05:34|    47104|K        |
|   381|Prg|CL_ALV_CML_VIEW_GRID==========CP        |   381|15.06.2023|07:05:34|    30720|K        |
|   382|Prg|CL_ALV_CML_VIEW_EXCEL=========CP        |   382|15.06.2023|07:05:34|    87040|K        |
|   383|Prg|CL_ALV_CML_ACCESS_BDS_SAVE====CP        |   383|16.05.2023|13:23:36|    15360|K        |
|   384|Prg|CL_ALV_CML_VIEW_CRYSTAL=======CP        |   384|15.06.2023|07:05:34|    71680|K        |
|   385|Prg|CL_ALV_CML_ACCESS_CRYSTAL_SAVECP        |   385|15.06.2023|07:05:34|    47104|K        |
|   386|Prg|CL_ALV_CML_GRAPHICS===========CP        |   386|15.06.2023|07:05:34|    61440|K        |
|   387|Prg|IF_ALV_CML_MODEL_ITEM=========IP        |   369|26.01.2023|17:38:19|    10240|K        |
|   388|Prg|CL_ALV_CML_LAYOUT_SAVE_AS=====CP        |   388|26.01.2023|17:38:18|    22528|K        |
|   389|Prg|CL_ALV_CML_LAYOUT_SAVE_WITH===CP        |   389|16.05.2023|13:23:36|    25600|K        |
|   390|Prg|CL_ALV_CML_LAYOUT_CHOOSE======CP        |   390|26.01.2023|17:38:18|    22528|K        |
|   391|Prg|CL_ALV_CML_TABLE_SETTINGS=====CP        |   391|15.06.2023|07:05:34|    54272|K        |
|   392|Prg|CL_SALV_GRM_LISTVIEW_SERVICE==CP        |   392|03.05.2025|07:02:06|    61440|K        |
|   393|Prg|CL_SALV_GRM_VMODEL_GRID=======CP        |   393|03.05.2025|07:02:06|    24576|K        |
|   394|Prg|IF_SALV_GRM_VMODEL_GRID=======IP        |   358|03.05.2025|07:03:00|    14336|K        |
|   395|Prg|IF_SALV_GRM_LISTVIEW_SERVICE==IP        |   393|03.05.2025|07:03:00|    20480|K        |
|   396|Prg|IF_SALV_GRM_VMODEL_CTRL=======IP        |   359|10.11.2022|19:49:50|     9216|K        |
|   397|Prg|CL_SALV_GRU_VIEW_GRID=========CP        |   397|15.06.2023|07:05:34|    44032|K        |
|   398|Prg|IF_SALV_GRU_VIEW_GRID=========IP        |   358|16.05.2023|13:23:45|     9216|K        |
|   399|Prg|IF_ALV_Z======================IP        |   359|03.05.2025|07:11:14|    20480|K        |
|   400|Typ|LVC_S_COL                               |     0|21.12.1999|19:15:32|     3072|         |
|   401|Prg|CL_SALV_GUI_SRAL_HANDLER======CP        |   401|03.05.2025|07:02:08|    83968|K        |
|   402|Prg|IF_ALV_RM_VARIANT_FRIEND======IP        |   260|18.06.2009|09:23:13|     5120|K        |
|   403|Prg|IF_SALV_GUI_FACADE============IP        |   260|16.05.2023|13:26:35|     9216|K        |
|   404|Prg|IF_SALV_GUI_GRID_FACADE=======IP        |   260|03.05.2025|07:03:01|     9216|K        |
|   405|Prg|CL_DYNP_RAL_API===============CP        |   405|26.01.2023|17:37:52|   119808|K        |
|   406|Typ|DYSTRUCT                                |     0|28.05.2018|11:31:39|     3072|         |
|   407|Prg|IF_EX_EEWAEL_LIST=============IP        |   132|26.02.2015|05:43:32|    13312|K        |
|   408|Typ|SYCH_TABLES_ENTRY                       |     0|17.06.2009|13:37:47|     2048|         |
|   409|Typ|LVC_S_DD01                              |     0|18.05.2004|14:06:56|     3072|         |
|   410|Typ|EWAEL_SGRID_TITLE                       |     0|05.04.2016|13:50:12|     3072|         |
|   411|Typ|EWASGRIDCOMBODEF                        |     0|28.05.2018|11:32:14|     4096|         |
|   412|Typ|EWASFOPPARAMETER                        |     0|28.05.2018|11:32:14|     5120|         |
|   413|Typ|EWASWDOC_WORKAREA_DP                    |     0|25.02.2015|23:58:21|     5120|         |
|   414|Prg|CL_EEWA_BO_WDOCACTIVITY=======CP        |   414|05.09.2025|15:09:31|   113664|K        |
|   415|Typ|EWASWDOC_ACTIVITY                       |     0|25.02.2015|23:58:21|     4096|         |
|   416|Typ|TEWA_WDOC_ACTIV                         |     0|26.02.2015|01:02:14|     3072|         |
|   417|Prg|CL_EX_EEWA_BA_WDOCACTIVITY====CP        |   417|26.02.2015|05:41:06|    14336|K        |
|   418|Prg|IF_EX_EEWA_BA_WDOCACTIVITY====IP        |    24|26.02.2015|05:43:33|     5120|K        |
|   419|Typ|TEWA_WDOC_ACTIVT                        |     0|26.02.2015|01:02:15|     3072|         |
|   420|Typ|ISU_AUTHOR                              |     0|24.02.2005|13:45:13|     3072|         |
|   421|Prg|SAPLEE00                                |   421|17.06.2026|07:11:44|   345088|K        |
|   422|Typ|DD04V                                   |     0|01.04.2022|13:52:51|    10240|         |
|   423|Prg|SAPLSDIF                                |   423|31.03.2023|13:52:11|   475136|K        |
|   424|Prg|CL_SALV_VERI_RUN==============CP        |   424|18.03.2023|11:02:55|    80896|K        |
|   425|Prg|SAPLKKBL                                |   425|03.05.2025|08:07:25|  2125824|K        |
|   426|Prg|SAPLSKBB                                |   426|15.06.2023|07:05:35|   104448|K        |
|   427|Typ|LVC_S_RPRP                              |     0|04.12.2000|12:48:05|     3072|         |
|   428|Typ|LVC_S_SELF                              |     0|04.12.2000|12:48:07|     4096|         |
|   429|Prg|%_CKKBLO                                |   370|16.12.2022|13:58:46|   161792|K        |
|   430|Typ|DTC_S_LAYO                              |     0|28.05.2018|11:31:36|     7168|         |
|   431|Typ|ALV_S_PCTL                              |     0|10.10.2014|18:26:23|     9216|         |
|   432|Typ|PRI_PARAMS                              |     0|31.05.2013|08:08:32|     6144|         |
|   433|Typ|ARC_PARAMS                              |     0|10.10.2014|18:26:25|     6144|         |
|   434|Prg|CL_ABAP_LIST_UTILITIES========CP        |   434|10.11.2022|19:49:27|    22528|K        |
|   435|Prg|SAPLSKBH                                |   435|15.06.2023|07:05:35|  1029120|K        |
|   436|Prg|SAPLSSN1                                |   436|16.12.2022|13:58:47|    32768|K        |
|   437|Typ|DDFIXVALUE                              |     0|27.08.1999|18:04:56|     3072|         |
|   438|Prg|CL_DD_TEXT_PROVIDER===========CP        |   438|31.03.2023|13:47:28|    61440|K        |
|   439|Prg|CL_DD_DFIES_CACHE=============CP        |   439|10.11.2022|19:49:32|    12288|K        |
|   440|Prg|CL_DD_EXPIMP_CACHES===========CP        |   440|08.11.2019|10:05:11|     8192|K        |
|   441|Typ|DDDFIESCACHE                            |     0|08.11.2019|08:53:27|     3072|         |
|   442|Prg|CL_SALV_GUI_UI_MASKING========CP        |   442|03.05.2025|08:07:07|    52224|K        |
|   443|Prg|ALV_GUI_BADI_UI_MASKING=======CP        |   443|08.08.2024|11:08:52|    14336|K        |
|   444|Prg|CL_SALV_GZT_ADMIN=============CP        |   444|15.06.2023|07:05:20|    18432|K        |
|   445|Prg|IF_SALV_CSQ_LOG===============IP        |   359|10.11.2022|19:49:50|    15360|K        |
|   446|Prg|IF_ALV_CML====================IP        |   369|26.01.2023|17:38:19|    14336|K        |
|   447|Prg|IF_ALV_CUL====================IP        |   353|16.05.2023|13:23:36|    12288|K        |
|   448|Prg|CL_ALV_GUI_VARIANT_DB_ACCESS==CP        |   448|03.05.2025|07:01:56|   158720|K        |
|   449|Prg|CL_ALV_CUL_CONTROLLER=========CP        |   449|03.05.2025|07:01:56|   153600|K        |
|   450|Prg|SAPLSLVC_DIALOG                         |   450|15.06.2023|07:05:35|   815104|K        |
|   451|Prg|CL_ALV_CUL_LAYOUT_SAVE_AS=====CP        |   451|15.06.2023|07:05:34|   101376|K        |
|   452|Typ|LTVARIANT                               |     0|31.05.2013|08:05:52|     6144|         |
|   453|Typ|LVC_S_RNGS                              |     0|10.11.1999|17:41:05|     3072|         |
|   454|Prg|SAPLSKBS                                |   454|15.06.2023|07:05:19|   669696|K        |
|   455|Typ|LTDXT                                   |     0|15.11.2000|17:38:31|     4096|         |
|   456|Typ|LTDXS                                   |     0|15.11.2000|17:38:30|     3072|         |
|   457|Prg|CL_ALV_CML_CONFIGURATION_COPY=CP        |   457|26.01.2023|17:38:18|    31744|K        |
|   458|Typ|EUINFO                                  |     0|31.05.2013|08:02:19|     7168|         |
|   459|Prg|CL_DD_DFIES_PROVIDER_DTEL=====CP        |   459|31.03.2023|13:47:28|    23552|K        |
|   460|Prg|IF_DD_NAMETAB_CONSTANTS=======IP        |    37|16.12.2022|13:58:23|    10240|K        |
|   461|Prg|CL_DD_DATATYPE================CP        |   461|16.05.2023|13:23:45|    76800|K        |
|   462|Typ|EWAEL_LIST_OPTIONS                      |     0|28.05.2018|11:32:14|    24576|         |
|   463|Prg|CL_ALV_GUI_GRID_UIM_API=======CP        |   463|03.05.2025|08:08:02|    28672|K        |
|   464|Prg|CL_I18N_BIDI==================CP        |   464|26.01.2023|17:38:05|    70656|K        |
|   465|Prg|CL_ABAP_CONV_IN_CE============CP        |   465|10.11.2022|19:49:27|    25600|K        |
|   466|Prg|CL_SALV_BS_RUNTIME_INFO=======CP        |   466|31.03.2023|15:25:35|    66560|K        |
|   467|Prg|CX_SALV_BS_SC_RUNTIME_INFO====CP        |   467|10.11.2022|19:49:48|    13312|K        |
|   468|Prg|CX_SALV_BS_SC_MSG=============CP        |   468|10.11.2022|19:49:48|    12288|K        |
|   469|Prg|CX_SALV_BS_SC=================CP        |   469|10.11.2022|19:49:48|    10240|K        |
|   470|Prg|SAPLSLVX                                |   470|16.12.2022|13:58:47|    41984|K        |
|   471|Typ|LVC_S_STBL                              |     0|30.11.1998|15:53:11|     2048|         |
|   472|Prg|IF_SALV_GUI_GRID_BRIDGE_CALLS=IP        |   359|10.11.2022|19:49:50|     9216|K        |
|   473|Prg|IF_ALV_CML_CONFIGURATION======IP        |   359|15.06.2023|07:05:35|    54272|K        |
|   474|Prg|IF_SALV_GRM_TABLE_MODEL=======IP        |   359|03.05.2025|07:03:00|    18432|K        |
|   475|Typ|LVC_S_ADDI                              |     0|28.05.2018|11:33:57|     3072|         |
|   476|Prg|CL_SALV_GRM_VMODEL_TOOLBAR====CP        |   476|03.05.2025|07:02:06|    41984|K        |
|   477|Prg|CL_SALV_GRU_VIEW_TOOLBAR======CP        |   477|31.03.2023|15:25:22|    18432|K        |
|   478|Prg|IF_SALV_GRU_VIEW_TOOLBAR======IP        |   358|10.11.2022|19:49:50|     5120|K        |
|   479|Prg|IF_SALV_GUI_GRID_EVENTS=======IP        |   370|03.05.2025|07:03:01|     5120|K        |
|   480|Typ|LVC_S_LAYL                              |     0|28.05.2018|11:34:02|     3072|         |
|   481|Typ|LVC_S_INFO                              |     0|28.05.2018|11:34:01|     7168|         |
|   482|Typ|LVC_S_CO01                              |     0|28.05.2018|11:33:58|     2048|         |
|   483|Prg|IF_DATAPONDEMAND==============IP        |   272|01.06.2015|12:30:31|     5120|K        |
|   484|Typ|LVC_S_COL2                              |     0|28.05.2018|11:33:58|     2048|         |
|   485|Typ|LVC_S_DATA                              |     0|28.05.2018|11:33:58|     5120|         |
|   486|Typ|LVC_S_COLL                              |     0|28.05.2018|11:33:58|     3072|         |
|   487|Prg|SAPFS_SECLOG                            |   401|10.11.2022|19:49:52|    25600|K        |
|   488|Typ|INDX                                    |     0|29.07.1998|18:38:15|     7168|         |
|   489|Typ|DYNP_RAL_SCREEN                         |     0|31.05.2013|08:00:52|     3072|         |
|   490|Prg|CL_SRAL_FACTORY_DYNP==========CP        |   490|26.01.2023|17:38:14|    45056|K        |
|   491|Prg|CL_SRAL_SWITCH_CLIENT=========CP        |   491|10.11.2022|19:49:43|    10240|K        |
|   492|Prg|CL_SRAL_DB_PARAMS=============CP        |   492|10.11.2022|19:49:43|    23552|K        |
|   493|Typ|LVC_S_ROID                              |     0|28.05.2018|11:34:06|     2048|         |
|   494|Prg|IF_SALV_GUI_UI_FUNCTION_READ==IP        |   323|03.05.2025|07:03:08|     7168|K        |
|   495|Prg|CL_SALV_GUI_TB_CONFIGURATOR===CP        |   495|03.05.2025|07:03:17|   103424|K        |
|   496|Prg|IF_SALV_GUI_TB_INTERNAL_API===IP        |   323|03.05.2025|07:03:05|    43008|K        |
|   497|Prg|IF_SALV_GUI_TB_TOOLBAR_BUILDERIP        |   323|03.05.2025|07:03:06|    13312|K        |
|   498|Prg|CL_ALV_CHECK_THIRD_PARTY======CP        |   498|15.06.2023|07:05:19|    12288|K        |
|   499|Prg|CL_SALV_BS_SESSION_CACHE======CP        |   499|10.11.2022|19:49:41|    15360|K        |
|   500|Typ|ARFCSDATA                               |     0|08.09.2011|14:17:09|     7168|         |
|   501|Prg|IF_GUI_DYNAMIC_DATA===========IP        |   216|01.06.2015|12:30:31|     5120|K        |
|   502|Prg|IF_CACHED_PROP================IP        |   216|01.06.2015|12:30:31|     5120|K        |
|   503|Prg|CL_SALV_BOQ_PARAMS============CP        |   503|10.11.2022|19:49:41|    30720|K        |
|   504|Prg|CL_GUI_CRYSTALVIEWER==========CP        |   504|10.11.2022|19:49:36|    52224|K        |
|   505|Prg|SAPLAQXI                                |   505|01.06.2015|12:30:31|    24576|K        |
|   506|Typ|AQXINTI                                 |     0|14.04.1998|11:17:17|     3072|         |
|   507|Rty|type000000000003                        |     0|17.06.2026|13:31:00|        0|         |
|   508|Rty|type000000000004                        |     0|17.06.2026|13:31:00|        0|         |
|   509|Rty|type000000000005                        |     0|17.06.2026|13:31:00|        0|         |
|   510|Prg|CL_SADL_ENTITY_FACTORY========CP        |   510|21.08.2023|10:35:32|    74752|K        |
|   511|Prg|SADL_ENTITY_METADATA_PROVIDER=CP        |   511|21.08.2023|10:35:32|    30720|K        |
|   512|Prg|CL_SADL_ENTITY_PROVIDER_CDS===CP        |   512|18.04.2025|12:30:38|   290816|K        |
|   513|Prg|CL_SADL_ENTITY_PROVIDER=======CP        |   513|16.05.2023|13:23:44|    18432|K        |
|   514|Prg|IF_SADL_ENTITY_FACTORY========IP        |   512|16.05.2023|13:23:44|    21504|K        |
|   515|Prg|CL_SADL_TRACE_FACTORY=========CP        |   515|26.01.2023|17:38:14|    27648|K        |
|   516|Prg|CL_SADL_TRACE_FORMATTING======CP        |   516|26.01.2023|17:38:14|    16384|K        |
|   517|Prg|CL_SADL_TRACE=================CP        |   517|28.04.2023|07:29:48|    68608|K        |
|   518|Prg|CL_SADL_UTIL==================CP        |   518|16.05.2023|13:23:10|    36864|K        |
|   519|Prg|CL_SADL_ENTITY_CDS============CP        |   519|18.04.2025|12:30:38|   331776|K        |
|   520|Prg|CL_SADL_ENTITY================CP        |   520|16.05.2023|13:23:44|   100352|K        |
|   521|Prg|CL_SADL_ENTITY_PROV_FACTORY===CP        |   521|16.05.2023|13:23:44|    18432|K        |
|   522|Prg|CL_SADL_CDS_DDIC_API==========CP        |   522|18.04.2025|12:30:23|   123904|K        |
|   523|Prg|CL_SADL_TRACE_BUILDER_INACTIVECP        |   523|26.01.2023|17:37:55|    21504|K        |
|   524|Prg|CL_DD_DDL_ANNOTATION_SERVICE==CP        |   524|15.06.2023|07:05:23|  1347584|K        |
|   525|Prg|CL_DD_DRTY_ANNO_PROVIDER======CP        |   525|15.06.2023|07:05:20|    47104|K        |
|   526|Prg|CL_DD_DTEL_ANNO_PROVIDER======CP        |   526|12.02.2025|20:57:20|    25600|K        |
|   527|Prg|CL_DD_EXPRESSION_TYPE_INFO====CP        |   527|16.05.2023|13:04:57|    37888|K        |
|   528|Prg|CL_DD_EXPRESSION_DTEL_INFO====CP        |   528|18.04.2025|12:30:37|    53248|K        |
|   529|Prg|CL_DD_EXPRSSN_SIMPLE_TYPE_INFOCP        |   529|03.05.2025|08:07:03|    89088|K        |
|   530|Prg|CL_RTDD_ACCESS================CP        |   530|16.05.2023|13:23:34|    67584|K        |
|   531|Prg|IF_RTDD_OBJECT================IP        |   524|08.08.2024|10:14:16|    20480|K        |
|   532|Prg|CX_SADL_NO_DATA_FROM_DDIC=====CP        |   532|26.01.2023|17:38:15|    19456|K        |
|   533|Prg|CX_SADL_NO_CHECK==============CP        |   533|10.11.2022|19:49:48|    10240|K        |
|   534|Prg|CL_SADL_EXCEPTION_TRACE=======CP        |   534|26.01.2023|17:38:13|    30720|K        |
|   535|Prg|CL_SADL_TRACE_SXT_COMP_PROV===CP        |   535|26.01.2023|17:38:14|    13312|K        |
|   536|Prg|CX_SADL_UNKNOWN_ENTITY========CP        |   536|16.05.2023|13:23:44|    34816|K        |
|   537|Prg|CX_SADL_ENTITY================CP        |   537|16.05.2023|13:23:44|    31744|K        |
|   538|Prg|CX_SADL_CONTRACT_VIOLATION====CP        |   538|10.11.2022|19:49:48|    19456|K        |
|   539|Prg|CX_SADL_DYNAMIC===============CP        |   539|10.11.2022|19:49:48|    10240|K        |
|   540|Prg|CL_SALV_IDA_STRUCTDESCR=======CP        |   540|10.05.2019|14:04:45|   159744|K        |
|   541|Prg|CL_SADL_ENTITY_DDIC_PROVIDER==CP        |   541|15.06.2023|07:05:20|    52224|K        |
|   542|Prg|CL_SADL_ENTITY_DDIC_TABLE_VIEWCP        |   542|16.05.2023|13:23:44|    84992|K        |
|   543|Typ|DD12V                                   |     0|31.03.2023|13:52:10|     9216|         |
|   544|Typ|DD17V                                   |     0|20.08.1998|10:50:57|     4096|         |
|   545|Typ|DD03P                                   |     0|23.04.2019|09:15:19|    15360|         |
|   546|Typ|DD05M                                   |     0|25.02.2015|23:57:15|     4096|         |
|   547|Typ|DD08V                                   |     0|20.08.1998|11:35:49|     5120|         |
|   548|Typ|DD35V                                   |     0|08.09.2011|14:19:37|     3072|         |
|   549|Typ|DD36M                                   |     0|25.02.2015|23:57:15|     5120|         |
|   550|Typ|DD02V                                   |     0|01.04.2022|13:52:49|    12288|         |
|   551|Typ|DD09V                                   |     0|14.01.2020|13:31:29|     6144|         |
|   552|Prg|SAPLSDTB                                |   552|18.04.2025|12:30:37|   658432|K        |
|   553|Typ|DDCACHE                                 |     0|30.03.1995|13:12:06|     2048|         |
|   554|Typ|DCTABDGET                               |     0|13.05.1997|12:49:28|     3072|         |
|   555|Prg|SAPLSDSG                                |   555|16.12.2022|13:59:06|   147456|K        |
|   556|Typ|DD02L                                   |     0|19.11.2020|09:53:21|    11264|         |
|   557|Typ|DDSTATE                                 |     0|07.05.1997|12:49:07|     2048|         |
|   558|Typ|DD12L                                   |     0|31.03.2023|13:47:28|     9216|         |
|   559|Typ|DCTABLGET                               |     0|04.06.1997|19:16:24|     3072|         |
|   560|Prg|CL_SADL_QUERY_UTIL============CP        |   560|16.05.2023|13:23:14|    13312|K        |
|   561|Rty|type000000000006                        |     0|17.06.2026|13:31:00|        0|         |
|   562|Prg|IF_SADL_API_VERSION===========IP        |   540|07.04.2016|14:31:19|     5120|K        |
|   563|Prg|CL_SADL_ENTITY_CONSUMP_INFO===CP        |   563|15.06.2023|07:05:35|   128000|K        |
|   564|Prg|IF_SADL_ENTITY================IP        |   540|16.05.2023|13:23:44|    64512|K        |
|   565|Prg|IF_SADL_TYPES=================IP        |   542|16.05.2023|13:23:44|    80896|K        |
|   566|Prg|CL_SALV_IDA_TEXT_SEARCH_PROV==CP        |   566|26.01.2018|15:28:03|   103424|K        |
|   567|Prg|IF_SALV_BS_LOG_EXP_OPERAND====IP        |   345|23.04.2019|09:42:06|     6144|K        |
|   568|Prg|%_CSCTX                                 |   288|01.06.2015|12:30:30|    10240|K        |
|   569|Typ|STB_BTNMNU                              |     0|04.12.2000|12:53:55|     3072|         |
|   570|Prg|CL_CTXMNU_MGR=================CP        |   570|28.05.2018|11:34:26|    20480|K        |
|   571|Prg|IF_SALV_GRM_VMODEL_TOOLBAR====IP        |   476|10.11.2022|19:49:50|     8192|K        |
|   572|Prg|CL_EEWA_CONVERT===============CP        |   572|05.03.2021|22:20:42|    40960|K        |
|   573|Prg|CL_EEWA_HASH==================CP        |   573|26.02.2015|07:25:05|    13312|K        |
|   574|Prg|SAPLSECH                                |   574|18.03.2023|11:02:28|    65536|K        |
|   575|Typ|LVC_S_ROW                               |     0|02.11.1998|09:45:47|     3072|         |
|   576|Typ|EWAEL_SOURCEKEY                         |     0|25.02.2015|23:58:18|     4096|         |
|   577|Prg|SAPLSCNT                                |   577|28.05.2018|11:34:27|    32768|K        |
|   578|Typ|DYCBOX                                  |     0|20.08.1998|11:16:53|     3072|         |
|   579|Prg|SAPLSVSM                                |   579|10.11.2022|19:49:55|    31744|K        |
|   580|Prg|SAPSHDTV                                |   577|31.03.2023|15:25:24|    91136|K        |
|   581|Typ|SHDSTU                                  |     0|14.12.1998|23:15:34|     3072|         |
|   582|Typ|SHDSTCIU                                |     0|14.12.1998|23:15:34|     3072|         |
|   583|Prg|SDYNPRAL                                |   401|02.05.2016|13:52:39|     8192|K        |
|   584|Prg|SAPLSDYNPRAL                            |   584|10.11.2022|19:49:54|    16384|K        |
|   585|Typ|DYNP_RAL_ID                             |     0|27.11.2013|14:25:51|     3072|         |
|   586|Prg|CL_SRAL_RECORDER_FACTORY_DYNP=CP        |   586|26.01.2023|17:38:14|    21504|K        |
|   587|Prg|SRAL_DYNP_ADAPTER                       |   405|10.11.2022|19:49:55|    27648|K        |
|   588|Prg|RADSHPBOPREFLIGHT                       |     0|10.11.2022|19:49:51|    18432|K        |
|   589|Prg|RADSHPBOSTATUS                          |     0|10.11.2022|19:49:51|    20480|K        |
|   590|Prg|RADSHPBOSWITCH                          |     0|10.11.2022|19:49:51|    21504|K        |
|   591|Prg|RADSHUPGRADE                            |     0|10.11.2022|19:49:51|    21504|K        |
|   592|Prg|RADSHPBO                                |     0|18.03.2023|11:02:30|    62464|K        |
|   593|Prg|CL_DSH_SCREEN_ASSIGNMENT======CP        |   593|16.05.2023|13:23:10|    43008|K        |
|   594|Prg|CL_ABAP_MESSAGE_DIGEST========CP        |   594|16.05.2023|13:24:01|    46080|K        |
|   595|Prg|CL_DSH_DDIC_ASSIGNMENT========CP        |   595|28.04.2023|07:29:55|    59392|K        |
|   596|Prg|IF_DSH_VALUE_HELP_PROCESSOR===IP        |     0|03.11.2021|19:19:43|    16384|K        |
|   597|Prg|WPBHELP_REGISTRATION                    |     0|06.04.2021|14:01:49|    11264|K        |
|   598|Prg|SDYNCONTEXT                             |     0|01.04.2022|13:52:52|     9216|K        |
|   599|Prg|CL_USER_CONTEXT===============CP        |   599|10.11.2022|19:49:46|    16384|K        |
|   600|Prg|/PERSONAS/DYNP_INTEGRATION              |     0|12.02.2025|20:56:50|    24576|K        |
|   601|Prg|/PERSONAS/CL_API==============CP        |   601|12.02.2025|20:56:50|    12288|K        |
|   602|Prg|/PERSONAS/CL_HANDLER_FACTORY==CP        |   602|12.02.2025|20:56:50|    35840|K        |
|   603|Prg|/PERSONAS/IF_CONTEXT_TYPES====IP        |   601|12.02.2025|18:36:08|    64512|K        |
|   604|Prg|/PERSONAS/CL_SETTINGS_DATA====CP        |   604|12.02.2025|20:56:50|   178176|K        |
|   605|Prg|/PERSONAS/IF_SETTINGS_TYPES===IP        |   604|12.02.2025|18:35:57|    16384|K        |
|   606|Prg|/PERSONAS/CL_AUTH_UTILS=======CP        |   606|12.02.2025|18:36:08|    50176|K        |
|   607|Prg|/PERSONAS/IF_AUTH_TYPES=======IP        |   606|12.02.2025|18:36:08|    19456|K        |
|   608|Prg|/PERSONAS/CL_AUTHENTICATOR====CP        |   608|12.02.2025|18:35:43|    15360|K        |
|   609|Typ|RSEXC                                   |     0|17.06.2009|13:36:19|     2048|         |
|   610|Typ|RSEXP                                   |     0|31.05.2013|08:10:41|     4096|         |
|   611|Typ|RSIMP                                   |     0|31.05.2013|08:10:43|     4096|         |
|   612|Typ|RSTBL                                   |     0|31.05.2013|08:11:14|     4096|         |
|   613|Prg|SAPLSUNI                                |   613|28.04.2023|07:29:53|   290816|K        |
|   614|Typ|RSCHA                                   |     0|31.05.2013|08:10:18|     4096|         |
|   615|Typ|RSFDO                                   |     0|13.08.1997|12:52:58|     3072|         |
|   616|Typ|TFDIR                                   |     0|29.03.2018|15:55:42|     4096|         |
|   617|Typ|ENLFDIR                                 |     0|01.06.2015|10:51:41|     4096|         |
|   618|Typ|RS38L                                   |     0|29.03.2018|15:54:09|    33792|         |
|   619|Prg|SAPMS38L                                |   613|16.05.2023|13:23:24|  1150976|K        |
|   620|Prg|SAPLSEWORKINGAREA                       |   620|26.05.2023|05:06:55|   660480|K        |
|   621|Typ|FUPARAREF                               |     0|31.05.2013|07:57:17|     5120|         |
|   622|Typ|USVALUES                                |     0|31.05.2013|08:16:39|     3072|         |
|   623|Prg|SAPLSUSE                                |   623|17.06.2024|13:21:25|   271360|K        |
|   624|Typ|USER12                                  |     0|08.09.2011|14:28:36|     3072|         |
|   625|Prg|CL_ABAP_SYST==================CP        |   625|24.03.2016|13:14:20|    13312|K        |
|   626|Prg|CL_SUSR_AUTHORIZATIONS========CP        |   626|17.06.2026|07:11:43|   125952|K        |
|   627|Prg|CL_SUID_TOOLS=================CP        |   627|17.06.2026|07:11:43|   311296|K        |
|   628|Typ|SUID_ST_BNAME                           |     0|08.09.2011|14:23:36|     2048|         |
|   629|Prg|SAPFS_DYNPRO_HOOKS                      |     0|26.01.2018|15:16:09|     7168|K        |
|   630|Prg|SAPLSALV_IDA_IMPL_UI_SCREEN             |   630|18.03.2023|11:03:29|    16384|K        |
|   631|Prg|CL_SALV_GUI_ROUNDTRIP_BROKER==CP        |   631|18.03.2023|11:03:29|    53248|K        |
|   632|Prg|CL_IXML_MINI==================CP        |   632|28.05.2018|11:32:46|     9216|K        |
|   633|Prg|CL_IXML_MINI_DOM==============CP        |   633|18.02.2005|14:13:24|    18432|K        |
|   634|Prg|IF_IXML_MINI_NODE=============IP        |   633|09.09.2004|14:18:21|     8192|K        |
|   635|Typ|IXMLTDOM                                |     0|18.05.2004|14:07:00|     3072|         |
|   636|Prg|CL_IXML_MINI_NODE=============CP        |   636|18.02.2005|14:13:24|    22528|K        |
|   637|Prg|IF_SIMPLEPROPTREE_XML=========IP        |   216|13.11.2000|16:49:03|     5120|K        |
|   638|Prg|CL_IXML_MINI_RENDERER=========CP        |   638|18.02.2005|14:13:24|    12288|K        |
|   639|Prg|SAPLSPOF                                |   639|18.07.2023|12:08:17|   220160|K        |
|   640|Prg|SITSPMON                                |   228|10.11.2022|19:48:33|   328704|K        |
|   641|Prg|SAPLHTTP_RUNTIME                        |   641|18.07.2023|12:08:17|   742400|K        |
|   642|Typ|ICFLOGIN                                |     0|18.03.2023|10:50:19|    13312|         |
|   643|Typ|ICFALIAS                                |     0|28.05.2018|11:32:37|    19456|         |
|   644|Prg|CL_UCON_HELPER================CP        |   644|18.03.2023|11:02:29|    64512|K        |
|   645|Prg|CL_UCON_KRN_SERVICES==========CP        |   645|26.01.2023|17:38:15|    25600|K        |
|   646|Prg|CL_ICF_SHM_AREA===============CP        |   646|10.11.2022|19:49:37|    46080|K        |
|   647|Typ|ICFHANDLER                              |     0|08.09.2011|14:15:58|     3072|         |
|   648|Typ|ICFSHMHANDLST                           |     0|08.09.2011|14:26:33|     8192|         |
|   649|Typ|ICF_SHMRUNT                             |     0|18.03.2023|10:50:20|    31744|         |
|   650|Typ|ICFALIAS_SHMRUNT                        |     0|19.11.2020|09:41:27|     3072|         |
|   651|Prg|CL_ICF_SHM_ROOT===============CP        |   651|19.11.2020|11:23:07|    41984|K        |
|   652|Typ|ICF_PATHFIELDS                          |     0|08.09.2011|14:26:43|     3072|         |
|   653|Typ|ICFHANDLST                              |     0|28.05.2018|11:32:38|    19456|         |
|   654|Prg|IF_SALV_GUI_GRID_LVC_WRITER===IP        |   397|03.05.2025|07:03:01|    17408|K        |
|   655|Prg|SAPLRHFE                                |   655|29.03.2018|15:55:41|    21504|K        |
|   656|Prg|SAPLSETRM                               |   656|16.05.2023|13:23:54|   327680|K        |
|   657|Typ|BAL_S_MSG                               |     0|31.05.2013|07:57:43|     8192|         |
|   658|Typ|EWA_WDOC_WAREAT                         |     0|26.02.2015|00:21:31|     3072|         |
|   659|Prg|CL_EEWA_SECURITY_DPP_ACCESS===CP        |   659|17.06.2026|07:11:56|    32768|K        |
|   660|Prg|CL_EEWA_SECURITY_DPP_CACHE====CP        |   660|29.05.2015|12:33:34|    22528|K        |
|   661|Prg|CL_EEWA_IMPL_BO_CALLBACK_MLOG=CP        |   661|05.09.2025|15:09:31|    20480|K        |
|   662|Typ|SRFC_SMETH                              |     0|06.11.2003|20:31:19|     3072|         |
|   663|Prg|%_CIHTTP                                |   641|03.05.2025|08:07:53|    53248|K        |
|   664|Typ|EWASDPP_INTERFACE_ENTRY                 |     0|25.02.2015|23:58:19|     3072|         |
|   665|Prg|CL_EEWA_SECURITY_DPP_ACC_CBP==CP        |   665|29.05.2015|12:33:34|    21504|K        |
|   666|Prg|SAPLEEWA_WDORDERRESULT                  |   666|17.06.2026|07:11:44|   801792|K        |
|   667|Prg|CL_EEWA_FORM_TREE=============CP        |   667|05.09.2025|15:09:31|   539648|K        |
|   668|Prg|IF_EEWA_WDOC_APPLICATION======IP        |   666|26.02.2015|05:43:30|     7168|K        |
|   669|Prg|CL_GUI_SIMPLE_CONTAINER=======CP        |   669|28.05.2018|11:34:26|    44032|K        |
|   670|Prg|CL_EEWA_MSGLOG_COLLECTOR_BPEM=CP        |   670|26.02.2015|07:25:07|    13312|K        |
|   671|Prg|CL_EEWA_MSGLOG_COLLECTOR======CP        |   671|29.05.2015|12:31:23|    15360|K        |
|   672|Prg|IF_EEWA_WDOC_BPEM=============IP        |    15|26.02.2015|05:43:30|     5120|K        |
|   673|Prg|IF_EEWA_WDOC_SUBAPP_CREA======IP        |    15|20.12.2021|16:29:42|     6144|K        |
|   674|Prg|CL_EX_EEWA_BD_WDORDERRSLT=====CP        |   674|10.03.2025|17:31:15|    14336|K        |
|   675|Prg|IF_EX_EEWA_BD_WDORDERRSLT=====IP        |    12|10.03.2025|17:31:16|     5120|K        |
|   676|Prg|CL_EEWA_DRAGOBJECT_TREE=======CP        |   676|26.02.2015|05:40:38|    11264|K        |
|   677|Prg|SAPLEEWA_WDORDER                        |   677|09.04.2026|00:02:57|   657408|K        |
|   678|Prg|CL_EEWA_FORM_LIST=============CP        |   678|05.09.2025|15:09:31|   424960|K        |
|   679|Prg|CL_EEWA_BO_WDORDER============CP        |   679|17.06.2026|07:11:42|   843776|K        |
|   680|Prg|CL_EEWA_TA_SUBCONTRACTING_BASECP        |   680|17.06.2026|07:11:42|   274432|K        |
|   681|Prg|CL_EEWA_TRANSACTION===========CP        |   681|05.09.2025|15:09:31|    38912|K        |
|   682|Prg|IF_EEWA_WDOC_SUBAPPLICATION===IP        |    15|26.02.2015|05:43:31|     6144|K        |
|   683|Typ|EWASWDORDER                             |     0|05.09.2025|15:07:44|    23552|         |
|   684|Typ|EWA_ORDER_HEAD                          |     0|05.09.2025|15:09:30|    21504|         |
|   685|Typ|EWASJSTAT                               |     0|25.02.2015|23:58:20|     3072|         |
|   686|Typ|EWASORDFILLK                            |     0|20.11.2020|06:09:32|     7168|         |
|   687|Typ|EWA_ORDER_RESOBJ                        |     0|08.08.2024|08:36:18|     8192|         |
|   688|Prg|CL_EEWA_BO_WDORDERITEM========CP        |   688|17.06.2026|07:11:56|  1520640|K        |
|   689|Typ|EWASWDORDERITEMDET                      |     0|09.04.2026|00:02:04|    56320|         |
|   690|Typ|EWASSUB_WDOSUB_DMRPOS_DET               |     0|22.01.2021|17:14:16|     5120|         |
|   691|Typ|EWAS_SUB_WDO_PO_DET                     |     0|05.03.2021|19:56:40|     4096|         |
|   692|Prg|CL_EX_EEWA_BA_WDORDER=========CP        |   692|26.02.2015|05:41:07|    14336|K        |
|   693|Prg|IF_EX_EEWA_BA_WDORDER=========IP        |    24|26.02.2015|05:43:33|     5120|K        |
|   694|Prg|/WATP/CL_IM_BA_WDORDER========CP        |   694|08.08.2024|10:53:29|     8192|K        |
|   695|Prg|SAPLASTAT_TRIG                          |   695|01.06.2015|12:30:31|    17408|K        |
|   696|Prg|/WATP/CL_BA_WDORDER===========CP        |   696|09.04.2026|00:02:53|    28672|K        |
|   697|Prg|CL_EEWA_BD_BO_BASE============CP        |   697|26.02.2015|05:40:14|    15360|K        |
|   698|Prg|IF_EX_EEWAEL_BA_BASE==========IP        |    24|01.04.2022|21:44:02|     8192|K        |
|   699|Prg|IF_EX_EEWA_BO_WDORDER=========IP        |   679|26.02.2015|05:43:34|     8192|K        |
|   700|Prg|/WATP/CL_CALLBACK_CONNECTOR===CP        |   700|12.02.2025|20:57:09|    22528|K        |
|   701|Prg|/WATP/CL_BO_OBJTYPE===========CP        |   701|12.02.2025|20:57:09|   350208|K        |
|   702|Prg|/WATP/CL_CACHE_BO=============CP        |   702|12.02.2025|20:57:09|   126976|K        |
|   703|Prg|/WATP/CL_IMPL_BO_CALLBACK_CACHCP        |   703|12.02.2025|18:36:48|    24576|K        |
|   704|Prg|/WATP/CL_PLUGINLIST_BO========CP        |   704|12.02.2025|20:57:11|    58368|K        |
|   705|Prg|/WATP/CL_MISC=================CP        |   705|12.02.2025|20:57:09|   353280|K        |
|   706|Prg|/WATP/CL_SAPSYSTEMINFO========CP        |   706|12.02.2025|20:57:11|   125952|K        |
|   707|Prg|SAPLTIMEEXT                             |   707|01.06.2015|12:30:31|    12288|K        |
|   708|Prg|SAPLSTR4                                |   708|10.11.2022|19:49:55|    93184|K        |
|   709|Prg|%_CTRNSP                                |   708|28.02.2019|15:30:04|    10240|K        |
|   710|Prg|SAPLSTRD                                |   710|15.06.2023|07:05:19|  1218560|K        |
|   711|Typ|TRNSPACE                                |     0|28.02.2019|15:30:04|     3072|         |
|   712|Prg|/WATP/IF_BO_CALLBACK_DETAIL===IP        |    74|08.08.2024|09:25:10|     5120|K        |
|   713|Prg|/WATP/CL_DDICINFO=============CP        |   713|12.02.2025|20:56:03|   292864|K        |
|   714|Typ|/WATP/SOBJTYPE_BO                       |     0|08.08.2024|08:35:48|    15360|         |
|   715|Typ|/WATP/TTOBJTYPE                         |     0|08.08.2024|08:30:14|     9216|         |
|   716|Typ|/WATP/TTOBJTYPEM                        |     0|08.08.2024|08:30:14|     5120|         |
|   717|Typ|/WATP/SDDICINCLUDES                     |     0|08.08.2024|08:35:51|     2048|         |
|   718|Typ|/WATP/SBO_RELATION                      |     0|08.08.2024|08:35:49|     4096|         |
|   719|Typ|/WATP/TTOBJTYPEB                        |     0|08.08.2024|08:30:14|     3072|         |
|   720|Typ|/WATP/TTOBJTYPED                        |     0|24.10.2024|09:00:49|     5120|         |
|   721|Typ|/WATP/SBO_INITDATA                      |     0|08.08.2024|08:35:49|     5120|         |
|   722|Typ|/WATP/TTOBJTYPEA                        |     0|08.08.2024|08:30:14|     3072|         |
|   723|Typ|/WATP/SBO_LANGFIELD                     |     0|08.08.2024|08:35:49|     4096|         |
|   724|Typ|/WATP/SRANGE                            |     0|08.08.2024|08:35:49|     3072|         |
|   725|Prg|/WATP/CL_OBJECTREADER=========CP        |   725|12.02.2025|20:57:09|    87040|K        |
|   726|Typ|/WATP/SOBJREAD_KEY                      |     0|08.08.2024|08:35:48|     3072|         |
|   727|Prg|/WATP/IF_INVOKE===============IP        |    15|08.08.2024|09:25:19|     6144|K        |
|   728|Prg|/WATP/IF_PROGRESS=============IP        |    15|08.08.2024|09:25:22|     5120|K        |
|   729|Prg|/WATP/IF_PROTOCOL=============IP        |    15|08.08.2024|09:25:22|     6144|K        |
|   730|Prg|/WATP/IF_RUNTIME_ANALYSIS=====IP        |    15|08.08.2024|09:25:22|     5120|K        |
|   731|Prg|/WATP/IF_STRINGS_SUBSTITUTION=IP        |    15|08.08.2024|09:25:04|     5120|K        |
|   732|Typ|/WATP/TTOBJTYPET                        |     0|08.08.2024|08:30:14|     4096|         |
|   733|Typ|/WATP/TTOBJTYPMT                        |     0|08.08.2024|08:30:14|     4096|         |
|   734|Typ|/WATP/SBO_DETAILRECORD                  |     0|08.08.2024|08:35:49|     4096|         |
|   735|Prg|/WATP/CL_STRINGS==============CP        |   735|12.02.2025|20:55:26|   135168|K        |
|   736|Prg|CL_ABAP_CONV_UC_NUMBER========CP        |   736|10.11.2022|19:49:27|    14336|K        |
|   737|Prg|CL_ABAP_CONV_OBJ==============CP        |   737|16.05.2023|13:23:09|    31744|K        |
|   738|Typ|TCP00                                   |     0|12.02.2001|19:31:42|     6144|         |
|   739|Prg|/WATP/CL_XML_TRANSFORMER======CP        |   739|12.02.2025|18:36:51|    24576|K        |
|   740|Prg|/WATP/CL_BO_OBJREADER=========CP        |   740|12.02.2025|20:57:09|   216064|K        |
|   741|Typ|/WATP/SOBJREAD                          |     0|24.10.2024|08:59:55|     9216|         |
|   742|Typ|/WATP/TTOBJREAD                         |     0|24.10.2024|09:00:48|     6144|         |
|   743|Typ|/WATP/TTOBJREADM                        |     0|08.08.2024|08:30:14|     4096|         |
|   744|Typ|/WATP/TTOBJREADT                        |     0|08.08.2024|08:30:14|     4096|         |
|   745|Prg|/WATP/CL_OR_IMPLEMENTBOADDON==CP        |   745|12.02.2025|20:57:10|    56320|K        |
|   746|Prg|/WATP/IF_OBJECTREADER=========IP        |   740|08.08.2024|09:25:20|     9216|K        |
|   747|Prg|/WATP/CL_RANGES===============CP        |   747|12.02.2025|20:56:08|   106496|K        |
|   748|Prg|/WATP/CL_BO_EWAORDER==========CP        |   748|09.04.2026|00:02:54|   262144|K        |
|   749|Prg|/WATP/CL_BO_EWAORDER_BASE=====CP        |   749|09.04.2026|00:02:54|   146432|K        |
|   750|Prg|/WATP/IF_EX_BA_BASE===========IP        |   696|08.08.2024|09:25:11|     8192|K        |
|   751|Typ|/WATP/SBO_EWAORDER                      |     0|05.09.2025|15:07:27|    23552|         |
|   752|Typ|/WATP/TSAPJOBJNR                        |     0|08.08.2024|08:30:13|     3072|         |
|   753|Typ|EWATORDFILLK                            |     0|20.11.2020|06:41:55|     7168|         |
|   754|Prg|/WATP/CL_SAPSTATUS============CP        |   754|12.02.2025|20:55:26|    51200|K        |
|   755|Prg|SAPLBSVT                                |   755|21.08.2023|09:57:35|    79872|K        |
|   756|Typ|TJ03T                                   |     0|31.05.2013|08:15:13|     3072|         |
|   757|Prg|IF_EEWA_PLUGIN_BO_BASE========IP        |    24|16.06.2015|09:33:31|    11264|K        |
|   758|Prg|CL_EEWA_SAPSTATUS=============CP        |   758|05.09.2025|15:09:31|    55296|K        |
|   759|Prg|CL_EEWA_TRANSACTION_STATE=====CP        |   759|29.05.2015|12:31:23|    12288|K        |
|   760|Prg|SAPLEEWA_EL_BASE                        |   760|05.09.2025|15:09:32|   251904|K        |
|   761|Prg|SAPMSSY0                                |   761|31.03.2023|15:25:25|   120832|K        |
|   762|Typ|EWASWDORDER_DP                          |     0|05.09.2025|15:07:47|    26624|         |
|   763|Typ|EWASWDO_RESOURCES_DP                    |     0|08.08.2024|08:36:17|    10240|         |
|   764|Prg|CL_EX_EEWA_BD_WDORDER_LIST====CP        |   764|10.03.2025|17:31:16|    15360|K        |
|   765|Prg|IF_EX_EEWA_BD_WDORDER_LIST====IP        |    12|10.03.2025|17:31:16|     5120|K        |
|   766|Typ|EWASSYNC_ITEM                           |     0|25.02.2015|23:58:21|     4096|         |
|   767|Typ|EWASSYNC_CONTROLPAIR                    |     0|25.02.2015|23:58:21|     4096|         |
|   768|Prg|CL_EEWA_FORM_ORDERCNFM_MASK===CP        |   768|05.09.2025|15:09:31|   252928|K        |
|   769|Typ|EWASSUB_WDOSUB_DMRPOS_DET_DP            |     0|22.01.2021|17:14:16|     6144|         |
|   770|Prg|CL_EX_EEWA_BD_WDORDER=========CP        |   770|10.03.2025|17:31:15|    15360|K        |
|   771|Prg|IF_EX_EEWA_BD_WDORDER=========IP        |    12|10.03.2025|17:31:16|     5120|K        |
|   772|Prg|CL_EEWA_GRID_MASK=============CP        |   772|10.03.2025|17:31:15|   267264|K        |
|   773|Typ|V_TAR_DYN                               |     0|31.05.2013|08:19:25|     4096|         |
|   774|Prg|SAPLEEWA_WDOITEM                        |   774|17.06.2026|07:11:44|   702464|K        |
|   775|Typ|EWATORDERDMRPOS                         |     0|15.03.2020|23:55:25|     5120|         |
|   776|Prg|ZCL_WR_EEWA_BO_WDORDERITEM====CP        |   776|17.06.2026|07:14:58|   453632|K        |
|   777|Prg|CL_EX_ISU_WA_HAZARD_WASTE=====CP        |   777|17.06.2026|07:11:10|   273408|K        |
|   778|Prg|IF_EX_ISU_WA_HAZARD_WASTE=====IP        |   150|17.06.2026|07:11:10|   196608|K        |
|   779|Typ|EWASORDOBJFILLK                         |     0|20.11.2020|06:09:32|     9216|         |
|   780|Typ|EWA_ORDER_BULKY                         |     0|03.05.2025|05:24:22|     7168|         |
|   781|Typ|EWA_ORDER_OBJ_SD                        |     0|13.04.2016|15:02:09|     4096|         |
|   782|Typ|EWASWDORDERITEM_VBAP                    |     0|11.12.2025|08:31:12|   100352|         |
|   783|Typ|EWASORDERDMRPOS_DET                     |     0|15.03.2020|23:41:25|     5120|         |
|   784|Typ|EWASSUB_WDOISUB_DMRPOS_DET              |     0|22.01.2021|17:14:16|     5120|         |
|   785|Typ|EWAS_SUB_WDOI_PO_DET                    |     0|05.03.2021|19:56:40|     4096|         |
|   786|Typ|ZWR_SWDOITEM_DIFFCULT                   |     0|16.09.2025|12:32:28|     4096|         |
|   787|Prg|CL_EX_EEWA_BA_WDORDERITEM=====CP        |   787|26.02.2015|05:41:07|    14336|K        |
|   788|Prg|IF_EX_EEWA_BA_WDORDERITEM=====IP        |    24|26.02.2015|05:43:33|     5120|K        |
|   789|Typ|EWASWDORDERITEM_DP                      |     0|09.04.2026|00:02:05|    71680|         |
|   790|Typ|EWASWDORDERITEM_BULKY_DP                |     0|03.05.2025|05:24:21|     7168|         |
|   791|Typ|EWASWDORDERITEM_VBAP_DP                 |     0|11.12.2025|08:31:12|   100352|         |
|   792|Typ|EWASORDERDMRPOS_DP                      |     0|15.03.2020|23:41:25|     5120|         |
|   793|Typ|EWASSUB_WDOISUB_DMRPOS_DET_DP           |     0|22.01.2021|17:14:16|     6144|         |
|   794|Prg|CL_EEWA_BO_OBJBEHAVIOR========CP        |   794|05.09.2025|15:09:31|    82944|K        |
|   795|Typ|TEWA_EL_OBJBHT                          |     0|26.02.2015|01:02:08|     4096|         |
|   796|Prg|ZCL_BHAP_EWAORDERITEMUI=======CP        |   796|03.12.2025|18:00:55|    24576|K        |
|   797|Prg|CL_EEWA_BEHAVIOR_UI===========CP        |   797|05.09.2025|15:09:31|    23552|K        |
|   798|Prg|CL_EEWA_BEHAVIOR==============CP        |   798|05.09.2025|15:09:31|    11264|K        |
|   799|Prg|IF_EEWA_BEHAVIOR_UI===========IP        |    99|16.06.2015|09:33:31|     7168|K        |
|   800|Prg|IF_EEWA_BEHAVIOR==============IP        |    99|26.02.2015|07:26:08|     5120|K        |
|   801|Typ|TEWA_EL_OBJBHUIF                        |     0|15.06.2015|16:51:12|     4096|         |
|   802|Prg|EEWA_BD_WDOITEM===============CP        |   802|12.08.2025|15:11:14|    15360|K        |
|   803|Prg|ZCL_IM_ENH_BD_WDOI_WR_WDOC====CP        |   803|12.08.2025|15:11:10|     8192|K        |
|   804|Prg|IF_EX_EEWA_BD_BADI============IP        |    12|10.03.2025|17:31:16|     5120|K        |
|   805|Prg|IF_EX_EEWAEL_BD_BASE==========IP        |    12|10.03.2025|17:31:16|     8192|K        |
|   806|Prg|IF_BADI_INTERFACE=============IP        |    12|01.06.2015|12:30:31|     5120|K        |
|   807|Prg|IF_EX_EEWA_BD_WDOITEM=========IP        |    12|10.03.2025|17:31:16|     5120|K        |
|   808|Prg|SAPLZWR_WDOC_BD_WDOITEM                 |   808|17.06.2026|07:14:58|   360448|K        |
|   809|Prg|CL_EEWA_BD_FORM_BASE==========CP        |   809|10.03.2025|17:31:16|    17408|K        |
|   810|Typ|EWA_ORDER_OBJECT                        |     0|09.04.2026|00:02:53|    51200|         |
|   811|Typ|EWASWDORDERITEM                         |     0|09.04.2026|00:02:03|    55296|         |
|   812|Typ|SCXTAB_TABSTRIP                         |     0|06.11.2003|20:31:00|     3072|         |
|   813|Prg|IF_EEWA_PLUGIN_FORM_BASE======IP        |    12|10.03.2025|17:31:16|    12288|K        |
|   814|Prg|ZCL_IM_EEWA_WDOC_WDOITEM======CP        |   814|07.11.2025|23:02:49|     9216|K        |
|   815|Prg|IF_EX_EEWA_PLUGIN_FORM========IP        |    12|29.06.2015|14:08:26|     5120|K        |
|   816|Prg|SAPLZWR_WDOC_PLG_WDOITEM                |   816|17.06.2026|07:11:44|   266240|K        |
|   817|Prg|CL_EEWA_PLUGIN_FORM===========CP        |   817|05.09.2025|15:09:31|    54272|K        |
|   818|Typ|ZWR_SWDOITEM_DIFFCULT_GRID              |     0|16.09.2025|12:32:28|     7168|         |
|   819|Typ|ZWR_SWDOC_WDOITEM_DP                    |     0|09.04.2026|00:02:52|    71680|         |
|   820|Typ|EWASFORM_TEXTS                          |     0|25.02.2015|23:58:20|    10240|         |
|   821|Typ|SMP_DYNTXT                              |     0|25.01.1999|13:03:11|     3072|         |
|   822|Prg|SAPLEEWA_WDOWEIGH                       |   822|09.04.2026|00:02:57|   481280|K        |
|   823|Prg|CL_EEWA_BO_WDORDERWEIGH=======CP        |   823|17.06.2026|07:11:42|   422912|K        |
|   824|Typ|EWASWDORDERWEIGH                        |     0|26.05.2025|11:31:15|    25600|         |
|   825|Typ|EWA_ORDER_WEIGH                         |     0|26.05.2025|11:31:26|    25600|         |
|   826|Typ|EWASORDWEIGHFILLK                       |     0|30.04.2022|09:31:48|    11264|         |
|   827|Typ|EWASSUB_WDOWSUB_DMRPOS_DET              |     0|22.01.2021|17:14:16|     6144|         |
|   828|Typ|EWAS_SUB_WDOW_PO_DET                    |     0|05.03.2021|19:56:40|     4096|         |
|   829|Prg|CL_EX_EEWA_BA_WDORDERWEIGH====CP        |   829|26.02.2015|05:41:07|    14336|K        |
|   830|Prg|IF_EX_EEWA_BA_WDORDERWEIGH====IP        |    24|26.02.2015|05:43:33|     5120|K        |
|   831|Typ|EWASWDORDERWEIGH_DP                     |     0|26.05.2025|11:31:16|    30720|         |
|   832|Typ|EWASSUB_WDOWSUB_DMRPOS_DET_DP           |     0|22.01.2021|17:14:16|     6144|         |
|   833|Prg|CL_EX_EEWA_BD_WDOWEIGH========CP        |   833|10.03.2025|17:31:16|    15360|K        |
|   834|Prg|IF_EX_EEWA_BD_WDOWEIGH========IP        |    12|10.03.2025|17:31:16|     5120|K        |
|   835|Prg|/PLCP/CL_IM_WR_WDOC_TREE======CP        |   835|17.06.2026|07:11:41|   357376|K        |
|   836|Prg|ZCL_IM_EEWA_WDOC_TREE=========CP        |   836|07.11.2025|23:02:42|   345088|K        |
|   837|Prg|IF_EX_EEWAEL_TREE=============IP        |    15|02.06.2015|09:22:50|    11264|K        |
|   838|Prg|ZCL_WR_MISC_GOS===============CP        |   838|14.05.2026|18:01:39|    47104|K        |
|   839|Prg|SAPLSDB4                                |   839|08.11.2019|08:57:15|     9216|K        |
|   840|Prg|CL_EEWA_HTML_VIEWER===========CP        |   840|08.08.2024|10:25:43|    59392|K        |
|   841|Prg|CL_GUI_HTML_VIEWER============CP        |   841|03.05.2025|08:07:04|   174080|K        |
|   842|Prg|CL_ABAP_FORMAT================CP        |   842|03.04.2020|20:51:33|    13312|K        |
|   843|Prg|%_CCNHT                                 |    14|01.06.2015|12:30:30|     8192|K        |
|   844|Prg|SAPLSDC_MIME                            |   844|17.06.2024|13:17:01|  1078272|K        |
|   845|Typ|W3HTML                                  |     0|29.01.1997|19:03:54|     3072|         |
|   846|Prg|SAPLS_NLS_BIDI                          |   846|03.03.2023|16:55:28|    36864|K        |
|   847|Prg|CL_DATAPROVIDER===============CP        |   847|28.05.2018|11:34:26|    53248|K        |
|   848|Prg|CL_TREE_CONTROL_BASE==========CP        |   848|10.11.2022|19:49:46|   134144|K        |
|   849|Typ|TREEV_HHDR                              |     0|28.05.2018|11:38:02|     4096|         |
|   850|Typ|TREEV_LHDR                              |     0|14.02.2002|14:05:11|     3072|         |
|   851|Prg|CL_GUI_TOOLBAR================CP        |   851|10.11.2022|19:49:36|    63488|K        |
|   852|Prg|CL_EEWA_LIST_TREE=============CP        |   852|24.04.2017|18:31:29|    95232|K        |
|   853|Prg|CL_GUI_LIST_TREE==============CP        |   853|10.11.2022|19:49:36|    86016|K        |
|   854|Prg|CL_ITEM_TREE_CONTROL==========CP        |   854|10.11.2022|19:49:37|   124928|K        |
|   855|Prg|IF_EEWA_GUI_TREECONTROL=======IP        |   667|26.02.2015|07:26:09|     5120|K        |
|   856|Typ|EWAEL_TREEV_NODE                        |     0|28.05.2018|11:32:14|     7168|         |
|   857|Typ|EWAEL_TREEV_PARENT                      |     0|28.05.2018|11:32:14|     4096|         |
|   858|Prg|CL_EEWA_WDOC_OBJECT_STATE=====CP        |   858|09.04.2026|00:02:55|   106496|K        |
|   859|Prg|EEWA_BF_WDOC_OBJ_STATE========CP        |   859|21.08.2023|09:18:33|    13312|K        |
|   860|Prg|SAPLBSVA                                |   860|01.06.2015|15:05:09|   701440|K        |
|   861|Typ|JCDO                                    |     0|28.05.2015|10:40:19|     4096|         |
|   862|Typ|ONR00                                   |     0|14.12.1992|11:16:42|     2048|         |
|   863|Typ|JCDS                                    |     0|28.05.2015|10:40:20|     4096|         |
|   864|Typ|TJ01T                                   |     0|31.05.2013|08:15:13|     3072|         |
|   865|Typ|JSTAT                                   |     0|30.11.1993|14:11:44|     2048|         |
|   866|Typ|THEAD                                   |     0|31.05.2013|08:15:00|     8192|         |
|   867|Typ|TLINE                                   |     0|08.03.1992|00:19:59|     3072|         |
|   868|Prg|SAPLBSVX                                |   868|08.09.2011|15:57:51|    95232|K        |
|   869|Prg|ZCL_WR_EEWA_OR_WDORDER========CP        |   869|21.05.2026|18:09:40|    44032|K        |
|   870|Prg|CL_EEWA_OR_WDORDER============CP        |   870|17.06.2026|07:11:42|   157696|K        |
|   871|Typ|EWASWDOC_WDORDER_DATE                   |     0|25.02.2015|23:58:21|     3072|         |
|   872|Prg|CL_EEWA_SQL_MISC==============CP        |   872|06.04.2021|23:23:44|    26624|K        |
|   873|Prg|%_CSYDES                                |    26|01.06.2015|12:30:30|     9216|K        |
|   874|Prg|CL_EEWA_WDORDER_MISC==========CP        |   874|17.06.2026|07:11:56|   177152|K        |
|   875|Typ|EWASWDOC_DATELEVEL                      |     0|25.02.2015|23:58:21|     2048|         |
|   876|Typ|EWASWDOCTREE_WDORDER_INFO               |     0|05.03.2021|19:56:40|     4096|         |
|   877|Typ|EWAROUTE                                |     0|03.05.2025|05:24:20|    16384|         |
|   878|Typ|JSTO_PRE                                |     0|30.11.1993|14:11:48|     2048|         |
|   879|Prg|SAPLAIEV                                |   879|10.11.2022|19:49:53|    14336|K        |
|   880|Prg|SAPLSMSG                                |   880|03.03.2023|14:53:21|   141312|K        |
|   881|Typ|EWASWDOC_STATE                          |     0|25.02.2015|23:58:21|     4096|         |
|   882|Prg|SAPLICON                                |   882|01.06.2015|12:30:31|    54272|K        |
|   883|Typ|ICONT                                   |     0|31.05.2013|08:04:28|     3072|         |
|   884|Typ|TREEV_NODE                              |     0|28.05.2018|11:38:03|     5120|         |
|   885|Typ|EWAEL_PROPVALUE                         |     0|24.02.2005|13:43:36|     3072|         |
|   886|Typ|EWAEL_TREEV_ITEM                        |     0|28.05.2018|11:32:14|     6144|         |
|   887|Typ|EWAEL_TREEV_NODE_INFO                   |     0|02.06.2015|08:58:40|     3072|         |
|   888|Prg|CL_GUI_EVENT==================CP        |   888|28.05.2018|11:34:26|    11264|K        |
|   889|Prg|CL_EEWA_OR_WDORDERWEIGH=======CP        |   889|26.05.2025|11:31:27|    76800|K        |
|   890|Prg|CL_EEWA_OR_WDORDERWEIGH_PSUBCTCP        |   890|09.04.2026|00:02:55|   106496|K        |
|   891|Typ|EWAEL_TREEV_NODE_BASE                   |     0|28.05.2018|11:32:14|     6144|         |
|   892|Prg|ZCL_WR_HELPER_TREE============CP        |   892|17.06.2026|07:14:58|    57344|K        |
|   893|Prg|ZCL_WR_EEWA_OR_WDORDERITEM====CP        |   893|21.05.2026|18:09:40|    48128|K        |
|   894|Prg|CL_EEWA_OR_WDORDERITEM========CP        |   894|09.04.2026|00:02:55|   294912|K        |
|   895|Prg|CL_EEWA_OR_WDORDERITEM_SD=====CP        |   895|17.06.2026|07:11:42|   165888|K        |
|   896|Typ|EWASWDOCTREE_WDORDERITEM_INFO           |     0|25.02.2015|23:58:21|     5120|         |
|   897|Typ|EWA_ORDER_OBJECT_IKEY                   |     0|25.02.2015|23:58:23|     3072|         |
|   898|Prg|CL_EEWA_BO_WDPLANT============CP        |   898|17.06.2026|07:11:42|   372736|K        |
|   899|Prg|CL_EEWA_BO_MARA===============CP        |   899|09.04.2026|00:02:55|   115712|K        |
|   900|Prg|CL_EEWA_BO_PARTNER============CP        |   900|17.06.2026|07:11:56|   270336|K        |
|   901|Typ|EWASWDOCTREE_WDORDERWGH_INFO            |     0|26.05.2025|11:31:15|    25600|         |
|   902|Typ|JSTO                                    |     0|28.05.2015|10:40:20|     3072|         |
|   903|Typ|JEST                                    |     0|28.05.2015|10:40:20|     3072|         |
|   904|Prg|SAPCNVE                                 |   904|26.01.2018|14:57:35|     8192|K        |
|   905|Prg|CL_EEWA_DCFL==================CP        |   905|17.06.2026|07:11:42|   581632|K        |
|   906|Typ|EEWABO_DETAILRECORD                     |     0|26.11.2018|19:13:29|     4096|         |
|   907|Typ|SIBFLPORB                               |     0|15.11.2000|17:53:55|     3072|         |
|   908|Typ|SWOTRK                                  |     0|08.09.2011|14:28:18|     6144|         |
|   909|Prg|SAPLSWOR                                |   909|31.03.2023|15:25:25|   480256|K        |
|   910|Typ|SWOTRETURN                              |     0|31.05.2013|08:13:33|     4096|         |
|   911|Prg|SAPLSWOH                                |   911|01.06.2015|12:30:31|     9216|K        |
|   912|Typ|SWOTENV                                 |     0|08.12.2004|18:00:33|     5120|         |
|   913|Prg|SAPLSWOD                                |   913|16.05.2023|13:23:18|   886784|K        |
|   914|Prg|SAPLRHS0                                |   914|03.03.2023|16:55:28|    26624|K        |
|   915|Typ|TOJTD                                   |     0|08.09.2011|14:24:02|     3072|         |
|   916|Typ|TOJTB                                   |     0|18.08.2015|12:17:42|    10240|         |
|   917|Prg|CL_SWO_KEY_HANDLING===========CP        |   917|26.01.2023|17:38:15|    64512|K        |
|   918|Prg|ZCL_WR_BO_METAGOS_BASE========CP        |   918|17.06.2026|07:14:58|   190464|K        |
|   919|Prg|ZCL_WR_OR_METAGOS_BASE========CP        |   919|14.05.2026|18:01:23|    81920|K        |
|   920|Typ|ZWR_GOS_META_KEY                        |     0|05.09.2025|15:09:16|     2048|         |
|   921|Typ|EWASNODEPATHITEM                        |     0|25.02.2015|23:58:20|     3072|         |
|   922|Prg|CL_EEWA_BO_OBJTRANSACTION=====CP        |   922|05.09.2025|15:09:31|   117760|K        |
|   923|Prg|CL_EEWA_TA_SUBCONTRACTING_SD==CP        |   923|17.06.2026|07:11:56|   253952|K        |
|   924|Typ|EWASOBJTRA_KEY                          |     0|25.02.2015|23:58:20|     2048|         |
|   925|Typ|EWA_EL_OBJECTTRANSACTION_BO             |     0|25.02.2015|23:58:23|    10240|         |
|   926|Typ|TEWA_EL_OBJTRA                          |     0|26.02.2015|01:02:08|     6144|         |
|   927|Typ|TEWA_EL_OBJTRAM                         |     0|26.02.2015|01:02:09|     5120|         |
|   928|Typ|TEWA_EL_OBJTRAT                         |     0|26.02.2015|01:02:09|     3072|         |
|   929|Prg|ZCL_WR_EEWA_TA_SUBCON_SD======CP        |   929|21.05.2026|18:09:40|    88064|K        |
|   930|Prg|EEWA_PLUGIN_TA================CP        |   930|21.08.2023|09:25:42|    15360|K        |
|   931|Prg|CL_EEWA_PLUGINLIST_TA=========CP        |   931|05.09.2025|15:09:31|    22528|K        |
|   932|Prg|/WATP/CL_IM_EWAPLGTA_CACHE_BO=CP        |   932|08.08.2024|10:54:03|     8192|K        |
|   933|Prg|/WATP/CL_IM_EEWA_PLUGIN_TA====CP        |   933|12.02.2025|20:57:09|    21504|K        |
|   934|Prg|IF_EX_EEWA_PLUGIN_TA==========IP        |   681|29.06.2015|14:08:26|     5120|K        |
|   935|Prg|/WATP/CL_EEWA_PLG_TA_CACHE_BO=CP        |   935|12.02.2025|20:55:25|    15360|K        |
|   936|Typ|EWASSUBCNT_CONFIG_COMMON                |     0|07.02.2022|19:48:34|     6144|         |
|   937|Typ|EWASSUBCNT_CONFIG_WDO                   |     0|20.11.2020|06:09:32|     7168|         |
|   938|Typ|EWASSUBCNT_CONFIG_WDOI                  |     0|20.11.2020|06:09:32|     7168|         |
|   939|Typ|EWASSUBCNT_CONFIG_WDOW                  |     0|20.11.2020|06:09:32|     8192|         |
|   940|Typ|EWASSUBCNT_CREA                         |     0|20.11.2020|06:09:32|     3072|         |
|   941|Typ|EWASSUBCONTRACTINGVALLOOKUP             |     0|16.06.2015|08:28:24|     4096|         |
|   942|Typ|EWAS_MATDOC_DATA                        |     0|20.11.2020|06:09:33|     3072|         |
|   943|Typ|EWASSUBCONTR_GM_DATA                    |     0|26.11.2018|19:14:35|     5120|         |
|   944|Typ|EWA_ORDER_HEAD_IKEY                     |     0|25.02.2015|23:58:23|     2048|         |
|   945|Typ|EWA_ORDER_WEIGH_IKEY                    |     0|25.02.2015|23:58:23|     3072|         |
|   946|Typ|EWATSUBCNTCNFSDE                        |     0|05.03.2021|20:17:08|     4096|         |
|   947|Typ|EWASSUBCONTRACTINGCONFIG                |     0|15.06.2015|16:34:46|     6144|         |
|   948|Typ|EWASSUBCONTRACTINGRSLTDATAVAL           |     0|02.06.2015|08:58:41|     3072|         |
|   949|Typ|/WATP/SJSTAT                            |     0|08.08.2024|08:35:52|     3072|         |
|   950|Typ|VBAP                                    |     0|11.12.2025|08:34:38|    99328|         |
|   951|Prg|SAPLV45I                                |   951|21.05.2026|18:09:56|   982016|K        |
|   952|Typ|VBAPVB                                  |     0|11.12.2025|08:33:43|   126976|         |
|   953|Prg|CL_DCFL_UTILITIES_MODE========CP        |   953|31.05.2013|09:53:12|    17408|K        |
|   954|Prg|BADI_DCFL_GET_BF_SWITCH=======CP        |   954|21.08.2023|09:26:35|    15360|K        |
|   955|Prg|BADI_DCFL_SET_DECOUPLING_MODE=CP        |   955|21.08.2023|09:26:35|    13312|K        |
|   956|Prg|CL_DCFL_GET_BF_SWITCH=========CP        |   956|14.09.2011|14:40:16|     8192|K        |
|   957|Prg|IF_DCFL_GET_BF_SWITCH=========IP        |   953|08.09.2011|17:12:43|     5120|K        |
|   958|Prg|CL_FIN_DCFL_SWITCH_CHECK======CP        |   958|14.09.2011|14:40:16|     8192|K        |
|   959|Typ|EWASSUBCONTRACTINGSLSCNTRPKEY           |     0|02.06.2015|08:58:41|     3072|         |
|   960|Typ|EWASWDORDERITEM_LOOKUP                  |     0|20.11.2020|06:09:33|    17408|         |
|   961|Prg|CL_EEWA_BO_ADRC===============CP        |   961|05.09.2025|15:09:31|   159744|K        |
|   962|Typ|ADDR1_SEL                               |     0|31.05.2013|07:56:55|     3072|         |
|   963|Typ|ADDR1_VAL                               |     0|17.06.2024|13:15:42|    20480|         |
|   964|Prg|SAPLSZA0                                |   964|17.06.2024|13:21:41|  3220480|K        |
|   965|Typ|AD1_FLAGS                               |     0|08.09.2011|14:17:07|     4096|         |
|   966|Typ|ADDR1_TEXT                              |     0|08.09.2011|14:25:39|     3072|         |
|   967|Typ|ADDR_ERROR                              |     0|31.05.2013|07:56:57|     4096|         |
|   968|Prg|SAPLSZA12                               |   968|17.06.2024|12:56:02|    25600|K        |
|   969|Typ|ADCOMTYPES                              |     0|08.09.2011|14:25:29|     3072|         |
|   970|Prg|CL_ADDR_TSADRV_MNGR===========CP        |   970|07.04.2016|14:03:16|     8192|K        |
|   971|Prg|CL_ADDR_TSADRV================CP        |   971|08.06.2020|09:46:51|    28672|K        |
|   972|Typ|ADRC                                    |     0|17.06.2024|12:54:39|    24576|         |
|   973|Typ|ADRCT                                   |     0|07.04.2016|14:03:02|     3072|         |
|   974|Typ|ADRP                                    |     0|17.05.2022|14:43:52|    10240|         |
|   975|Typ|ADCP                                    |     0|07.04.2016|14:02:56|     9216|         |
|   976|Typ|ADR2                                    |     0|07.04.2016|14:02:57|     5120|         |
|   977|Typ|ADR3                                    |     0|07.04.2016|14:02:57|     5120|         |
|   978|Typ|ADR4                                    |     0|07.04.2016|14:02:57|     5120|         |
|   979|Typ|ADR5                                    |     0|07.04.2016|14:02:57|     5120|         |
|   980|Typ|ADR6                                    |     0|07.04.2016|14:02:58|     6144|         |
|   981|Typ|ADR7                                    |     0|07.04.2016|14:02:58|     5120|         |
|   982|Typ|ADR8                                    |     0|07.04.2016|14:02:58|    10240|         |
|   983|Typ|ADR9                                    |     0|07.04.2016|14:02:58|     5120|         |
|   984|Typ|ADR10                                   |     0|07.04.2016|14:02:56|     4096|         |
|   985|Typ|ADR11                                   |     0|07.04.2016|14:02:56|     9216|         |
|   986|Typ|ADR12                                   |     0|07.04.2016|14:02:56|     9216|         |
|   987|Typ|ADR13                                   |     0|07.04.2016|14:02:56|     5120|         |
|   988|Typ|ADRT                                    |     0|07.04.2016|14:03:04|     4096|         |
|   989|Typ|ADRU                                    |     0|07.04.2016|14:03:04|     4096|         |
|   990|Typ|ADRCOMC                                 |     0|07.04.2016|14:03:02|     3072|         |
|   991|Typ|ADRV                                    |     0|07.04.2016|14:03:04|     4096|         |
|   992|Typ|ADRVP                                   |     0|07.04.2016|14:03:05|     4096|         |
|   993|Prg|SAPLSZAU                                |   993|17.06.2024|13:22:16|    35840|K        |
|   994|Typ|ADDR_ADDR_PERS_LINE                     |     0|08.09.2011|14:17:04|     3072|         |
|   995|Typ|VADRC                                   |     0|17.06.2024|13:12:09|    24576|         |
|   996|Typ|ADDR_ADDRNUMBER_LINE                    |     0|08.09.2011|14:25:39|     2048|         |
|   997|Prg|SAPLSZA13                               |   997|17.06.2024|13:12:09|   560128|K        |
|   998|Prg|SAPLSZAD                                |   998|17.06.2024|13:17:03|   107520|K        |
|   999|Typ|VADRCT                                  |     0|07.04.2016|14:02:39|     4096|         |
|  1000|Typ|EWAEL_ADDR_STRUCT                       |     0|25.02.2015|23:58:17|     5120|         |
|  1001|Typ|EWASADDRESS                             |     0|17.06.2024|13:24:35|    20480|         |
|  1002|Prg|SAPLESAD                                |  1002|17.06.2026|07:12:09|   217088|K        |
|  1003|Typ|ISU02_ADDRESS                           |     0|08.08.2024|08:36:33|    46080|         |
|  1004|Typ|ISU_REGA                                |     0|26.02.2015|00:03:07|     3072|         |
|  1005|Typ|ISU_REGG                                |     0|24.02.2005|13:45:22|     3072|         |
|  1006|Typ|ISU_REGK                                |     0|26.02.2015|00:03:07|     2048|         |
|  1007|Typ|ISU_REGR                                |     0|26.02.2015|00:03:07|     2048|         |
|  1008|Typ|ISU_REGS                                |     0|24.02.2005|13:45:22|     3072|         |
|  1009|Typ|EKUN_EXT                                |     0|17.06.2026|07:10:06|    52224|         |
|  1010|Prg|SAPLBUPA_SELECT                         |  1010|17.06.2026|07:12:14|  1807360|K        |
|  1011|Prg|SAPLBUPA_INIT_SELECT                    |  1011|29.03.2018|15:54:08|    10240|K        |
|  1012|Typ|BUT000                                  |     0|17.06.2026|07:11:50|    37888|         |
|  1013|Typ|BAPIRET2                                |     0|28.05.2018|11:29:59|     5120|         |
|  1014|Typ|BUP_PARTNERGUID_S                       |     0|28.05.2013|10:37:14|     2048|         |
|  1015|Prg|SAPLOM_UTILITIES                        |  1015|10.11.2022|19:49:53|   185344|K        |
|  1016|Prg|SAPLBUA1                                |  1016|17.06.2026|07:12:14|   377856|K        |
|  1017|Typ|BUT021                                  |     0|08.09.2011|14:19:01|     3072|         |
|  1018|Typ|BUT021_FS                               |     0|08.09.2011|14:25:46|     4096|         |
|  1019|Typ|BUS020_EXT                              |     0|03.05.2025|05:23:26|    20480|         |
|  1020|Typ|BUT020                                  |     0|03.05.2025|05:20:06|     5120|         |
|  1021|Prg|BUPA_ADDR_IMPORT==============CP        |  1021|17.06.2026|07:11:54|    17408|K        |
|  1022|Prg|CL_BUP_PFIELD_STATUS_SET======CP        |  1022|17.12.2018|10:00:33|    22528|K        |
|  1023|Prg|CL_SACF=======================CP        |  1023|26.01.2023|17:38:13|   100352|K        |
|  1024|Typ|TOBJ_CHK_CTRL_RH                        |     0|10.10.2014|18:27:35|     3072|         |
|  1025|Prg|SAPLBUPA_DIALOG_JOEL                    |  1025|17.06.2026|07:12:07|  1686528|K        |
|  1026|Prg|SAPLBUBA_SERV                           |  1026|17.06.2026|07:12:07|   116736|K        |
|  1027|Prg|BUPA_EXTERN_FLAG==============CP        |  1027|21.08.2023|09:59:08|    15360|K        |
|  1028|Prg|FSBP_SPL_IM_CHECKMODE_ONLY====CP        |  1028|16.06.2015|10:06:37|     8192|K        |
|  1029|Prg|IF_EX_BUPA_EXTERN_FLAG========IP        |  1026|08.09.2011|16:05:39|     5120|K        |
|  1030|Prg|CL_BUP_TD_REFERENCE_TIMEZONE==CP        |  1030|10.11.2022|19:49:29|     8192|K        |
|  1031|Typ|BUS_ADDRNUMBER                          |     0|08.09.2011|14:25:47|     2048|         |
|  1032|Prg|SAPLBUBA_5                              |  1032|17.06.2026|07:12:07|   685056|K        |
|  1033|Prg|CL_MD_BP_MAINTAIN=============CP        |  1033|17.06.2026|07:12:01|   378880|K        |
|  1034|Prg|SAPLBUA_CHECK                           |  1034|31.05.2013|08:28:45|   141312|K        |
|  1035|Prg|SAPLBUD1                                |  1035|17.06.2026|07:12:14|  1889280|K        |
|  1036|Prg|BUPA_GENERAL_IMPORT===========CP        |  1036|17.06.2026|07:11:54|    17408|K        |
|  1037|Typ|ADSUSE_UUID                             |     0|31.05.2013|07:57:01|     3072|         |
|  1038|Typ|VADR2                                   |     0|07.04.2016|14:02:38|     6144|         |
|  1039|Typ|ADRU_UPD                                |     0|07.04.2016|13:57:06|     4096|         |
|  1040|Typ|VADRT                                   |     0|07.04.2016|14:02:39|     4096|         |
|  1041|Typ|ADTEL                                   |     0|31.05.2013|07:57:01|     5120|         |
|  1042|Typ|ADSUSE                                  |     0|31.05.2013|07:57:00|     3072|         |
|  1043|Typ|EADRDAT                                 |     0|17.06.2024|13:24:06|    22528|         |
|  1044|Typ|EADRLN                                  |     0|21.01.2014|10:05:08|     4096|         |
|  1045|Prg|SAPLSZAQ                                |  1045|01.06.2015|12:30:31|    16384|K        |
|  1046|Typ|TSAD9                                   |     0|08.09.2011|14:24:50|     2048|         |
|  1047|Prg|SAPLSADR                                |  1047|17.06.2024|13:17:11|   445440|K        |
|  1048|Typ|ADBUPA_TD                               |     0|08.09.2011|14:17:06|     3072|         |
|  1049|Prg|CL_ADDR_MAP===================CP        |  1049|17.06.2024|13:17:10|    76800|K        |
|  1050|Typ|ADDR_VERS                               |     0|31.05.2013|07:56:58|     2048|         |
|  1051|Typ|ADAGROUPS                               |     0|08.09.2011|14:17:07|     2048|         |
|  1052|Typ|ADDRS_VERSION                           |     0|08.08.2024|08:35:50|    17408|         |
|  1053|Typ|ADDRS_PERSON_NAME                       |     0|21.01.2014|10:04:39|     6144|         |
|  1054|Typ|ADDRS_WORKPLACE                         |     0|08.09.2011|14:25:36|     3072|         |
|  1055|Typ|ADDRS_ORGANIZATION_NAME                 |     0|08.09.2011|14:25:39|     4096|         |
|  1056|Typ|ADDRS_LOCATION                          |     0|10.10.2014|18:26:21|    10240|         |
|  1057|Typ|ADDRS_COMMUNICATION_PREFERENCE          |     0|31.05.2013|07:56:56|     3072|         |
|  1058|Prg|CL_ADDR_PRINTFORM_MNGR========CP        |  1058|17.06.2024|13:05:43|     8192|K        |
|  1059|Prg|CL_ADDR_PRINTFORM=============CP        |  1059|17.06.2024|13:05:43|   422912|K        |
|  1060|Prg|CL_ADDRESS_PRINTFORM_CONTEXT==CP        |  1060|03.05.2022|08:42:37|     7168|K        |
|  1061|Typ|T005                                    |     0|20.11.2020|06:48:20|    13312|         |
|  1062|Typ|T005T                                   |     0|27.11.2013|14:27:42|     4096|         |
|  1063|Prg|IF_ADDR_TYPES=================IP        |  1059|08.06.2020|09:46:51|    45056|K        |
|  1064|Typ|TSAD3                                   |     0|16.03.2020|10:25:07|     3072|         |
|  1065|Prg|SAPLSCP5                                |  1065|03.11.2021|19:19:46|    29696|K        |
|  1066|Prg|SAPLOMCV                                |  1066|21.08.2023|09:17:43|    54272|K        |
|  1067|Prg|BADI_MATN1====================CP        |  1067|21.08.2023|09:29:37|    15360|K        |
|  1068|Prg|CL_EX_BADI_MATN1==============CP        |  1068|19.11.2020|17:33:17|    19456|K        |
|  1069|Prg|CX_BADI_NOT_IMPLEMENTED=======CP        |  1069|10.11.2022|19:49:47|    10240|K        |
|  1070|Prg|CX_BADI_NOT_SINGLE_USE========CP        |  1070|10.11.2022|19:49:47|    10240|K        |
|  1071|Prg|CX_BADI=======================CP        |  1071|10.11.2022|19:49:47|    10240|K        |
|  1072|Typ|TMCNV                                   |     0|15.06.2015|16:51:13|     5120|         |
|  1073|Prg|SAPLGERNR                               |  1073|19.10.2015|11:46:52|    14336|K        |
|  1074|Prg|SAPLADCR                                |  1074|16.06.2015|10:25:15|    18432|K        |
|  1075|Prg|SAPLILOX                                |  1075|14.11.2019|04:00:25|   236544|K        |
|  1076|Prg|/PLMB/CL_RFC_CONNECTOR========CP        |  1076|31.05.2013|12:26:05|    22528|K        |
|  1077|Typ|EWASWDORDER_INFO                        |     0|05.09.2025|15:07:47|    23552|         |
|  1078|Typ|EWA_ORDER_HEAD_MAIN                     |     0|08.08.2024|08:36:18|    11264|         |
|  1079|Typ|EWA_ORDER_RESULT_HEAD                   |     0|05.09.2025|15:07:54|    10240|         |
|  1080|Typ|ISU_ADMIN                               |     0|08.09.2011|14:21:07|     3072|         |
|  1081|Typ|EWA_WDOORD_INCL_EEW_PS                  |     0|03.05.2025|05:24:22|     2048|         |
|  1082|Typ|EWASSUBCONTRACTINGDATA                  |     0|20.11.2020|06:09:32|     3072|         |
|  1083|Typ|EWASSUBCONTRACTINGDATAVAL               |     0|13.04.2016|15:02:09|     3072|         |
|  1084|Typ|/WATP/ATPWDOHEAD                        |     0|08.08.2024|08:35:47|     3072|         |
|  1085|Typ|EKPO_KEY                                |     0|13.08.1997|12:31:17|     2048|         |
|  1086|Typ|SALES_ITEM_KEY                          |     0|13.04.2016|15:03:44|     2048|         |
|  1087|Typ|/WATP/STPWDOHEAD                        |     0|08.08.2024|08:35:50|     3072|         |
|  1088|Typ|CI_EWA_ORDER_RESULT_HEAD                |     0|05.09.2025|15:07:35|     3072|         |
|  1089|Typ|EWASSUBCONTRACTINGRESULTDATA            |     0|25.02.2015|23:58:21|     3072|         |
|  1090|Typ|/PLCP/AEWA_ORDER_RESULT_HEAD            |     0|08.08.2024|08:36:18|     4096|         |
|  1091|Typ|ZWR_SEWA_ORDER_RESULT_HEAD              |     0|05.09.2025|15:09:24|     2048|         |
|  1092|Typ|EWASPURCHASECONTRACTPOSKEY              |     0|25.02.2015|23:58:20|     3072|         |
|  1093|Typ|/PLCP/SWDO_RSLT_ENH                     |     0|08.08.2024|08:36:19|     4096|         |
|  1094|Typ|ISU_ADMINL                              |     0|24.02.2005|13:45:13|     3072|         |
|  1095|Prg|CL_GOS_MANAGER================CP        |  1095|01.06.2015|11:34:06|    25600|K        |
|  1096|Prg|CL_GOS_VIEW_MANAGER===========CP        |  1096|26.11.2018|10:52:29|    26624|K        |
|  1097|Typ|SIBFLPOR                                |     0|15.11.2000|17:53:54|     3072|         |
|  1098|Typ|BORIDENT                                |     0|28.08.1997|08:38:50|     3072|         |
|  1099|Prg|SAPLSUU1                                |  1099|17.06.2024|13:06:33|   528384|K        |
|  1100|Prg|CL_SUSR_BASIC_TOOLS===========CP        |  1100|17.06.2026|07:11:43|    79872|K        |
|  1101|Typ|SUSR_S_REF_2_RNG                        |     0|13.01.2010|09:06:27|     2048|         |
|  1102|Typ|SUSR_S_RANGE_4_XUBNAME                  |     0|08.09.2011|14:24:15|     3072|         |
|  1103|Typ|USR02                                   |     0|08.09.2011|14:17:43|    10240|         |
|  1104|Prg|CL_GOS_TOOLBOX_MODEL==========CP        |  1104|26.11.2018|11:02:15|    60416|K        |
|  1105|Typ|SWOTRI                                  |     0|03.01.1996|15:29:13|     3072|         |
|  1106|Prg|CL_EX_GOS_SRV_SELECT==========CP        |  1106|11.04.2001|13:16:27|    15360|K        |
|  1107|Prg|IF_EX_GOS_SRV_SELECT==========IP        |   150|04.12.2000|16:22:18|     6144|K        |
|  1108|Prg|ZCL_IM_ENH_WR_GOS_SELECT======CP        |  1108|05.09.2025|15:10:01|     9216|K        |
|  1109|Prg|CL_IM_FIN_GOS_HTMLGUI=========CP        |  1109|06.06.2015|15:15:57|     9216|K        |
|  1110|Typ|SGOS_MVOBJ                              |     0|24.07.2001|09:18:06|     4096|         |
|  1111|Prg|CL_GOS_PUBLICATION============CP        |  1111|12.06.2008|12:52:27|    18432|K        |
|  1112|Prg|SAPLSGOSHIST                            |  1112|10.11.2022|19:49:54|    76800|K        |
|  1113|Prg|CL_GOS_STARTER================CP        |  1113|01.06.2015|11:34:06|    26624|K        |
|  1114|Prg|CL_SX_PARAM===================CP        |  1114|26.01.2023|15:40:21|    23552|K        |
|  1115|Prg|CL_GUI_GOS_CONTAINER==========CP        |  1115|13.11.2000|16:32:45|    38912|K        |
|  1116|Prg|%_CICON                                 |   882|16.04.2021|19:43:34|   117760|K        |
|  1117|Prg|CL_BCS_VSI_PROFILE============CP        |  1117|26.01.2023|15:40:21|    15360|K        |
|  1118|Typ|EWASGRIDMASKITEMORDERITEM               |     0|09.04.2026|00:02:00|    61440|         |
|  1119|Prg|CL_EEWA_ORDER_FILLUP_RESULT===CP        |  1119|17.06.2026|07:11:42|    48128|K        |
|  1120|Prg|EEWA_BF_WDOC_FILLUP_RESULTS===CP        |  1120|21.08.2023|09:18:33|    14336|K        |
|  1121|Prg|CL_EX_EEWA_BF_WDOC_FILLUP_RESDCP        |  1121|09.04.2026|00:02:55|    34816|K        |
|  1122|Prg|IF_EX_EEWA_BF_WDOC_FILLUP_RES=IP        |  1119|02.06.2015|09:22:50|     8192|K        |
|  1123|Prg|CL_EEWA_CONSTANTS_CONFIRMATIONCP        |  1123|15.11.2022|09:55:25|    12288|K        |
|  1124|Prg|CL_EEWA_CONSTANTS=============CP        |  1124|26.02.2015|07:25:25|     7168|K        |
|  1125|Prg|ZCL_WR_WDOC_FILLUP_CONTCOUNT==CP        |  1125|09.04.2026|00:02:57|    28672|K        |
|  1126|Prg|CL_EEWA_WDOC_FILLUP_CONTCOUNT=CP        |  1126|09.04.2026|00:02:55|    30720|K        |
|  1127|Prg|CL_EEWA_WDOC_FILLUPMETHOD=====CP        |  1127|09.04.2026|00:02:55|    50176|K        |
|  1128|Prg|IF_EEWA_WDOC_FILLUPMETHOD=====IP        |  1121|09.04.2026|00:02:56|    10240|K        |
|  1129|Prg|CL_EEWA_WDOC_FILLUP_CONTTYPE==CP        |  1129|09.04.2026|00:02:55|    31744|K        |
|  1130|Prg|CL_EEWA_WDOC_FILLUP_ORDDURAT==CP        |  1130|17.06.2026|07:11:42|    52224|K        |
|  1131|Prg|CL_EEWA_WDOC_FILLUP_WASTE=====CP        |  1131|09.04.2026|00:02:55|    38912|K        |
|  1132|Prg|ZCL_WR_WDOC_FILLUP_WEIGHTS====CP        |  1132|09.04.2026|00:02:57|    31744|K        |
|  1133|Prg|CL_EEWA_WDOC_FILLUP_WEIGHTS===CP        |  1133|17.06.2026|07:11:42|    53248|K        |
|  1134|Prg|CL_EEWA_BO_MEASUREMENT========CP        |  1134|05.09.2025|15:09:31|    98304|K        |
|  1135|Prg|SAPLSCV0                                |  1135|10.11.2022|19:49:54|   187392|K        |
|  1136|Typ|T006D                                   |     0|09.11.2000|14:23:49|     4096|         |
|  1137|Prg|CL_EEWA_WDOC_FILLUP_REQVERIFY=CP        |  1137|17.06.2026|07:11:42|    72704|K        |
|  1138|Prg|CL_EEWA_ORDER_FILLUP_CONTEXT==CP        |  1138|17.06.2026|07:11:42|   122880|K        |
|  1139|Typ|EWASCOPYFIELDNAME                       |     0|25.02.2015|23:58:19|     3072|         |
|  1140|Typ|EWA_ORDER_RESULT_EMPTYING               |     0|26.02.2026|21:09:11|    17408|         |
|  1141|Typ|EWA_ORDER_RESULT_WEIGH_EXT              |     0|26.05.2025|11:31:19|    20480|         |
|  1142|Prg|CX_SABP_DD_CTLG_INVALID_TYPE==CP        |  1142|26.01.2023|17:38:15|    16384|K        |
|  1143|Prg|CL_DD_RTOBJ_FACTORY===========CP        |  1143|16.05.2023|13:23:57|    86016|K        |
|  1144|Prg|IF_DD_RTOBJ_SPECIAL===========IP        |  1143|17.04.2020|19:46:06|     8192|K        |
|  1145|Prg|CL_DD_DIST_UTILS==============CP        |  1145|16.05.2023|13:23:57|    38912|K        |
|  1146|Typ|DDSUX030L                               |     0|02.05.2016|13:41:41|     7168|         |
|  1147|Prg|CL_DD_RTOBJ_UTILITIES=========CP        |  1147|16.05.2023|13:23:55|    84992|K        |
|  1148|Prg|IF_DD_RTOBJ_TYPES=============IP        |  1143|16.05.2023|13:23:55|    74752|K        |
|  1149|Prg|CL_DD_RTOBJ_TABLE_TYPE========CP        |  1149|16.05.2023|13:23:55|    61440|K        |
|  1150|Prg|CL_DD_RTOBJ_BASE==============CP        |  1150|16.05.2023|13:23:54|    47104|K        |
|  1151|Prg|IF_DD_RTOBJ===================IP        |  1143|16.05.2023|13:23:55|    32768|K        |
|  1152|Prg|CL_SXML_STRING_READER=========CP        |  1152|10.11.2022|19:49:45|    13312|K        |
|  1153|Prg|CL_SXML_READER================CP        |  1153|10.11.2022|19:49:45|    31744|K        |
|  1154|Prg|CL_FX_READER==================CP        |  1154|10.11.2022|19:49:36|    24576|K        |
|  1155|Prg|IF_SXML_READER================IP        |   120|10.11.2022|19:49:51|    11264|K        |
|  1156|Prg|CL_DD_DFIES_PROVIDER_TTYP=====CP        |  1156|16.05.2023|13:23:52|    25600|K        |
|  1157|Prg|IF_DD_TTYP_CONSTANTS==========IP        |    44|29.03.2018|16:37:36|     8192|K        |
|  1158|Typ|LVC_S_REFS                              |     0|02.11.1998|09:45:47|     3072|         |
|  1159|Typ|LVC_S_DEPS                              |     0|04.12.2000|12:48:02|     3072|         |
|  1160|Typ|LVC_S_MOCE                              |     0|28.05.2018|11:34:04|     4096|         |
|  1161|Typ|LVC_S_SCOL                              |     0|31.05.2013|08:05:54|     3072|         |
|  1162|Typ|LVC_S_GRPL                              |     0|28.05.2018|11:34:01|     4096|         |
|  1163|Typ|EWASGRIDMASKITEM                        |     0|20.11.2020|06:09:32|     7168|         |
|  1164|Typ|LVC_S_STYL                              |     0|28.05.2018|11:34:07|     3072|         |
|  1165|Rty|type000000000007                        |     0|17.06.2026|13:31:00|        0|         |
|  1166|Typ|/WATP/AEWAWDOI_INFO                     |     0|24.10.2024|08:59:52|     3072|         |
|  1167|Typ|ZWR_AEWMD_EWAELOCSD_NEW                 |     0|09.04.2026|00:02:44|     3072|         |
|  1168|Typ|EWA_ORDER_OBJECT_KEY                    |     0|25.02.2015|23:58:23|     3072|         |
|  1169|Typ|EWA_ORDER_OBJECT_MAIN                   |     0|20.11.2020|06:09:35|    22528|         |
|  1170|Typ|EWA_WDOITEM_INCL_EEW_PS                 |     0|03.05.2025|05:24:22|     2048|         |
|  1171|Typ|/WATP/AORDERTXT                         |     0|24.10.2024|08:59:52|     3072|         |
|  1172|Typ|/WATP/APRINTINFO                        |     0|08.08.2024|08:35:47|     3072|         |
|  1173|Typ|/WATP/ATPWDOITEM                        |     0|08.08.2024|08:35:47|     5120|         |
|  1174|Typ|/WATP/AWAMM_EWA_ORDER_OBJECT            |     0|08.08.2024|08:35:47|     5120|         |
|  1175|Typ|ZWR_AEEWA_EWA_ORDER_OBJECT              |     0|09.04.2026|00:02:40|     7168|         |
|  1176|Rty|type000000000008                        |     0|17.06.2026|13:25:55|        0|         |
|  1177|Rty|type000000000009                        |     0|17.06.2026|13:31:00|        0|         |
|  1178|Typ|EWA_EHS_DATA                            |     0|25.02.2015|23:58:23|     6144|         |
|  1179|Rty|type000000000010                        |     0|17.06.2026|13:31:00|        0|         |
|  1180|Rty|type000000000011                        |     0|17.06.2026|13:31:00|        0|         |
|  1181|Rty|type000000000012                        |     0|17.06.2026|13:31:00|        0|         |
|  1182|Rty|type000000000013                        |     0|17.06.2026|13:31:00|        0|         |
|  1183|Rty|type000000000014                        |     0|17.06.2026|13:31:00|        0|         |
|  1184|Rty|type000000000015                        |     0|17.06.2026|13:31:00|        0|         |
|  1185|Rty|type000000000016                        |     0|17.06.2026|13:31:00|        0|         |
|  1186|Rty|type000000000017                        |     0|17.06.2026|13:31:00|        0|         |
|  1187|Rty|type000000000018                        |     0|17.06.2026|13:31:00|        0|         |
|  1188|Rty|type000000000019                        |     0|17.06.2026|13:31:00|        0|         |
|  1189|Rty|type000000000020                        |     0|17.06.2026|13:31:00|        0|         |
|  1190|Rty|type000000000021                        |     0|17.06.2026|13:31:00|        0|         |
|  1191|Rty|type000000000022                        |     0|17.06.2026|13:31:00|        0|         |
|  1192|Rty|type000000000023                        |     0|17.06.2026|13:31:00|        0|         |
|  1193|Rty|type000000000024                        |     0|17.06.2026|13:31:00|        0|         |
|  1194|Rty|type000000000025                        |     0|17.06.2026|13:31:00|        0|         |
|  1195|Rty|type000000000026                        |     0|17.06.2026|13:31:00|        0|         |
|  1196|Rty|type000000000027                        |     0|17.06.2026|13:31:00|        0|         |
|  1197|Rty|type000000000028                        |     0|17.06.2026|13:31:00|        0|         |
|  1198|Rty|type000000000029                        |     0|17.06.2026|13:31:00|        0|         |
|  1199|Rty|type000000000030                        |     0|17.06.2026|13:31:00|        0|         |
|  1200|Rty|type000000000031                        |     0|17.06.2026|13:31:00|        0|         |
|  1201|Rty|type000000000032                        |     0|17.06.2026|13:31:00|        0|         |
|  1202|Rty|type000000000033                        |     0|17.06.2026|13:31:00|        0|         |
|  1203|Rty|type000000000034                        |     0|17.06.2026|13:31:00|        0|         |
|  1204|Rty|type000000000035                        |     0|17.06.2026|13:31:00|        0|         |
|  1205|Rty|type000000000036                        |     0|17.06.2026|13:31:00|        0|         |
|  1206|Rty|type000000000037                        |     0|17.06.2026|13:31:00|        0|         |
|  1207|Rty|type000000000038                        |     0|17.06.2026|13:31:00|        0|         |
|  1208|Rty|type000000000039                        |     0|17.06.2026|13:31:00|        0|         |
|  1209|Rty|type000000000040                        |     0|17.06.2026|13:31:00|        0|         |
|  1210|Rty|type000000000041                        |     0|17.06.2026|13:31:00|        0|         |
|  1211|Rty|type000000000042                        |     0|17.06.2026|13:31:00|        0|         |
|  1212|Rty|type000000000043                        |     0|17.06.2026|13:31:00|        0|         |
|  1213|Rty|type000000000044                        |     0|17.06.2026|13:31:00|        0|         |
|  1214|Rty|type000000000045                        |     0|17.06.2026|13:31:00|        0|         |
|  1215|Rty|type000000000046                        |     0|17.06.2026|13:31:00|        0|         |
|  1216|Rty|type000000000047                        |     0|17.06.2026|13:31:00|        0|         |
|  1217|Rty|type000000000048                        |     0|17.06.2026|13:31:00|        0|         |
|  1218|Rty|type000000000049                        |     0|17.06.2026|13:31:00|        0|         |
|  1219|Rty|type000000000050                        |     0|17.06.2026|13:31:00|        0|         |
|  1220|Rty|type000000000051                        |     0|17.06.2026|13:31:00|        0|         |
|  1221|Rty|type000000000052                        |     0|17.06.2026|13:31:00|        0|         |
|  1222|Rty|type000000000053                        |     0|17.06.2026|13:31:00|        0|         |
|  1223|Rty|type000000000054                        |     0|17.06.2026|13:31:00|        0|         |
|  1224|Rty|type000000000055                        |     0|17.06.2026|13:31:00|        0|         |
|  1225|Rty|type000000000056                        |     0|17.06.2026|13:31:00|        0|         |
|  1226|Rty|type000000000057                        |     0|17.06.2026|13:31:00|        0|         |
|  1227|Rty|type000000000058                        |     0|17.06.2026|13:31:00|        0|         |
|  1228|Rty|type000000000059                        |     0|17.06.2026|13:26:13|        0|         |
|  1229|Rty|type000000000060                        |     0|17.06.2026|13:31:00|        0|         |
|  1230|Rty|type000000000061                        |     0|17.06.2026|13:31:00|        0|         |
|  1231|Rty|type000000000062                        |     0|17.06.2026|13:31:00|        0|         |
|  1232|Rty|type000000000063                        |     0|17.06.2026|13:31:00|        0|         |
|  1233|Rty|type000000000064                        |     0|17.06.2026|13:31:00|        0|         |
|  1234|Rty|type000000000065                        |     0|17.06.2026|13:31:00|        0|         |
|  1235|Rty|type000000000066                        |     0|17.06.2026|13:31:00|        0|         |
|  1236|Rty|type000000000067                        |     0|17.06.2026|13:31:00|        0|         |
|  1237|Rty|type000000000068                        |     0|17.06.2026|13:31:00|        0|         |
|  1238|Rty|type000000000069                        |     0|17.06.2026|13:31:00|        0|         |
|  1239|Rty|type000000000070                        |     0|17.06.2026|13:31:00|        0|         |
|  1240|Rty|type000000000071                        |     0|17.06.2026|13:31:00|        0|         |
|  1241|Rty|type000000000072                        |     0|17.06.2026|13:31:00|        0|         |
|  1242|Rty|type000000000073                        |     0|17.06.2026|13:31:00|        0|         |
|  1243|Rty|type000000000074                        |     0|17.06.2026|13:31:00|        0|         |
|  1244|Rty|type000000000075                        |     0|17.06.2026|13:31:00|        0|         |
|  1245|Rty|type000000000076                        |     0|17.06.2026|13:31:00|        0|         |
|  1246|Rty|type000000000077                        |     0|17.06.2026|13:31:00|        0|         |
|  1247|Rty|type000000000078                        |     0|17.06.2026|13:31:00|        0|         |
|  1248|Rty|type000000000079                        |     0|17.06.2026|13:31:00|        0|         |
|  1249|Rty|type000000000080                        |     0|17.06.2026|13:31:00|        0|         |
|  1250|Typ|CI_EWA_ORDER_RESULT_EMPTYING            |     0|26.02.2026|21:08:57|     3072|         |
|  1251|Typ|/PLCP/AEWA_ORDER_RESULT_ITEM            |     0|08.01.2025|16:36:16|     3072|         |
|  1252|Typ|/WATP/AEWA_ORDER_RESULT                 |     0|08.08.2024|08:35:47|     5120|         |
|  1253|Rty|type000000000081                        |     0|17.06.2026|13:31:00|        0|         |
|  1254|Rty|type000000000082                        |     0|17.06.2026|13:31:00|        0|         |
|  1255|Rty|type000000000083                        |     0|17.06.2026|13:31:00|        0|         |
|  1256|Rty|type000000000084                        |     0|17.06.2026|13:31:00|        0|         |
|  1257|Rty|type000000000085                        |     0|17.06.2026|13:31:00|        0|         |
|  1258|Rty|type000000000086                        |     0|17.06.2026|13:31:00|        0|         |
|  1259|Rty|type000000000087                        |     0|17.06.2026|13:31:00|        0|         |
|  1260|Rty|type000000000088                        |     0|17.06.2026|13:31:00|        0|         |
|  1261|Rty|type000000000089                        |     0|17.06.2026|13:31:00|        0|         |
|  1262|Rty|type000000000090                        |     0|17.06.2026|13:31:00|        0|         |
|  1263|Rty|type000000000091                        |     0|17.06.2026|13:31:00|        0|         |
|  1264|Rty|type000000000092                        |     0|17.06.2026|13:31:00|        0|         |
|  1265|Rty|type000000000093                        |     0|17.06.2026|13:31:00|        0|         |
|  1266|Rty|type000000000094                        |     0|17.06.2026|13:31:00|        0|         |
|  1267|Rty|type000000000095                        |     0|17.06.2026|13:31:00|        0|         |
|  1268|Rty|type000000000096                        |     0|17.06.2026|13:31:00|        0|         |
|  1269|Rty|type000000000097                        |     0|17.06.2026|13:31:00|        0|         |
|  1270|Rty|type000000000098                        |     0|17.06.2026|13:31:00|        0|         |
|  1271|Rty|type000000000099                        |     0|17.06.2026|13:31:00|        0|         |
|  1272|Rty|type000000000100                        |     0|17.06.2026|13:31:00|        0|         |
|  1273|Rty|type000000000101                        |     0|17.06.2026|13:31:00|        0|         |
|  1274|Rty|type000000000102                        |     0|17.06.2026|13:31:00|        0|         |
|  1275|Rty|type000000000103                        |     0|17.06.2026|13:31:00|        0|         |
|  1276|Rty|type000000000104                        |     0|17.06.2026|13:31:00|        0|         |
|  1277|Rty|type000000000105                        |     0|17.06.2026|13:31:00|        0|         |
|  1278|Rty|type000000000106                        |     0|17.06.2026|13:31:00|        0|         |
|  1279|Rty|type000000000107                        |     0|17.06.2026|13:31:00|        0|         |
|  1280|Typ|ZWR_SEWA_ORDER_RESULT_ITEM              |     0|26.02.2026|21:09:49|     3072|         |
|  1281|Rty|type000000000108                        |     0|17.06.2026|13:31:00|        0|         |
|  1282|Rty|type000000000109                        |     0|17.06.2026|13:31:00|        0|         |
|  1283|Rty|type000000000110                        |     0|17.06.2026|13:31:00|        0|         |
|  1284|Rty|type000000000111                        |     0|17.06.2026|13:31:00|        0|         |
|  1285|Rty|type000000000112                        |     0|17.06.2026|13:31:00|        0|         |
|  1286|Rty|type000000000113                        |     0|17.06.2026|13:31:00|        0|         |
|  1287|Rty|type000000000114                        |     0|17.06.2026|13:25:55|        0|         |
|  1288|Rty|type000000000115                        |     0|17.06.2026|13:31:43|        0|         |
|  1289|Rty|type000000000116                        |     0|17.06.2026|13:31:43|        0|         |
|  1290|Rty|type000000000117                        |     0|17.06.2026|13:31:43|        0|         |
|  1291|Rty|type000000000118                        |     0|17.06.2026|13:31:43|        0|         |
|  1292|Typ|/PLCP/SWDOI_RSLT_ENH                    |     0|08.01.2025|16:36:20|     3072|         |
|  1293|Rty|type000000000119                        |     0|17.06.2026|13:25:55|        0|         |
|  1294|Typ|/WATP/SAEWA_ORDER_RESULT_AT             |     0|08.08.2024|08:35:50|     3072|         |
|  1295|Rty|type000000000120                        |     0|17.06.2026|13:31:00|        0|         |
|  1296|Rty|type000000000121                        |     0|17.06.2026|13:31:00|        0|         |
|  1297|Rty|type000000000122                        |     0|17.06.2026|13:31:00|        0|         |
|  1298|Rty|type000000000123                        |     0|17.06.2026|13:31:00|        0|         |
|  1299|Rty|type000000000124                        |     0|17.06.2026|13:31:00|        0|         |
|  1300|Rty|type000000000125                        |     0|17.06.2026|13:31:00|        0|         |
|  1301|Rty|type000000000126                        |     0|17.06.2026|13:31:00|        0|         |
|  1302|Rty|type000000000127                        |     0|17.06.2026|13:31:00|        0|         |
|  1303|Rty|type000000000128                        |     0|17.06.2026|13:26:13|        0|         |
|  1304|Typ|/WATP/SORDERTXT                         |     0|24.10.2024|08:59:59|     3072|         |
|  1305|Rty|type000000000129                        |     0|17.06.2026|13:26:13|        0|         |
|  1306|Typ|/WATP/SPRINTINFO                        |     0|08.08.2024|08:35:49|     3072|         |
|  1307|Rty|type000000000130                        |     0|17.06.2026|13:31:43|        0|         |
|  1308|Typ|/WATP/STPWDOITEM                        |     0|08.08.2024|08:35:50|     4096|         |
|  1309|Rty|type000000000131                        |     0|17.06.2026|13:31:43|        0|         |
|  1310|Typ|/WATP/SWAMM_EWA_ORDER_OBJECT            |     0|08.08.2024|08:35:51|     5120|         |
|  1311|Rty|type000000000132                        |     0|17.06.2026|13:26:13|        0|         |
|  1312|Typ|ZWR_SEEWA_EWA_ORDER_OBJECT_EXT          |     0|09.04.2026|00:02:47|     7168|         |
|  1313|Typ|ZWR_SEEWA_CUSTFIELDS_PDSERVICE          |     0|23.10.2025|13:03:21|     2048|         |
|  1314|Typ|EWASADMIN                               |     0|25.02.2015|23:58:19|     4096|         |
|  1315|Rty|type000000000133                        |     0|17.06.2026|13:31:43|        0|         |
|  1316|Rty|type000000000134                        |     0|17.06.2026|13:31:43|        0|         |
|  1317|Rty|type000000000135                        |     0|17.06.2026|13:31:43|        0|         |
|  1318|Rty|type000000000136                        |     0|17.06.2026|13:31:43|        0|         |
|  1319|Rty|type000000000137                        |     0|17.06.2026|13:31:43|        0|         |
|  1320|Rty|type000000000138                        |     0|17.06.2026|13:31:43|        0|         |
|  1321|Rty|type000000000139                        |     0|17.06.2026|13:31:43|        0|         |
|  1322|Rty|type000000000140                        |     0|17.06.2026|13:31:00|        0|         |
|  1323|Rty|type000000000141                        |     0|17.06.2026|13:31:00|        0|         |
|  1324|Rty|type000000000142                        |     0|17.06.2026|13:31:00|        0|         |
|  1325|Rty|type000000000143                        |     0|17.06.2026|13:31:43|        0|         |
|  1326|Rty|type000000000144                        |     0|17.06.2026|13:31:43|        0|         |
|  1327|Rty|type000000000145                        |     0|17.06.2026|13:31:43|        0|         |
|  1328|Rty|type000000000146                        |     0|17.06.2026|13:31:43|        0|         |
|  1329|Rty|type000000000147                        |     0|17.06.2026|13:31:43|        0|         |
|  1330|Rty|type000000000148                        |     0|17.06.2026|13:31:00|        0|         |
|  1331|Rty|type000000000149                        |     0|17.06.2026|13:30:59|        0|         |
|  1332|Rty|type000000000150                        |     0|17.06.2026|13:31:43|        0|         |
|  1333|Rty|type000000000151                        |     0|17.06.2026|13:31:43|        0|         |
|  1334|Rty|type000000000152                        |     0|17.06.2026|13:31:43|        0|         |
|  1335|Rty|type000000000153                        |     0|17.06.2026|13:31:43|        0|         |
|  1336|Rty|type000000000154                        |     0|17.06.2026|13:31:43|        0|         |
|  1337|Rty|type000000000155                        |     0|17.06.2026|13:31:43|        0|         |
|  1338|Rty|type000000000156                        |     0|17.06.2026|13:31:43|        0|         |
|  1339|Rty|type000000000157                        |     0|17.06.2026|13:30:59|        0|         |
|  1340|Rty|type000000000158                        |     0|17.06.2026|13:30:59|        0|         |
|  1341|Rty|type000000000159                        |     0|17.06.2026|13:25:55|        0|         |
|  1342|Prg|CX_SADL_ENTITY_CONSUMP_SERVICECP        |  1342|10.11.2022|19:49:48|    27648|K        |
|  1343|Typ|LVC_S_COLO                              |     0|31.05.2013|08:05:53|     3072|         |
|  1344|Prg|CL_ENH_BADI_RUNTIME_FUNCTIONS=CP        |  1344|28.04.2023|07:26:23|   151552|K        |
|  1345|Prg|CL_BADI_MAIN==================CP        |  1345|10.11.2022|19:49:28|    97280|K        |
|  1346|Typ|BADI_IMPL                               |     0|28.05.2018|11:29:54|     3072|         |
|  1347|Prg|CL_SWITCH=====================CP        |  1347|06.04.2021|13:57:09|    35840|K        |
|  1348|Prg|SAPLPA_PACKAGE_SERVICES                 |  1348|28.04.2023|07:29:40|   295936|K        |
|  1349|Prg|CL_BADI_IMPLEMENTATION========CP        |  1349|10.11.2022|19:49:28|    53248|K        |
|  1350|Prg|CL_R3STANDARD_PERSISTENCE=====CP        |  1350|28.04.2023|07:26:24|   974848|K        |
|  1351|Typ|TADIR                                   |     0|26.01.2018|14:08:30|     6144|         |
|  1352|Typ|GTADIR                                  |     0|08.09.2011|14:20:45|     4096|         |
|  1353|Prg|SAPLSTR9                                |  1353|26.01.2023|17:38:07|    63488|K        |
|  1354|Prg|CONTEXT_X_TR_SYS_PARAMS                 |  1353|25.02.2022|14:55:08|    47104|K        |
|  1355|Typ|TCONT                                   |     0|31.05.2013|08:14:44|     3072|         |
|  1356|Typ|CONTEXTB                                |     0|31.05.2013|07:59:43|     7168|         |
|  1357|Prg|SAPLSCP3                                |  1357|01.06.2015|12:30:31|     9216|K        |
|  1358|Typ|TLOCK_INT                               |     0|02.11.1998|09:51:40|     3072|         |
|  1359|Typ|DDERR                                   |     0|31.05.2013|08:00:31|     4096|         |
|  1360|Typ|OBJH                                    |     0|10.11.2022|16:53:56|     6144|         |
|  1361|Typ|E071                                    |     0|31.05.2013|08:00:54|     4096|         |
|  1362|Typ|TRERR                                   |     0|31.05.2013|08:15:48|     4096|         |
|  1363|Prg|SAPLSCTS_UNICODE                        |  1363|10.11.2022|19:49:54|    11264|K        |
|  1364|Typ|BADI_CHAR_COND                          |     0|28.05.2018|11:29:54|     5120|         |
|  1365|Typ|BADI_STRING_COND                        |     0|28.05.2018|11:29:54|     4096|         |
|  1366|Typ|BADI_INT_COND                           |     0|28.05.2018|11:29:54|     4096|         |
|  1367|Typ|BADI_NUM_COND                           |     0|28.05.2018|11:29:54|     5120|         |
|  1368|Typ|BADI_PACKED_COND                        |     0|28.05.2018|11:29:54|     4096|         |
|  1369|Prg|CL_BADI_DEFINITION============CP        |  1369|10.11.2022|19:49:28|    51200|K        |
|  1370|Prg|CL_SFW_CONFLICT_SWITCH_AUX====CP        |  1370|26.01.2023|17:38:06|    20480|K        |
|  1371|Typ|EWATSUB_WDOISPOS                        |     0|22.01.2021|17:32:21|     5120|         |
|  1372|Typ|EWASBO_DETAILINDEX                      |     0|25.02.2015|23:58:19|     2048|         |
|  1373|Typ|EWAS_SUB_WDOI_DMRPOS_KEY                |     0|22.01.2021|17:14:17|     3072|         |
|  1374|Typ|EWAS_SUB_WDOI_DMRPOS_DATA               |     0|22.01.2021|17:14:17|     4096|         |
|  1375|Rty|type000000000160                        |     0|17.06.2026|13:31:43|        0|         |
|  1376|Typ|EWAS_SUB_DMRPOS_DATA                    |     0|22.01.2021|17:14:17|     4096|         |
|  1377|Rty|type000000000161                        |     0|17.06.2026|13:31:43|        0|         |
|  1378|Rty|type000000000162                        |     0|17.06.2026|13:31:43|        0|         |
|  1379|Typ|EWA_EL_ABAPMETHODDESCRIPTION            |     0|25.02.2015|23:58:23|     4096|         |
|  1380|Typ|EWA_EL_ABAPMETHODPARAMDESC              |     0|28.05.2018|11:32:15|     5120|         |
|  1381|Prg|CL_EEWA_BO_WDORESULTFLDGRP====CP        |  1381|05.09.2025|15:09:31|    82944|K        |
|  1382|Prg|CL_EEWA_BO_WDORESULTTYP=======CP        |  1382|05.09.2025|15:09:31|   100352|K        |
|  1383|Typ|TEWARESULTZ                             |     0|30.01.2017|19:22:57|     5120|         |
|  1384|Typ|EWASGRIDMASKFIELDCATITEM                |     0|25.02.2015|23:58:20|     4096|         |
|  1385|Typ|LVC_S_SORT                              |     0|28.05.2018|11:34:06|     4096|         |
|  1386|Prg|CX_SY_CONVERSION_NO_NUMBER====CP        |  1386|10.11.2022|19:49:48|    10240|K        |
|  1387|Prg|CX_SY_CONVERSION_ERROR========CP        |  1387|10.11.2022|19:49:48|    10240|K        |
|  1388|Prg|SAPLKKBL_EXT                            |  1388|16.12.2022|13:58:47|    21504|K        |
|  1389|Typ|ABAP_SORTORDER                          |     0|13.02.2005|17:31:39|     3072|         |
|  1390|Prg|SAPLEEWA_EL_UNITCONVERSION              |  1390|20.11.2020|09:03:41|    21504|K        |
|  1391|Prg|CL_EEWA_BO_SERVICETYPE========CP        |  1391|05.09.2025|15:09:31|    92160|K        |
|  1392|Typ|TESERVICETYPE                           |     0|01.04.2022|19:32:23|     4096|         |
|  1393|Typ|EWA_EL_WDPLANT                          |     0|07.11.2025|23:02:34|    10240|         |
|  1394|Typ|EWAOBJ                                  |     0|03.05.2025|05:24:19|    19456|         |
|  1395|Typ|MARA                                    |     0|10.11.2025|17:50:47|    67584|         |
|  1396|Typ|IFLOT                                   |     0|15.11.2022|06:07:32|    14336|         |
|  1397|Typ|AUFK                                    |     0|15.11.2022|06:05:22|    24576|         |
|  1398|Typ|EEWA_PROP_H                             |     0|25.02.2015|23:58:00|     5120|         |
|  1399|Typ|T006                                    |     0|31.05.2013|08:13:44|     6144|         |
|  1400|Typ|TEWACONFTYPE                            |     0|06.04.2021|21:38:35|     5120|         |
|  1401|Typ|T001L                                   |     0|20.11.2020|06:48:19|     6144|         |
|  1402|Prg|SAPLMFAW                                |  1402|31.05.2013|09:25:33|    83968|K        |
|  1403|Typ|TFAWY                                   |     0|29.07.1998|19:48:52|     4096|         |
|  1404|Prg|SAPLSCVU                                |  1404|10.11.2022|19:49:54|   195584|K        |
|  1405|Prg|SAPLSDSD                                |  1405|16.05.2023|13:23:46|   501760|K        |
|  1406|Prg|%_CVRM                                  |  1405|01.06.2015|12:30:30|     8192|K        |
|  1407|Typ|SHLP_DESCR                              |     0|26.02.2015|00:11:10|    15360|         |
|  1408|Typ|DDSHOFFLD                               |     0|18.05.2004|14:07:58|     6144|         |
|  1409|Typ|SHLP_INTDS                              |     0|21.01.2014|10:06:03|     6144|         |
|  1410|Typ|DDSHIFACE                               |     0|20.08.1998|10:51:59|     4096|         |
|  1411|Typ|DDSHFPROP                               |     0|08.09.2011|14:26:09|     3072|         |
|  1412|Typ|DDSHSELOPT                              |     0|20.08.1998|10:52:01|     3072|         |
|  1413|Typ|DDSHTEXTSEARCH                          |     0|21.01.2014|10:05:05|     3072|         |
|  1414|Prg|SAPLSUGZ                                |  1414|15.06.2023|07:05:25|    86016|K        |
|  1415|Prg|SDSH_BADI_HANDLER                       |  1405|26.01.2023|17:38:15|    40960|K        |
|  1416|Prg|SDSH_SEARCH_HELP_METADATA=====CP        |  1416|21.08.2023|10:26:48|    17408|K        |
|  1417|Prg|/SCWM/CL_BUPA_SHLP_AUTH_CHECK=CP        |  1417|15.06.2023|07:05:32|    34816|K        |
|  1418|Prg|CL_BUPA_SHLP_AUTH_CHECK=======CP        |  1418|15.06.2023|07:05:32|    65536|K        |
|  1419|Prg|CL_CV_SEARCH_HELP_FILTER======CP        |  1419|05.05.2026|14:22:42|    28672|K        |
|  1420|Prg|CL_DPR_BUPA_SHLP_AUTH_CHECK===CP        |  1420|26.01.2023|17:38:12|    27648|K        |
|  1421|Prg|IF_DSH_EX_SEARCH_HELP_METADATAIP        |  1405|01.06.2015|12:30:31|     9216|K        |
|  1422|Prg|CL_BUP_DATA_CONTROLLER_PURPOSECP        |  1422|15.06.2023|07:05:32|    14336|K        |
|  1423|Prg|CL_FEATURE_TOGGLE=============CP        |  1423|13.07.2023|08:36:34|    91136|K        |
|  1424|Prg|IF_BUP_DATA_CONTROLLER_PURPOSEIP        |  1422|15.06.2023|07:05:32|     6144|K        |
|  1425|Prg|CL_FTGL_CUSTOMIZING===========CP        |  1425|13.07.2023|08:36:34|    99328|K        |
|  1426|Prg|CL_FTGL_DEPRECATION===========CP        |  1426|13.07.2023|08:36:33|    16384|K        |
|  1427|Prg|CX_SY_ITAB_LINE_NOT_FOUND=====CP        |  1427|10.11.2022|19:49:48|    11264|K        |
|  1428|Prg|CX_SY_ITAB_ERROR==============CP        |  1428|10.11.2022|19:49:48|    10240|K        |
|  1429|Prg|CL_FTG_RUNTIME_STATE==========CP        |  1429|03.05.2022|08:42:39|     8192|K        |
|  1430|Prg|CL_FTG========================CP        |  1430|16.05.2023|13:23:47|    20480|K        |
|  1431|Prg|CX_SY_DYN_CALL_ILLEGAL_FUNC===CP        |  1431|10.11.2022|19:49:48|    12288|K        |
|  1432|Prg|CL_DSH_GET_DEFAULT_VALUES=====CP        |  1432|10.11.2022|19:49:34|    12288|K        |
|  1433|Prg|%_CSHLP                                 |  1405|01.06.2015|12:30:30|    16384|K        |
|  1434|Prg|SAPLSDH4                                |  1434|15.06.2023|07:05:35|   509952|K        |
|  1435|Prg|CL_CONFIGURATION_DEPRECATION==CP        |  1435|28.04.2023|07:29:54|    36864|K        |
|  1436|Prg|IF_RTDD_STRUCTURED============IP        |  1435|08.08.2024|10:14:16|    25600|K        |
|  1437|Prg|CL_DSH_F4_UTILITIES===========CP        |  1437|18.04.2025|12:30:23|    33792|K        |
|  1438|Prg|/1BCF41/TESERVICETYPE                   |  1405|05.05.2025|10:33:18|    37888|K        |
|  1439|Prg|SAPLSDH3                                |  1439|16.05.2023|13:23:59|   113664|K        |
|  1440|Prg|CL_DSH_FUZZY_SEARCH_PROCESSOR=CP        |  1440|18.04.2025|12:30:23|   180224|K        |
|  1441|Prg|CL_DSH_CONFIG_HANDLER=========CP        |  1441|03.03.2023|16:55:26|    22528|K        |
|  1442|Prg|CL_SADL_CAPABILITY_SRV_FACTORYCP        |  1442|15.06.2023|07:05:20|    28672|K        |
|  1443|Prg|CL_SADL_QUERY_ENGINE_FACTORY==CP        |  1443|16.05.2023|13:23:44|    21504|K        |
|  1444|Prg|CL_SADL_SQL_QUERY_ENGINE_FCTRYCP        |  1444|16.05.2023|13:23:44|    21504|K        |
|  1445|Prg|CL_SADL_SQL_QUERY_ENGINE======CP        |  1445|15.06.2023|07:05:20|   111616|K        |
|  1446|Prg|CL_SADL_SQL_QUERY_BUILDER=====CP        |  1446|15.06.2023|07:05:20|   231424|K        |
|  1447|Prg|CL_SADL_CAPABILITY_SERVICE====CP        |  1447|16.05.2023|13:23:44|    24576|K        |
|  1448|Prg|IF_SADL_CAPABILITY_SERVICE====IP        |  1447|10.11.2022|19:49:50|     9216|K        |
|  1449|Prg|SDSH_SEARCH_HELP_RESULTS======CP        |  1449|17.06.2026|07:12:11|    23552|K        |
|  1450|Prg|/SAPAPO/CL_LOC_SHLP_AUTH_CHECKCP        |  1450|31.03.2023|21:27:53|    29696|K        |
|  1451|Prg|/SCMB/CL_BP_SHLP_AUTHCHECK_DPPCP        |  1451|27.01.2023|18:43:38|    22528|K        |
|  1452|Prg|CL_MDBPCV_DCP_SRCH_HLP_AUTHCHKCP        |  1452|17.06.2026|07:12:01|    62464|K        |
|  1453|Prg|CL_PCA_SHLP_AUTH_CHECK========CP        |  1453|28.05.2018|12:02:02|    15360|K        |
|  1454|Prg|CL_SHLP_FILTER_OBSOLETE_RES===CP        |  1454|21.08.2023|10:21:01|    27648|K        |
|  1455|Prg|CL_CV_SEARCH_HELP_AUTH========CP        |  1455|21.08.2023|09:33:05|    48128|K        |
|  1456|Prg|CL_SU2X_TOOLS=================CP        |  1456|03.05.2025|07:02:12|   549888|K        |
|  1457|Prg|CL_SUSR_TOOLS_KERNEL==========CP        |  1457|10.11.2022|19:49:44|    29696|K        |
|  1458|Prg|IF_DSH_EX_SEARCH_HELP_RESULTS=IP        |  1405|01.06.2015|12:24:42|    12288|K        |
|  1459|Prg|CL_MDBPCV_DCP_UTIL============CP        |  1459|17.06.2026|07:12:01|    91136|K        |
|  1460|Prg|SHLP_GET_DOMNAME==============CP        |  1460|21.08.2023|10:21:01|    16384|K        |
|  1461|Prg|SHLP_FILTER_OBSOLETE==========CP        |  1461|21.08.2023|10:21:01|    17408|K        |
|  1462|Prg|CL_CO_SHLP_GET_DOMNAME_KOKRS==CP        |  1462|31.01.2018|16:19:28|     9216|K        |
|  1463|Prg|CL_FI_SHLP_GET_DOMNAME_BUKRS==CP        |  1463|31.01.2018|16:32:25|     9216|K        |
|  1464|Prg|CL_FI_SHLP_GET_DOMNAME_FORMAT=CP        |  1464|27.11.2018|00:36:38|     9216|K        |
|  1465|Prg|CL_FKK_SHLP_GET_DOMNAME_FORMATCP        |  1465|14.11.2019|04:33:19|     9216|K        |
|  1466|Prg|CL_PSM_SHLP_GET_DOMNAME_FIKRS=CP        |  1466|31.01.2018|16:48:57|     9216|K        |
|  1467|Prg|IF_SHLP_GET_DOMNAME===========IP        |  1454|31.01.2018|17:19:24|     6144|K        |
|  1468|Prg|SAPLSDF4                                |  1468|16.05.2023|13:23:22|   111616|K        |
|  1469|Prg|SAPLSDDT                                |  1469|16.05.2023|13:23:57|   181248|K        |
|  1470|Typ|DSH_INDX                                |     0|21.01.2014|10:04:47|     6144|         |
|  1471|Prg|CL_DSH_DYNPRO_PROPERTIES======CP        |  1471|26.01.2023|17:38:05|    30720|K        |
|  1472|Prg|CL_DYNPRO=====================CP        |  1472|10.11.2022|19:49:34|    38912|K        |
|  1473|Typ|DYNPVALUE                               |     0|26.02.2001|12:00:36|     2048|         |
|  1474|Typ|DYNP_EL_ID                              |     0|18.05.2004|14:07:47|     3072|         |
|  1475|Prg|SAPLSEXM                                |  1475|15.06.2023|07:05:35|   229376|K        |
|  1476|Typ|EWASWDORDER_LOOKUP                      |     0|05.03.2021|19:56:40|     5120|         |
|  1477|Typ|/WATP/SAUTHORITY_FIELDVALUE             |     0|08.08.2024|08:35:47|     3072|         |
|  1478|Typ|/WATP/SAUTHORITY                        |     0|08.08.2024|08:35:47|    18432|         |
|  1479|Typ|EWASGRIDMASKITEMORDER                   |     0|05.09.2025|15:07:41|    28672|         |
|  1480|Prg|CX_ENH_BADI_NOT_FOUND=========CP        |  1480|08.09.2011|15:38:21|    17408|K        |
|  1481|Prg|CX_ENH_ROOT===================CP        |  1481|10.11.2022|19:49:48|    25600|K        |
|  1482|Typ|BAL_S_SORT                              |     0|30.11.1998|15:52:33|     3072|         |
|  1483|Prg|SAPLSBAL_CNTL                           |  1483|15.06.2023|07:05:35|   294912|K        |
|  1484|Typ|BAL_S_LFIL                              |     0|03.11.2021|18:01:47|    18432|         |
|  1485|Typ|BAL_S_MFIL                              |     0|07.04.2016|13:57:08|    15360|         |
|  1486|Prg|SAPLSBAL_DISPLAY_BASE                   |  1486|15.06.2023|07:05:35|   497664|K        |
|  1487|Typ|BAL_S_IPRF                              |     0|08.11.2019|08:51:37|    99328|         |
|  1488|Typ|BAL_S_SFLD                              |     0|26.02.1999|14:47:03|     3072|         |
|  1489|Prg|CL_BAL_DISPLAY================CP        |  1489|30.01.2017|10:07:20|     8192|K        |
|  1490|Typ|BAL_S_NTAB                              |     0|31.05.2013|07:57:43|     5120|         |
|  1491|Typ|BAL_S_GENR                              |     0|26.02.1999|14:47:01|     3072|         |
|  1492|Typ|BAL_S_COL                               |     0|25.01.1999|12:55:55|     3072|         |
|  1493|Typ|BAL_S_OUTP                              |     0|31.05.2013|07:57:43|     5120|         |
|  1494|Typ|BAL_S_READ                              |     0|03.09.1999|09:48:05|     5120|         |
|  1495|Typ|BAL_S_PUSH                              |     0|04.12.2000|12:45:14|     4096|         |
|  1496|Typ|BAL_S_GRIDTITLE                         |     0|03.03.2023|14:53:20|     3072|         |
|  1497|Typ|BAL_S_DTTM                              |     0|08.09.2011|14:17:15|     3072|         |
|  1498|Typ|BAL_S_COLA                              |     0|26.02.1999|14:47:01|     3072|         |
|  1499|Typ|BAL_S_COLT                              |     0|28.05.2018|11:29:54|     3072|         |
|  1500|Typ|BAL_S_CODE                              |     0|28.05.2018|11:29:54|    12288|         |
|  1501|Typ|BAL_S_SMTY                              |     0|28.05.2018|11:29:54|     9216|         |
|  1502|Typ|BAL_S_LSEL                              |     0|28.05.2018|11:29:54|     4096|         |
|  1503|Typ|BAL_S_CBUC                              |     0|31.05.2013|07:57:42|    13312|         |
|  1504|Typ|BALMSGHNDL                              |     0|02.11.1998|09:43:06|     3072|         |
|  1505|Typ|BAL_S_EPP                               |     0|03.11.2021|18:01:47|     3072|         |
|  1506|Typ|BAL_S_SHOW                              |     0|10.10.2014|18:26:26|    13312|         |
|  1507|Typ|BAL_S_SHOW_COL                          |     0|31.05.2013|07:57:43|     3072|         |
|  1508|Typ|BAL_S_DDAT                              |     0|10.10.2014|18:26:26|    23552|         |
|  1509|Prg|%_T02KOK                                |  1486|17.06.2026|13:26:13|    41984|         |
|  1510|Typ|BAL_S_DMSG                              |     0|31.05.2013|07:57:42|     4096|         |
|  1511|Typ|BAL_S_DTRE                              |     0|14.02.2005|17:05:27|     3072|         |
|  1512|Prg|SAPLSBAL_TXT                            |  1512|10.11.2022|19:49:54|    75776|K        |
|  1513|Prg|SAPLSDTO                                |  1513|04.04.2023|11:22:51|    66560|K        |
|  1514|Typ|TREEV_HDR                               |     0|28.05.2018|11:38:02|     4096|         |
|  1515|Typ|DD07V                                   |     0|10.10.2014|18:27:48|     4096|         |
|  1516|Typ|BAL_S_ITEM                              |     0|28.05.2018|11:29:54|     6144|         |
|  1517|Prg|CL_GUI_COLUMN_TREE============CP        |  1517|10.11.2022|19:49:36|   183296|K        |
|  1518|Prg|CL_SALV_TOOLTIP===============CP        |  1518|18.03.2023|11:03:29|    16384|K        |
|  1519|Prg|CX_SY_STRUCT_COMP_NAME========CP        |  1519|08.11.2019|10:17:24|    12288|K        |
|  1520|Prg|CX_SY_STRUCT_CREATION=========CP        |  1520|08.11.2019|10:17:32|    11264|K        |
|  1521|Prg|CX_SY_TYPE_CREATION===========CP        |  1521|08.11.2019|10:17:42|    10240|K        |
|  1522|Prg|CL_SALV_DATA_DESCR============CP        |  1522|16.05.2023|13:23:39|    37888|K        |
|  1523|Typ|EWA_ORDER_RESOURCE_KEY                  |     0|02.06.2015|08:58:43|     3072|         |
|  1524|Typ|EWA_ORDER_RESOURCE_RESULT               |     0|25.02.2015|23:58:23|     3072|         |
|  1525|Typ|/PLCP/AEWA_ORDER_RESULT_RESS            |     0|08.08.2024|08:36:18|     3072|         |
|  1526|Typ|/WATP/AEWA_ORDER_RESOBJ1                |     0|08.08.2024|08:35:47|     2048|         |
|  1527|Typ|/WATP/ATPRP_PLANTIMES                   |     0|08.08.2024|08:35:47|     3072|         |
|  1528|Typ|EWA_ORDER_RESOURCE_IKEY                 |     0|02.06.2015|08:58:43|     3072|         |
|  1529|Typ|/PLCP/SWDOR_RSLT_ENH                    |     0|08.08.2024|08:36:19|     2048|         |
|  1530|Typ|/WATP/STPRP_PLANTIMES                   |     0|08.08.2024|08:35:50|     3072|         |
|  1531|Typ|EWATSUB_WDOSPOS                         |     0|22.01.2021|17:32:21|     5120|         |
|  1532|Typ|EWAS_SUB_WDO_DMRPOS_KEY                 |     0|22.01.2021|17:14:17|     3072|         |
|  1533|Typ|EWAS_SUB_WDO_DMRPOS_DATA                |     0|22.01.2021|17:14:17|     4096|         |
|  1534|Prg|CX_EEWA_BO_SUBCONTRACTING=====CP        |  1534|22.01.2021|19:36:54|    54272|K        |
|  1535|Typ|T006A                                   |     0|31.05.2013|08:13:44|     3072|         |
|  1536|Typ|EWASORDERDMRPOS_KEY                     |     0|25.02.2015|23:58:20|     3072|         |
|  1537|Typ|EWASORDERDMRPOS_DATA                    |     0|15.03.2020|23:41:25|     4096|         |
|  1538|Typ|TDS_SD_PRODH_UNIV_NODE                  |     0|13.05.2019|15:52:13|     3072|         |
|  1539|Typ|HANDOVER_LOCATION                       |     0|27.11.2013|14:26:10|     2048|         |
|  1540|Typ|HANDOVER_DATETIME                       |     0|27.11.2013|14:26:10|     3072|         |
|  1541|Typ|J_1BSD_TC_DET                           |     0|27.11.2013|14:26:29|     3072|         |
|  1542|Typ|VBAP_STATUS                             |     0|16.05.2021|13:25:39|     7168|         |
|  1543|Typ|TDS_SD_VCM_INTEGRATION                  |     0|20.11.2020|06:31:01|     3072|         |
|  1544|Typ|TDS_SD_LMD_INTEGRATION                  |     0|16.05.2021|13:25:32|     2048|         |
|  1545|Typ|SDSALESDOCITEM_INCL_EEW_PS              |     0|11.12.2025|08:31:40|     8192|         |
|  1546|Typ|ARBERP_S_POQUAN                         |     0|24.04.2017|15:50:11|     3072|         |
|  1547|Typ|MILL_INC_VBAP                           |     0|01.03.2005|10:55:01|     2048|         |
|  1548|Typ|MILL_INC_VBAP_2                         |     0|26.02.2015|00:04:05|     2048|         |
|  1549|Typ|VBAP_ANALYTICS                          |     0|15.03.2020|23:50:27|     8192|         |
|  1550|Typ|VBAP_CRM                                |     0|08.06.2020|14:54:54|     3072|         |
|  1551|Typ|VBAP_EXT_GLO                            |     0|16.03.2020|19:50:07|     2048|         |
|  1552|Typ|TXS_ITEM_INFO_BR                        |     0|06.04.2021|21:33:43|     3072|         |
|  1553|Typ|/CPD/PFP_VBAP_TIMESTMP                  |     0|05.06.2015|20:12:16|     2048|         |
|  1554|Typ|/DMBE/SP_APPEND_OPTIONALITIES           |     0|06.04.2021|21:14:58|     6144|         |
|  1555|Typ|/ILE/TILVBAP                            |     0|14.04.2016|11:21:59|     3072|         |
|  1556|Typ|/OLC/VBAP_APP                           |     0|10.03.2010|10:03:48|     2048|         |
|  1557|Typ|/WATP/AARB_CABN_AVV_VBAP                |     0|08.08.2024|08:35:47|     3072|         |
|  1558|Typ|/WATP/ACS_CABN_ADRC_VBAP                |     0|08.08.2024|08:35:47|     3072|         |
|  1559|Typ|/WATP/AMNB_VBAP                         |     0|08.08.2024|08:35:47|     3072|         |
|  1560|Typ|/WATP/AVBAP                             |     0|08.08.2024|08:35:47|     3072|         |
|  1561|Typ|AD01VBAP                                |     0|30.06.2015|07:59:10|     2048|         |
|  1562|Typ|ADSPC2000                               |     0|30.06.2015|07:59:10|     3072|         |
|  1563|Typ|ADSPC2000_2014                          |     0|30.06.2015|07:59:11|     2048|         |
|  1564|Typ|ARUN_VBAP_APPEND                        |     0|18.05.2020|09:01:26|     3072|         |
|  1565|Typ|DIEDS                                   |     0|30.06.2015|07:59:13|     3072|         |
|  1566|Typ|FERC_VBAP                               |     0|03.07.2006|16:09:06|     2048|         |
|  1567|Typ|FSH_VBAP                                |     0|24.04.2019|10:05:42|     5120|         |
|  1568|Typ|GMADDONVBAP                             |     0|26.02.2015|00:00:03|     3072|         |
|  1569|Typ|ISU_CRM_VBAP                            |     0|17.06.2024|12:53:26|     3072|         |
|  1570|Typ|IUID_VBAP                               |     0|10.03.2010|10:10:16|     2048|         |
|  1571|Typ|J_3GVBAPA                               |     0|30.06.2015|07:59:23|     4096|         |
|  1572|Typ|PRSS_VBAP_APP                           |     0|10.03.2010|10:12:50|     3072|         |
|  1573|Typ|PRS_VBAP_CPILS                          |     0|10.03.2010|10:12:51|     3072|         |
|  1574|Typ|PSM_FMFG_FIELDS_ITEM                    |     0|26.02.2015|00:08:24|     4096|         |
|  1575|Typ|RFM_PSST_VBAP_APPEND                    |     0|13.06.2017|13:01:00|     3072|         |
|  1576|Typ|RRICB_VBAP                              |     0|10.08.2011|11:33:00|     2048|         |
|  1577|Typ|VBAPOLC_APP                             |     0|10.03.2010|10:17:22|     2048|         |
|  1578|Typ|VLC_VBAP_AS                             |     0|30.06.2015|07:59:29|     2048|         |
|  1579|Typ|WRF_VBAP_APPEND                         |     0|26.02.2015|00:15:40|     3072|         |
|  1580|Typ|WTYSC_VBAP_AS                           |     0|26.02.2015|00:15:45|     3072|         |
|  1581|Rty|type000000000163                        |     0|17.06.2026|13:31:00|        0|         |
|  1582|Rty|type000000000164                        |     0|17.06.2026|13:31:00|        0|         |
|  1583|Rty|type000000000165                        |     0|17.06.2026|13:31:00|        0|         |
|  1584|Rty|type000000000166                        |     0|17.06.2026|13:31:00|        0|         |
|  1585|Rty|type000000000167                        |     0|17.06.2026|13:31:00|        0|         |
|  1586|Rty|type000000000168                        |     0|17.06.2026|13:31:00|        0|         |
|  1587|Rty|type000000000169                        |     0|17.06.2026|13:31:00|        0|         |
|  1588|Rty|type000000000170                        |     0|17.06.2026|13:31:00|        0|         |
|  1589|Rty|type000000000171                        |     0|17.06.2026|13:31:00|        0|         |
|  1590|Rty|type000000000172                        |     0|17.06.2026|13:31:00|        0|         |
|  1591|Rty|type000000000173                        |     0|17.06.2026|13:31:00|        0|         |
|  1592|Rty|type000000000174                        |     0|17.06.2026|13:31:00|        0|         |
|  1593|Rty|type000000000175                        |     0|17.06.2026|13:31:00|        0|         |
|  1594|Rty|type000000000176                        |     0|17.06.2026|13:31:00|        0|         |
|  1595|Rty|type000000000177                        |     0|17.06.2026|13:31:00|        0|         |
|  1596|Rty|type000000000178                        |     0|17.06.2026|13:31:00|        0|         |
|  1597|Rty|type000000000179                        |     0|17.06.2026|13:31:00|        0|         |
|  1598|Rty|type000000000180                        |     0|17.06.2026|13:31:00|        0|         |
|  1599|Rty|type000000000181                        |     0|17.06.2026|13:31:00|        0|         |
|  1600|Rty|type000000000182                        |     0|17.06.2026|13:31:00|        0|         |
|  1601|Rty|type000000000183                        |     0|17.06.2026|13:31:00|        0|         |
|  1602|Rty|type000000000184                        |     0|17.06.2026|13:31:00|        0|         |
|  1603|Rty|type000000000185                        |     0|17.06.2026|13:31:00|        0|         |
|  1604|Rty|type000000000186                        |     0|17.06.2026|13:31:00|        0|         |
|  1605|Rty|type000000000187                        |     0|17.06.2026|13:31:00|        0|         |
|  1606|Rty|type000000000188                        |     0|17.06.2026|13:31:00|        0|         |
|  1607|Rty|type000000000189                        |     0|17.06.2026|13:31:00|        0|         |
|  1608|Rty|type000000000190                        |     0|17.06.2026|13:31:00|        0|         |
|  1609|Rty|type000000000191                        |     0|17.06.2026|13:31:00|        0|         |
|  1610|Rty|type000000000192                        |     0|17.06.2026|13:31:00|        0|         |
|  1611|Rty|type000000000193                        |     0|17.06.2026|13:31:00|        0|         |
|  1612|Rty|type000000000194                        |     0|17.06.2026|13:31:00|        0|         |
|  1613|Rty|type000000000195                        |     0|17.06.2026|13:31:00|        0|         |
|  1614|Rty|type000000000196                        |     0|17.06.2026|13:31:00|        0|         |
|  1615|Rty|type000000000197                        |     0|17.06.2026|13:31:00|        0|         |
|  1616|Rty|type000000000198                        |     0|17.06.2026|13:31:00|        0|         |
|  1617|Rty|type000000000199                        |     0|17.06.2026|13:31:00|        0|         |
|  1618|Rty|type000000000200                        |     0|17.06.2026|13:31:00|        0|         |
|  1619|Rty|type000000000201                        |     0|17.06.2026|13:31:00|        0|         |
|  1620|Rty|type000000000202                        |     0|17.06.2026|13:31:00|        0|         |
|  1621|Rty|type000000000203                        |     0|17.06.2026|13:31:00|        0|         |
|  1622|Rty|type000000000204                        |     0|17.06.2026|13:31:00|        0|         |
|  1623|Rty|type000000000205                        |     0|17.06.2026|13:31:00|        0|         |
|  1624|Rty|type000000000206                        |     0|17.06.2026|13:31:00|        0|         |
|  1625|Rty|type000000000207                        |     0|17.06.2026|13:31:00|        0|         |
|  1626|Rty|type000000000208                        |     0|17.06.2026|13:31:00|        0|         |
|  1627|Rty|type000000000209                        |     0|17.06.2026|13:31:00|        0|         |
|  1628|Rty|type000000000210                        |     0|17.06.2026|13:31:00|        0|         |
|  1629|Rty|type000000000211                        |     0|17.06.2026|13:31:00|        0|         |
|  1630|Rty|type000000000212                        |     0|17.06.2026|13:31:00|        0|         |
|  1631|Rty|type000000000213                        |     0|17.06.2026|13:31:00|        0|         |
|  1632|Rty|type000000000214                        |     0|17.06.2026|13:31:00|        0|         |
|  1633|Rty|type000000000215                        |     0|17.06.2026|13:31:00|        0|         |
|  1634|Rty|type000000000216                        |     0|17.06.2026|13:31:00|        0|         |
|  1635|Rty|type000000000217                        |     0|17.06.2026|13:31:00|        0|         |
|  1636|Rty|type000000000218                        |     0|17.06.2026|13:31:00|        0|         |
|  1637|Rty|type000000000219                        |     0|17.06.2026|13:31:00|        0|         |
|  1638|Rty|type000000000220                        |     0|17.06.2026|13:31:00|        0|         |
|  1639|Rty|type000000000221                        |     0|17.06.2026|13:31:00|        0|         |
|  1640|Rty|type000000000222                        |     0|17.06.2026|13:31:00|        0|         |
|  1641|Rty|type000000000223                        |     0|17.06.2026|13:31:00|        0|         |
|  1642|Rty|type000000000224                        |     0|17.06.2026|13:31:00|        0|         |
|  1643|Rty|type000000000225                        |     0|17.06.2026|13:31:00|        0|         |
|  1644|Rty|type000000000226                        |     0|17.06.2026|13:31:00|        0|         |
|  1645|Rty|type000000000227                        |     0|17.06.2026|13:31:00|        0|         |
|  1646|Rty|type000000000228                        |     0|17.06.2026|13:31:00|        0|         |
|  1647|Rty|type000000000229                        |     0|17.06.2026|13:31:00|        0|         |
|  1648|Rty|type000000000230                        |     0|17.06.2026|13:31:00|        0|         |
|  1649|Rty|type000000000231                        |     0|17.06.2026|13:31:00|        0|         |
|  1650|Rty|type000000000232                        |     0|17.06.2026|13:31:00|        0|         |
|  1651|Rty|type000000000233                        |     0|17.06.2026|13:31:00|        0|         |
|  1652|Rty|type000000000234                        |     0|17.06.2026|13:31:00|        0|         |
|  1653|Rty|type000000000235                        |     0|17.06.2026|13:31:00|        0|         |
|  1654|Rty|type000000000236                        |     0|17.06.2026|13:31:00|        0|         |
|  1655|Rty|type000000000237                        |     0|17.06.2026|13:31:00|        0|         |
|  1656|Rty|type000000000238                        |     0|17.06.2026|13:31:00|        0|         |
|  1657|Rty|type000000000239                        |     0|17.06.2026|13:31:00|        0|         |
|  1658|Rty|type000000000240                        |     0|17.06.2026|13:31:00|        0|         |
|  1659|Rty|type000000000241                        |     0|17.06.2026|13:31:00|        0|         |
|  1660|Rty|type000000000242                        |     0|17.06.2026|13:31:00|        0|         |
|  1661|Rty|type000000000243                        |     0|17.06.2026|13:31:00|        0|         |
|  1662|Rty|type000000000244                        |     0|17.06.2026|13:31:00|        0|         |
|  1663|Rty|type000000000245                        |     0|17.06.2026|13:31:00|        0|         |
|  1664|Rty|type000000000246                        |     0|17.06.2026|13:31:00|        0|         |
|  1665|Rty|type000000000247                        |     0|17.06.2026|13:31:00|        0|         |
|  1666|Rty|type000000000248                        |     0|17.06.2026|13:31:00|        0|         |
|  1667|Rty|type000000000249                        |     0|17.06.2026|13:31:00|        0|         |
|  1668|Rty|type000000000250                        |     0|17.06.2026|13:31:00|        0|         |
|  1669|Rty|type000000000251                        |     0|17.06.2026|13:31:00|        0|         |
|  1670|Rty|type000000000252                        |     0|17.06.2026|13:31:00|        0|         |
|  1671|Rty|type000000000253                        |     0|17.06.2026|13:31:00|        0|         |
|  1672|Rty|type000000000254                        |     0|17.06.2026|13:31:00|        0|         |
|  1673|Rty|type000000000255                        |     0|17.06.2026|13:31:00|        0|         |
|  1674|Rty|type000000000256                        |     0|17.06.2026|13:31:00|        0|         |
|  1675|Rty|type000000000257                        |     0|17.06.2026|13:31:00|        0|         |
|  1676|Rty|type000000000258                        |     0|17.06.2026|13:31:00|        0|         |
|  1677|Rty|type000000000259                        |     0|17.06.2026|13:31:00|        0|         |
|  1678|Rty|type000000000260                        |     0|17.06.2026|13:31:00|        0|         |
|  1679|Rty|type000000000261                        |     0|17.06.2026|13:31:00|        0|         |
|  1680|Rty|type000000000262                        |     0|17.06.2026|13:31:00|        0|         |
|  1681|Rty|type000000000263                        |     0|17.06.2026|13:31:00|        0|         |
|  1682|Rty|type000000000264                        |     0|17.06.2026|13:31:00|        0|         |
|  1683|Rty|type000000000265                        |     0|17.06.2026|13:31:00|        0|         |
|  1684|Rty|type000000000266                        |     0|17.06.2026|13:31:00|        0|         |
|  1685|Rty|type000000000267                        |     0|17.06.2026|13:31:00|        0|         |
|  1686|Rty|type000000000268                        |     0|17.06.2026|13:31:00|        0|         |
|  1687|Rty|type000000000269                        |     0|17.06.2026|13:31:00|        0|         |
|  1688|Rty|type000000000270                        |     0|17.06.2026|13:31:00|        0|         |
|  1689|Rty|type000000000271                        |     0|17.06.2026|13:31:00|        0|         |
|  1690|Rty|type000000000272                        |     0|17.06.2026|13:31:00|        0|         |
|  1691|Rty|type000000000273                        |     0|17.06.2026|13:31:00|        0|         |
|  1692|Rty|type000000000274                        |     0|17.06.2026|13:31:00|        0|         |
|  1693|Rty|type000000000275                        |     0|17.06.2026|13:31:00|        0|         |
|  1694|Rty|type000000000276                        |     0|17.06.2026|13:31:00|        0|         |
|  1695|Rty|type000000000277                        |     0|17.06.2026|13:31:00|        0|         |
|  1696|Rty|type000000000278                        |     0|17.06.2026|13:31:00|        0|         |
|  1697|Rty|type000000000279                        |     0|17.06.2026|13:31:00|        0|         |
|  1698|Rty|type000000000280                        |     0|17.06.2026|13:31:00|        0|         |
|  1699|Rty|type000000000281                        |     0|17.06.2026|13:31:00|        0|         |
|  1700|Rty|type000000000282                        |     0|17.06.2026|13:31:00|        0|         |
|  1701|Rty|type000000000283                        |     0|17.06.2026|13:31:00|        0|         |
|  1702|Rty|type000000000284                        |     0|17.06.2026|13:31:00|        0|         |
|  1703|Rty|type000000000285                        |     0|17.06.2026|13:31:00|        0|         |
|  1704|Rty|type000000000286                        |     0|17.06.2026|13:31:00|        0|         |
|  1705|Rty|type000000000287                        |     0|17.06.2026|13:31:00|        0|         |
|  1706|Rty|type000000000288                        |     0|17.06.2026|13:31:00|        0|         |
|  1707|Rty|type000000000289                        |     0|17.06.2026|13:31:00|        0|         |
|  1708|Rty|type000000000290                        |     0|17.06.2026|13:31:00|        0|         |
|  1709|Rty|type000000000291                        |     0|17.06.2026|13:31:00|        0|         |
|  1710|Rty|type000000000292                        |     0|17.06.2026|13:31:00|        0|         |
|  1711|Rty|type000000000293                        |     0|17.06.2026|13:31:00|        0|         |
|  1712|Rty|type000000000294                        |     0|17.06.2026|13:31:00|        0|         |
|  1713|Rty|type000000000295                        |     0|17.06.2026|13:31:00|        0|         |
|  1714|Rty|type000000000296                        |     0|17.06.2026|13:31:00|        0|         |
|  1715|Rty|type000000000297                        |     0|17.06.2026|13:31:00|        0|         |
|  1716|Rty|type000000000298                        |     0|17.06.2026|13:31:00|        0|         |
|  1717|Rty|type000000000299                        |     0|17.06.2026|13:31:00|        0|         |
|  1718|Rty|type000000000300                        |     0|17.06.2026|13:31:00|        0|         |
|  1719|Rty|type000000000301                        |     0|17.06.2026|13:31:00|        0|         |
|  1720|Rty|type000000000302                        |     0|17.06.2026|13:31:00|        0|         |
|  1721|Rty|type000000000303                        |     0|17.06.2026|13:31:00|        0|         |
|  1722|Rty|type000000000304                        |     0|17.06.2026|13:31:00|        0|         |
|  1723|Rty|type000000000305                        |     0|17.06.2026|13:31:00|        0|         |
|  1724|Typ|ZZ1_66AO73KNT6OFYYGF4JH7HJWBZY          |     0|25.11.2025|18:49:00|     2048|         |
|  1725|Typ|ZZ1_6SVLCKWES4DRRZ6GFXDK53J2QA          |     0|16.09.2025|22:10:28|     2048|         |
|  1726|Typ|ZZ1_722Z4MVWMB3K5MFDMFDEICP6TE          |     0|30.09.2025|17:30:30|     2048|         |
|  1727|Typ|ZZ1_CN6GDYWJA3JYKWWJKDYPOFEIDQ          |     0|06.10.2025|16:26:29|     2048|         |
|  1728|Typ|ZZ1_DJNSMOGY4U6ODAHKQZKZEJNFUQ          |     0|16.09.2025|22:10:38|     2048|         |
|  1729|Typ|ZZ1_GKL2TVB2GICKJL4C6LJQGOIJZI          |     0|06.10.2025|16:26:32|     2048|         |
|  1730|Typ|ZZ1_ID25AHJ7GSCXVUCFDR6A3TVMFA          |     0|06.10.2025|16:26:32|     2048|         |
|  1731|Typ|ZZ1_LJYIREVB2A24HBMY3WTSOPSM3U          |     0|16.09.2025|22:10:50|     3072|         |
|  1732|Typ|ZZ1_PBE5C7GL7SWH7NCZFDXUE3ETQQ          |     0|26.09.2025|14:44:18|     2048|         |
|  1733|Typ|ZZ1_PBEDCG3645JKXZE3ZE5NTQBFEI          |     0|06.10.2025|16:26:35|     2048|         |
|  1734|Typ|ZZ1_QSSLFXTH5TNW2P7AY6K3JZVCYM          |     0|16.09.2025|22:10:57|     2048|         |
|  1735|Typ|ZZ1_RNXLRKE7JSCKDQTZURM4ROYPWQ          |     0|16.09.2025|22:10:59|     2048|         |
|  1736|Typ|ZZ1_SCS4CQV3AUPGF36HZD3S2PAOSE          |     0|06.10.2025|16:26:36|     2048|         |
|  1737|Typ|ZZ1_XTSZAMJEUY57J5JXM6NMYVBTWE          |     0|06.10.2025|16:26:40|     2048|         |
|  1738|Typ|ZZ1_YETZLBKFAND7IVIEEVFODY62GM          |     0|11.12.2025|08:34:06|     2048|         |
|  1739|Typ|ZZ1_YNAD2JBWNRQPPE2CZOIGNGKOAU          |     0|16.09.2025|22:11:08|     2048|         |
|  1740|Rty|type000000000306                        |     0|17.06.2026|13:31:00|        0|         |
|  1741|Rty|type000000000307                        |     0|17.06.2026|13:31:00|        0|         |
|  1742|Rty|type000000000308                        |     0|17.06.2026|13:31:00|        0|         |
|  1743|Rty|type000000000309                        |     0|17.06.2026|13:31:00|        0|         |
|  1744|Rty|type000000000310                        |     0|17.06.2026|13:31:00|        0|         |
|  1745|Rty|type000000000311                        |     0|17.06.2026|13:31:00|        0|         |
|  1746|Rty|type000000000312                        |     0|17.06.2026|13:31:00|        0|         |
|  1747|Rty|type000000000313                        |     0|17.06.2026|13:31:00|        0|         |
|  1748|Rty|type000000000314                        |     0|17.06.2026|13:31:00|        0|         |
|  1749|Rty|type000000000315                        |     0|17.06.2026|13:31:00|        0|         |
|  1750|Rty|type000000000316                        |     0|17.06.2026|13:31:00|        0|         |
|  1751|Rty|type000000000317                        |     0|17.06.2026|13:31:00|        0|         |
|  1752|Rty|type000000000318                        |     0|17.06.2026|13:31:00|        0|         |
|  1753|Rty|type000000000319                        |     0|17.06.2026|13:31:00|        0|         |
|  1754|Rty|type000000000320                        |     0|17.06.2026|13:31:00|        0|         |
|  1755|Rty|type000000000321                        |     0|17.06.2026|13:31:00|        0|         |
|  1756|Rty|type000000000322                        |     0|17.06.2026|13:31:00|        0|         |
|  1757|Rty|type000000000323                        |     0|17.06.2026|13:31:00|        0|         |
|  1758|Rty|type000000000324                        |     0|17.06.2026|13:31:00|        0|         |
|  1759|Rty|type000000000325                        |     0|17.06.2026|13:31:00|        0|         |
|  1760|Typ|VBAK_ANA                                |     0|15.03.2020|23:50:27|     5120|         |
|  1761|Typ|VBKD_ANA                                |     0|15.03.2020|23:50:27|     3072|         |
|  1762|Typ|VBPA_SD_ANA                             |     0|13.11.2019|22:14:24|     3072|         |
|  1763|Rty|type000000000326                        |     0|17.06.2026|13:31:00|        0|         |
|  1764|Rty|type000000000327                        |     0|17.06.2026|13:31:00|        0|         |
|  1765|Typ|VBPA_ANA                                |     0|01.03.2019|13:58:32|     3072|         |
|  1766|Rty|type000000000328                        |     0|17.06.2026|13:31:00|        0|         |
|  1767|Rty|type000000000329                        |     0|17.06.2026|13:31:00|        0|         |
|  1768|Rty|type000000000330                        |     0|17.06.2026|13:31:00|        0|         |
|  1769|Rty|type000000000331                        |     0|17.06.2026|13:31:00|        0|         |
|  1770|Rty|type000000000332                        |     0|17.06.2026|13:31:00|        0|         |
|  1771|Rty|type000000000333                        |     0|17.06.2026|13:31:00|        0|         |
|  1772|Typ|/DMBE/SP_EXT_FOR_OPTIONALITIES          |     0|06.04.2021|21:14:58|     3072|         |
|  1773|Typ|/DMBE/SP_PAYMENT_DEEMED                 |     0|06.04.2021|21:14:59|     2048|         |
|  1774|Typ|/DMBE/SM_CROSS_CTRITEM_FIELD            |     0|06.04.2021|21:14:57|     4096|         |
|  1775|Rty|type000000000334                        |     0|17.06.2026|13:31:00|        0|         |
|  1776|Rty|type000000000335                        |     0|17.06.2026|13:31:00|        0|         |
|  1777|Rty|type000000000336                        |     0|17.06.2026|13:31:00|        0|         |
|  1778|Rty|type000000000337                        |     0|17.06.2026|13:31:00|        0|         |
|  1779|Rty|type000000000338                        |     0|17.06.2026|13:31:00|        0|         |
|  1780|Rty|type000000000339                        |     0|17.06.2026|13:31:00|        0|         |
|  1781|Rty|type000000000340                        |     0|17.06.2026|13:31:00|        0|         |
|  1782|Rty|type000000000341                        |     0|17.06.2026|13:31:00|        0|         |
|  1783|Rty|type000000000342                        |     0|17.06.2026|13:31:00|        0|         |
|  1784|Typ|/WATP/SARB_CABN_AVV_VBAP                |     0|08.08.2024|08:35:53|     2048|         |
|  1785|Rty|type000000000343                        |     0|17.06.2026|13:31:00|        0|         |
|  1786|Typ|/WATP/SCS_CABN_ADRC_VBAP                |     0|08.08.2024|08:35:50|     2048|         |
|  1787|Rty|type000000000344                        |     0|17.06.2026|13:31:00|        0|         |
|  1788|Typ|/WATP/SMNB_VBAP_ENH                     |     0|08.08.2024|08:35:53|     3072|         |
|  1789|Rty|type000000000345                        |     0|17.06.2026|13:31:00|        0|         |
|  1790|Typ|/WATP/SOBJECTINFOSAP                    |     0|08.08.2024|08:35:48|     2048|         |
|  1791|Rty|type000000000346                        |     0|17.06.2026|13:31:00|        0|         |
|  1792|Rty|type000000000347                        |     0|17.06.2026|13:31:00|        0|         |
|  1793|Typ|ADSPCFIELDS                             |     0|01.03.2005|10:53:39|     3072|         |
|  1794|Rty|type000000000348                        |     0|17.06.2026|13:31:00|        0|         |
|  1795|Rty|type000000000349                        |     0|17.06.2026|13:31:00|        0|         |
|  1796|Rty|type000000000350                        |     0|17.06.2026|13:31:00|        0|         |
|  1797|Rty|type000000000351                        |     0|17.06.2026|13:31:00|        0|         |
|  1798|Rty|type000000000352                        |     0|17.06.2026|13:31:00|        0|         |
|  1799|Typ|FSH_VAS_VBAPDATA                        |     0|29.06.2015|13:13:26|     3072|         |
|  1800|Rty|type000000000353                        |     0|17.06.2026|13:31:00|        0|         |
|  1801|Rty|type000000000354                        |     0|17.06.2026|13:31:00|        0|         |
|  1802|Rty|type000000000355                        |     0|17.06.2026|13:31:00|        0|         |
|  1803|Rty|type000000000356                        |     0|17.06.2026|13:31:00|        0|         |
|  1804|Rty|type000000000357                        |     0|17.06.2026|13:31:00|        0|         |
|  1805|Rty|type000000000358                        |     0|17.06.2026|13:31:00|        0|         |
|  1806|Rty|type000000000359                        |     0|17.06.2026|13:31:00|        0|         |
|  1807|Rty|type000000000360                        |     0|17.06.2026|13:31:00|        0|         |
|  1808|Rty|type000000000361                        |     0|17.06.2026|13:31:00|        0|         |
|  1809|Rty|type000000000362                        |     0|17.06.2026|13:31:00|        0|         |
|  1810|Rty|type000000000363                        |     0|17.06.2026|13:31:00|        0|         |
|  1811|Rty|type000000000364                        |     0|17.06.2026|13:31:00|        0|         |
|  1812|Rty|type000000000365                        |     0|17.06.2026|13:31:00|        0|         |
|  1813|Rty|type000000000366                        |     0|17.06.2026|13:31:00|        0|         |
|  1814|Rty|type000000000367                        |     0|17.06.2026|13:31:00|        0|         |
|  1815|Typ|J_3GVBAPS                               |     0|01.03.2005|10:54:51|     3072|         |
|  1816|Rty|type000000000368                        |     0|17.06.2026|13:31:00|        0|         |
|  1817|Typ|PRSS_VBAP_EXT                           |     0|10.03.2010|10:12:50|     2048|         |
|  1818|Rty|type000000000369                        |     0|17.06.2026|13:31:00|        0|         |
|  1819|Rty|type000000000370                        |     0|17.06.2026|13:31:00|        0|         |
|  1820|Rty|type000000000371                        |     0|17.06.2026|13:31:00|        0|         |
|  1821|Rty|type000000000372                        |     0|17.06.2026|13:31:00|        0|         |
|  1822|Rty|type000000000373                        |     0|17.06.2026|13:31:00|        0|         |
|  1823|Rty|type000000000374                        |     0|17.06.2026|13:31:00|        0|         |
|  1824|Rty|type000000000375                        |     0|17.06.2026|13:31:00|        0|         |
|  1825|Rty|type000000000376                        |     0|17.06.2026|13:31:00|        0|         |
|  1826|Typ|WTYSC_S_CLAIMITEM_RESB                  |     0|26.02.2015|00:15:45|     3072|         |
|  1827|Typ|WTYSC_S_RESB                            |     0|26.02.2015|00:15:45|     2048|         |
|  1828|Typ|WTYSC_S_CLMITEM                         |     0|26.02.2015|00:15:45|     2048|         |
|  1829|Rty|type000000000377                        |     0|17.06.2026|13:31:00|        0|         |
|  1830|Rty|type000000000378                        |     0|17.06.2026|13:31:00|        0|         |
|  1831|Rty|type000000000379                        |     0|17.06.2026|13:31:00|        0|         |
|  1832|Rty|type000000000380                        |     0|17.06.2026|13:31:00|        0|         |
|  1833|Prg|/1BCDWBEN/SAPLEEN0000                   |  1833|05.09.2025|15:10:17|   266240|K        |
|  1834|Prg|SAPLSENA                                |  1834|16.05.2023|13:23:57|    60416|K        |
|  1835|Prg|CL_RLFW_SERVER_GROUP==========CP        |  1835|03.03.2023|16:55:44|    32768|K        |
|  1836|Typ|TEWA_EL_WDPL_CST                        |     0|28.02.2022|11:45:40|     2048|         |
|  1837|Typ|EWASWDORDERWEIGH_LOOKUP                 |     0|16.06.2015|08:28:24|     5120|         |
|  1838|Typ|EWAEL_WDPLANT_SCR                       |     0|15.01.2020|11:28:50|     3072|         |
|  1839|Typ|EWASEL_WDPLFRMW_DET                     |     0|25.02.2015|23:58:19|     5120|         |
|  1840|Typ|EWAS_EL_WDPLFRMAF_DET                   |     0|15.11.2022|05:43:30|     5120|         |
|  1841|Typ|EWASEL_WDPLACHST_DET                    |     0|30.01.2017|18:52:31|     5120|         |
|  1842|Prg|CL_EX_EEWAEL_BA_WDPLANT=======CP        |  1842|26.02.2015|05:41:06|    16384|K        |
|  1843|Prg|IF_EX_EEWAEL_BA_WDPLANT=======IP        |    24|26.02.2015|05:43:32|     5120|K        |
|  1844|Typ|EWASGRIDMASKITEMORDERWEIGH              |     0|26.05.2025|11:31:14|    31744|         |
|  1845|Prg|IF_DD_TABL_CONSTANTS==========IP        |    46|31.03.2023|13:47:28|     9216|K        |
|  1846|Typ|EWA_ORDER_WEIGH_KEY                     |     0|25.02.2015|23:58:23|     3072|         |
|  1847|Typ|EWA_ORDERWEIGH_INCL_EEW_PS              |     0|03.05.2025|05:24:22|     2048|         |
|  1848|Typ|EWA_ORDER_RESULT_WEIGH                  |     0|26.05.2025|11:31:17|    17408|         |
|  1849|Typ|EWA_ORDER_RESULT_WEIGH_WN               |     0|02.06.2015|08:58:43|     4096|         |
|  1850|Typ|EWA_ORDER_WEIGH_COMMON                  |     0|26.05.2025|11:31:20|    10240|         |
|  1851|Typ|/PLCP/AEWA_ORDER_RESULT_WEIGH           |     0|08.08.2024|08:36:18|     3072|         |
|  1852|Typ|/WATP/AEWA_ORDER_RESULT_WEIGH           |     0|08.08.2024|08:35:47|     5120|         |
|  1853|Typ|/WATP/AWAMM_EWA_ORD_RES_WEIGH           |     0|08.08.2024|08:35:47|     6144|         |
|  1854|Typ|CI_EWA_ORDER_RESULT_WEIGH               |     0|26.05.2025|11:31:12|     3072|         |
|  1855|Typ|EEWAS_PLANTLG_DST                       |     0|28.02.2022|11:34:09|     3072|         |
|  1856|Rty|type000000000381                        |     0|17.06.2026|13:31:43|        0|         |
|  1857|Rty|type000000000382                        |     0|17.06.2026|13:31:43|        0|         |
|  1858|Rty|type000000000383                        |     0|17.06.2026|13:31:43|        0|         |
|  1859|Rty|type000000000384                        |     0|17.06.2026|13:31:43|        0|         |
|  1860|Rty|type000000000385                        |     0|17.06.2026|13:31:43|        0|         |
|  1861|Rty|type000000000386                        |     0|17.06.2026|13:31:43|        0|         |
|  1862|Rty|type000000000387                        |     0|17.06.2026|13:31:43|        0|         |
|  1863|Rty|type000000000388                        |     0|17.06.2026|13:31:43|        0|         |
|  1864|Rty|type000000000389                        |     0|17.06.2026|13:31:43|        0|         |
|  1865|Typ|ZWR_SWAA_MWGH_ITEM_EXT                  |     0|26.05.2025|11:31:26|     3072|         |
|  1866|Rty|type000000000390                        |     0|17.06.2026|13:31:43|        0|         |
|  1867|Rty|type000000000391                        |     0|17.06.2026|13:31:43|        0|         |
|  1868|Typ|/PLCP/SWDOW_RSLT_ENH                    |     0|08.08.2024|08:36:19|     2048|         |
|  1869|Rty|type000000000392                        |     0|17.06.2026|13:31:43|        0|         |
|  1870|Typ|/WATP/SEWA_ORDER_RESULT_WEIGH           |     0|08.08.2024|08:35:51|     4096|         |
|  1871|Rty|type000000000393                        |     0|17.06.2026|13:31:43|        0|         |
|  1872|Typ|/WATP/SWAMM_EWA_ORD_RES_WEIGH           |     0|08.08.2024|08:35:51|     5120|         |
|  1873|Rty|type000000000394                        |     0|17.06.2026|13:31:43|        0|         |
|  1874|Rty|type000000000395                        |     0|17.06.2026|13:31:43|        0|         |
|  1875|Rty|type000000000396                        |     0|17.06.2026|13:31:43|        0|         |
|  1876|Rty|type000000000397                        |     0|17.06.2026|13:31:43|        0|         |
|  1877|Rty|type000000000398                        |     0|17.06.2026|13:31:43|        0|         |
|  1878|Rty|type000000000399                        |     0|17.06.2026|13:31:43|        0|         |
|  1879|Rty|type000000000400                        |     0|17.06.2026|13:31:43|        0|         |
|  1880|Rty|type000000000401                        |     0|17.06.2026|13:31:43|        0|         |
|  1881|Rty|type000000000402                        |     0|17.06.2026|13:31:43|        0|         |
|  1882|Rty|type000000000403                        |     0|17.06.2026|13:31:43|        0|         |
|  1883|Rty|type000000000404                        |     0|17.06.2026|13:31:43|        0|         |
|  1884|Rty|type000000000405                        |     0|17.06.2026|13:31:43|        0|         |
|  1885|Rty|type000000000406                        |     0|17.06.2026|13:31:43|        0|         |
|  1886|Rty|type000000000407                        |     0|17.06.2026|13:31:43|        0|         |
|  1887|Rty|type000000000408                        |     0|17.06.2026|13:31:43|        0|         |
|  1888|Typ|EWATSUB_WDOWSPOS                        |     0|22.01.2021|17:32:21|     5120|         |
|  1889|Typ|EWAS_SUB_WDOW_DMRPOS_KEY                |     0|22.01.2021|17:14:17|     3072|         |
|  1890|Typ|EWAS_SUB_WDOW_DMRPOS_DATA               |     0|22.01.2021|17:14:17|     4096|         |
|  1891|Rty|type000000000409                        |     0|17.06.2026|13:31:44|        0|         |
|  1892|Rty|type000000000410                        |     0|17.06.2026|13:31:44|        0|         |
|  1893|Rty|type000000000411                        |     0|17.06.2026|13:31:44|        0|         |
|  1894|Prg|CL_EEWA_BO_ROUTE==============CP        |  1894|05.05.2026|18:05:14|   150528|K        |
|  1895|Prg|EEWA_BA_ROUTE=================CP        |  1895|21.08.2023|09:27:05|    14336|K        |
|  1896|Typ|EWASWDORDERITEM_INFO                    |     0|09.04.2026|00:02:05|    71680|         |
|  1897|Typ|EWASSUBCONTRACTINGLOOKUP                |     0|16.06.2015|08:28:24|     3072|         |
|  1898|Typ|PRODH_DISP                              |     0|31.05.2013|08:08:32|     3072|         |
|  1899|Prg|SAPLV81A                                |  1899|10.11.2025|17:51:41|   120832|K        |
|  1900|Prg|CL_SD_PRODH_FACTORY===========CP        |  1900|11.12.2025|08:34:53|    10240|K        |
|  1901|Prg|CL_SD_PRODH_ERP===============CP        |  1901|30.09.2025|17:30:47|    88064|K        |
|  1902|Prg|SAPLSDDM                                |  1902|16.05.2023|13:23:18|    23552|K        |
|  1903|Prg|CL_DD_MAP_TYPES===============CP        |  1903|16.05.2023|13:23:17|    23552|K        |
|  1904|Typ|TDS_SD_PRODH_INFO                       |     0|13.05.2019|15:52:13|    11264|         |
|  1905|Typ|PRODHS                                  |     0|04.04.1995|16:05:58|     3072|         |
|  1906|Prg|IF_DATAPTABLECACHE_MODIFY=====IP        |   272|28.05.2018|11:34:27|     6144|K        |
|  1907|Prg|IF_DATAPTABLECACHE_MODIFY2====IP        |   216|01.06.2015|12:30:31|     6144|K        |
|  1908|Typ|T001W                                   |     0|31.01.2018|14:32:04|    14336|         |
|  1909|Typ|T301                                    |     0|31.05.2013|08:13:49|     3072|         |
|  1910|Typ|LAGP                                    |     0|07.01.2002|18:03:54|    10240|         |
|  1911|Prg|CL_DD_SOBJECT_FACTORY=========CP        |  1911|16.05.2023|13:04:57|    20480|K        |
|  1912|Prg|CL_DD_SOBJECT=================CP        |  1912|18.04.2025|12:30:23|   788480|K        |
|  1913|Prg|IF_DD_SOBJECT_CONSTANTS=======IP        |  1912|19.11.2020|11:24:13|    11264|K        |
|  1914|Typ|DD02B                                   |     0|15.03.2020|19:32:50|     6144|         |
|  1915|Prg|RADBTDDF1                               |   250|16.05.2023|13:06:47|   393216|K        |
|  1916|Typ|DD02BV                                  |     0|15.03.2020|19:31:19|     6144|         |
|  1917|Prg|RADBONDUT                               |  1912|16.05.2023|13:06:47|   193536|K        |
|  1918|Typ|DD02BND                                 |     0|18.03.2023|10:51:03|     9216|         |
|  1919|Typ|DD02BNDV                                |     0|18.03.2023|10:50:13|    10240|         |
|  1920|Typ|DD03ND                                  |     0|10.11.2022|16:53:47|    11264|         |
|  1921|Typ|DD03NE                                  |     0|05.04.2022|09:47:35|    20480|         |
|  1922|Typ|DD03NP                                  |     0|05.04.2022|09:47:35|    19456|         |
|  1923|Typ|DDTABNAME                               |     0|20.08.1998|10:52:03|     2048|         |
|  1924|Typ|DD12B                                   |     0|10.10.2014|18:26:35|     3072|         |
|  1925|Typ|DD12BV                                  |     0|10.10.2014|18:26:35|     3072|         |
|  1926|Typ|DD08B                                   |     0|10.11.2022|16:41:20|     9216|         |
|  1927|Typ|DD08BV                                  |     0|04.04.2023|11:21:44|    10240|         |
|  1928|Typ|DCOPCONTRL                              |     0|01.06.2015|10:53:25|     5120|         |
|  1929|Typ|DD10B                                   |     0|10.11.2022|16:53:47|     6144|         |
|  1930|Typ|DD10BV                                  |     0|16.12.2022|11:15:02|     6144|         |
|  1931|Typ|DD09B                                   |     0|30.03.2016|07:32:01|     4096|         |
|  1932|Typ|DD09BV                                  |     0|30.03.2016|07:29:15|     4096|         |
|  1933|Typ|DVAL                                    |     0|04.04.1995|15:48:07|     3072|         |
|  1934|Typ|SEAHLPRES                               |     0|21.01.2014|10:06:00|     5120|         |
|  1935|Typ|DDF4LBBUF1                              |     0|18.05.2004|14:08:00|     6144|         |
|  1936|Prg|RADBTNA1                                |  1468|16.05.2023|13:23:10|    60416|K        |
|  1937|Typ|DSH_INDX_TA_DYNPRO                      |     0|04.08.2014|15:41:54|     6144|         |
|  1938|Typ|SDSH_V_FIELD_TA1                        |     0|25.03.2019|14:08:30|     4096|         |
|  1939|Typ|SDSH_V_DOMAIN_FX                        |     0|25.03.2019|14:08:30|     3072|         |
|  1940|Typ|SDSH_V_ENTITYTAB                        |     0|25.03.2019|14:08:30|     3072|         |
|  1941|Typ|DSH_INDX_TA_DDIC                        |     0|04.08.2014|15:41:54|     6144|         |
|  1942|Prg|SAPLSPO1                                |  1942|16.05.2023|13:23:19|    73728|K        |
|  1943|Typ|SPAR                                    |     0|27.08.1994|17:26:58|     3072|         |
|  1944|Prg|SAPLCSTR                                |  1944|26.01.2023|17:38:15|    77824|K        |
|  1945|Prg|SAPMSHLP                                |  1945|25.02.2022|14:55:09|    64512|K        |
|  1946|Typ|PHELP                                   |     0|26.02.2015|00:07:28|    11264|         |
|  1947|Typ|PHELP_PBAG                              |     0|26.02.2001|12:00:48|     3072|         |
|  1948|Typ|DSELC                                   |     0|20.08.1998|11:16:48|     3072|         |
|  1949|Typ|HELP_INFO                               |     0|28.05.2018|11:32:33|    13312|         |
|  1950|Typ|RSMDY                                   |     0|14.02.1998|09:39:27|     4096|         |
|  1951|Prg|SAPLSHL2                                |  1951|15.06.2023|07:05:19|  1007616|K        |
|  1952|Typ|USRBF2                                  |     0|31.05.2013|08:16:38|     3072|         |
|  1953|Prg|CL_SUSR_AUTH_TOOLS============CP        |  1953|26.01.2023|17:38:07|   174080|K        |
|  1954|Typ|DTELINFO                                |     0|25.02.2015|23:57:25|     8192|         |
|  1955|Typ|DTYPINFO                                |     0|25.02.2015|23:57:26|     6144|         |
|  1956|Prg|SAPLSCHL                                |  1956|10.11.2022|19:48:11|   331776|K        |
|  1957|Typ|OBJS                                    |     0|20.08.1998|11:36:58|     4096|         |
|  1958|Typ|DYNPRO_ID                               |     0|09.11.2000|14:07:21|     3072|         |
|  1959|Typ|DPOV610                                 |     0|18.12.2001|16:52:52|     4096|         |
|  1960|Typ|DYNPREAD                                |     0|31.05.2013|08:00:52|     4096|         |
|  1961|Typ|STRUCINFO                               |     0|26.02.2015|00:11:21|     8192|         |
|  1962|Prg|CX_SABP_DD_CATALOG_IO_NOT_FND=CP        |  1962|10.11.2022|19:49:48|    13312|K        |
|  1963|Prg|CX_SABP_DD_CATALOG_IO_ERROR===CP        |  1963|10.11.2022|19:49:48|    12288|K        |
|  1964|Prg|CX_DD_RTOBJ_NOT_FOUND=========CP        |  1964|16.05.2023|13:23:55|    56320|K        |
|  1965|Prg|CX_DD_RTOBJ===================CP        |  1965|16.05.2023|13:23:55|    60416|K        |
|  1966|Prg|CX_DD_STATIC_CHECK============CP        |  1966|03.03.2023|16:55:26|    20480|K        |
|  1967|Prg|CX_SY_ASSIGN_CAST_ILLEGAL_CASTCP        |  1967|10.11.2022|19:49:48|    10240|K        |
|  1968|Prg|CX_SY_ASSIGN_CAST_ERROR=======CP        |  1968|10.11.2022|19:49:48|    10240|K        |
|  1969|Prg|CX_SY_ASSIGN_ERROR============CP        |  1969|10.11.2022|19:49:48|    10240|K        |
|  1970|Typ|DDSHDESCR                               |     0|03.09.1997|03:05:16|     4096|         |
|  1971|Typ|DD36S                                   |     0|20.08.1998|11:36:03|     3072|         |
|  1972|Prg|CL_F4_SRAL_SETTINGS_DIALOG====CP        |  1972|10.11.2022|19:49:35|    41984|K        |
|  1973|Typ|DD31V                                   |     0|08.09.2011|14:26:02|     3072|         |
|  1974|Typ|DD33V                                   |     0|08.09.2011|14:18:19|     4096|         |
|  1975|Prg|SAPLSMTR_NAVIGATION_MODULES             |  1975|17.06.2024|13:06:43|   529408|K        |
|  1976|Typ|TSTCC                                   |     0|18.12.2001|16:57:59|     4096|         |
|  1977|Typ|ISTCCLAS_C                              |     0|09.11.2000|14:08:32|     3072|         |
|  1978|Prg|SAPLURL_GENERATION                      |  1978|16.12.2022|11:36:13|   384000|K        |
|  1979|Typ|UIDEPEND                                |     0|31.05.2013|08:16:18|     4096|         |
|  1980|Prg|SAPLWDTM                                |  1980|26.01.2023|17:37:54|   312320|K        |
|  1981|Typ|DDSHRETVAL                              |     0|21.04.2017|11:57:45|     4096|         |
|  1982|Prg|SAPLSITS_SERVICE_PARAMETER              |  1982|10.11.2022|19:49:54|    23552|K        |
|  1983|Prg|CL_ICF_ITS_SERVICE============CP        |  1983|10.11.2022|19:49:37|    13312|K        |
|  1984|Typ|DDSHF4CTRL                              |     0|29.03.2018|15:49:07|     6144|         |
|  1985|Typ|DDSHOCXINT                              |     0|31.05.2013|08:00:33|     3072|         |
|  1986|Typ|DDSHTECINF                              |     0|31.05.2013|08:00:33|     5120|         |
|  1987|Typ|MCTBC_CTRL                              |     0|28.05.2018|11:34:18|     6144|         |
|  1988|Typ|MCTBC_HI                                |     0|31.05.2013|08:06:03|     3072|         |
|  1989|Typ|MCTBC_CMSG                              |     0|28.05.2018|11:34:18|     3072|         |
|  1990|Typ|MCTBC_TBFN                              |     0|20.08.1998|11:20:46|     4096|         |
|  1991|Prg|SAPLMATE                                |  1991|10.11.2025|17:51:38|   312320|K        |
|  1992|Prg|SAPLCOM_SE_F4_HELP                      |  1992|14.09.2011|14:41:45|    63488|K        |
|  1993|Prg|COM_SE_F4_STEP_SELONE=========CP        |  1993|21.08.2023|09:32:40|    14336|K        |
|  1994|Prg|CL_COM_SE_CUSTOM_READ=========CP        |  1994|01.06.2015|11:33:47|    92160|K        |
|  1995|Prg|CL_IM_EAM_F4_EAMORD===========CP        |  1995|24.04.2018|16:30:15|    19456|K        |
|  1996|Prg|CL_IM_COM_SE_F4_QMEL2_TREX====CP        |  1996|06.06.2015|15:15:30|    17408|K        |
|  1997|Prg|IF_COM_SE_F4_STEP_SELONE======IP        |  1992|07.02.2006|10:38:40|    13312|K        |
|  1998|Prg|CL_QQM_TREX_SWITCH_CHECK======CP        |  1998|15.01.2020|14:44:35|    11264|K        |
|  1999|Prg|SAPLESH_IF_F4                           |  1999|14.09.2011|14:45:27|    55296|K        |
|  2000|Prg|BADI_ESH_IF_F4_MODIFY_STEP====CP        |  2000|21.08.2023|09:15:07|    18432|K        |
|  2001|Prg|CL_ESH_DB_DASHBOARD===========CP        |  2001|15.06.2023|07:05:20|   395264|K        |
|  2002|Typ|ESH_S_DB_CONNECTION_DATA                |     0|29.03.2018|15:49:13|     3072|         |
|  2003|Prg|CL_ESH_ADM_UTILITIES==========CP        |  2003|15.06.2023|07:05:20|    65536|K        |
|  2004|Prg|CL_ESH_ADM_PARAM==============CP        |  2004|16.05.2023|13:23:24|   156672|K        |
|  2005|Prg|CL_ESH_CA_CACHE===============CP        |  2005|10.11.2022|19:49:34|    36864|K        |
|  2006|Prg|CL_ESH_ADM_PARAM_CACHECLIENT==CP        |  2006|10.11.2022|19:49:34|    15360|K        |
|  2007|Prg|IF_ESH_CA_CACHE_CLIENT========IP        |  2005|10.11.2022|19:49:50|    10240|K        |
|  2008|Prg|IF_ESH_CA_CACHE===============IP        |  2005|10.11.2022|19:49:50|    11264|K        |
|  2009|Prg|CL_ESH_CA_SOURCE_STORAGE======CP        |  2009|10.11.2022|19:49:34|    21504|K        |
|  2010|Prg|CL_ESH_CA_SHARED_OBJECTS======CP        |  2010|10.11.2022|19:49:34|    36864|K        |
|  2011|Prg|CL_ESH_CA_SHARED_OBJECTS_AREA=CP        |  2011|10.11.2022|19:49:34|    51200|K        |
|  2012|Prg|CL_ESH_CA_SHARED_OBJECTS_ROOT=CP        |  2012|01.06.2015|12:30:30|     8192|K        |
|  2013|Prg|CL_ESH_ADM_PARAM_CACHEDATA====CP        |  2013|26.11.2018|10:05:41|     9216|K        |
|  2014|Prg|IF_ESH_CA_DATA_CONTAINER======IP        |  2010|01.06.2015|12:30:31|     5120|K        |
|  2015|Typ|ESH_S_ADM_PARAM_CACHE                   |     0|26.11.2018|10:05:41|     6144|         |
|  2016|Prg|CL_ESH_ADM_CONSTANTS==========CP        |  2016|28.02.2019|16:11:56|    19456|K        |
|  2017|Prg|CL_SQL_CONNECTION=============CP        |  2017|16.05.2023|13:23:18|    31744|K        |
|  2018|Prg|CL_ESH_CDSABAP_CONST==========CP        |  2018|16.05.2023|13:23:49|    29696|K        |
|  2019|Prg|CL_ESH_CDSABAP_VALIDATE_SEARCHCP        |  2019|15.06.2023|07:05:20|   118784|K        |
|  2020|Prg|CL_ESH_CDSABAP_VALIDATE_ES====CP        |  2020|03.05.2025|07:02:00|   283648|K        |
|  2021|Typ|ESH_ADM_GEN_CONN                        |     0|25.02.2015|23:58:12|     4096|         |
|  2022|Prg|CL_SQL_STATEMENT==============CP        |  2022|16.05.2023|13:23:14|    34816|K        |
|  2023|Prg|CL_SQL_RESULT_SET=============CP        |  2023|16.05.2023|13:23:14|    29696|K        |
|  2024|Prg|CL_ESH_SE_CONN_ID_CONNECTOR===CP        |  2024|06.02.2024|14:54:08|    36864|K        |
|  2025|Prg|CL_ESH_TREX_PROXY_ADMIN=======CP        |  2025|08.08.2024|10:13:17|   413696|K        |
|  2026|Prg|CL_ESH_TREX_PROXY=============CP        |  2026|15.06.2023|07:05:20|   139264|K        |
|  2027|Typ|ESH_S_HDB_CONNECTIVITY                  |     0|23.04.2019|09:15:26|     8192|         |
|  2028|Prg|CL_ESH_TREX_CONSTANTS=========CP        |  2028|16.12.2022|13:57:54|    32768|K        |
|  2029|Prg|CL_ESH_OM_CONSTANTS===========CP        |  2029|16.12.2022|13:58:11|   174080|K        |
|  2030|Prg|CL_ESH_ADM_GEN_CONNECTIVITY===CP        |  2030|10.11.2022|19:49:34|    44032|K        |
|  2031|Typ|ESH_S_ADM_GEN_CONNECTION                |     0|25.02.2015|23:58:13|     4096|         |
|  2032|Typ|ESH_S_ADM_GEN_CONN_PARAMETER            |     0|08.09.2011|14:26:18|     3072|         |
|  2033|Typ|CVERS                                   |     0|09.11.2000|14:05:49|     3072|         |
|  2034|Prg|SAPLOCS_CRM                             |  2034|28.03.2025|19:25:30|    65536|K        |
|  2035|Typ|SPAM_CVERS                              |     0|06.11.2003|20:31:15|     3072|         |
|  2036|Typ|COMP_PROPS                              |     0|04.06.2013|13:48:31|     5120|         |
|  2037|Typ|CVERS_SUB                               |     0|31.05.2013|08:00:11|     3072|         |
|  2038|Typ|SPAM_CLAYR                              |     0|06.11.2003|20:31:15|     3072|         |
|  2039|Typ|SPAM_CPK                                |     0|24.02.2015|18:07:08|     3072|         |
|  2040|Prg|CL_ABAP_TSTMP=================CP        |  2040|08.08.2024|10:13:12|    64512|K        |
|  2041|Prg|SAPLTREX_EXT_ADMINISTRATION             |  2041|28.04.2023|07:29:51|   392192|K        |
|  2042|Prg|CL_TREX_DB_CONNECTION=========CP        |  2042|31.03.2023|15:25:26|    12288|K        |
|  2043|Prg|CL_TREX_USER_MANAGER==========CP        |  2043|03.05.2025|08:07:10|   100352|K        |
|  2044|Prg|CL_HDB_QUERY_CTOR_OPTIMIZER===CP        |  2044|03.05.2025|08:07:05|    52224|K        |
|  2045|Prg|SAPLTREX_DBS_ADMINISTRATION             |  2045|16.05.2023|13:23:19|   449536|K        |
|  2046|Prg|CL_TREX_FACTORY===============CP        |  2046|16.05.2023|13:23:19|    62464|K        |
|  2047|Prg|CL_TREX_SQL_STATEMENT=========CP        |  2047|16.05.2023|13:23:19|    38912|K        |
|  2048|Prg|SAPLSDBI_DBINFO                         |  2048|10.11.2022|19:49:54|    18432|K        |
|  2049|Prg|CL_DD_BLUEGREEN_UTILITIES=====CP        |  2049|10.11.2022|19:49:31|    12288|K        |
|  2050|Prg|CX_SY_CREATE_OBJECT_ERROR=====CP        |  2050|10.11.2022|19:49:48|    11264|K        |
|  2051|Prg|CL_SYSTEM_VIEW_LAYER==========CP        |  2051|26.01.2023|17:38:15|    10240|K        |
|  2052|Prg|CL_ESH_CDSABAP_TOOLS==========CP        |  2052|15.06.2023|07:05:20|   378880|K        |
|  2053|Prg|CL_ESH_CDSABAP_CONVERTER======CP        |  2053|15.06.2023|07:05:20|   398336|K        |
|  2054|Prg|CL_ESH_CDSABAP_DATA_PROVIDER==CP        |  2054|18.04.2025|12:30:37|   351232|K        |
|  2055|Typ|ESH_OM_AOTYP                            |     0|30.01.2017|09:54:25|    11264|         |
|  2056|Typ|ESH_S_DB_CONNECTOR_DATA                 |     0|26.11.2018|10:05:42|     4096|         |
|  2057|Prg|IF_ARS_API_CONSTANTS==========IP        |  2054|03.03.2023|16:55:52|    12288|K        |
|  2058|Prg|CL_ESH_RUBU_MEM_HELPER========CP        |  2058|16.12.2022|13:57:54|   184320|K        |
|  2059|Prg|CL_ESH_RUBU_MEM===============CP        |  2059|16.12.2022|11:15:24|   125952|K        |
|  2060|Prg|IF_ESH_RUBU_CONSTANTS=========IP        |  2059|16.12.2022|11:15:24|    11264|K        |
|  2061|Prg|CL_ESH_RUBU_STATUS============CP        |  2061|16.12.2022|13:59:58|    60416|K        |
|  2062|Prg|CL_ABAP_TIMESTAMP_UTIL========CP        |  2062|26.01.2023|17:38:04|    12288|K        |
|  2063|Prg|CL_ESH_RUBU_MEM_MAIN==========CP        |  2063|16.12.2022|11:15:24|   191488|K        |
|  2064|Prg|CL_ESH_RUBU_MEM_MAIN_AREA=====CP        |  2064|10.11.2022|19:49:35|    47104|K        |
|  2065|Prg|IF_ESH_RUBU_DDIC==============IP        |   201|16.12.2022|11:15:24|   238592|K        |
|  2066|Typ|ESH_S_CDS_ANNO_VAL                      |     0|16.12.2022|11:15:18|     4096|         |
|  2067|Typ|TREXS_JOIN_ATTRIBUTE                    |     0|08.09.2011|14:28:29|     3072|         |
|  2068|Typ|ESH_S_CDS_ALTKEY                        |     0|18.05.2016|15:52:40|     3072|         |
|  2069|Typ|ESH_S_CDS_PARAM                         |     0|26.11.2018|10:05:42|     3072|         |
|  2070|Typ|ESH_S_CDS_NLP_SEMDOM_PARAMS             |     0|19.11.2020|09:40:06|     3072|         |
|  2071|Typ|ESH_S_CDS_R_ACTIVEFUNC                  |     0|12.06.2017|09:09:52|     3072|         |
|  2072|Typ|ESH_S_CDS_R_PARAMBIND                   |     0|18.05.2016|15:52:40|     3072|         |
|  2073|Typ|TREXS_COMP_DESCR                        |     0|10.10.2014|18:27:35|     4096|         |
|  2074|Typ|HDBS_QUERY_ENTRY                        |     0|04.08.2014|15:42:02|     6144|         |
|  2075|Typ|TREXS_HINT_L                            |     0|31.05.2013|08:15:48|     2048|         |
|  2076|Typ|ESH_S_OM_ARELAT_EXT                     |     0|08.09.2011|14:20:04|    10240|         |
|  2077|Typ|ESH_S_OM_ARELATF_EXT                    |     0|08.09.2011|14:20:02|     7168|         |
|  2078|Typ|ESH_S_OM_ARELATFV_EXT                   |     0|08.09.2011|14:18:55|     8192|         |
|  2079|Typ|ESH_S_JOIN_PATH_STEP                    |     0|08.09.2011|14:20:01|    24576|         |
|  2080|Typ|ESH_S_JOIN_ATTRIBUTES_LIST              |     0|28.05.2018|11:32:11|    10240|         |
|  2081|Typ|ESH_S_NODE_AUTHORIZATION_LIST           |     0|08.09.2011|14:20:01|     4096|         |
|  2082|Prg|CL_ESH_RUBU_MEM_MAIN_ROOT=====CP        |  2082|16.12.2022|11:15:24|   109568|K        |
|  2083|Prg|CL_ESH_RUBU_MEM_PROXY=========CP        |  2083|31.03.2023|15:25:26|   282624|K        |
|  2084|Prg|CL_ESH_IF_MDATA_CONNECTOR=====CP        |  2084|31.03.2023|15:25:26|   252928|K        |
|  2085|Prg|CL_ESH_CO_ID_CONVERSIONS======CP        |  2085|31.03.2023|15:25:26|    73728|K        |
|  2086|Prg|CL_ESH_CO_LANGUAGE============CP        |  2086|06.02.2024|14:54:08|    32768|K        |
|  2087|Prg|CL_ESH_RUBU_MEM_LANGU=========CP        |  2087|16.12.2022|11:15:24|    29696|K        |
|  2088|Prg|CL_ESH_RUBU_MEM_LANGU_AREA====CP        |  2088|10.11.2022|19:49:35|    47104|K        |
|  2089|Prg|CL_ESH_RUBU_MEM_LANGU_ROOT====CP        |  2089|16.12.2022|11:15:24|    13312|K        |
|  2090|Prg|CL_SWITCH_CHECK_FASHION=======CP        |  2090|21.08.2023|10:29:39|    11264|K        |
|  2091|Prg|CL_OPS_SWITCH_CHECK===========CP        |  2091|06.10.2025|16:28:21|   145408|K        |
|  2092|Prg|CL_WEB_DYNPRO=================CP        |  2092|10.11.2022|19:49:47|     8192|K        |
|  2093|Prg|CL_EX_W_RETAILSYSTEM_IDENT====CP        |  2093|21.08.2023|09:39:33|    14336|K        |
|  2094|Prg|IF_EX_W_RETAILSYSTEM_IDENT====IP        |   150|06.06.2015|17:54:11|     5120|K        |
|  2095|Prg|W_RETAILSYSTEM_IDENT==========CP        |  2095|21.08.2023|09:39:33|    15360|K        |
|  2096|Prg|CL_RFM_CHARACTSTC_TYPE_CUST===CP        |  2096|18.04.2021|16:33:29|    13312|K        |
|  2097|Typ|RFM_CHAR_CUST                           |     0|18.04.2021|15:02:02|     2048|         |
|  2098|Prg|CL_F4CUSTOM===================CP        |  2098|16.05.2023|13:23:46|   138240|K        |
|  2099|Typ|DDF4USER_P_SETT                         |     0|16.05.2023|13:06:47|     3072|         |
|  2100|Typ|DDF4SHLP_P_SETT                         |     0|16.05.2023|13:06:47|     3072|         |
|  2101|Prg|CX_DD_SEARCH_HELP=============CP        |  2101|16.05.2023|13:23:31|    13312|K        |
|  2102|Prg|CX_DD_ROOT_STATIC=============CP        |  2102|10.11.2022|19:49:06|    15360|K        |
|  2103|Prg|IF_DSH_F4_UTILITIES===========IP        |  1437|19.11.2020|11:23:40|     6144|K        |
|  2104|Prg|SAPLLANG                                |  2104|26.01.2023|17:38:15|    18432|K        |
|  2105|Typ|T002                                    |     0|21.01.2014|10:06:21|     3072|         |
|  2106|Prg|SAPLCLSL                                |  2106|17.06.2024|13:21:07|    88064|K        |
|  2107|Prg|/IWBEP/CL_DIAGNOSTICS_FACADE==CP        |  2107|08.06.2015|13:50:33|    11264|K        |
|  2108|Prg|/IWBEP/CL_SUTIL_RUNTIME=======CP        |  2108|12.02.2025|20:53:39|   265216|K        |
|  2109|Prg|SAPLSF4U                                |  2109|04.04.2023|11:22:53|   128000|K        |
|  2110|Prg|SAPLWRF_DIMENSION_VALUE                 |  2110|21.05.2026|18:09:37|   135168|K        |
|  2111|Prg|SAPLMPN_SUCHE_HILFE                     |  2111|10.11.2025|17:51:39|    49152|K        |
|  2112|Prg|SAPLEAN_SHLPEXIT_RFQ                    |  2112|12.09.2007|09:32:44|    24576|K        |
|  2113|Typ|MCTBC_DP                                |     0|28.05.2018|11:34:18|     4096|         |
|  2114|Typ|MCTBC_DI                                |     0|28.05.2018|11:34:18|     4096|         |
|  2115|Typ|DDSHSELOPTEX                            |     0|10.05.2019|12:57:40|     4096|         |
|  2116|Prg|SAPLSDHI                                |  2116|16.05.2023|13:23:46|   204800|K        |
|  2117|Typ|SHSM_M_MAT1M                            |     0|10.11.2025|17:50:30|     3072|         |
|  2118|Prg|IF_SRAL_RT_CTRL_F4LIST_DYNP===IP        |  1405|01.06.2015|12:27:36|    12288|K        |
|  2119|Typ|DDSHLSCTRL                              |     0|03.05.2025|05:25:01|    66560|         |
|  2120|Typ|DDSHLPACK                               |     0|03.05.2025|05:25:01|    29696|         |
|  2121|Typ|DDSHWROBJ                               |     0|18.05.2004|14:07:58|     6144|         |
|  2122|Typ|DDSHLCOLIN                              |     0|31.05.2013|08:00:33|    14336|         |
|  2123|Typ|MCTBC_COLN                              |     0|06.04.2021|13:56:45|     3072|         |
|  2124|Typ|OBJSUB                                  |     0|31.05.2013|08:06:51|     5120|         |
|  2125|Typ|OBJSUBT                                 |     0|31.05.2013|08:06:51|     3072|         |
|  2126|Typ|SCHL_AT                                 |     0|15.01.1999|16:50:53|     4096|         |
|  2127|Typ|SCHL_CA                                 |     0|04.12.2000|12:52:26|     4096|         |
|  2128|Typ|DDFTX                                   |     0|20.01.2014|18:52:16|     5120|         |
|  2129|Typ|DD30V                                   |     0|21.01.2014|10:06:39|     6144|         |
|  2130|Typ|DDSHLPVERS                              |     0|28.10.1998|13:07:28|     3072|         |
|  2131|Prg|SAPLSDM2                                |  2131|01.04.2022|13:52:53|   116736|K        |
|  2132|Prg|%H_T006_EXP                             |  1405|07.02.2022|07:48:01|    57344|K        |
|  2133|Typ|SHSTRUC                                 |     0|26.02.2015|00:11:12|     5120|         |
|  2134|Typ|H_T006_EXP                              |     0|09.11.1997|11:02:24|     4096|         |
|  2135|Typ|T006T                                   |     0|31.05.2013|08:13:44|     3072|         |
|  2136|Prg|CL_DYNP_GUI_STATE=============CP        |  2136|10.11.2022|19:49:34|    21504|K        |
|  2137|Prg|CX_DYNP_EVENT_NOT_PBO=========CP        |  2137|22.01.2021|09:24:52|    10240|K        |
|  2138|Prg|CX_DYNP_WRONG_EVENT===========CP        |  2138|22.01.2021|09:24:53|    10240|K        |
|  2139|Prg|CX_DYNP_ERROR=================CP        |  2139|22.01.2021|09:24:52|    10240|K        |
|  2140|Typ|T006B                                   |     0|31.05.2013|08:13:44|     3072|         |
|  2141|Prg|SAPLSHLC                                |  2141|21.08.2023|09:44:31|   115712|K        |
|  2142|Prg|CALENDAR_DEFINITION===========CP        |  2142|21.08.2023|09:44:31|    16384|K        |
|  2143|Typ|F4SETTING                               |     0|26.01.2018|13:54:39|     3072|         |
|  2144|Prg|CL_ABAP_DATFM=================CP        |  2144|26.01.2023|17:38:09|    88064|K        |
|  2145|Prg|CL_I18N_SYSTEM_SETTINGS=======CP        |  2145|18.03.2023|11:02:29|    73728|K        |
|  2146|Prg|CL_ISO_CALENDAR_DEFINITION====CP        |  2146|16.05.2023|13:23:38|     9216|K        |
|  2147|Prg|IF_CALENDAR_DEFINITION========IP        |  2141|16.05.2023|13:23:38|     7168|K        |
|  2148|Prg|SAPLSCAC                                |  2148|21.08.2023|09:44:31|    65536|K        |
|  2149|Prg|SAPLSCON                                |  2149|28.05.2018|11:38:51|    27648|K        |
|  2150|Prg|CL_GUI_CALENDAR===============CP        |  2150|10.11.2022|19:49:36|    80896|K        |
|  2151|Typ|USDEF                                   |     0|08.09.2011|14:25:06|     4096|         |
|  2152|Prg|SAPLSDEX                                |  2152|04.04.2023|11:22:51|    57344|K        |
|  2153|Typ|DD07L                                   |     0|10.10.2014|18:26:35|     3072|         |
|  2154|Typ|T246                                    |     0|31.05.2013|08:13:48|     3072|         |
|  2155|Prg|SAPLSCA5                                |  2155|21.08.2023|09:44:31|   161792|K        |
|  2156|Typ|T247                                    |     0|31.05.2013|08:13:48|     3072|         |
|  2157|Prg|%_CCNCA                                 |    14|01.06.2015|12:37:17|    11264|K        |
|  2158|Typ|CASDAYATTR                              |     0|04.05.1995|18:45:23|     4096|         |
|  2159|Prg|SAPLSCAL                                |  2159|16.05.2023|13:24:00|    67584|K        |
|  2160|Prg|SAPLSCA1                                |  2160|10.11.2022|19:49:54|    48128|K        |
|  2161|Typ|BAPIDEFAUL                              |     0|14.03.2022|07:59:50|     4096|         |
|  2162|Prg|CL_SUID_USER_RUNTIME==========CP        |  2162|04.04.2023|11:22:58|    20480|K        |
|  2163|Typ|USDEFAULTS                              |     0|08.09.2011|14:25:04|     4096|         |
|  2164|Typ|USR01                                   |     0|03.05.2022|08:37:48|     5120|         |
|  2165|Prg|CL_EEWA_RFC_WDO===============CP        |  2165|17.06.2026|07:11:56|   486400|K        |
|  2166|Typ|EWASDDICFKEY                            |     0|25.02.2015|23:58:19|     4096|         |
|  2167|Typ|EKKO                                    |     0|06.10.2025|16:26:51|    44032|         |
|  2168|Typ|EWASDDICFKEYTABLE                       |     0|25.02.2015|23:58:19|     4096|         |
|  2169|Typ|MKPF                                    |     0|05.04.2016|13:54:18|     8192|         |
|  2170|Typ|EKPO                                    |     0|06.10.2025|16:28:02|    87040|         |
|  2171|Typ|VBAK                                    |     0|21.05.2026|18:09:15|    56320|         |
|  2172|Typ|VBUP                                    |     0|05.11.2021|08:39:33|    11264|         |
|  2173|Typ|VBUK                                    |     0|05.11.2021|08:39:32|    17408|         |
|  2174|Typ|ISUWA_ORDER                             |     0|17.06.2026|07:11:10|   548864|         |
|  2175|Prg|/WATP/CL_IM_ARBHAZARDWASTE====CP        |  2175|17.06.2026|07:11:41|   378880|K        |
|  2176|Prg|/WATP/CL_IMPL_BO_CALLBACK_BAPICP        |  2176|12.02.2025|18:36:48|    16384|K        |
|  2177|Prg|/WATP/CL_MSGLOG_REDIRECT_PROTOCP        |  2177|12.02.2025|20:55:25|    17408|K        |
|  2178|Typ|EWASORDWEIGHFILLK_DATA                  |     0|30.04.2022|09:31:48|     9216|         |
|  2179|Prg|SAPLE71WASTE                            |  2179|17.06.2026|07:11:44|   374784|K        |
|  2180|Prg|SAPLRSA2                                |  2180|16.12.2022|11:36:13|   522240|K        |
|  2181|Prg|SAPLRSAN                                |  2181|18.06.2009|10:06:03|    54272|K        |
|  2182|Prg|SAPLRSSCM                               |  2182|17.06.2024|13:17:00|  1064960|K        |
|  2183|Typ|RSSCM_BWCLNT                            |     0|25.03.2003|17:47:51|     2048|         |
|  2184|Typ|T000                                    |     0|08.09.2011|14:23:54|     5120|         |
|  2185|Prg|SAPLSCD0                                |  2185|12.02.2025|20:54:40|   197632|K        |
|  2186|Typ|CDPOS_UID                               |     0|01.06.2015|16:00:31|     4096|         |
|  2187|Typ|CDPOS_STR                               |     0|01.06.2015|16:00:31|     6144|         |
|  2188|Typ|CDTXT                                   |     0|31.05.2013|07:59:10|     3072|         |
|  2189|Typ|CDPOS                                   |     0|03.06.2015|15:16:20|     6144|         |
|  2190|Prg|SAPLSCD8                                |  2190|10.11.2022|19:49:54|    94208|K        |
|  2191|Prg|CL_CHDO_OBJECT_TOOLS==========CP        |  2191|15.06.2023|07:05:36|   337920|K        |
|  2192|Prg|CL_CBO_CHANGE_DOC_INQUIRY=====CP        |  2192|12.02.2025|20:53:50|    22528|K        |
|  2193|Prg|IF_CBO_CONSTANTS==============IP        |  2192|17.05.2022|15:15:56|    29696|K        |
|  2194|Prg|SAPLSNR3                                |  2194|28.04.2023|07:29:38|   248832|K        |
|  2195|Typ|BNRIV                                   |     0|08.09.2011|14:25:41|     4096|         |
|  2196|Prg|CL_RSSCD_ENHANCE==============CP        |  2196|12.02.2025|20:54:27|   264192|K        |
|  2197|Typ|CDHDR                                   |     0|01.06.2015|16:00:31|     5120|         |
|  2198|Prg|CL_SOI_DEPLOY_PARAMETERS======CP        |  2198|03.03.2023|16:56:24|    41984|K        |
|  2199|Prg|CL_SOI_LOGGER=================CP        |  2199|03.03.2023|16:56:25|    67584|K        |
|  2200|Typ|UVERS                                   |     0|09.11.2000|14:16:07|     4096|         |
|  2201|Prg|SAPLRLFW                                |  2201|03.03.2023|16:55:27|    66560|K        |
|  2202|Prg|CX_SY_DYNAMIC_OSQL_SEMANTICS==CP        |  2202|10.11.2022|19:49:48|    15360|K        |
|  2203|Prg|CX_SY_DYNAMIC_OSQL_ERROR======CP        |  2203|10.11.2022|19:49:48|    13312|K        |
|  2204|Prg|CX_SY_OPEN_SQL_ERROR==========CP        |  2204|10.11.2022|19:49:48|    11264|K        |
|  2205|Prg|CX_SY_SQL_ERROR===============CP        |  2205|10.11.2022|19:49:48|    10240|K        |
|  2206|Prg|SAPLSCD1                                |  2206|12.02.2025|20:54:40|   300032|K        |
|  2207|Typ|BDI_CDPOS                               |     0|20.08.2001|12:12:41|     6144|         |
|  2208|Prg|SAPLSCP1                                |  2208|16.12.2022|11:36:13|    53248|K        |
|  2209|Typ|TBDA1                                   |     0|04.04.1995|17:42:19|     2048|         |
|  2210|Typ|TBDA2                                   |     0|09.11.2000|14:23:55|     3072|         |
|  2211|Typ|TBD62                                   |     0|20.08.1998|11:28:11|     3072|         |
|  2212|Prg|CL_ABAP_BEHV_TRANSACTION_NTFR=CP        |  2212|16.04.2021|19:43:16|     9216|K        |
|  2213|Prg|CL_OS_TRANSACTION_END_NOTIFIERCP        |  2213|05.03.2021|08:21:43|     9216|K        |
|  2214|Prg|SAPLSWEB                                |  2214|18.03.2023|11:02:32|   143360|K        |
|  2215|Prg|CD_BADI_DATA1=================CP        |  2215|21.08.2023|10:32:05|    14336|K        |
|  2216|Prg|/SPE/CL_EI_EWM_MAT_DISTR_FULL=CP        |  2216|26.01.2023|17:38:09|    21504|K        |
|  2217|Prg|CL_BEL_CHGDOC_CONNECTOR=======CP        |  2217|03.05.2025|07:24:40|    23552|K        |
|  2218|Prg|CL_BEL_CHGDOC_EVENT_ADAPTER===CP        |  2218|03.05.2025|07:24:40|   112640|K        |
|  2219|Prg|CL_POC_CD_DATA================CP        |  2219|21.08.2023|10:20:00|    24576|K        |
|  2220|Prg|CL_DFS_ORGM_CD_IMPL===========CP        |  2220|13.07.2023|08:37:15|    26624|K        |
|  2221|Prg|CL_MEP_BADI_CLG===============CP        |  2221|10.11.2022|19:49:38|    13312|K        |
|  2222|Prg|CD_IF_DATA====================IP        |  2185|31.05.2013|08:58:58|     8192|K        |
|  2223|Prg|CL_POC_SETTINGS===============CP        |  2223|21.08.2023|10:20:00|    17408|K        |
|  2224|Prg|CL_POC_APPLICATION_LOG========CP        |  2224|03.03.2023|14:53:21|    27648|K        |
|  2225|Prg|IF_POC_MAIN_C=================IP        |  2223|10.11.2022|19:49:50|    33792|K        |
|  2226|Prg|CL_MEP_ADMIN==================CP        |  2226|10.11.2022|19:49:38|    72704|K        |
|  2227|Prg|CL_MEP_CLG_EVENT_PROVIDER=====CP        |  2227|16.05.2023|13:23:13|    72704|K        |
|  2228|Prg|CL_MEP_EVENT_PROVIDER_BASE====CP        |  2228|10.11.2022|19:49:38|    11264|K        |
|  2229|Prg|CL_MEP_PROVIDER_BASE==========CP        |  2229|10.11.2022|19:49:38|     8192|K        |
|  2230|Typ|VADRP                                   |     0|17.05.2022|14:43:29|    11264|         |
|  2231|Typ|VADCP                                   |     0|07.04.2016|14:02:38|    10240|         |
|  2232|Typ|VADR3                                   |     0|07.04.2016|14:02:38|     6144|         |
|  2233|Typ|VADR4                                   |     0|07.04.2016|14:02:39|     5120|         |
|  2234|Typ|VADR5                                   |     0|07.04.2016|14:02:39|     5120|         |
|  2235|Typ|VADR6                                   |     0|07.04.2016|14:02:39|     6144|         |
|  2236|Typ|VADR7                                   |     0|07.04.2016|14:02:39|     5120|         |
|  2237|Typ|VADR8                                   |     0|07.04.2016|14:02:39|    11264|         |
|  2238|Typ|VADR9                                   |     0|07.04.2016|14:02:39|     5120|         |
|  2239|Typ|VADR10                                  |     0|07.04.2016|14:02:38|     5120|         |
|  2240|Typ|VADR11                                  |     0|07.04.2016|14:02:38|    10240|         |
|  2241|Typ|VADR12                                  |     0|07.04.2016|14:02:38|     9216|         |
|  2242|Typ|VADR13                                  |     0|07.04.2016|14:02:38|     6144|         |
|  2243|Typ|ADDRS_ADDRESS_KEY                       |     0|08.09.2011|14:18:18|     4096|         |
|  2244|Prg|CL_ADDR_MAIN_PORT_SZA0========CP        |  2244|10.11.2022|19:49:27|     9216|K        |
|  2245|Prg|CL_ADDR_MAIN==================CP        |  2245|17.06.2026|07:11:54|   466944|K        |
|  2246|Prg|CL_ADDR_PERSISTENCY_SZA0======CP        |  2246|17.06.2024|13:17:10|   146432|K        |
|  2247|Prg|CL_ADDR_ENCODE================CP        |  2247|10.11.2022|19:49:27|    62464|K        |
|  2248|Prg|CL_ADDR_TD_CFG_TSADVC_ALL=====CP        |  2248|01.06.2015|12:30:30|    17408|K        |
|  2249|Typ|TSADVC_CLIDEP                           |     0|31.05.2013|08:15:53|     3072|         |
|  2250|Prg|CL_ADDR_CHECK=================CP        |  2250|17.06.2024|13:17:10|   156672|K        |
|  2251|Prg|CL_ADDR_METADATA_MNGR=========CP        |  2251|08.06.2020|09:46:51|     8192|K        |
|  2252|Prg|CL_ADDR_METADATA==============CP        |  2252|03.11.2021|19:19:31|    76800|K        |
|  2253|Prg|CL_ADDR_COMPARE_MNGR==========CP        |  2253|19.05.2020|14:16:46|     8192|K        |
|  2254|Prg|CL_ADDR_COMPARE===============CP        |  2254|03.11.2021|19:19:31|   103424|K        |
|  2255|Prg|CL_CMD_APPLLOG================CP        |  2255|26.01.2023|17:38:04|   276480|K        |
|  2256|Prg|IF_CMD_APPLLOG_CONV===========IP        |  2255|03.11.2021|19:19:43|    11264|K        |
|  2257|Prg|SAPLBDLS                                |  2257|16.05.2023|13:23:32|   513024|K        |
|  2258|Prg|IF_CMD_APPLLOG_CONST==========IP        |  2255|01.06.2015|12:30:31|     8192|K        |
|  2259|Typ|CMD_S_LOGS                              |     0|08.09.2011|14:19:17|     3072|         |
|  2260|Prg|CL_ADDR_MAIN_BEFORE_IMAGE=====CP        |  2260|10.11.2022|19:49:27|    92160|K        |
|  2261|Prg|CL_ADDR_RETRIEVE_DB===========CP        |  2261|17.06.2024|13:17:10|   180224|K        |
|  2262|Typ|BUS000___I                              |     0|17.06.2026|07:09:19|    39936|         |
|  2263|Typ|VBUT000_TD                              |     0|17.06.2026|07:11:36|    31744|         |
|  2264|Typ|TREEV_UNOD                              |     0|28.05.2018|11:38:04|     7168|         |
|  2265|Prg|BSV_OBJECT_ACTIVATION=========CP        |  2265|21.08.2023|09:57:35|    14336|K        |
|  2266|Typ|TJ05                                    |     0|30.11.1993|16:45:10|     2048|         |
|  2267|Typ|TJ01                                    |     0|01.03.1999|13:50:12|     6144|         |
|  2268|Typ|TJ07                                    |     0|30.11.1993|16:45:13|     3072|         |
|  2269|Prg|BSV_STATUS_PROFILE============CP        |  2269|21.08.2023|09:27:45|    14336|K        |
|  2270|Typ|TJ03                                    |     0|22.02.2001|10:04:57|     4096|         |
|  2271|Typ|EWA_EL_ABAPMETHODPARAMVALUE             |     0|28.05.2018|11:32:15|     6144|         |
|  2272|Prg|%_CPAR                                  |   688|16.12.2022|11:24:16|     9216|K        |
|  2273|Typ|EWASORDFILLRESULT                       |     0|05.09.2025|15:07:42|    11264|         |
|  2274|Typ|EWASORDFILLK_DATA                       |     0|20.11.2020|06:09:32|     6144|         |
|  2275|Prg|IF_EEWA_ORDER_FILLUP_CONTEXT==IP        |  1138|09.04.2026|00:02:56|    37888|K        |
|  2276|Typ|EWASORDOBJFILLRESULT                    |     0|26.02.2026|21:09:02|    17408|         |
|  2277|Typ|EWASORDOBJFILLK_DATA                    |     0|20.11.2020|06:09:32|     8192|         |
|  2278|Typ|EWASORDWGHFILLRESULT                    |     0|26.05.2025|11:31:14|    21504|         |
|  2279|Prg|IF_EEWA_PROTOCOL==============IP        |   101|31.01.2018|17:57:10|     6144|K        |
|  2280|Typ|EWA_ROUTE_DISPO                         |     0|14.07.2016|07:52:36|     3072|         |
|  2281|Prg|SAPLEEWA_BRF                            |  2281|17.06.2026|07:11:44|    63488|K        |
|  2282|Prg|CL_CONTROLLER_POOL_BRF========CP        |  2282|08.09.2011|14:59:05|    12288|K        |
|  2283|Prg|CL_APPLICATION_CLASS_BRF======CP        |  2283|08.09.2011|15:08:06|    10240|K        |
|  2284|Prg|CL_DB_BRF=====================CP        |  2284|30.01.2017|10:07:36|  3148800|K        |
|  2285|Typ|TBRF100T                                |     0|08.09.2011|14:24:30|     3072|         |
|  2286|Typ|TBRF133                                 |     0|31.05.2013|08:14:35|     5120|         |
|  2287|Typ|TBRF210                                 |     0|31.05.2013|08:14:35|     9216|         |
|  2288|Typ|TBRF110T                                |     0|31.05.2013|08:14:35|     4096|         |
|  2289|Typ|TABLENAME                               |     0|01.04.1998|01:54:17|     3072|         |
|  2290|Prg|CL_CONTROLLER_BRF=============CP        |  2290|08.09.2011|14:59:04|    31744|K        |
|  2291|Prg|IF_CONTROLLER_BRF=============IP        |  2282|08.09.2011|15:50:36|     9216|K        |
|  2292|Prg|CL_ACTION_PROCESSOR_BRF=======CP        |  2292|22.01.2021|09:24:26|    23552|K        |
|  2293|Prg|CL_TRACE_POOL_BRF=============CP        |  2293|08.09.2011|15:28:39|    11264|K        |
|  2294|Prg|CL_TRACE_BRF==================CP        |  2294|08.09.2011|15:28:38|    32768|K        |
|  2295|Prg|IF_TRACE_BRF==================IP        |  2293|08.09.2011|15:46:18|    15360|K        |
|  2296|Typ|BRF_TRACE_OBJECTS                       |     0|08.09.2011|14:17:37|     3072|         |
|  2297|Typ|BRF_CONTROLLER_POOL                     |     0|08.09.2011|14:17:33|     3072|         |
|  2298|Prg|CL_OBJECT_POOL_BRF============CP        |  2298|08.09.2011|15:15:00|    15360|K        |
|  2299|Prg|CL_CLASS_BRF==================CP        |  2299|01.06.2015|11:33:46|    22528|K        |
|  2300|Typ|TBRF180T                                |     0|31.05.2013|08:14:35|     5120|         |
|  2301|Prg|CL_EVENT_BRF==================CP        |  2301|08.09.2011|15:04:40|    20480|K        |
|  2302|Prg|CL_EVENT_BASE_BRF=============CP        |  2302|08.09.2011|15:04:34|    38912|K        |
|  2303|Prg|IF_EVENT_BRF==================IP        |  2298|08.09.2011|15:51:59|    16384|K        |
|  2304|Typ|SBRF110A                                |     0|08.09.2011|14:28:07|     7168|         |
|  2305|Prg|IF_RULE_COMPONENT_BRF=========IP        |  2302|08.09.2011|15:39:52|     7168|K        |
|  2306|Typ|TBRF185                                 |     0|31.05.2013|08:14:35|     3072|         |
|  2307|Typ|TBRF110                                 |     0|31.05.2013|08:14:35|     6144|         |
|  2308|Prg|CL_DET_ASSIGNMNT_RL_BRF=======CP        |  2308|08.09.2011|15:14:21|    19456|K        |
|  2309|Prg|IF_DETERMINE_ASSIGNMENT_BRF===IP        |  2302|08.09.2011|15:51:04|     7168|K        |
|  2310|Prg|CL_DET_ASSIGNMNT_RS_BRF=======CP        |  2310|08.09.2011|15:14:21|    19456|K        |
|  2311|Typ|TBRF280                                 |     0|31.05.2013|08:14:35|     6144|         |
|  2312|Typ|TBRF280T                                |     0|31.05.2013|08:14:35|     4096|         |
|  2313|Typ|SBRF210A                                |     0|08.09.2011|14:23:54|    11264|         |
|  2314|Prg|CL_EXPRESSION_POOL_BRF========CP        |  2314|01.06.2015|11:34:00|    47104|K        |
|  2315|Prg|CX_SY_PROGRAM_NOT_FOUND=======CP        |  2315|18.02.2005|14:13:43|    10240|K        |
|  2316|Typ|/WATP/TARB_AVK                          |     0|08.08.2024|08:30:54|     8192|         |
|  2317|Typ|/WATP/TARBNOTES                         |     0|08.08.2024|08:30:53|    36864|         |
|  2318|Typ|/WATP/TARBNW                            |     0|08.08.2024|08:30:53|    35840|         |
|  2319|Typ|/WATP/TTARB_TMM                         |     0|08.08.2024|08:30:14|     5120|         |
|  2320|Typ|/WATP/TTARB_WOT                         |     0|08.08.2024|08:30:14|     5120|         |
|  2321|Typ|V_T006_L                                |     0|31.05.2013|08:23:06|     3072|         |
|  2322|Typ|V_T006_T                                |     0|31.05.2013|08:23:06|     3072|         |
|  2323|Prg|ZCL_WR_MISC_EEWA==============CP        |  2323|08.05.2026|09:28:15|    26624|K        |
|  2324|Prg|CL_EEWA_IMPL_BO_CALLBACK_LIST=CP        |  2324|26.02.2015|05:40:44|    16384|K        |
|  2325|Prg|SAPLEEWA_ORDER_INT                      |  2325|17.06.2026|07:11:44|  1637376|K        |
|  2326|Prg|SAPLEEWA_ORDER                          |  2326|17.06.2026|07:15:00|  2672640|K        |
|  2327|Typ|EWATDEVLOC                              |     0|16.06.2015|08:35:55|     7168|         |
|  2328|Prg|ZCX_EEWA_BO_ORD===============CP        |  2328|05.05.2026|18:06:03|    39936|K        |
|  2329|Prg|CX_EEWA_SILENT================CP        |  2329|27.11.2018|02:20:11|    20480|K        |
|  2330|Prg|IF_T100_DYN_MSG===============IP        |    15|10.11.2022|19:49:51|     6144|K        |
|  2331|Prg|SAPLMESS                                |  2331|25.03.2019|14:20:51|    18432|K        |
|  2332|Typ|T100C                                   |     0|03.08.2015|16:04:01|     3072|         |
|  2333|Typ|EMMA_PROCOBJ                            |     0|24.02.2005|13:43:30|     3072|         |
|  2334|Prg|CL_ISU_WA_ORDER_BPEM==========CP        |  2334|26.02.2015|05:42:40|    13312|K        |
|  2335|Prg|SAPLEMMA_BW                             |  2335|26.02.2015|06:57:15|    80896|K        |
|  2336|Prg|CL_EMMA_BPC===================CP        |  2336|26.02.2015|06:57:03|    57344|K        |
|  2337|Prg|SAPLSBAL_DB                             |  2337|16.12.2022|11:24:16|   337920|K        |
|  2338|Prg|SAPLSDNTHIST                            |  2338|16.05.2023|13:23:55|    61440|K        |
|  2339|Prg|CL_DBI_DDNTT_HIST_ACCESS======CP        |  2339|16.05.2023|13:23:54|    40960|K        |
|  2340|Prg|IF_DATA_TEMPERATURE===========IP        |  2337|01.06.2015|12:30:31|     5120|K        |
|  2341|Prg|SAPLSBAL_TOOLBOX                        |  2341|10.11.2022|19:49:54|   251904|K        |
|  2342|Typ|BAL_S_LGNM                              |     0|31.05.2013|07:57:43|     3072|         |
|  2343|Typ|BALHDR                                  |     0|03.11.2021|18:10:15|    10240|         |
|  2344|Typ|BALDAT                                  |     0|10.10.2014|18:26:25|     4096|         |
|  2345|Prg|/SDF/SAPLSMD_E2E_TRACE                  |  2345|16.05.2023|13:23:52|   274432|K        |
|  2346|Typ|BAL_S_DB                                |     0|03.11.2021|18:01:47|    10240|         |
|  2347|Prg|CL_DATA_TEMPERATURE===========CP        |  2347|01.06.2015|12:30:30|     9216|K        |
|  2348|Prg|CL_ABAP_STACK_TEMPERATURE=====CP        |  2348|10.11.2022|19:49:27|    17408|K        |
|  2349|Typ|SEQTA                                   |     0|10.10.2014|18:27:21|     3072|         |
|  2350|Prg|CL_DBI_TRANSACTION_STATE======CP        |  2350|01.06.2015|12:30:30|     8192|K        |
|  2351|Prg|SAPLALFA                                |  2351|01.06.2015|12:30:31|     8192|K        |
|  2352|Prg|SAPLEMSG                                |  2352|08.09.2011|16:00:35|   110592|K        |
|  2353|Prg|%_CEEMSG                                |  2352|08.09.2011|15:55:17|    29696|K        |
|  2354|Typ|EMSG_MSG                                |     0|31.05.2013|08:01:32|     4096|         |
|  2355|Prg|CX_EEWA_BO_WDOC===============CP        |  2355|27.01.2023|18:40:55|   122880|K        |
|  2356|Prg|CL_EEWA_DATE==================CP        |  2356|23.06.2016|14:54:36|    20480|K        |
|  2357|Typ|TEDIFFCULT                              |     0|31.07.2015|11:55:43|     3072|         |
|  2358|Prg|IF_SALV_GUI_GRID_USER_ACTION==IP        |   341|03.05.2025|07:03:02|    12288|K        |
|  2359|Prg|CL_ALV_A_LVC==================CP        |  2359|08.08.2024|10:13:12|    67584|K        |
|  2360|Prg|SAPLCSCE                                |  2360|09.09.2004|14:18:34|     8192|K        |
|  2361|Prg|SAPLSCUC                                |  2361|08.09.2011|15:08:28|    27648|K        |
|  2362|Prg|SAPLCONV_EAN                            |  2362|08.09.2011|15:59:54|    14336|K        |
|  2363|Prg|SAPLWEA1                                |  2363|10.11.2025|17:51:41|   436224|K        |
|  2364|Prg|SAPLMATL                                |  2364|10.11.2025|17:51:38|  1924096|K        |
|  2365|Prg|SAPLV04B                                |  2365|03.08.2015|16:10:42|    24576|K        |
|  2366|Prg|SAPLSDC_CNV                             |  2366|10.11.2022|19:49:54|    17408|K        |
|  2367|Prg|SAPLSTS4                                |  2367|10.11.2022|19:49:55|    40960|K        |
|  2368|Typ|LVC_S_DETM                              |     0|31.05.2013|08:05:53|     3072|         |
|  2369|Prg|SAPLSLVC_SERVICES                       |  2369|15.06.2023|07:05:35|   282624|K        |
|  2370|Typ|LVC_S_DETA                              |     0|26.02.1999|14:47:50|     3072|         |
|  2371|Rty|type000000000412                        |     0|17.06.2026|13:59:47|        0|         |
|  2372|Typ|CAT_SVARS                               |     0|08.09.2011|14:17:48|     3072|         |
|  2373|Typ|DDXTT                                   |     0|23.04.2018|15:43:57|     8192|         |
|  2374|Typ|BAL_S_MDEF                              |     0|08.09.2011|14:25:43|     6144|         |
|  2375|Typ|BAL_S_CONF                              |     0|07.04.2016|13:57:08|    29696|         |
|  2376|Typ|BAL_S_COLL                              |     0|07.04.2016|13:57:08|    15360|         |
|  2377|Typ|BAL_S_DISP                              |     0|07.04.2016|13:57:08|    16384|         |
|  2378|Typ|MATCH_RESULT                            |     0|15.06.2005|11:38:22|     3072|         |
|  2379|Typ|DCDOMAGET                               |     0|30.03.1995|13:11:13|     3072|         |
|  2380|Typ|DCSQLTGET                               |     0|30.03.1995|13:11:14|     3072|         |
|  2381|Typ|LVC_S_MSG                               |     0|28.05.2018|11:34:04|     7168|         |
|  2382|Typ|FLT_S_LAYO                              |     0|04.12.2000|12:46:41|     4096|         |
|  2383|Prg|%_CSSCR                                 |   450|01.06.2015|12:30:30|    13312|K        |
|  2384|Typ|SITSPROF                                |     0|06.11.2003|20:31:07|     2048|         |
|  2385|Prg|%_CNF2TY                                |   656|29.03.2018|15:54:08|     8192|K        |
|  2386|Prg|%_CVIMTY                                |   844|09.06.2015|09:09:17|    36864|K        |
|  2387|Typ|TDTY4_SD_VBAP_ARRAY_READ_VBELN          |     0|11.12.2025|08:33:15|   101376|         |
|  2388|Typ|DDSHCUMAR                               |     0|31.05.2013|08:00:33|     3072|         |
|  2389|Typ|IAC_PARAMETER_2010                      |     0|08.09.2011|14:26:42|     3072|         |
|  2390|Typ|BDI_LSPAR                               |     0|31.05.2013|07:58:13|     5120|         |
|  2391|Prg|%_CISU2B                                |  2325|24.02.2005|17:20:59|     7168|K        |
|  2392|Typ|ISUWA_ORDER_AUTO                        |     0|09.04.2026|00:02:24|    36864|         |
|  2393|Typ|ISUWA_ORDER_LINES                       |     0|09.04.2026|00:02:26|   113664|         |
|  2394|Typ|ISUWA_ORDER_OBJECT                      |     0|09.04.2026|00:02:26|    63488|         |
|  2395|Prg|%_CISU21                                |  2326|17.06.2026|07:11:41|   416768|K        |
|  2396|Typ|EPRINTPARAMS                            |     0|19.11.2020|09:40:00|    20480|         |
|  2397|Typ|TOA_DARA                                |     0|30.11.1993|16:53:37|     4096|         |
|  2398|Typ|SWOTOBJID                               |     0|04.05.1995|15:20:52|     3072|         |
|  2399|Typ|EWA_ORDER_ALV                           |     0|09.04.2026|00:02:05|    55296|         |
|  2400|Typ|ISUWA_GP_DATA                           |     0|17.06.2026|07:11:02|   280576|         |
|  2401|Typ|ISUWA_ALL_OBJECTS                       |     0|17.06.2026|07:11:02|  2373632|         |
|  2402|Prg|CL_DRAGDROP===================CP        |  2402|28.05.2018|11:34:26|    40960|K        |
|  2403|Typ|SCREEN                                  |     0|18.05.2004|14:07:14|     5120|         |
|  2404|Prg|CX_EEWA_SECURITY==============CP        |  2404|20.12.2021|16:29:43|    28672|K        |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|Directory of Application Tables                                                                   |
----------------------------------------------------------------------------------------------------
|Name                                     Date       Time       Lngth                              |
|    Val.                                                                                          |
----------------------------------------------------------------------------------------------------
|Program  20260617132332                                                                           |
----------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_WDOCOCKPIT                                                                      |
----------------------------------------------------------------------------------------------------
|SYST                                       .  .       :  :     00004612                           |
|    \x0001\0\0\0\0\0\0\0\0\0\0\0\x0014\0\0\0\0\0\0\0\0\0\x0001                                    |
|SY                                         .  .       :  :     00004612                           |
|    \x0001\0\0\0\0\0\0\0\0\0\0\0\x0014\0\0\0\0\0\0\0\0\0\x0001                                    |
----------------------------------------------------------------------------------------------------
|Program  CL_GUI_PROPS_CONSUMER=========CP                                                         |
----------------------------------------------------------------------------------------------------
|EUDB                                       .  .       :  :     00004068                           |
|    %CCB057860B5A71FD19AC7DF24B7EC5ED9         \0\0\0                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPLOLEA                                                                                 |
----------------------------------------------------------------------------------------------------
|TOLE                                       .  .       :  :     00000394                           |
|    SAP.DATAPROVIDER.1                    {799104C5-04C5-11D0-                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPFGUICNTL                                                                              |
----------------------------------------------------------------------------------------------------
|INDX                                       .  .       :  :     00003104                           |
|    110%L%_CFWExcludedCallers_%\0\0\0  00000000                                                   |
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_WDOCWORKAREA                                                                    |
----------------------------------------------------------------------------------------------------
|EWASWDOC_WORKAREA_DLG                    25.02.2015 23:58:21   00001172                           |
|    110ALL_ITEMS                     RESULT                                                       |
----------------------------------------------------------------------------------------------------
|Program  SAPLCNDP                                                                                 |
----------------------------------------------------------------------------------------------------
|EUDB                                       .  .       :  :     00004068                           |
|    DPEWAEL_TREEV_ITEM                         \0\0\0                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPLSLVC                                                                                 |
----------------------------------------------------------------------------------------------------
|EUINFO                                     .  .       :  :     00003184                           |
|    %FLVC_S_DETAK VALUE                            \0\0\0                                         |
----------------------------------------------------------------------------------------------------
|Program  CL_DD_DFIES_CACHE=============CP                                                         |
----------------------------------------------------------------------------------------------------
|DDDFIESCACHE                               .  .       :  :     00000076                           |
|    DFLVC_S_DETA                     \0\x0014\0\x0001\0|                                          |
----------------------------------------------------------------------------------------------------
|Program  SAPLSKBS                                                                                 |
----------------------------------------------------------------------------------------------------
|LTDXS                                      .  .       :  :     00000130                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPFS_SECLOG                                                                             |
----------------------------------------------------------------------------------------------------
|INDX                                       .  .       :  :     00003104                           |
|    110%L%_SeclogExcludedCaller\0\0\0  00000000                                                   |
----------------------------------------------------------------------------------------------------
|Program  CL_SALV_BS_SESSION_CACHE======CP                                                         |
----------------------------------------------------------------------------------------------------
|EUDB                                       .  .       :  :     00004068                           |
|    VACB057860B5A71FD19AC7DF24B7EC5ED9         \0\0\0                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPSHDTV                                                                                 |
----------------------------------------------------------------------------------------------------
|SHDSTU                                     .  .       :  :     00000106                           |
|                                                         |                                        |
|SHDSTCIU                                   .  .       :  :     00000100                           |
|                                                      |                                           |
----------------------------------------------------------------------------------------------------
|Program  CL_DSH_SCREEN_ASSIGNMENT======CP                                                         |
----------------------------------------------------------------------------------------------------
|DSH_INDX_TA_DYNPRO                         .  .       :  :     00003064                           |
|    YYSAPLEEWA_WDOWEIGH                       0220\0\0                                            |
----------------------------------------------------------------------------------------------------
|Program  CL_DSH_DDIC_ASSIGNMENT========CP                                                         |
----------------------------------------------------------------------------------------------------
|DSH_INDX_TA_DDIC                           .  .       :  :     00003044                           |
|    YYEWASWDORDERWEIGH_DP           WERKS                                                         |
----------------------------------------------------------------------------------------------------
|Program  SAPLSUNI                                                                                 |
----------------------------------------------------------------------------------------------------
|RS38L                                      .  .       :  :     00004994                           |
|                                                                                                  |
|TFDIR                                      .  .       :  :     00000290                           |
|    CONV_EXIT_EAN11_OUTPUT_INTERN SAPLWEA1                                                        |
|ENLFDIR                                    .  .       :  :     00000146                           |
|    SUSR_USER_AUTH_FOR_OBJ_GET    SUSE                      X                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPLRHFE                                                                                 |
----------------------------------------------------------------------------------------------------
|TFDIR                                      .  .       :  :     00000290                           |
|    ECATT_SAVE_BALMSG             SAPLSETRM                                                       |
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_WDORDER                                                                         |
----------------------------------------------------------------------------------------------------
|EWASWDORDER_DP                           05.09.2025 15:07:47   00003512                           |
|    1100000000000000000264720260617000000000000000000000000000                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLSTRD                                                                                 |
----------------------------------------------------------------------------------------------------
|DD02L                                      .  .       :  :     00000464                           |
|                                   0000                                                           |
----------------------------------------------------------------------------------------------------
|Program  SAPLBSVT                                                                                 |
----------------------------------------------------------------------------------------------------
|TJ03T                                      .  .       :  :     00000068                           |
|    WOHKBortskaffelsesordre top       |                                                           |
|TJ05                                       .  .       :  :     00000014                           |
|    WOPWA16|                                                                                      |
|INDX                                       .  .       :  :     00003104                           |
|    11000BSV_SWITCH_BUFFER     \0\0\0  00000000                                                   |
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_WDOITEM                                                                         |
----------------------------------------------------------------------------------------------------
|EWASWDORDERITEM_DP                       09.04.2026 00:02:05   00011352                           |
|    110000000000000000026460003\0000000000010054884MAXI_10M3_L                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLZWR_WDOC_BD_WDOITEM                                                                  |
----------------------------------------------------------------------------------------------------
|ZWR_SWDOC_WDOITEM_DP                       .  .       :  :     00011352                           |
|    110000000000000000026460003\0000000000010054884MAXI_10M3_L                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_WDOWEIGH                                                                        |
----------------------------------------------------------------------------------------------------
|EWASWDORDERWEIGH_DP                      26.05.2025 11:31:16   00002640                           |
|    110000000000000000026470001                     0000000000                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLBSVA                                                                                 |
----------------------------------------------------------------------------------------------------
|JSTO                                       .  .       :  :     00000096                           |
|    110WH00000000000000006816WOH         00100000000|                                             |
|TJ01                                       .  .       :  :     00000074                           |
|    WA16XX                               |                                                        |
|TJ03                                       .  .       :  :     00000230                           |
|    WOP                                        0000  000000                                       |
|TJ05                                       .  .       :  :     00000014                           |
|    WOPWA16|                                                                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPLICON                                                                                 |
----------------------------------------------------------------------------------------------------
|ICONT                                      .  .       :  :     00000130                           |
|    K@1C@Spørgsmål                                                                                |
----------------------------------------------------------------------------------------------------
|Program  SAPLSWOR                                                                                 |
----------------------------------------------------------------------------------------------------
|TOJTB                                      .  .       :  :     00000966                           |
|    WASTEODWGHXE          RESOWASTEORDERWEIGH                                                     |
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
|Program  SAPLOMCV                                                                                 |
----------------------------------------------------------------------------------------------------
|TMCNV                                      .  .       :  :     00000206                           |
|    110MATCONV \x0012\0\0\0\x0012                                                                 |
----------------------------------------------------------------------------------------------------
|Program  SAPLSCV0                                                                                 |
----------------------------------------------------------------------------------------------------
|T006                                       .  .       :  :     00000104                           |
|    110KG XX\x0003X   MASS  \0\x0001\0\x0001\0\0\0\0\x000C\0\0                                    |
|T006D                                      .  .       :  :     00000042                           |
|    110VOLUME\x0003\0\0\0\0\0\0M3 XX|                                                             |
----------------------------------------------------------------------------------------------------
|Program  CONTEXT_X_TR_SYS_PARAMS                                                                  |
----------------------------------------------------------------------------------------------------
|CONTEXTB                                   .  .       :  :     00002992                           |
|    110EBTR_SYS_PARAMS       000006000100000000000000\0\0\0\0                                     |
|TCONT                                      .  .       :  :     00000068                           |
|    TR_SYS_PARAMS       20260615143315|                                                           |
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_WDOITEM                                                                         |
----------------------------------------------------------------------------------------------------
|EWASWDORDERITEM                          09.04.2026 00:02:03   00007208                           |
|                           0000\0                                                                 |
|EWA_ORDER_RESULT_EMPTYING                26.02.2026 21:09:11   00001632                           |
|                                                                                                  |
|TESERVICETYPE                            01.04.2022 19:32:23   00000036                           |
|              \0\0      |                                                                         |
|EWA_EL_WDPLANT                           14.11.2025 18:31:17   00001224                           |
|                                                                                                  |
|EWAOBJ                                   25.03.2026 09:25:20   00002216                           |
|                 \0\0\0\0\0\0\0\0\0                    0000                                       |
|MARA                                     10.11.2025 17:50:47   00004416                           |
|                                               \00000000000000                                    |
|IFLOT                                    08.08.2024 08:36:28   00001434                           |
|                                                                                                  |
|AUFK                                     03.05.2025 05:23:32   00001624                           |
|                       00                        00000000                                         |
|EEWA_PROP_H                              03.05.2025 05:24:09   00000392                           |
|                                                 \0\x000C                                         |
|BUT000                                   17.06.2026 07:11:50   00003818                           |
|                                                                                                  |
|T006                                     16.12.2022 11:23:54   00000104                           |
|            \0          \0\0\0\0\0\0\0\0\x000C\0\0    \0\0\0\0                                    |
|TEWACONFTYPE                             06.08.2021 21:10:31   00000064                           |
|                                    |                                                             |
|T001L                                    03.05.2025 05:26:19   00000296                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLMFAW                                                                                 |
----------------------------------------------------------------------------------------------------
|TFAWY                                      .  .       :  :     00000328                           |
|                                                   000                                            |
----------------------------------------------------------------------------------------------------
|Program  SAPLSCVU                                                                                 |
----------------------------------------------------------------------------------------------------
|T006                                       .  .       :  :     00000104                           |
|    110KG XX\x0003X   MASS  \0\x0001\0\x0001\0\0\0\0\x000C\0\0                                    |
|T006A                                      .  .       :  :     00000112                           |
|    110KUG UG UG    µg        µg                            |                                     |
|T006B                                      .  .       :  :     00000020                           |
|    110KKG KG |                                                                                   |
----------------------------------------------------------------------------------------------------
|Program  SAPLSDSD                                                                                 |
----------------------------------------------------------------------------------------------------
|DDSHOFFLD                                  .  .       :  :     00003020                           |
|    IFEWASWDORDERWEIGH_DP           WEIGHING_TYPE                                                 |
|DDF4LBBUF1                                 .  .       :  :     00002968                           |
|                                         \0\0\0\0\0\0\0\0\0\0                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPLSDF4                                                                                 |
----------------------------------------------------------------------------------------------------
|DDSHLPVERS                                 .  .       :  :     00000032                           |
|    0000000000000000|                                                                             |
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_WDORDER                                                                         |
----------------------------------------------------------------------------------------------------
|EWA_EL_WDPLANT                           14.11.2025 18:31:17   00001224                           |
|                                                                                                  |
|T006                                     16.12.2022 11:23:54   00000104                           |
|            \0          \0\0\0\0\0\0\0\0\x000C\0\0    \0\0\0\0                                    |
----------------------------------------------------------------------------------------------------
|Program  SAPLEEWA_WDOWEIGH                                                                        |
----------------------------------------------------------------------------------------------------
|MARA                                     10.11.2025 17:50:47   00004416                           |
|                                               \00000000000000                                    |
|T006                                     16.12.2022 11:23:54   00000104                           |
|            \0          \0\0\0\0\0\0\0\0\x000C\0\0    \0\0\0\0                                    |
|EWA_EL_WDPLANT                           14.11.2025 18:31:17   00001224                           |
|                                                                                                  |
|T001W                                    03.05.2025 05:26:19   00000700                           |
|                                                                                                  |
|T001L                                    03.05.2025 05:26:19   00000296                           |
|                                                                                                  |
|T301                                     10.09.2003 16:46:23   00000024                           |
|                |                                                                                 |
|LAGP                                     05.01.2005 16:13:59   00000380                           |
|                                   \0ఀ\0ఀ\0\0\x000Cఀ \0\0ఀ                                        |
----------------------------------------------------------------------------------------------------
|Program  SAPMSHLP                                                                                 |
----------------------------------------------------------------------------------------------------
|PHELP                                    03.05.2025 05:25:21   00003212                           |
|    SAPLEEWA_WDOWEIGH                       0210EWASWDORDERWEI                                    |
|HELP_INFO                                  .  .       :  :     00003548                           |
|    VFSAPLEEWA_WDOWEIGH                       0210EWASWDORDERW                                    |
|RSMDY                                      .  .       :  :     00000810                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLSHL2                                                                                 |
----------------------------------------------------------------------------------------------------
|HELP_INFO                                  .  .       :  :     00003548                           |
|                                                                                                  |
|DD02L                                      .  .       :  :     00000464                           |
|    T006                          A0000TRANSP                                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPLSCHL                                                                                 |
----------------------------------------------------------------------------------------------------
|OBJSUB                                     .  .       :  :     00000428                           |
|                                                                                                  |
----------------------------------------------------------------------------------------------------
|Program  SAPLLANG                                                                                 |
----------------------------------------------------------------------------------------------------
|T002                                       .  .       :  :     00000010                           |
|    KS3DA|                                                                                        |
----------------------------------------------------------------------------------------------------
|Program  SAPLSDEX                                                                                 |
----------------------------------------------------------------------------------------------------
|DD07L                                      .  .       :  :     00000120                           |
|    XUDATFM                       A00120000C                                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPLRSAN                                                                                 |
----------------------------------------------------------------------------------------------------
|T000                                       .  .       :  :     00000214                           |
|    110SAP SE                   Walldorf                 DKK                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPLSCP1                                                                                 |
----------------------------------------------------------------------------------------------------
|TBDA1                                      .  .       :  :     00000008                           |
|    110X|                                                                                         |
----------------------------------------------------------------------------------------------------
|Program  SAPLBDLS                                                                                 |
----------------------------------------------------------------------------------------------------
|T000                                       .  .       :  :     00000214                           |
|    110SAP SE                   Walldorf                 DKK                                      |
----------------------------------------------------------------------------------------------------
|Program  SAPLMESS                                                                                 |
----------------------------------------------------------------------------------------------------
|T100C                                      .  .       :  :     00000080                           |
|                                            |                                                     |
----------------------------------------------------------------------------------------------------
|Program  SAPLSBAL_DB                                                                              |
----------------------------------------------------------------------------------------------------
|BALDAT                                     .  .       :  :     00000604                           |
|    110ALomLuOBMd7{6Qo4JESLcjaG000001\0\x0002\000000000£湙鲘젻蛩㠽恷                                    |
----------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------
|ABAP Control Blocks (CONT)                                                                        |
----------------------------------------------------------------------------------------------------
|Index|Name|Fl|PAR0|PAR1|PAR2|PAR3|PAR4|PAR5|PAR6|Source Code                             |Line    |
----------------------------------------------------------------------------------------------------
|  660|LOOP|05|0000|C002|C003|C00D|0000|0000|0000|ZCL_WR_HELPER_TREE============CM003     |      36|
|  664|BRAN|05|0027|    |    |    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      36|
|  665|comp|00|0386|C010|C00F|    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      36|
|  667|BRAF|02|0003|    |    |    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      36|
|  668|COMP|00|000A|C011|0025|    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      36|
|  670|BRAF|02|FFF6|    |    |    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      36|
|  671|BREL|80|0000|    |    |    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      36|
|  672|LOOP|10|0000|0000|0000|0000|0000|0000|0000|ZCL_WR_HELPER_TREE============CM003     |      36|
|  676|mvqw|04|0580|01D0|0008|    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      37|
|  678|TREA|44|FFFF|C007|C00B|0001|0002|0045|0002|ZCL_WR_HELPER_TREE============CM003     |      39|
|  682|PAR4|80|0000|0001|0000|0000|0000|0000|0001|ZCL_WR_HELPER_TREE============CM003     |      39|
|  686|PAR2|01|0387|003F|C013|    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      39|
|>>>>>|STRG|09|C013|    |    |    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      40|
|  689|STRG|03|0023|    |    |    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      40|
|  690|STRG|03|C014|    |    |    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      40|
|  691|STRG|02|C013|    |    |    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      40|
|  692|mvqs|04|0080|0030|0002|    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      41|
|  694|mvqs|04|0080|0034|0002|    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      42|
|  696|mvqs|04|0080|0040|0003|    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      43|
|  698|TMOD|0C|C012|C002|C003|    |    |    |    |ZCL_WR_HELPER_TREE============CM003     |      44|
----------------------------------------------------------------------------------------------------
                                                                                                                                                                  
