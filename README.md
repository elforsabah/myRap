class ZCL_PD_PROC_WASTE_ORDERS definition
  public
  inheriting from /PLCE/CL_PD_PROC_WASTE_ORDERS
  create public .

public section.

  types:
    begin of ZZ_TY_DATA,
    SRV_UPD      type table for update /PLCE/R_PDSERVICE,
    SRV_UPD_UN   type table for update /PLCE/R_PDSERVICEEXTUNI,
    SRV_UPD_WA   type table for update /PLCE/R_PDSERVICEEXTWR,
    SRV_UPD_CUST type table for update /PLCE/R_PDServiceExtCustom,
    TSKS_DELE    type table for delete /PLCE/R_PDSERVICETASK,
    TSKS_DELE_UN type table for delete /PLCE/R_PDSERVICETASK,
    TSKS_DELE_WA type table for delete /PLCE/R_PDSERVICETASK,
    TSKS_CREA    type table for create /PlCE/R_PDSERVICE\_SERVICETASK,
    TSKS_CREA_UN type table for create /PLCE/R_PDSERVICETASK\_EXTUNIVERSAL,
    TSKS_CREA_WA type table for create /PLCE/R_PDSERVICETASK\_EXTWASTE.
  types end of ZZ_TY_DATA .
  types:
    begin of ZTP_SERVICE_EXT,
        SERVICE      type ref to /PLCE/R_PDSERVICE,
        EXTUNIVERSAL type ref to /PLCE/R_PDServiceExtUNI,
        EXTWASTE     type ref to /PLCE/R_PDServiceExtWR,
        EXTCustom    type ref to /PLCE/R_PDServiceExtCustom,
      end of ZTP_SERVICE_EXT .
  types:
    begin of ZTS_MODIFY_SERVICE,
        CID            type ABP_BEHV_CID,
        SERVICE        type /PLCE/R_PDSERVICE,
        EXTUNIVERSAL   type /PLCE/R_PDServiceExtUNI,
        EXTWASTE       type /PLCE/R_PDServiceExtWR,
        EXTCustom    type ref to /PLCE/R_PDServiceExtCustom,
        T_SERVICE_TASK type TP_SERVICE_TASK,
      end of ZTS_MODIFY_SERVICE .
  types:
    ZTP_MODIFY_SERVICE type sorted table of ZTS_MODIFY_SERVICE with unique key CID .
  types:
    begin of ZTS_MODIFY_SERVICE_EXT,
        CID            type ABP_BEHV_CID,
        SERVICE        type /PLCE/R_PDSERVICE,
        EXTUNIVERSAL   type /PLCE/R_PDServiceExtUNI,
        EXTWASTE       type /PLCE/R_PDServiceExtWR,
        EXTCUSTOM      type /PLCE/R_PDServiceExtCustom,
        ATTACHMENTS    type TP_MODIFY_SERVICE_ATTACHEMNT,
        T_SERVICE_TASK type TP_MODIFY_SERVICE_TASK_EXT,
        T_SERVICE_NOTES type TP_MODIFY_SERVICE_NOTE_EXT,
      end of ZTS_MODIFY_SERVICE_EXT .
  types:
    ZTP_MODIFY_SERVICE_EXT type sorted table of ZTS_MODIFY_SERVICE_EXT with unique key CID .

  class-methods ZZ_CHANGE_SERVICE_TYPE
    importing
      !PAR_KEY type /PLCE/SPDSRV_KEY
      !PAR_SVTPTO type /PLCE/PDSERVICE_TYPE
      !PAR_LOG type ref to /PLCE/CL_APPLLOG_HELPER optional
      !PAR_COMMIT type KENNZX optional
      !PAR_SIMULATE type KENNZX optional
    raising
      /PLCE/CX_BASEEXCEPTION .
  class-methods ZZ_FILL_SERVICE_TASK
    importing
      !PAR_KEY type /PLCE/SPDSRV_KEY
      !PAR_SVTPTO type /PLCE/PDSERVICE_TYPE
      !PAR_LOG type ref to /PLCE/CL_APPLLOG_HELPER optional
    returning
      value(PAR_RESULT) type ZZ_TY_DATA
    raising
      /PLCE/CX_BASEEXCEPTION .

  methods PROCESS_WASTEORDERS
    redefinition .
protected section.

  data AD_SERVICE type ref to ZTP_SERVICE_EXT .
  data AT_SERVICE_TO_CREATE type ZTP_MODIFY_SERVICE_EXT .

  methods FILL_SERVICES
    redefinition .
  methods FILL_SERVICE_DATA
    redefinition .
  methods FILL_SERVICE_TASK_DATA
    redefinition .
  methods PROCESS_UPDATE
    redefinition .
  methods SAVE_SERVICE
    redefinition .
private section.
ENDCLASS.



CLASS ZCL_PD_PROC_WASTE_ORDERS IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PD_PROC_WASTE_ORDERS->FILL_SERVICES
* +-------------------------------------------------------------------------------------------------+
* | [--->] IT_WASTE_ORDER_ITEM_DATA       TYPE        /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WASTEORDER
* | [!CX!] /PLCE/CX_BASEEXCEPTION
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method FILL_SERVICES.

    data:
        LSEQUENCE type I.

*    try.

    loop at IT_WASTE_ORDER_ITEM_DATA reference into FR_WASTE_ORDER_ITEM.
      clear LSEQUENCE.
*          if SHOULD_SERVICE_BE_CREATED( ).
      data(LV_ACTION) = GET_ACTION( ).
      if LV_ACTION is initial.
        FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
          exporting
            IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-ERROR
            IV_ID         = '/PLCE/MC_PD_MSG'
            IV_NUMBER     = '110'
            IV_VARIABLE_1 = conv #( FR_WASTE_ORDER_ITEM->WASTE_TYPE )
        ).
      endif.
      data(LST_TEMPLS) = READ_SERVICE_TEMPLATES( exporting IV_ACTION = LV_ACTION ).
      loop at LST_TEMPLS assigning field-symbol(<SERVICETMPL>)
        where SemanticTemplateName <> 'ST_SERVICE_NOTE'.
        insert value #( CID = GET_NEXT_CID( ) ) into table AT_SERVICE_TO_CREATE assigning field-symbol(<LS_SERVICE_TO_CREATE>).
        create data AD_SERVICE.
        AD_SERVICE->SERVICE = ref #( <LS_SERVICE_TO_CREATE>-SERVICE ).
        AD_SERVICE->EXTUNIVERSAL = ref #( <LS_SERVICE_TO_CREATE>-EXTUNIVERSAL ).
        AD_SERVICE->EXTWASTE = ref #( <LS_SERVICE_TO_CREATE>-EXTWASTE ).
        AD_SERVICE->EXTCUSTOM = ref #( <LS_SERVICE_TO_CREATE>-EXTCUSTOM ).
*              append initial line to FT_SERVICE_TO_CREATE reference into AD_SERVICE.
        FILL_SERVICE_DATA(
          exporting
            IS_SERVICE_TEMPLATE = <SERVICETMPL>
        ).
        if LINES( FR_WASTE_ORDER_ITEM->NOTES ) is not initial.
          loop at FR_WASTE_ORDER_ITEM->NOTES reference into data(LWASTESERVICENOTEREF).
            insert initial line into table <LS_SERVICE_TO_CREATE>-T_SERVICE_NOTES reference into data(LSERVICENOTEREF).
            FO_ACCESS_POOL->SET_CONTEXT(
              IV_SEMANTIC_CONTEXT = 'EWA_WASTE_SERVICE_NOTE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
              IR_DATA             = LWASTESERVICENOTEREF
            ).
            FO_ACCESS_POOL->SET_CONTEXT(
              IV_SEMANTIC_CONTEXT = 'SERVICE_NOTE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
              IR_DATA             = ref #( LSERVICENOTEREF->SERVICE_NOTE )
            ).

            FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
              IV_PROFILE                = GET_PROFILE( )
              IV_SEMANTIC_TEMPLATE_NAME = 'ST_SERVICE_NOTE'
            ).

          endloop.

        endif.


        if LINES( FR_WASTE_ORDER_ITEM->ATTACHMENTS ) is not initial.
          loop at FR_WASTE_ORDER_ITEM->ATTACHMENTS reference into data(LWASTEATTACHMENTREF).
            insert initial line into table <LS_SERVICE_TO_CREATE>-ATTACHMENTS reference into data(LSERVICEATTAREF).
*                FO_ACCESS_POOL->SET_CONTEXT(
*                  IV_SEMANTIC_CONTEXT = 'EWA_WASTE_SERVICE_NOTE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
*                  IR_DATA             = LWASTESERVICENOTEREF
*                ).
*                FO_ACCESS_POOL->SET_CONTEXT(
*                  IV_SEMANTIC_CONTEXT = 'SERVICE_NOTE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
*                  IR_DATA             = ref #( LSERVICENOTEREF->SERVICE_NOTE )
*                ).
*
*                FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
*                  IV_PROFILE                = GET_PROFILE( )
*                  IV_SEMANTIC_TEMPLATE_NAME = 'ST_SERVICE_NOTE'
*                ).
            LSERVICEATTAREF->ATTACHMENT-Filename = LWASTEATTACHMENTREF->FILENAME.
            LSERVICEATTAREF->ATTACHMENT-Attachment = LWASTEATTACHMENTREF->ATTACHMENT.
            LSERVICEATTAREF->ATTACHMENT-MimeType = LWASTEATTACHMENTREF->MIMETYPE.
            LSERVICEATTAREF->ATTACHMENT-CreatedByUpload = 'X'.

          endloop.

        endif.


        data(LSTT_TEMPLS) = READ_SERVICE_TASK_TEMPLATES( AD_SERVICE->SERVICE->SERVICETYPE ).
        loop at LSTT_TEMPLS assigning field-symbol(<LS_SERVICE_TASK_TEMPLATE>)
          where TaskType <> 'ADDITIONAL_SERVICE'.
          append initial line to <LS_SERVICE_TO_CREATE>-T_SERVICE_TASK assigning field-symbol(<SERVICE_TASK_TO_CREATE>).
          create data FR_SERVICE_TASK.
          FR_SERVICE_TASK->SERVICETASK = ref #( <SERVICE_TASK_TO_CREATE>-SERVICETASK ).
          FR_SERVICE_TASK->EXTUNIVERSAL = ref #( <SERVICE_TASK_TO_CREATE>-EXTUNIVERSAL ).
          FR_SERVICE_TASK->EXTWASTE = ref #( <SERVICE_TASK_TO_CREATE>-EXTWASTE ).
          FILL_SERVICE_TASK_DATA( <LS_SERVICE_TASK_TEMPLATE> ).
        endloop.
        if LINES( FR_WASTE_ORDER_ITEM->ADDITIONAL_SERVICES ) is not initial.
          LSEQUENCE = <SERVICE_TASK_TO_CREATE>-SERVICETASK-SequenceNumber.

          loop at FR_WASTE_ORDER_ITEM->ADDITIONAL_SERVICES reference into data(LADDSERVICEREF).
            loop at LSTT_TEMPLS assigning <LS_SERVICE_TASK_TEMPLATE>
              where TaskType = 'ADDITIONAL_SERVICE'.

              append initial line to <LS_SERVICE_TO_CREATE>-T_SERVICE_TASK assigning <SERVICE_TASK_TO_CREATE>.
              create data FR_SERVICE_TASK.
              FR_SERVICE_TASK->SERVICETASK = ref #( <SERVICE_TASK_TO_CREATE>-SERVICETASK ).
              FR_SERVICE_TASK->EXTUNIVERSAL = ref #( <SERVICE_TASK_TO_CREATE>-EXTUNIVERSAL ).
              FR_SERVICE_TASK->EXTWASTE = ref #( <SERVICE_TASK_TO_CREATE>-EXTWASTE ).

              FO_ACCESS_POOL->SET_CONTEXT(
                IV_SEMANTIC_CONTEXT = 'EWA_WASTE_ADDITIONAL_SERVICE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
                IR_DATA             = LADDSERVICEREF
              ).
              FILL_SERVICE_TASK_DATA( <LS_SERVICE_TASK_TEMPLATE> ).
              LSEQUENCE = LSEQUENCE + 10.
              <SERVICE_TASK_TO_CREATE>-SERVICETASK-SequenceNumber = LSEQUENCE.

*                  FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
*                    IV_PROFILE                = GET_PROFILE( )
*                    IV_SEMANTIC_TEMPLATE_NAME = <LS_SERVICE_TASK_TEMPLATE>-SemanticTemplateName
*                  ).

            endloop.
          endloop.
        endif.
      endloop.
*          endif.
    endloop.

*      catch /PLCE/CX_BASEEXCEPTION into data(LBASEEX).
*        FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_EXCEPTION(
*          exporting
**           IV_SEVERITY  =
*            IV_EXCEPTION = LBASEEX
*        ).
*    endtry.

  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PD_PROC_WASTE_ORDERS->FILL_SERVICE_DATA
* +-------------------------------------------------------------------------------------------------+
* | [--->] IS_SERVICE_TEMPLATE            TYPE        LINE OF TP_SERVICE_TMPL(optional)
* | [!CX!] /PLCE/CX_BASEEXCEPTION
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method FILL_SERVICE_DATA.
    field-symbols:
      <FIELD_FROM> type DATA,
      <FIELD_TO>   type DATA.

*    AD_SERVICE->* = corresponding #( IS_SERVICE_TEMPLATE mapping SequenceNumber = SqncNr ).
    AD_SERVICE->SERVICE->* = corresponding #( IS_SERVICE_TEMPLATE ).

    if FR_WASTE_ORDER_ITEM is not initial.
*      AD_SERVICE->SERVICE->ReferenceId         = |{ FR_WASTE_ORDER_ITEM->ORDERNR alpha = out }/{ FR_WASTE_ORDER_ITEM->ORDER_LAUFNR }|. condense AD_SERVICE->SERVICE->ReferenceId.
      AD_SERVICE->SERVICE->ReferenceId         = |{ FR_WASTE_ORDER_ITEM->WDOITEMID alpha = out }|. condense AD_SERVICE->SERVICE->ReferenceId.
      AD_SERVICE->SERVICE->ReferenceInternalId = FR_WASTE_ORDER_ITEM->POBJNR.
*      AD_SERVICE->SERVICE->ExternalOrderType  = AD_SERVICE_ORDER_ITEM->ServiceOrderItemCategory. "ServiceDocumentItemObjectType
      AD_SERVICE->SERVICE->Profile = GET_PROFILE( ).

      FO_ACCESS_POOL->SET_CONTEXT(
        IV_SEMANTIC_CONTEXT = /PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-SERVICE
        IR_DATA             = AD_SERVICE
      ).
      FO_ACCESS_POOL->SET_CONTEXT(
        IV_SEMANTIC_CONTEXT = 'EWA_WASTE_ORDER_ITEM' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-??
        IR_DATA             = FR_WASTE_ORDER_ITEM
      ).
      FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
        IV_PROFILE                = GET_PROFILE( )
        IV_SEMANTIC_TEMPLATE_NAME = IS_SERVICE_TEMPLATE-SemanticTemplateName
      ).
    endif.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PD_PROC_WASTE_ORDERS->FILL_SERVICE_TASK_DATA
* +-------------------------------------------------------------------------------------------------+
* | [--->] IS_SRVC_TSK_TMPL               TYPE        LINE OF TP_SERVICE_TASK_TMPL(optional)
* | [!CX!] /PLCE/CX_BASEEXCEPTION
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method FILL_SERVICE_TASK_DATA.

    FR_SERVICE_TASK->SERVICETASK->* = corresponding #( IS_SRVC_TSK_TMPL mapping SequenceNumber = SqncNr ).
    FR_SERVICE_TASK->SERVICETASK->ServiceUUID = AD_SERVICE->SERVICE->ServiceUUID.

    FO_ACCESS_POOL->SET_CONTEXT(
      IV_SEMANTIC_CONTEXT = /PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-SERVICE
      IR_DATA             = AD_SERVICE
    ).
    FO_ACCESS_POOL->SET_CONTEXT(
      IV_SEMANTIC_CONTEXT = 'EWA_WASTE_ORDER_ITEM' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
      IR_DATA             = FR_WASTE_ORDER_ITEM
    ).
    FO_ACCESS_POOL->SET_CONTEXT(
      IV_SEMANTIC_CONTEXT = /PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-SERVICE_TASK
      IR_DATA             = FR_SERVICE_TASK
    ).
    FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
      IV_PROFILE                = GET_PROFILE( )
      IV_SEMANTIC_TEMPLATE_NAME = IS_SRVC_TSK_TMPL-SemanticTemplateName
    ).
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PD_PROC_WASTE_ORDERS->PROCESS_UPDATE
* +-------------------------------------------------------------------------------------------------+
* | [!CX!] /PLCE/CX_BASEEXCEPTION
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method PROCESS_UPDATE.
    types:
      begin of _TATTAKEY,
        ServiceUUID    type /PLCE/PDSERVICE_UUID,
        AttachmentUUID type /PLCE/PDATTACHMENT_UUID,
      end of _TATTAKEY.

    data:
      LSRVKEYS    type /PLCE/PPDSRV_KEY,
      LSRVTSKKEYS type /PLCE/PPDSRVTSK_KEY.


    data:
      LRS_UPD       type table for update /PLCE/R_PDSERVICE,
      LRSUNI_UPD    type table for update /PLCE/R_PDSERVICEEXTUNI,
      LRSWA_UPD     type table for update /PLCE/R_PDSERVICEEXTWR,
      LRSCT_UPD     type table for update /PLCE/R_PDServiceExtCustom,
      LRSTSK_UPD    type table for update /PLCE/R_PDSERVICETASK,
      LRSTSKUNI_UPD type table for update /PLCE/R_PDSERVICETASKEXTUNI,
      LRSTSKWA_UPD  type table for update /PLCE/R_PDSERVICETASKEXTWR,
      LRS_ATTA_CREA type table for create /PLCE/R_PDSERVICE\_Attachments.
*      LRS_ATTA_CREA type table for create /PLCE/R_PDServiceAttachment.

    data:
      LSRVKEYS_DEL        type /PLCE/PPDSRV_KEY,
      LSRVKEYS_UNI_DEL    type /PLCE/PPDSRV_KEY,
      LSRVKEYS_WA_DEL     type /PLCE/PPDSRV_KEY,
      LSRVKEYS_CT_DEL     type /PLCE/PPDSRV_KEY,
      LSRVTSKKEYS_DEL     type /PLCE/PPDSRVTSK_KEY,
      LSRVTSKKEYS_UNI_DEL type /PLCE/PPDSRVTSK_KEY,
      LSRVTSKKEYS_WA_DEL  type /PLCE/PPDSRVTSK_KEY,
      LSRATTAKEYS_DEL     type standard table of _TATTAKEY.

    types:
      begin of _TPOBJNR,
        POBJNR type C length 30,
      end of _TPOBJNR.

    data:
      LPOBJNRS type standard table of _TPOBJNR.
****  FILL_KEYS
    if LINES( FT_WASTE_ORDER_ITEM_UPD ) is not initial.
      LPOBJNRS = corresponding #( FT_WASTE_ORDER_ITEM_UPD ).

      if LINES( LPOBJNRS ) is not initial.
        select SERVICEUUID from /PLCE/R_PDSERVICE
          for all entries in @LPOBJNRS
          where REFERENCEINTERNALID = @LPOBJNRS-POBJNR
          into table @LSRVKEYS.
      endif.
      if LINES( LSRVKEYS ) is not initial.

        select SERVICETASKUUID from /PLCE/R_PDSERVICETASK
          for all entries in @LSRVKEYS
          where SERVICEUUID = @LSRVKEYS-SERVICE_UUID
          into table @LSRVTSKKEYS.

      endif.

**** READ_ENTITIES

      read entities of /PLCE/R_PDSERVICE
        entity SERVICE
          all fields
          with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
*    with corresponding #( LKEYS mapping from entity )
          result data(LRS)
*      result ( LRS )
          by \_EXTUNIVERSAL all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSUNI)
          by \_EXTWASTE all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSWA)
          by \_ExtCustom all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSCT)
          by \_SERVICETASK all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSTSK)
          by \_ATTACHMENTS all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSATTA).

      read entities of /PLCE/R_PDSERVICE
        entity SERVICETASK
          by \_EXTWASTE all fields with corresponding #( LRSTSK )  "value #( for L in LRSTSK ( %KEY-ServiceTaskUUID = L-%KEY-ServiceTaskUUID ) )
          result data(LRSTSKWA)
          by \_EXTUNIVERSAL all fields with corresponding #( LRSTSK )  "value #( for L in LRSTSK ( %KEY-ServiceTaskUUID = L-%KEY-ServiceTaskUUID ) )
          result data(LRSTSKUNI).


      LRS_UPD = corresponding #( LRS ).
      LRSUNI_UPD = corresponding #( LRSUNI ).
      LRSWA_UPD = corresponding #( LRSWA ).
      LRSCT_UPD = corresponding #( LRSWA ).
      LRSTSK_UPD = corresponding #( LRSTSK ).
      LRSTSKUNI_UPD = corresponding #( LRSTSKUNI ).
      LRSTSKWA_UPD = corresponding #( LRSTSKWA ).

**** READ_ENTITIES

**** MAP_DATA

      data:
        LTSRV     type standard table of /PLCE/TPDSRV,
        LSRV_CREA type /PLCE/TPDSRV,
        LSRV_UPD  type /PLCE/TPDSRV.

      LTSRV = corresponding #( LRS_UPD mapping from entity  ).

      loop at LRS_UPD reference into data(LRSUPDREF).

*    LRSUPDREF->

**** Eigentlich kann es nur einen geben!!!!
        loop at AT_SERVICE_TO_CREATE reference into data(LSERVICECREATEREF)
          where SERVICE-REFERENCEINTERNALID = LRSUPDREF->REFERENCEINTERNALID. "#EC CI_SORTSEQ

**** Base entity
          LSRV_CREA = corresponding #( LSERVICECREATEREF->SERVICE mapping from entity ).
          LSRV_UPD = corresponding #( LRSUPDREF->* mapping from entity ).

          clear:
            LSRV_UPD-SERVICE_UUID, LSRV_UPD-SERVICE_ID.

          if LSRV_UPD-DATA <> LSRV_CREA-DATA.
* match!
            LRSUPDREF->%DATA = corresponding #( base ( LRSUPDREF->%DATA ) LSERVICECREATEREF->SERVICE except %KEY SERVICEID ).
            /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSUPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).

          endif.

*** EXTUNI
          read table LRSUNI_UPD reference into data(LRSUNI_UPDREF)
            with table key ENTITY components %KEY-SERVICEUUID = LRSUPDREF->%KEY-SERVICEUUID.

          if SY-SUBRC is initial.
            LRSUNI_UPDREF->%DATA = corresponding #( base ( LRSUNI_UPDREF->%DATA ) LSERVICECREATEREF->EXTUNIVERSAL except %KEY ).
            /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSUNI_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
          else.
*** new relation
*          insert LSERVICECREATEREF->EXTUNIVERSAL into table ...
          endif.

*** EXTWA
          read table LRSWA_UPD reference into data(LRSWA_UPDREF)
            with table key ENTITY components %KEY-SERVICEUUID = LRSUPDREF->%KEY-SERVICEUUID.

          if SY-SUBRC is initial.
            LRSWA_UPDREF->%DATA = corresponding #( base ( LRSWA_UPDREF->%DATA ) LSERVICECREATEREF->EXTWASTE except %KEY ).
            /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT =  LRSWA_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
*            LRSWA_UPDREF->* = corresponding #( base ( LRSWA_UPDREF->* ) LSERVICECREATEREF->EXTWASTE changing control except SERVICEUUID %KEY ).
          else.
*** new relation
*          insert LSERVICECREATEREF->EXTWASTE into table ...
          endif.

          read table LRSCT_UPD reference into data(LRSCT_UPDREF)
            with table key ENTITY components %KEY-SERVICEUUID = LRSUPDREF->%KEY-SERVICEUUID.

          if SY-SUBRC is initial.
            LRSCT_UPDREF->%DATA = corresponding #( base ( LRSCT_UPDREF->%DATA ) LSERVICECREATEREF->EXTCUSTOM except %KEY ).
            /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT =  LRSCT_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
*            LRSWA_UPDREF->* = corresponding #( base ( LRSWA_UPDREF->* ) LSERVICECREATEREF->EXTWASTE changing control except SERVICEUUID %KEY ).
          else.
*** new relation
*          insert LSERVICECREATEREF->EXTWASTE into table ...
          endif.

**** Attachments <- always
          if LINES( LSERVICECREATEREF->ATTACHMENTS ) is not initial.
            insert initial line into table LRS_ATTA_CREA assigning field-symbol(<ATTA_CREA>).
            loop at LSERVICECREATEREF->ATTACHMENTS reference into data(LATTAREF).
              insert initial line into table <ATTA_CREA>-%TARGET assigning field-symbol(<ATTA_CREA_LINE>).
              <ATTA_CREA_LINE> = corresponding #( LATTAREF->ATTACHMENT ).
              <ATTA_CREA_LINE>-ServiceUUID = LRSUPDREF->ServiceUUID.
              <ATTA_CREA_LINE>-%CID = GET_NEXT_CID( ).

              /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = ref #( <ATTA_CREA_LINE> ) ).
              <ATTA_CREA_LINE>-%CONTROL-ServiceUUID = IF_ABAP_BEHV=>MK-OFF.
            endloop.
            <ATTA_CREA>-ServiceUUID = LRSUPDREF->ServiceUUID.
*            <ATTA_CREA>-%CID_REF = LRSUPDREF->ServiceUUID.
          endif.

***** Tasks

**** upd & delete

***  update/insert
          loop at LSERVICECREATEREF->T_SERVICE_TASK reference into data(LSERVICETASKCREATEREF).

            read table LRSTSK_UPD reference into data(LRSTSK_UPDREF)
              with key
                       SERVICEUUID = LRSUPDREF->SERVICEUUID  " todo --- Fehler nachstellen
                       TASKTYPE = LSERVICETASKCREATEREF->SERVICETASK-TASKTYPE
                       SEQUENCENUMBER = LSERVICETASKCREATEREF->SERVICETASK-SEQUENCENUMBER.

            if SY-SUBRC is initial.

              LRSTSK_UPDREF->%DATA = corresponding #( base ( LRSTSK_UPDREF->%DATA ) LSERVICETASKCREATEREF->SERVICETASK
                except %KEY SERVICEUUID PLANNINGSTATUS TOURUUID ).
              /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSTSK_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
              LRSTSK_UPDREF->%CONTROL-ServiceUUID = IF_ABAP_BEHV=>MK-OFF.
              LRSTSK_UPDREF->%CONTROL-SequenceNumber = IF_ABAP_BEHV=>MK-OFF.

              read table LRSTSKUNI_UPD reference into data(LRSTSKUNI_UPDREF)
                with table key ENTITY components SERVICETASKUUID = LRSTSK_UPDREF->SERVICETASKUUID.

              if SY-SUBRC is initial.
                LRSTSKUNI_UPDREF->%DATA = corresponding #( base ( LRSTSKUNI_UPDREF->%DATA ) LSERVICETASKCREATEREF->EXTUNIVERSAL except %KEY SERVICEUUID ).
                /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSTSKUNI_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
                LRSTSKUNI_UPDREF->%CONTROL-ServiceUUID = IF_ABAP_BEHV=>MK-OFF.
              else.
***           insert new UNI Task
              endif.

              read table LRSTSKWA_UPD reference into data(LRSTSKWA_UPDREF)
                with table key ENTITY components SERVICETASKUUID = LRSTSK_UPDREF->SERVICETASKUUID.

              if SY-SUBRC is initial.
                LRSTSKWA_UPDREF->%DATA = corresponding #( base ( LRSTSKWA_UPDREF->%DATA ) LSERVICETASKCREATEREF->EXTWASTE except %KEY SERVICEUUID ).
                /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSTSKWA_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
                LRSTSKWA_UPDREF->%CONTROL-ServiceUUID = IF_ABAP_BEHV=>MK-OFF.
              else.
***           insert new WA Task
              endif.


            else.
***       alle Task Teile müssen angelegt werden.

            endif.

          endloop.


****  Delete

          if LSERVICETASKCREATEREF->EXTUNIVERSAL is initial.
            read table LRSUNI_UPD reference into LRSUNI_UPDREF
              with table key ENTITY components %KEY = corresponding #( LRSUPDREF->%KEY ).

            if SY-SUBRC is initial.
              if LSERVICECREATEREF->EXTUNIVERSAL is initial.
                insert corresponding #( LRSUNI_UPDREF->%KEY ) into table LSRVKEYS_UNI_DEL.
              endif.
            endif.
          endif.

          if LSERVICETASKCREATEREF->EXTWASTE is initial.
            read table LRSWA_UPD reference into LRSWA_UPDREF
              with table key ENTITY components %KEY = corresponding #( LRSUPDREF->%KEY ).

            if SY-SUBRC is initial.
              if LSERVICECREATEREF->EXTWASTE is initial.
                insert corresponding #( LRSWA_UPDREF->%KEY ) into table LSRVKEYS_WA_DEL.
              endif.
            endif.
          endif.


          loop at LRSTSK_UPD reference into LRSTSK_UPDREF where SERVICEUUID = LRSUPDREF->SERVICEUUID.
            read table LSERVICECREATEREF->T_SERVICE_TASK  reference into LSERVICETASKCREATEREF "transporting no fields
              with key "SERVICETASK-SERVICEUUID = LRSTSK_UPDREF->SERVICEUUID
                       SERVICETASK-TASKTYPE = LRSTSK_UPDREF->TASKTYPE
                       SERVICETASK-SEQUENCENUMBER = LRSTSK_UPDREF->SEQUENCENUMBER.

            if SY-SUBRC is not initial.
              insert corresponding #( LRSTSK_UPDREF->%KEY ) into table LSRVTSKKEYS_DEL.
            else.

              read table LRSTSKUNI_UPD reference into LRSTSKUNI_UPDREF
                with table key ENTITY components %KEY = corresponding #( LRSTSK_UPDREF->%KEY ).

              if SY-SUBRC is initial.
                if LSERVICETASKCREATEREF->EXTUNIVERSAL is initial.
                  insert corresponding #( LRSTSK_UPDREF->%KEY ) into table LSRVTSKKEYS_UNI_DEL.
                endif.
              endif.

              read table LRSTSKWA_UPD reference into LRSTSKWA_UPDREF
                  with table key ENTITY components %KEY = corresponding #( LRSTSK_UPDREF->%KEY ).

              if SY-SUBRC is initial.
                if LSERVICETASKCREATEREF->EXTWASTE is initial.
                  insert corresponding #( LRSTSK_UPDREF->%KEY ) into table LSRVTSKKEYS_WA_DEL.
                endif.
              endif.

            endif.

          endloop.

        endloop.
      endloop.

*** Attachments
      LSRATTAKEYS_DEL = value #( for LATTALINE in LRSATTA where ( CreatedByUpload is not initial )
        ( corresponding #( LATTALINE ) ) ).

**** SAVE
      delete:
        LRS_UPD where %CONTROL is initial,
        LRSUNI_UPD where %CONTROL is initial,
        LRSWA_UPD where %CONTROL is initial,
        LRSCT_UPD where %CONTROL is initial,
        LRSTSK_UPD where %CONTROL is initial,
        LRSTSKWA_UPD where %CONTROL is initial,
        LRSTSKUNI_UPD where %CONTROL is initial.


      modify entities of /PLCE/R_PDSERVICE
        entity SERVICE
          update from LRS_UPD
*          update set fields with corresponding #( LRS_UPD ) "set fields with LRS_UPD  ##SETFIELDS_OK
          delete from corresponding #( LSRVKEYS_DEL )
          create by \_Attachments from corresponding #( LRS_ATTA_CREA )
        entity EXTUNIVERSAL
          update from LRSUNI_UPD
*          update set fields with corresponding #( LRSUNI_UPD ) ##SETFIELDS_OK
          delete from corresponding #( LSRVKEYS_UNI_DEL )
        entity EXTWASTE
*          update set fields with corresponding #( LRSWA_UPD ) ##SETFIELDS_OK
          update from LRSWA_UPD
          delete from corresponding #( LSRVKEYS_WA_DEL )
        entity ExtCustom
*          update set fields with corresponding #( LRSWA_UPD ) ##SETFIELDS_OK
          update from LRSCT_UPD
          delete from corresponding #( LSRVKEYS_CT_DEL )
        entity Attachments
          delete from corresponding #( LSRATTAKEYS_DEL )
        entity SERVICETASK
          update from LRSTSK_UPD
*          update set fields with corresponding #( LRSTSK_UPD except SERVICEUUID SEQUENCENUMBER ) ##SETFIELDS_OK
          delete from corresponding #( LSRVTSKKEYS_DEL )
        entity EXTTASKUNIVERSAL
          update from LRSTSKUNI_UPD
*          update set fields with corresponding #( LRSTSKUNI_UPD except SERVICEUUID ) ##SETFIELDS_OK
          delete from corresponding #( LSRVTSKKEYS_UNI_DEL )
        entity EXTTASKWASTE
          update from LRSTSKWA_UPD
*          update set fields with corresponding #( LRSTSKWA_UPD except SERVICEUUID  ) ##SETFIELDS_OK
          delete from corresponding #( LSRVTSKKEYS_WA_DEL )
        mapped data(MAPPED_UPDATE)
        failed data(FAILED_UPDATE)
        reported data(REPORTED_UPDATE).

      if FAILED_UPDATE is initial.
*        modify entities of /PLCE/R_PDSERVICE
*          entity SERVICE
**            update from LRS_UPD
**  *          update set fields with corresponding #( LRS_UPD ) "set fields with LRS_UPD  ##SETFIELDS_OK
**            delete from corresponding #( LSRVKEYS_DEL )
*            create by \_Attachments from corresponding #( LRS_ATTA_CREA )
*          mapped data(MAPPED_UPDATE_ATTA)
*          failed data(FAILED_UPDATE_ATTA)
*          reported data(REPORTED_UPDATE_ATTA).

*        if FAILED_UPDATE_ATTA is initial.
          commit entities
            response of /PLCE/R_PDSERVICE
              failed data(FAILED_COMMIT_SRVC)
              reported data(REPORTED_COMMIT_SRVC).
*        else.
*          rollback entities.                          "#EC CI_ROLLBACK.
*        endif.
      else.
        rollback entities.                            "#EC CI_ROLLBACK.
      endif.

      /PLCE/CL_BASE_MISC=>CHECK_RESPONSE(
        exporting
          IS_RESPONSE = REPORTED_UPDATE
          IR_LOG      = FO_MSGLOG                 " APPLOG
      ).

    endif.

  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_PD_PROC_WASTE_ORDERS->PROCESS_WASTEORDERS
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROFILE                     TYPE        /PLCE/PDPROFILE
* | [--->] IT_WASTEORDER                  TYPE        /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WASTEORDER
* | [--->] IO_MSGLOG                      TYPE REF TO /PLCE/CL_APPLLOG_HELPER(optional)
* | [<---] ET_SRV_MAPPING                 TYPE        /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WDOI_SRV_MAPPING
* | [<---] ET_SRV_KEY                     TYPE        /PLCE/PPDSRV_KEY
* | [!CX!] /PLCE/CX_BASEEXCEPTION
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method PROCESS_WASTEORDERS.
    clear:
      AT_SERVICE_TO_CREATE.

    super->PROCESS_WASTEORDERS(
      exporting
        IV_PROFILE     = IV_PROFILE
        IT_WASTEORDER  = IT_WASTEORDER
        IO_MSGLOG      = IO_MSGLOG
      importing
        ET_SRV_MAPPING = ET_SRV_MAPPING
        ET_SRV_KEY     = ET_SRV_KEY
    ).
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PD_PROC_WASTE_ORDERS->SAVE_SERVICE
* +-------------------------------------------------------------------------------------------------+
* | [<---] ET_SERVICE_KEY                 TYPE        /PLCE/PPDSRV_KEY
* | [<---] ET_SRV_MAPPING                 TYPE        /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WDOI_SRV_MAPPING
* | [!CX!] /PLCE/CX_BASEEXCEPTION
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method SAVE_SERVICE.
    check AT_SERVICE_TO_CREATE is not initial.

    data: LT_SERVICE_TO_CREATE         type table for create /PLCE/R_PDSERVICE,
          LT_SERVICE_TASK_TO_CREATE    type table for create /PLCE/R_PDSERVICE\_SERVICETASK,
          LT_SERVICE_UNIVERSAL         type table for create /PLCE/R_PDSERVICE\_EXTUNIVERSAL,
          LT_SERVICE_WASTE             type table for create /PLCE/R_PDSERVICE\_EXTWASTE,
          LT_SERVICE_CUSTOM            type table for create /PLCE/R_PDSERVICE\_EXTCUSTOM,
          LT_SERVICE_TASK_TO_CREATE_UN type table for create /PLCE/R_PDSERVICETASK\_EXTUNIVERSAL,
          LT_SERVICE_TASK_TO_CREATE_WA type table for create /PLCE/R_PDSERVICETASK\_EXTWASTE,
          LT_SERVICE_NOTES_TO_CREATE   type table for create /PLCE/R_PDSERVICE\_Notes,
          LT_SERVICE_ATTAS_TO_CREATE   type table for create /PLCE/R_PDSERVICE\_Attachments,
          LV_SERVICETASKCID            type ABP_BEHV_CID,
          LINDEX                       type N length 8.

    loop at AT_SERVICE_TO_CREATE reference into data(LR_SERVICE_TO_CREATE).
      insert value #( %CID = LR_SERVICE_TO_CREATE->CID %DATA = LR_SERVICE_TO_CREATE->SERVICE ) into table LT_SERVICE_TO_CREATE.

      if LR_SERVICE_TO_CREATE->EXTUNIVERSAL is not initial.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_UNIVERSAL assigning field-symbol(<LS_SERVICE_UNIVERSAL>).
        insert value #( %CID = |{ LR_SERVICE_TO_CREATE->CID }_EXTUN| %DATA = LR_SERVICE_TO_CREATE->EXTUNIVERSAL ) into table <LS_SERVICE_UNIVERSAL>-%TARGET.
      endif.
      if LR_SERVICE_TO_CREATE->EXTWASTE is not initial.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_WASTE assigning field-symbol(<LS_SERVICE_WASTE>).
        insert value #( %CID = |{ LR_SERVICE_TO_CREATE->CID }_EXTWA| %DATA = LR_SERVICE_TO_CREATE->EXTWASTE ) into table <LS_SERVICE_WASTE>-%TARGET.
      endif.

      if LR_SERVICE_TO_CREATE->EXTCUSTOM is not initial.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_CUSTOM assigning field-symbol(<LS_SERVICE_CUSTOM>).
        insert value #( %CID = |{ LR_SERVICE_TO_CREATE->CID }_EXTCUSTOM| %DATA = LR_SERVICE_TO_CREATE->EXTCUSTOM  ) into table <LS_SERVICE_CUSTOM>-%TARGET.
      endif.

*      insert value #( %CID = LR_SERVICE_TO_CREATE->CID %DATA = LR_SERVICE_TO_CREATE->SERVICE ) into table LT_SERVICE_TO_CREATE.

      if LINES( LR_SERVICE_TO_CREATE->T_SERVICE_TASK ) > 0.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_TASK_TO_CREATE assigning field-symbol(<LS_SERVICE_TASK_TO_CREATE>).
        loop at LR_SERVICE_TO_CREATE->T_SERVICE_TASK reference into data(LR_SERVICE_TASK).
          concatenate LR_SERVICE_TO_CREATE->CID '_' LR_SERVICE_TASK->SERVICETASK-SEQUENCENUMBER into LV_SERVICETASKCID.
          insert value #( %CID = LV_SERVICETASKCID %DATA = LR_SERVICE_TASK->SERVICETASK ) into table <LS_SERVICE_TASK_TO_CREATE>-%TARGET.

          if LR_SERVICE_TASK->EXTUNIVERSAL is not initial.
            insert value #( %CID_REF = LV_SERVICETASKCID ) into table LT_SERVICE_TASK_TO_CREATE_UN assigning field-symbol(<LS_SERVICETASK_UNIVERSAL>).
            insert value #( %CID = |{ LV_SERVICETASKCID }_EXTUN| %DATA = LR_SERVICE_TASK->EXTUNIVERSAL ) into table <LS_SERVICETASK_UNIVERSAL>-%TARGET.
          endif.
          if LR_SERVICE_TO_CREATE->EXTWASTE is not initial.
            insert value #( %CID_REF = LV_SERVICETASKCID ) into table LT_SERVICE_TASK_TO_CREATE_WA assigning field-symbol(<LS_SERVICETASK_WASTE>).
            insert value #( %CID = |{ LV_SERVICETASKCID }_EXTWA| %DATA = LR_SERVICE_TASK->EXTWASTE ) into table <LS_SERVICETASK_WASTE>-%TARGET.
          endif.

        endloop.
      endif.
      if LINES( LR_SERVICE_TO_CREATE->T_SERVICE_NOTES ) > 0.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_NOTES_TO_CREATE assigning field-symbol(<LS_SERVICE_NOTES_TO_CREATE>).
        loop at LR_SERVICE_TO_CREATE->T_SERVICE_NOTES reference into data(LR_SERVICE_NOTE).
          LINDEX = LINDEX + 1.
          concatenate LR_SERVICE_TO_CREATE->CID '_NOTE' LINDEX into LV_SERVICETASKCID.
          insert value #( %CID = LV_SERVICETASKCID %DATA = LR_SERVICE_NOTE->SERVICE_NOTE ) into table <LS_SERVICE_NOTES_TO_CREATE>-%TARGET.
        endloop.
      endif.
      if LINES( LR_SERVICE_TO_CREATE->ATTACHMENTS ) > 0.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_ATTAS_TO_CREATE assigning field-symbol(<LS_SERVICE_ATTAS_TO_CREATE>).
        loop at LR_SERVICE_TO_CREATE->ATTACHMENTS reference into data(LR_SERVICE_ATTAREF).
          LINDEX = LINDEX + 1.
          concatenate LR_SERVICE_TO_CREATE->CID '_ATTA' LINDEX into LV_SERVICETASKCID.
          insert value #( %CID = LV_SERVICETASKCID %DATA = LR_SERVICE_ATTAREF->ATTACHMENT ) into table <LS_SERVICE_ATTAS_TO_CREATE>-%TARGET.
        endloop.
      endif.
    endloop.

    modify entities of /PLCE/R_PDSERVICE
      entity SERVICE
        create set fields with LT_SERVICE_TO_CREATE  ##SETFIELDS_OK
        create by \_SERVICETASK set fields with LT_SERVICE_TASK_TO_CREATE
*        create by \_ServiceTask ExtUniversal set fields with LT_SERVICE_TASK_TO_CREATE
        create by \_EXTUNIVERSAL set fields with LT_SERVICE_UNIVERSAL
        create by \_EXTWASTE set fields with LT_SERVICE_WASTE
        create by \_ExtCustom set fields with LT_SERVICE_CUSTOM
        create by \_Notes set fields with LT_SERVICE_NOTES_TO_CREATE
        create by \_Attachments set fields with LT_SERVICE_ATTAS_TO_CREATE
      entity SERVICETASK
        create by \_EXTUNIVERSAL set fields with LT_SERVICE_TASK_TO_CREATE_UN
        create by \_EXTWASTE set fields with LT_SERVICE_TASK_TO_CREATE_WA
      mapped data(MAPPED_CREATE)
      failed data(FAILED_CREATE)
      reported data(REPORTED_CREATE).


    loop at FAILED_CREATE-SERVICE assigning field-symbol(<FAILED_CREATE_SRVC>).
      FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
        exporting
          IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-ERROR
          IV_ID         = '/PLCE/MC_PD_MSG'
          IV_NUMBER     = '104'
          IV_VARIABLE_1 = conv #( text-005 )
          IV_VARIABLE_2 = conv #( <FAILED_CREATE_SRVC>-SERVICEUUID )
      ).
    endloop.

    commit entities
      response of /PLCE/R_PDSERVICE
        failed data(FAILED_COMMIT_SRVC)
        reported data(REPORTED_COMMIT_SRVC).

    if FAILED_COMMIT_SRVC is not initial or sy-subrc is not initial.
      loop at FAILED_COMMIT_SRVC-SERVICE assigning field-symbol(<FAILED_COMMIT_SRVC>).
        FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
          exporting
            IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-ERROR
            IV_ID         = '/PLCE/MC_PD_MSG'
            IV_NUMBER     = '106'
            IV_VARIABLE_1 = conv #( text-005 )
            IV_VARIABLE_2 = conv #( <FAILED_COMMIT_SRVC>-SERVICEUUID )
        ).
      endloop.
      rollback entities.                               "#EC CI_ROLLBACK
*      clear ET_SERVICE_KEY.
    else.
      if ET_SERVICE_KEY is requested or ET_SRV_MAPPING is requested.

        ET_SERVICE_KEY = corresponding #( MAPPED_CREATE-SERVICE ).
        loop at MAPPED_CREATE-SERVICE assigning field-symbol(<MAPPED_CREATE_SRVC>).
          ET_SERVICE_KEY = value #( base ET_SERVICE_KEY ( SERVICE_UUID = <MAPPED_CREATE_SRVC>-%KEY-SERVICEUUID ) ).
        endloop.

      endif.

      if ET_SRV_MAPPING is requested.

        read entities of /PLCE/R_PDSERVICE
          entity SERVICE
          fields ( SERVICEUUID REFERENCEINTERNALID )
*          with value #( for LLINE in MAPPED_CREATE-SERVICE ( %KEY-SERVICEUUID = LLINE-%KEY-SERVICEUUID ) )
          with corresponding #( MAPPED_CREATE-SERVICE )
          result data(LRS).

        ET_SRV_MAPPING = corresponding #( LRS mapping POBJNR = ReferenceInternalId SERVICE_UUID = ServiceUUID ).

      endif.
    endif.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_PD_PROC_WASTE_ORDERS=>ZZ_CHANGE_SERVICE_TYPE
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_KEY                        TYPE        /PLCE/SPDSRV_KEY
* | [--->] PAR_SVTPTO                     TYPE        /PLCE/PDSERVICE_TYPE
* | [--->] PAR_LOG                        TYPE REF TO /PLCE/CL_APPLLOG_HELPER(optional)
* | [--->] PAR_COMMIT                     TYPE        KENNZX(optional)
* | [--->] PAR_SIMULATE                   TYPE        KENNZX(optional)
* | [!CX!] /PLCE/CX_BASEEXCEPTION
* +--------------------------------------------------------------------------------------</SIGNATURE>
method ZZ_CHANGE_SERVICE_TYPE.
  data:
    LDATA type ZZ_TY_DATA.

  try.
      LDATA = ZZ_FILL_SERVICE_TASK(
        exporting
          PAR_KEY    = PAR_KEY
          PAR_SVTPTO = PAR_SVTPTO
          PAR_LOG    = PAR_LOG ).

* -------------------------------------------------------------------
      modify entities of /PLCE/R_PDService
        entity SERVICE
          update from corresponding #( LDATA-SRV_UPD )
          create by \_SERVICETASK set fields with LDATA-TSKS_CREA
        entity SERVICETASK
          delete from corresponding #( LDATA-TSKS_DELE )
          create by \_EXTWASTE set fields with LDATA-TSKS_CREA_WA
        entity EXTTASKUNIVERSAL
          delete from corresponding #( LDATA-TSKS_DELE_UN )
        entity EXTTASKWASTE
          delete from corresponding #( LDATA-TSKS_DELE_WA )
        mapped   data(LT_SRV_MAP)
        reported data(LT_SRV_REP)
        failed   data(LT_SRV_FAI) ##SETFIELDS_OK.

      if LT_SRV_FAI is initial.
        if PAR_SIMULATE is initial.
          commit entities
            response of /PLCE/R_PDSERVICE
              failed   data(SRV_COMMIT_FAI)
              reported data(SRV_COMMIT_REP).
        endif.
      endif.

      /PLCE/CL_BASE_MISC=>CHECK_RESPONSE(
        exporting
          IS_RESPONSE = LT_SRV_REP
          IR_LOG      = PAR_LOG  ).               " APPLOG

    catch /PLCE/CX_BASEEXCEPTION into data(LEX_PD).
      PAR_LOG->ADD_EXCEPTION( IV_SEVERITY = 'W' IRO_EXCEPTION = LEX_PD ).
*    catch CX_SADL_SHORTDUMP into data(LEX_SADL).
*      data(LX_ROOT_CAUSE) = new CX_SADL_CONTRACT_VIOLATION( ).
*      CL_ABAP_UNIT_ASSERT=>ASSERT_EQUALS( ACT = LEX_SADL->PREVIOUS
*                                          EXP = LX_ROOT_CAUSE ).
  endtry.

endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_PD_PROC_WASTE_ORDERS=>ZZ_FILL_SERVICE_TASK
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_KEY                        TYPE        /PLCE/SPDSRV_KEY
* | [--->] PAR_SVTPTO                     TYPE        /PLCE/PDSERVICE_TYPE
* | [--->] PAR_LOG                        TYPE REF TO /PLCE/CL_APPLLOG_HELPER(optional)
* | [<-()] PAR_RESULT                     TYPE        ZZ_TY_DATA
* | [!CX!] /PLCE/CX_BASEEXCEPTION
* +--------------------------------------------------------------------------------------</SIGNATURE>
method ZZ_FILL_SERVICE_TASK.
  data:
    LINDEX   type SYINDEX,
    LSRVKEYS type /PLCE/PPDSRV_KEY,
    LSEQNR   type /PLCE/SEQUENCE_NUMBER,
    LEX_PD   type ref to /PLCE/CX_BASEEXCEPTION,
    LPROC    type ref to ZCL_PD_PROC_WASTE_ORDERS.

  data:
    LDATA         type ZZ_TY_DATA,
    LACTION      type /PLCE/PDACTION,
    LWDO         type /PLCE/SWR_WASTEORDERITEM_IMP,
    LWDOS        type /PLCE/PWR_WASTEORDERITEM_IMP,
    LTSKS_CREA_X type /PLCE/CL_PD_PROC_WASTE_ORDERS=>TP_MODIFY_SERVICE_TASK_EXT ##NEEDED,
    LTSKS_WDOI   type standard table of /Plce/R_PDServicetask
                  with non-unique sorted key SEC_KEY components ServiceUUID SequenceNumber.

  try.
      LPROC = new #( ).
      LSRVKEYS = value #( ( PAR_KEY ) ).

* -------------------------------------------------------------------
      read entities of /PLCE/R_PDSERVICE
      entity SERVICE
        all fields "with corresponding #( LSRVKEYS mapping from entity )
          with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LSRV)
        by \_EXTUNIVERSAL all fields
          with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LSRV_UN)
        by \_EXTWASTE all fields
          with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LSRV_WA)
        by \_ExtCustom all fields
          with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LSRV_CT)
        by \_SERVICETASK all fields
          with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSTSK).

      read entities of /PLCE/R_PDSERVICE
        entity SERVICETASK
          by \_EXTWASTE all fields with corresponding #( LRSTSK )
          result data(LRSTSK_WA)
          by \_EXTUNIVERSAL all fields with corresponding #( LRSTSK )
          result data(LRSTSK_UN).

      PAR_RESULT-SRV_UPD    = corresponding #( LSRV ).
      PAR_RESULT-SRV_UPD_UN = corresponding #( LSRV_UN ).
      PAR_RESULT-SRV_UPD_WA = corresponding #( LSRV_WA ).
      PAR_RESULT-SRV_UPD_CUST = corresponding #( LSRV_CT ).

* -------------------------------------------------------------------
      create data LPROC->AD_SERVICE.
      create data LPROC->AD_SERVICE->SERVICE.
      if LINE_EXISTS( LSRV[ 1 ] ).
        LPROC->AD_SERVICE->SERVICE->* = corresponding #(  LSRV[ 1 ] ).
        LPROC->SET_PROFILE( LSRV[ 1 ]-PROFILE ).
        LWDO-POBJNR = LSRV[ 1 ]-REFERENCEINTERNALID. "POBJNR.
        LWDOS = value #( ( LWDO ) ).
        LPROC->SET_DATA( LWDOS ) .

        select single TMPL~ACTION from /PLCE/CPDSTMPL as TMPL "/PLCE/R_PDServiceTemplate
          inner join /PLCE/CPDACTION as ACT on TMPL~ACTION = ACT~ACTION "Z Action
          where PROFILE      eq @LPROC->FV_PROFILE
            and SERVICE_TYPE eq @PAR_SVTPTO
          into @LACTION.
        data(LST_TEMPLS)      = LPROC->READ_SERVICE_TEMPLATES( exporting IV_ACTION = LACTION ).
        data(LSTT_TEMPLS_OLD) = LPROC->READ_SERVICE_TASK_TEMPLATES( LPROC->AD_SERVICE->SERVICE->SERVICETYPE ).
        data(LSTT_TEMPLS_NEW) = LPROC->READ_SERVICE_TASK_TEMPLATES( PAR_SVTPTO ).
      endif.

      loop at PAR_RESULT-SRV_UPD assigning field-symbol(<S_UPD>) .
        <S_UPD>-ServiceType = PAR_SVTPTO.
        <S_UPD>-%CONTROL-ServiceType = IF_ABAP_BEHV=>MK-ON.
        <S_UPD>-Action = LACTION.
        <S_UPD>-%CONTROL-Action      = IF_ABAP_BEHV=>MK-ON.
      endloop.

* -------------------------------------------------------------------
      if 1 = 2.  "Renew service completly
        loop at LST_TEMPLS assigning field-symbol(<SERVICETMPL>)
          where SemanticTemplateName <> 'ST_SERVICE_NOTE'.
          LPROC->FILL_SERVICE_DATA(
            exporting
              IS_SERVICE_TEMPLATE = <SERVICETMPL> ).
        endloop.
      endif.

      loop at LRSTSK assigning field-symbol(<TASK>).
        if LINE_EXISTS( LSTT_TEMPLS_OLD[ SqncNr = <TASK>-SequenceNumber TaskType = <TASK>-TaskType ] ).
          PAR_RESULT-TSKS_DELE    = value #( base PAR_RESULT-TSKS_DELE ( corresponding #( <TASK> ) ) ).
          if LRSTSK_UN is not initial.
            PAR_RESULT-TSKS_DELE_UN = value #( base PAR_RESULT-TSKS_DELE_UN ( corresponding #( <TASK> ) ) ).
          endif.
          if LRSTSK_WA is not initial.
            PAR_RESULT-TSKS_DELE_WA = value #( base PAR_RESULT-TSKS_DELE_WA ( corresponding #( <TASK> ) ) ).
          endif.
        else.
          LTSKS_WDOI = value #( base LTSKS_WDOI ( corresponding #( <TASK> ) ) ).
        endif.
      endloop.

      select * from /PLCE/R_MDFuncLocMDCat
        into table @data(FUNCLOCCATS). "#EC CI_ALL_FIELDS_NEEDED "#EC CI_NOWHERE

      data(LWDOI_TASK_CNT) = LINES( LTSKS_WDOI ) .
      if LWDOI_TASK_CNT > 0.
        read table LTSKS_WDOI index 1 reference into data(TASK_WDOI_MIN).
        if LWDOI_TASK_CNT > 1.
          LINDEX = LWDOI_TASK_CNT.
          read table LTSKS_WDOI index LINDEX reference into data(TASK_WDOI_MAX).
        endif.
      endif.

      if 1 = 2. "Errors
        loop at LSTT_TEMPLS_NEW assigning field-symbol(<TASKTEMLATE_X>)
*        where TaskType <> 'ADDITIONAL_SERVICE'.
          where IsServiceTaskOptional is initial.

          append initial line to LTSKS_CREA_X assigning field-symbol(<TASK_CREA_X>).
          create data LPROC->FR_SERVICE_TASK.
          LPROC->FR_SERVICE_TASK->SERVICETASK  = ref #( <TASK_CREA_X>-SERVICETASK ).
          LPROC->FR_SERVICE_TASK->EXTUNIVERSAL = ref #( <TASK_CREA_X>-EXTUNIVERSAL ).
          LPROC->FR_SERVICE_TASK->EXTWASTE     = ref #( <TASK_CREA_X>-EXTWASTE ).
          LPROC->FILL_SERVICE_TASK_DATA( <TASKTEMLATE_X> ).
        endloop.
      endif.

      loop at LSTT_TEMPLS_NEW assigning field-symbol(<TASKTEMLATE>)
        where IsServiceTaskOptional is initial.

        append initial line to PAR_RESULT-TSKS_CREA assigning field-symbol(<TASK_CREA>).
        insert initial line into table <TASK_CREA>-%TARGET assigning field-symbol(<TARGET>).
        <TARGET> = corresponding #( <TASKTEMLATE> mapping SequenceNumber = SqncNr ).

        if LWDOI_TASK_CNT is not initial and
           <TARGET>-SequenceNumber >= TASK_WDOI_MIN->SequenceNumber and
           <TARGET>-SequenceNumber <= TASK_WDOI_MAX->SequenceNumber.

          if LSEQNR is initial.
            LSEQNR = TASK_WDOI_MAX->SequenceNumber + 10.
          else.
            LSEQNR += 10.
          endif.
          <TARGET>-SequenceNumber   = LSEQNR.
        endif.

        case <TARGET>-FUNCTIONALLOCATIONCATEGORY.
          when /PLCE/IF_MD_CONSTANTS=>C_FUNCLOC_MDCATEGORIES-CUSTOMERLOCATION.
*           "filled already
          when /PLCE/IF_MD_CONSTANTS=>C_FUNCLOC_MDCATEGORIES-MATERIALDEPOT or
               /PLCE/IF_MD_CONSTANTS=>C_FUNCLOC_MDCATEGORIES-TRANSPORTPACKAGINGDEPOT.
            data(FLC) = value #( FUNCLOCCATS[ FUNCLOCMDCATEGORY = <TARGET>-FUNCTIONALLOCATIONCATEGORY ] optional ).
            if SY-SUBRC is initial.
              <TARGET>-DURATION     = FLC-STAYDURATION.
              <TARGET>-DURATIONUNIT = FLC-STAYDURATIONUNIT.
            endif.
          when others.
        endcase.

        <TARGET>-%CID = LPROC->GET_NEXT_CID( ).
*        <TARGET>-ServiceUUID = LPROC->FR_SERVICE->SERVICE->ServiceUUID. "must be empty for create
        <TASK_CREA>-ServiceUUID = LPROC->AD_SERVICE->SERVICE->ServiceUUID.

        /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = ref #( <TARGET> ) ).
*        <TARGET>-%CONTROL-ServiceUUID = IF_ABAP_BEHV=>MK-OFF.
      endloop.

*LDATA-SRV_UPD
*LDATA-TSKS_CREA
*LDATA-TSKS_CREA_WA
*LDATA-TSKS_DELE
*LDATA-TSKS_DELE_UN
*LDATA-TSKS_DELE_WA

    catch /PLCE/CX_BASEEXCEPTION into LEX_PD.
      PAR_LOG->ADD_EXCEPTION( IV_SEVERITY = 'W' IRO_EXCEPTION = LEX_PD ).
*    catch CX_SADL_SHORTDUMP into data(LEX_SADL).
*      data(LX_ROOT_CAUSE) = new CX_SADL_CONTRACT_VIOLATION( ).
*      CL_ABAP_UNIT_ASSERT=>ASSERT_EQUALS( ACT = LEX_SADL->PREVIOUS
*                                          EXP = LX_ROOT_CAUSE ).
  endtry.

endmethod.
ENDCLASS.
