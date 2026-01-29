This XML file does not appear to have any style information associated with it. The document tree is shown below.
<edmx:Edmx xmlns:edmx="http://docs.oasis-open.org/odata/ns/edmx" xmlns="http://docs.oasis-open.org/odata/ns/edm" Version="4.0">
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_COMMUNICATION',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.Communication.v1" Alias="Communication"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_PERSONALDATA',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.PersonalData.v1" Alias="PersonalData"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_ANALYTICS',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.Analytics.v1" Alias="Analytics"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_COMMON',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.Common.v1" Alias="SAP__common"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_MEASURES',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="Org.OData.Measures.V1" Alias="SAP__measures"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_CORE',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="Org.OData.Core.V1" Alias="SAP__core"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_CAPABILITIES',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="Org.OData.Capabilities.V1" Alias="SAP__capabilities"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_AGGREGATION',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="Org.OData.Aggregation.V1" Alias="SAP__aggregation"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_VALIDATION',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="Org.OData.Validation.V1" Alias="SAP__validation"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_CODELIST',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.CodeList.v1" Alias="SAP__CodeList"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_UI',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.UI.v1" Alias="SAP__UI"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_HTML5',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.HTML5.v1" Alias="SAP__HTML5"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_PDF',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.PDF.v1" Alias="SAP__PDF"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_SESSION',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.Session.v1" Alias="SAP__session"/>
</edmx:Reference>
<edmx:Reference Uri="/sap/opu/odata/IWFND/CATALOGSERVICE;v=2/Vocabularies(TechnicalName='%2FIWBEP%2FVOC_HIERARCHY',Version='0001',SAP__Origin='LOCAL')/$value">
<edmx:Include Namespace="com.sap.vocabularies.Hierarchy.v1" Alias="SAP__hierarchy"/>
</edmx:Reference>
<edmx:DataServices>
<Schema Namespace="com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001" Alias="SAP__self">
<Annotation Term="SAP__core.SchemaVersion" String="1.0.0"/>
<EntityType Name="ZI_LANF_ROOTType">
<Key>
<PropertyRef Name="Techkey"/>
</Key>
<Property Name="Techkey" Type="Edm.String" Nullable="false" MaxLength="30"/>
<Property Name="SAP__Messages" Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.SAP__Message)" Nullable="false"/>
</EntityType>
<ComplexType Name="ZI_LANF_RESPONSE">
<Property Name="TechKey" Type="Edm.String" Nullable="false" MaxLength="1"/>
<Property Name="Vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
<Property Name="_Messages" Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_MESSAGE)" Nullable="false"/>
</ComplexType>
<ComplexType Name="ZI_LANF_POSITION_INPUT">
<Property Name="TechKey" Type="Edm.String" Nullable="false" MaxLength="1"/>
<Property Name="Matnr" Type="Edm.String" Nullable="false" MaxLength="40"/>
<Property Name="Menge" Type="Edm.Decimal" Nullable="false" Precision="13" Scale="3"/>
<Property Name="Meins" Type="Edm.String" Nullable="false" MaxLength="3"/>
</ComplexType>
<ComplexType Name="ZI_MESSAGE">
<Property Name="TechKey" Type="Edm.String" Nullable="false" MaxLength="1"/>
<Property Name="Msgid" Type="Edm.String" Nullable="false" MaxLength="20"/>
<Property Name="Msgno" Type="Edm.String" Nullable="false" MaxLength="3"/>
<Property Name="Msgty" Type="Edm.String" Nullable="false" MaxLength="1"/>
<Property Name="Msgv1" Type="Edm.String" Nullable="false" MaxLength="50"/>
<Property Name="Msgv2" Type="Edm.String" Nullable="false" MaxLength="50"/>
<Property Name="Msgv3" Type="Edm.String" Nullable="false" MaxLength="50"/>
<Property Name="Msgv4" Type="Edm.String" Nullable="false" MaxLength="50"/>
</ComplexType>
<ComplexType Name="SAP__Message">
<Property Name="code" Type="Edm.String" Nullable="false"/>
<Property Name="message" Type="Edm.String" Nullable="false"/>
<Property Name="target" Type="Edm.String"/>
<Property Name="additionalTargets" Type="Collection(Edm.String)" Nullable="false"/>
<Property Name="transition" Type="Edm.Boolean" Nullable="false"/>
<Property Name="numericSeverity" Type="Edm.Byte" Nullable="false"/>
<Property Name="longtextUrl" Type="Edm.String"/>
</ComplexType>
<Action Name="AttachDocument" IsBound="true">
<Parameter Name="_it" Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_ROOTType)" Nullable="false"/>
<Parameter Name="TechKey" Type="Edm.String" Nullable="false" MaxLength="1"/>
<Parameter Name="Vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
<Parameter Name="Title" Type="Edm.String" Nullable="false" MaxLength="100"/>
<Parameter Name="Url" Type="Edm.String" Nullable="false" MaxLength="132"/>
<ReturnType Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_RESPONSE)" Nullable="true"/>
</Action>
<Action Name="CreateLanf" IsBound="true">
<Parameter Name="_it" Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_ROOTType)" Nullable="false"/>
<Parameter Name="TechKey" Type="Edm.String" Nullable="false" MaxLength="1"/>
<Parameter Name="ContractVbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
<Parameter Name="DeliveryDate" Type="Edm.Date" Nullable="true"/>
<Parameter Name="CustomerRef" Type="Edm.String" Nullable="false" MaxLength="35"/>
<Parameter Name="Description" Type="Edm.String" Nullable="false" MaxLength="225"/>
<Parameter Name="_Positions" Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_POSITION_INPUT)" Nullable="false"/>
<ReturnType Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_RESPONSE)" Nullable="true"/>
</Action>
<EntityContainer Name="Container">
<EntitySet Name="ZI_LANF_ROOT" EntityType="com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_ROOTType"/>
</EntityContainer>
<Annotations Target="SAP__self.ZI_LANF_POSITION_INPUT/Menge">
<Annotation Term="SAP__measures.Unit" Path="Meins"/>
<Annotation Term="SAP__common.Label" String="Zielmenge"/>
<Annotation Term="SAP__common.QuickInfo" String="Zielmenge in Verkaufsmengeneinheit"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_ROOTType">
<Annotation Term="SAP__common.Label" String="Root for LANF"/>
<Annotation Term="SAP__common.Messages" Path="SAP__Messages"/>
</Annotations>
<Annotations Target="SAP__self.Container/ZI_LANF_ROOT">
<Annotation Term="SAP__capabilities.SearchRestrictions">
<Record>
<PropertyValue Property="Searchable" Bool="false"/>
</Record>
</Annotation>
<Annotation Term="SAP__capabilities.InsertRestrictions">
<Record>
<PropertyValue Property="Insertable" Bool="false"/>
</Record>
</Annotation>
<Annotation Term="SAP__capabilities.DeleteRestrictions">
<Record>
<PropertyValue Property="Deletable" Bool="false"/>
</Record>
</Annotation>
<Annotation Term="SAP__capabilities.UpdateRestrictions">
<Record>
<PropertyValue Property="Updatable" Bool="false"/>
<PropertyValue Property="QueryOptions">
<Record>
<PropertyValue Property="SelectSupported" Bool="true"/>
</Record>
</PropertyValue>
</Record>
</Annotation>
<Annotation Term="SAP__core.OptimisticConcurrency">
<Collection/>
</Annotation>
</Annotations>
<Annotations Target="SAP__self.Container">
<Annotation Term="SAP__aggregation.ApplySupported">
<Record>
<PropertyValue Property="Transformations">
<Collection>
<String>aggregate</String>
<String>groupby</String>
<String>filter</String>
</Collection>
</PropertyValue>
<PropertyValue Property="Rollup" EnumMember="SAP__aggregation.RollupType/None"/>
</Record>
</Annotation>
<Annotation Term="SAP__common.ApplyMultiUnitBehaviorForSortingAndFiltering" Bool="true"/>
<Annotation Term="SAP__capabilities.FilterFunctions">
<Collection>
<String>eq</String>
<String>ne</String>
<String>gt</String>
<String>ge</String>
<String>lt</String>
<String>le</String>
<String>and</String>
<String>or</String>
<String>contains</String>
<String>startswith</String>
<String>endswith</String>
<String>any</String>
<String>all</String>
</Collection>
</Annotation>
<Annotation Term="SAP__capabilities.SupportedFormats">
<Collection>
<String>application/json</String>
<String>application/pdf</String>
</Collection>
</Annotation>
<Annotation Term="SAP__PDF.Features">
<Record>
<PropertyValue Property="DocumentDescriptionReference" String="../../../../default/iwbep/common/0001/$metadata"/>
<PropertyValue Property="DocumentDescriptionCollection" String="MyDocumentDescriptions"/>
<PropertyValue Property="ArchiveFormat" Bool="true"/>
<PropertyValue Property="Border" Bool="true"/>
<PropertyValue Property="CoverPage" Bool="true"/>
<PropertyValue Property="FitToPage" Bool="true"/>
<PropertyValue Property="FontName" Bool="true"/>
<PropertyValue Property="FontSize" Bool="true"/>
<PropertyValue Property="Margin" Bool="true"/>
<PropertyValue Property="Padding" Bool="true"/>
<PropertyValue Property="Signature" Bool="true"/>
<PropertyValue Property="HeaderFooter" Bool="true"/>
<PropertyValue Property="ResultSizeDefault" Int="20000"/>
<PropertyValue Property="ResultSizeMaximum" Int="20000"/>
</Record>
</Annotation>
<Annotation Term="SAP__capabilities.KeyAsSegmentSupported"/>
<Annotation Term="SAP__capabilities.AsynchronousRequestsSupported"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_RESPONSE/Vbeln">
<Annotation Term="SAP__common.Label" String="Verkaufsbeleg"/>
<Annotation Term="SAP__common.Heading" String="Verkaufsb."/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_RESPONSE/_Messages/Msgid">
<Annotation Term="SAP__common.Label" String="Nachrichtenklasse"/>
<Annotation Term="SAP__common.QuickInfo" String="Nachrichtenidentifikation"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_RESPONSE/_Messages/Msgno">
<Annotation Term="SAP__common.Label" String="Nachricht"/>
<Annotation Term="SAP__common.Heading" String="MsgNr"/>
<Annotation Term="SAP__common.QuickInfo" String="Nachrichtennummer"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_RESPONSE/_Messages/Msgty">
<Annotation Term="SAP__common.Label" String="Nachrichtenart"/>
<Annotation Term="SAP__common.Heading" String="NA"/>
<Annotation Term="SAP__common.QuickInfo" String="Messageart"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_RESPONSE/_Messages/Msgv1">
<Annotation Term="SAP__common.Label" String="Nachrichtenvariable"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_RESPONSE/_Messages/Msgv2">
<Annotation Term="SAP__common.Label" String="Nachrichtenvariable"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_RESPONSE/_Messages/Msgv3">
<Annotation Term="SAP__common.Label" String="Nachrichtenvariable"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_RESPONSE/_Messages/Msgv4">
<Annotation Term="SAP__common.Label" String="Nachrichtenvariable"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_POSITION_INPUT/Matnr">
<Annotation Term="SAP__common.Label" String="Material"/>
<Annotation Term="SAP__common.QuickInfo" String="Materialnummer"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_POSITION_INPUT/Meins">
<Annotation Term="SAP__common.Label" String="Basismengeneinheit"/>
<Annotation Term="SAP__common.Heading" String="BME"/>
</Annotations>
<Annotations Target="SAP__self.ZI_MESSAGE/Msgid">
<Annotation Term="SAP__common.Label" String="Nachrichtenklasse"/>
<Annotation Term="SAP__common.QuickInfo" String="Nachrichtenidentifikation"/>
</Annotations>
<Annotations Target="SAP__self.ZI_MESSAGE/Msgno">
<Annotation Term="SAP__common.Label" String="Nachricht"/>
<Annotation Term="SAP__common.Heading" String="MsgNr"/>
<Annotation Term="SAP__common.QuickInfo" String="Nachrichtennummer"/>
</Annotations>
<Annotations Target="SAP__self.ZI_MESSAGE/Msgty">
<Annotation Term="SAP__common.Label" String="Nachrichtenart"/>
<Annotation Term="SAP__common.Heading" String="NA"/>
<Annotation Term="SAP__common.QuickInfo" String="Messageart"/>
</Annotations>
<Annotations Target="SAP__self.ZI_MESSAGE/Msgv1">
<Annotation Term="SAP__common.Label" String="Nachrichtenvariable"/>
</Annotations>
<Annotations Target="SAP__self.ZI_MESSAGE/Msgv2">
<Annotation Term="SAP__common.Label" String="Nachrichtenvariable"/>
</Annotations>
<Annotations Target="SAP__self.ZI_MESSAGE/Msgv3">
<Annotation Term="SAP__common.Label" String="Nachrichtenvariable"/>
</Annotations>
<Annotations Target="SAP__self.ZI_MESSAGE/Msgv4">
<Annotation Term="SAP__common.Label" String="Nachrichtenvariable"/>
</Annotations>
<Annotations Target="SAP__self.SAP__VIRTUAL_CO_TY_2/_Positions/Matnr">
<Annotation Term="SAP__common.Label" String="Material"/>
<Annotation Term="SAP__common.QuickInfo" String="Materialnummer"/>
</Annotations>
<Annotations Target="SAP__self.SAP__VIRTUAL_CO_TY_2/_Positions/Menge">
<Annotation Term="SAP__common.Label" String="Zielmenge"/>
<Annotation Term="SAP__common.QuickInfo" String="Zielmenge in Verkaufsmengeneinheit"/>
<Annotation Term="SAP__measures.Unit" Path="_Positions/Meins"/>
</Annotations>
<Annotations Target="SAP__self.SAP__VIRTUAL_CO_TY_2/_Positions/Meins">
<Annotation Term="SAP__common.Label" String="Basismengeneinheit"/>
<Annotation Term="SAP__common.Heading" String="BME"/>
</Annotations>
<Annotations Target="SAP__self.AttachDocument(Collection(SAP__self.ZI_LANF_ROOTType))/Vbeln">
<Annotation Term="SAP__common.Label" String="Verkaufsbeleg"/>
<Annotation Term="SAP__common.Heading" String="Verkaufsb."/>
</Annotations>
<Annotations Target="SAP__self.AttachDocument(Collection(SAP__self.ZI_LANF_ROOTType))/Title">
<Annotation Term="SAP__common.Label" String="Text"/>
<Annotation Term="SAP__common.QuickInfo" String="Text Länge 100"/>
</Annotations>
<Annotations Target="SAP__self.AttachDocument(Collection(SAP__self.ZI_LANF_ROOTType))/Url">
<Annotation Term="SAP__common.Label" String="URL"/>
<Annotation Term="SAP__common.QuickInfo" String="Uniform resource locator"/>
</Annotations>
<Annotations Target="SAP__self.CreateLanf(Collection(SAP__self.ZI_LANF_ROOTType))/ContractVbeln">
<Annotation Term="SAP__common.Label" String="Verkaufsbeleg"/>
<Annotation Term="SAP__common.Heading" String="Verkaufsb."/>
</Annotations>
<Annotations Target="SAP__self.CreateLanf(Collection(SAP__self.ZI_LANF_ROOTType))/DeliveryDate">
<Annotation Term="SAP__common.Label" String="Bestelldatum"/>
<Annotation Term="SAP__common.Heading" String="Bestelldat"/>
<Annotation Term="SAP__common.QuickInfo" String="Kundenauftragsbestelldatum"/>
</Annotations>
<Annotations Target="SAP__self.CreateLanf(Collection(SAP__self.ZI_LANF_ROOTType))/CustomerRef">
<Annotation Term="SAP__common.Label" String="Kundenreferenz"/>
</Annotations>
</Schema>
</edmx:DataServices>
</edmx:Edmx>



CLASS lhc_lanfroot DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    TYPES tt_lanf_position_input TYPE STANDARD TABLE OF zi_lanf_position_input WITH DEFAULT KEY.
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR lanfroot RESULT result.

    METHODS read FOR READ
      IMPORTING keys FOR READ lanfroot RESULT result.

    METHODS lock FOR LOCK
      IMPORTING keys FOR LOCK lanfroot.

    METHODS attachdocument FOR MODIFY
      IMPORTING keys FOR ACTION  lanfroot~attachdocument RESULT result.

    METHODS createlanf FOR MODIFY
      IMPORTING keys FOR ACTION lanfroot~createlanf RESULT result.

    METHODS map_positions_to_contract
      IMPORTING iv_contract_vbeln TYPE vbeln_va
                it_positions      TYPE tt_lanf_position_input
      CHANGING  ct_order_items    TYPE  bapisditm_tt
                ct_order_itemsx   TYPE  bapisditmx_tt
                ct_return         TYPE bapiret2_tab.


ENDCLASS.

CLASS lhc_lanfroot IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD read.
  ENDMETHOD.

  METHOD lock.
  ENDMETHOD.


METHOD createlanf.

  DATA:
    ls_header_in     TYPE bapisdhead,
    lt_items_in      TYPE bapisditm_tt,
    lt_items_inx    TYPE bapisditmx_tt,
    lt_partners      TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,
    lt_return        TYPE bapiret2_tab,
    ls_return1       TYPE bapireturn1,
    lv_vbeln         TYPE vbeln_va,
    ls_contract_hdr  TYPE vbak,
    lv_vbtyp         TYPE tvak-vbtyp,
    lv_busobj        TYPE swo_objtyp.

  DATA: lt_extensionin TYPE STANDARD TABLE OF bapiparex WITH DEFAULT KEY,
        ls_extensionin TYPE bapiparex,
        ls_bape_vbak   TYPE bape_vbak,
        ls_bape_vbakx  TYPE bape_vbakx.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  " Contract VBELN normalize
  DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.

  SELECT SINGLE * FROM vbak INTO @ls_contract_hdr WHERE vbeln = @lv_contract.
  IF sy-subrc <> 0.
    APPEND VALUE #(
      %msg = new_message(
        id = '00' number = '001'
        v1 = |Contract { lv_contract } not found|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-------------------------
  " Determine BUSINESS_OBJECT from AUART (DOC_TYPE)
  "-------------------------
  DATA(lv_auart) = 'ZLRA'.  "

  SELECT SINGLE vbtyp FROM tvak INTO @lv_vbtyp WHERE auart = @lv_auart.
  IF sy-subrc = 0 AND lv_vbtyp IS NOT INITIAL.
    CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
      EXPORTING i_document_type   = lv_vbtyp
      IMPORTING e_business_object = lv_busobj.
  ENDIF.
  IF lv_busobj IS INITIAL.
    lv_busobj = 'BUS2032'. "fallback
  ENDIF.

  "-------------------------
  " Fill header (only fields that exist in BAPISDHEAD)
  "-------------------------
  CLEAR ls_header_in.
  ls_header_in-doc_type   = lv_auart.
  ls_header_in-sales_org  = ls_contract_hdr-vkorg.
  ls_header_in-distr_chan = ls_contract_hdr-vtweg.
  ls_header_in-division   = ls_contract_hdr-spart.

  ls_header_in-ref_doc    = lv_contract.
  ls_header_in-ref_doc_ca  = 'G'.  "contract reference

  ls_header_in-req_date_h = ls_input-deliverydate.
  ls_header_in-purch_no_c = ls_input-customerref.

  "-------------------------
  " Partners (from contract)
  "-------------------------
  SELECT * FROM vbpa
    WHERE vbeln = @lv_contract
    INTO TABLE @DATA(lt_contract_partners).

  lt_partners = CORRESPONDING #( lt_contract_partners
    MAPPING partn_role = parvw
            partn_numb = kunnr ).

  "-------------------------
  " Items (map MATNR -> contract posnr)
  "-------------------------
  CLEAR: lt_items_in.
  map_positions_to_contract(
    EXPORTING
      iv_contract_vbeln = ls_input-ContractVbeln
      it_positions      = CORRESPONDING #( ls_input-_positions )
    CHANGING
      ct_order_items         = lt_items_in
       ct_order_itemsx   = lt_items_inx
      ct_return         = lt_return ).

  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return ASSIGNING FIELD-SYMBOL(<m>) WHERE type CA 'EA'.
      APPEND VALUE #(
        %msg = new_message(
          id = <m>-id number = <m>-number
          v1 = <m>-message_v1 v2 = <m>-message_v2 v3 = <m>-message_v3 v4 = <m>-message_v4
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-------------------------
  " Optional: set VBAK-KTEXT via EXTENSIONIN
  "-------------------------
  CLEAR: lt_extensionin, ls_bape_vbak, ls_bape_vbakx.

*  ls_bape_vbak-ktext  = ls_input-description.
*  ls_bape_vbakx-ktext = abap_true.

  ls_extensionin-structure  = 'BAPE_VBAK'.
  ls_extensionin-valuepart1 = ls_bape_vbak.
  APPEND ls_extensionin TO lt_extensionin.

  CLEAR ls_extensionin.
  ls_extensionin-structure  = 'BAPE_VBAKX'.
  ls_extensionin-valuepart1 = ls_bape_vbakx.
  APPEND ls_extensionin TO lt_extensionin.

  "-------------------------
  " Call BAPI (correct types!)
  "-------------------------
  CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
    EXPORTING
      order_header_in = ls_header_in
      business_object = lv_busobj
      without_commit  = abap_true
    IMPORTING
      salesdocument   = lv_vbeln
      return          = ls_return1
    TABLES
      order_items_in  = lt_items_in
      order_partners  = lt_partners
      extensionin     = lt_extensionin.

  " Merge RETURN1 into RET2 for your response mapping
  APPEND VALUE bapiret2(
    type       = ls_return1-type
    id         = ls_return1-id
    number     = ls_return1-number
    message    = ls_return1-message
    message_v1 = ls_return1-message_v1
    message_v2 = ls_return1-message_v2
    message_v3 = ls_return1-message_v3
    message_v4 = ls_return1-message_v4
  ) TO lt_return.

  IF ls_return1-type CA 'EAX'.
    CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.

    LOOP AT lt_return ASSIGNING <m> WHERE type CA 'EAX'.
      APPEND VALUE #(
        %msg = new_message(
          id = <m>-id number = <m>-number
          v1 = <m>-message_v1 v2 = <m>-message_v2 v3 = <m>-message_v3 v4 = <m>-message_v4
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = abap_true.

  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_vbeln
        _messages = VALUE #(
          FOR r IN lt_return (
            techkey = 'X'
            msgid   = r-id
            msgno   = r-number
            msgty   = r-type
            msgv1   = r-message_v1
            msgv2   = r-message_v2
            msgv3   = r-message_v3
            msgv4   = r-message_v4 ) ) ) ) ).

ENDMETHOD.


METHOD attachdocument.

  DATA: ls_input         TYPE zi_attach_input,
        lt_return_attach TYPE bapiret2_tab,
        lv_vbtyp         TYPE vbak-vbtyp,
        lv_busobj        TYPE swo_objtyp.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  ls_input = CORRESPONDING #( ls_key-%param ).

  DATA(lv_vbeln) = |{ ls_input-vbeln ALPHA = IN }|.

  " Determine SD doc category (VBTYP)
  SELECT SINGLE vbtyp FROM vbak
    WHERE vbeln = @lv_vbeln
    INTO @lv_vbtyp.

  IF sy-subrc <> 0.
    APPEND VALUE #( type = 'E' id = '00' number = '001'
      message = |Sales document { lv_vbeln } not found| ) TO lt_return_attach.
  ELSE.
    " Determine correct BOR object for that category (avoids BUS2032/L issue)
    CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
      EXPORTING
        i_document_type   = lv_vbtyp
      IMPORTING
        e_business_object = lv_busobj.
    " (This FM is commonly used to map SD category -> BOR object) :contentReference[oaicite:3]{index=3}

    " Attach URL as GOS/BDS link
    CALL FUNCTION 'BDS_BUSINESSDOCUMENT_CRE_O_URL'
      EXPORTING
        classname       = lv_busobj
        classtype       = 'BO'
        object_key      = lv_vbeln
        url             = ls_input-url
        url_description = ls_input-title
      EXCEPTIONS
        nothing_found   = 1
        parameter_error = 2
        not_allowed     = 3
        error_kpro      = 4
        internal_error  = 5
        OTHERS          = 6.

    IF sy-subrc <> 0.
      APPEND VALUE #( type = 'E' id = sy-msgid number = sy-msgno
        message_v1 = sy-msgv1 message_v2 = sy-msgv2 message_v3 = sy-msgv3 message_v4 = sy-msgv4
        message = |Attach URL failed (SY-SUBRC={ sy-subrc })| ) TO lt_return_attach.
    ELSE.
      APPEND VALUE #( type = 'S' id = '00' number = '000'
        message = |URL attached successfully| ) TO lt_return_attach.
    ENDIF.
  ENDIF.

  " Return VBELN + messages
  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_vbeln
        _messages = VALUE #(
          FOR r IN lt_return_attach (
            techkey = 'X'
            msgid   = r-id
            msgno   = r-number
            msgty   = r-type
            msgv1   = r-message_v1
            msgv2   = r-message_v2
            msgv3   = r-message_v3
            msgv4   = r-message_v4 ) ) ) ) ).

ENDMETHOD.

METHOD map_positions_to_contract.

  SELECT posnr, matnr
    FROM vbap
    WHERE vbeln = @iv_contract_vbeln
    INTO TABLE @DATA(lt_contract_pos).

  LOOP AT it_positions ASSIGNING FIELD-SYMBOL(<pos>) WHERE menge > 0.

    DATA(lv_matnr) = <pos>-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    READ TABLE lt_contract_pos ASSIGNING FIELD-SYMBOL(<contr_pos>)
      WITH KEY matnr = lv_matnr.

    IF sy-subrc = 0.
      APPEND VALUE bapisditm(
        itm_number = <contr_pos>-posnr
        material   = lv_matnr
        target_qty = <pos>-menge
        target_qu  = <pos>-meins
        ref_doc    = iv_contract_vbeln
        ref_doc_it = <contr_pos>-posnr
        ref_doc_ca = 'G' ) TO ct_order_items.

      APPEND VALUE bapisditmx(
        itm_number = <contr_pos>-posnr
        updateflag = 'I'
        material   = 'X'
        target_qty = 'X'
        target_qu  = 'X'
        ref_doc    = 'X'
        ref_doc_it = 'X'
        ref_doc_ca = 'X' ) TO ct_order_itemsx.
    ELSE.
      APPEND VALUE bapiret2(
        type   = 'E'
        id     = '00'
        number = '001'
        message = |No position for MATNR { lv_matnr } in contract { iv_contract_vbeln }| ) TO ct_return.
    ENDIF.

  ENDLOOP.

ENDMETHOD.



ENDCLASS.

CLASS lsc_zi_lanf_root DEFINITION INHERITING FROM cl_abap_behavior_saver.
  PROTECTED SECTION.

    METHODS finalize REDEFINITION.

    METHODS check_before_save REDEFINITION.

    METHODS save REDEFINITION.

    METHODS cleanup REDEFINITION.

    METHODS cleanup_finalize REDEFINITION.

ENDCLASS.

CLASS lsc_zi_lanf_root IMPLEMENTATION.

  METHOD finalize.
  ENDMETHOD.

  METHOD check_before_save.
  ENDMETHOD.

  METHOD save.
  ENDMETHOD.

  METHOD cleanup.
  ENDMETHOD.

  METHOD cleanup_finalize.
  ENDMETHOD.

ENDCLASS.



<img width="1484" height="678" alt="image" src="https://github.com/user-attachments/assets/f2721d68-bda9-443d-a5ef-345056d8b871" />

<img width="1409" height="293" alt="image" src="https://github.com/user-attachments/assets/1f1cf888-26ff-4726-bac0-62aaf5c52abe" />

