Log-dbg.js:499 2025-09-02 08:18:18.740300 Failed to enhance query options for auto-$expand/$select as the path '/ZI_WR_WEIGHBRIDGE($uid=id-1756793898740-50)/Vbeln' does not point to a property -  sap.ui.model.odata.v4.ODataParentBinding
Log-dbg.js:499 2025-09-02 08:18:18.745199 Failed to enhance query options for auto-$expand/$select as the path '/ZI_WR_WEIGHBRIDGE($uid=id-1756793898740-50)/Sessionid' does not point to a property -  sap.ui.model.odata.v4.ODataParentBinding
Log-dbg.js:499 2025-09-02 08:18:18.748800 Failed to drill-down into ($uid=id-1756793898740-50)/Vbeln, invalid segment: Vbeln - /sap/opu/odata4/sap/zsb_weighingbrige1_v4/srvd/sap/zsb_wr_weighingbrige1/0001/ZI_WR_WEIGHBRIDGE?$select=IsActiveEntity,SalesDocument sap.ui.model.odata.v4.lib._Cache
Log-dbg.js:499 2025-09-02 08:18:18.749500 Failed to drill-down into ($uid=id-1756793898740-50)/Sessionid, invalid segment: Sessionid - /sap/opu/odata4/sap/zsb_weighingbrige1_v4/srvd/sap/zsb_wr_weighingbrige1/0001/ZI_WR_WEIGHBRIDGE?$select=IsActiveEntity,SalesDocument sap.ui.model.odata.v4.lib._Cache
Log-dbg.js:499 2025-09-02 08:18:18.751199 FormatException in property 'value' of 'Element sap.m.Input#com.prologa.zwrweighbrigefinal::ZI_WR_WEIGHBRIDGEMain--ipContract': Type 'sap.ui.model.odata.type.Raw' does not support formatting
Hint: single properties referenced in composite bindings and within binding expressions are automatically converted into the type of the bound control property, unless a different 'targetType' is specified. targetType:'any' may avoid the conversion and lead to the expected behavior. -  
jquery-dbg.js:10224 
 POST https://port8080-workspaces-ws-bhqcv.eu10.applicationstudio.cloud.sap/sap/o…data4/sap/zsb_weighingbrige1_v4/srvd/sap/zsb_wr_weighingbrige1/0001/$batch 500 (Internal Server Error)
Log-dbg.js:499 2025-09-02 08:18:19.391300 POST on 'ZI_WR_WEIGHBRIDGE' failed; will be repeated automatically - Error: HTTP request was not processed because $batch failed
    at /resources/sap/ui/co…v4.ODataListBinding
Log-dbg.js:499 2025-09-02 08:18:19.392800 $batch failed - ABAP Runtime error 'RAISE_SHORTDUMP'
Error: Communication error: 500 error
    at Object.createError (_Helper-dbg.js:582:15)
    at Object.<anonymous> (_Requestor-dbg.js:2061:24)
    at Object.<anonymous> (jquery-compat-dbg.js:725:30)
    at u (jquery-dbg.js:3500:31)
    at Object.fireWith [as rejectWith] (jquery-dbg.js:3630:7)
    at k (jquery-dbg.js:9913:14)
    at /test/XMLHttpRequest…odata.v4.ODataModel
Log-dbg.js:499 2025-09-02 08:18:19.394000 Failed to get contexts for /sap/opu/odata4/sap/zsb_weighingbrige1_v4/srvd/sap/zsb_wr_weighingbrige1/0001/ZI_WR_SALESITEM_CONTRACTVH with start index 0 and length 100 - Error: HTTP request was not processed because $batch failed
    at /resources/sap/ui/co…v4.ODataListBinding
Log-dbg.js:499 2025-09-02 08:18:19.394600 Failed to get contexts for /sap/opu/odata4/sap/zsb_weighingbrige1_v4/srvd/sap/zsb_wr_weighingbrige1/0001/ZI_WR_SALESITEM_CONTRACTVH with start index 0 and length 100 - Error: HTTP request was not processed because $batch failed
    at /resources/sap/ui/co…v4.ODataListBinding

﻿


<?xml version="1.0" encoding="utf-8"?>
<edmx:Edmx Version="4.0" xmlns:edmx="http://docs.oasis-open.org/odata/ns/edmx" xmlns="http://docs.oasis-open.org/odata/ns/edm">
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
        <Schema Namespace="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001" Alias="SAP__self">
            <Annotation Term="SAP__core.SchemaVersion" String="1.0.0"/>
            <EntityType Name="ZI_WR_WEIGHBRIDGEType">
                <Key>
                    <PropertyRef Name="SalesDocument"/>
                    <PropertyRef Name="IsActiveEntity"/>
                </Key>
                <Property Name="SalesDocument" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Property Name="SDDocumentCategory" Type="Edm.String" Nullable="false" MaxLength="4"/>
                <Property Name="SalesDocumentType" Type="Edm.String" Nullable="false" MaxLength="4"/>
                <Property Name="HasDraftEntity" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="DraftEntityCreationDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="DraftEntityLastChangeDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="HasActiveEntity" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="IsActiveEntity" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="__EntityControl" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.EntityControl"/>
                <Property Name="__OperationControl" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEOperationControl"/>
                <Property Name="SAP__Messages" Type="Collection(com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.SAP__Message)" Nullable="false"/>
                <NavigationProperty Name="DraftAdministrativeData" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.I_DraftAdministrativeDataType"/>
                <NavigationProperty Name="SiblingEntity" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType"/>
            </EntityType>
            <EntityType Name="I_DraftAdministrativeDataType">
                <Key>
                    <PropertyRef Name="DraftUUID"/>
                    <PropertyRef Name="DraftEntityType"/>
                </Key>
                <Property Name="DraftUUID" Type="Edm.Guid" Nullable="false"/>
                <Property Name="DraftEntityType" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="CreationDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="CreatedByUser" Type="Edm.String" Nullable="false" MaxLength="12"/>
                <Property Name="LastChangeDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="LastChangedByUser" Type="Edm.String" Nullable="false" MaxLength="12"/>
                <Property Name="DraftAccessType" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <Property Name="ProcessingStartDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="InProcessByUser" Type="Edm.String" Nullable="false" MaxLength="12"/>
                <Property Name="DraftIsKeptByUser" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="EnqueueStartDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="DraftIsCreatedByMe" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="DraftIsLastChangedByMe" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="DraftIsProcessedByMe" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="CreatedByUserDescription" Type="Edm.String" Nullable="false" MaxLength="80"/>
                <Property Name="LastChangedByUserDescription" Type="Edm.String" Nullable="false" MaxLength="80"/>
                <Property Name="InProcessByUserDescription" Type="Edm.String" Nullable="false" MaxLength="80"/>
            </EntityType>
            <EntityType Name="ZI_WR_SALESITEM_CONTRACTVHType">
                <Key>
                    <PropertyRef Name="SalesOrder"/>
                    <PropertyRef Name="SalesOrderitem"/>
                </Key>
                <Property Name="SalesOrder" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Property Name="SalesOrderitem" Type="Edm.String" Nullable="false" MaxLength="6"/>
                <Property Name="Material" Type="Edm.String" Nullable="false" MaxLength="18"/>
                <Property Name="MaterialText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="Language" Type="Edm.String" Nullable="false" MaxLength="2"/>
            </EntityType>
            <ComplexType Name="ZI_WR_WEIGHBRIDGEOperationControl">
                <Property Name="Edit" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="identifyCard" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="NextStep" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="setloadType" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="Submit" Type="Edm.Boolean" Nullable="false"/>
            </ComplexType>
            <ComplexType Name="EntityControl">
                <Property Name="Deletable" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="Updatable" Type="Edm.Boolean" Nullable="false"/>
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
            <Action Name="Submit" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="Resume" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="Activate" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="identifyCard" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
                <Parameter Name="vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="Discard" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="Prepare" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="NextStep" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="Edit" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
                <Parameter Name="PreserveChanges" Type="Edm.Boolean" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="setloadType" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
                <Parameter Name="vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <EntityContainer Name="Container">
                <EntitySet Name="I_DraftAdministrativeData" EntityType="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.I_DraftAdministrativeDataType"/>
                <EntitySet Name="ZI_WR_SALESITEM_CONTRACTVH" EntityType="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_SALESITEM_CONTRACTVHType"/>
                <EntitySet Name="ZI_WR_WEIGHBRIDGE" EntityType="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.ZI_WR_WEIGHBRIDGEType">
                    <NavigationPropertyBinding Path="DraftAdministrativeData" Target="I_DraftAdministrativeData"/>
                    <NavigationPropertyBinding Path="SiblingEntity" Target="ZI_WR_WEIGHBRIDGE"/>
                </EntitySet>
            </EntityContainer>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/SalesDocument">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/SalesDocumentType">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Sales Document Type"/>
                <Annotation Term="SAP__common.Heading" String="SaTy"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/HasDraftEntity">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Has Draft"/>
                <Annotation Term="SAP__common.QuickInfo" String="Draft - Indicator - Has draft document"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/DraftEntityCreationDateTime">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Created On"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/DraftEntityLastChangeDateTime">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Last Changed On"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/HasActiveEntity">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Has active"/>
                <Annotation Term="SAP__common.QuickInfo" String="Draft - Indicator - Has active document"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/IsActiveEntity">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Is active"/>
                <Annotation Term="SAP__common.QuickInfo" String="Draft - Indicator - Is active document"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__EntityControl">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__OperationControl">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/ZI_WR_WEIGHBRIDGE">
                <Annotation Term="SAP__common.DraftRoot">
                    <Record>
                        <PropertyValue Property="ActivationAction" String="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.Activate"/>
                        <PropertyValue Property="DiscardAction" String="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.Discard"/>
                        <PropertyValue Property="PreparationAction" String="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.Prepare"/>
                        <PropertyValue Property="EditAction" String="com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.Edit"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.SearchRestrictions">
                    <Record>
                        <PropertyValue Property="Searchable" Bool="false"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.FilterRestrictions">
                    <Record>
                        <PropertyValue Property="Filterable" Bool="true"/>
                        <PropertyValue Property="FilterExpressionRestrictions">
                            <Collection>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="DraftEntityCreationDateTime"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="DraftEntityLastChangeDateTime"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="NonFilterableProperties">
                            <Collection>
                                <PropertyPath>__EntityControl</PropertyPath>
                                <PropertyPath>__OperationControl</PropertyPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.SortRestrictions">
                    <Record>
                        <PropertyValue Property="NonSortableProperties">
                            <Collection>
                                <PropertyPath>__EntityControl</PropertyPath>
                                <PropertyPath>__OperationControl</PropertyPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.DeleteRestrictions">
                    <Record>
                        <PropertyValue Property="Deletable" Path="__EntityControl/Deletable"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.UpdateRestrictions">
                    <Record>
                        <PropertyValue Property="Updatable" Path="__EntityControl/Updatable"/>
                        <PropertyValue Property="QueryOptions">
                            <Record>
                                <PropertyValue Property="SelectSupported" Bool="true"/>
                            </Record>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__core.OptimisticConcurrency">
                    <Collection>
                        <PropertyPath>SalesDocument</PropertyPath>
                    </Collection>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.Submit(SAP__self.ZI_WR_WEIGHBRIDGEType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/Submit"/>
            </Annotations>
            <Annotations Target="SAP__self.identifyCard(SAP__self.ZI_WR_WEIGHBRIDGEType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/identifyCard"/>
            </Annotations>
            <Annotations Target="SAP__self.NextStep(SAP__self.ZI_WR_WEIGHBRIDGEType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/NextStep"/>
            </Annotations>
            <Annotations Target="SAP__self.Edit(SAP__self.ZI_WR_WEIGHBRIDGEType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/Edit"/>
            </Annotations>
            <Annotations Target="SAP__self.setloadType(SAP__self.ZI_WR_WEIGHBRIDGEType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/setloadType"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType">
                <Annotation Term="SAP__common.Label" String="Interface View for weighbridge"/>
                <Annotation Term="SAP__UI.SelectionFields">
                    <Collection>
                        <PropertyPath>SalesDocumentType</PropertyPath>
                        <PropertyPath>SDDocumentCategory</PropertyPath>
                        <PropertyPath>SalesDocument</PropertyPath>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Messages" Path="SAP__Messages"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/DraftEntityType">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Entity ID"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/CreatedByUser">
                <Annotation Term="SAP__common.Text" Path="CreatedByUserDescription"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/sap/i_draftadministrativeuservh/0001;ps='srvd-zsb_wr_weighingbrige1-0001';va='com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.et-i_draftadministrativedata.createdbyuser'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Label" String="Draft Created By"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/LastChangedByUser">
                <Annotation Term="SAP__common.Text" Path="LastChangedByUserDescription"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/sap/i_draftadministrativeuservh/0001;ps='srvd-zsb_wr_weighingbrige1-0001';va='com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.et-i_draftadministrativedata.lastchangedbyuser'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Label" String="Draft Last Changed By"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/DraftAccessType">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Access Type"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/InProcessByUser">
                <Annotation Term="SAP__common.Text" Path="InProcessByUserDescription"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft In Process By"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType">
                <Annotation Term="SAP__common.Label" String="Draft Administration Data"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/I_DraftAdministrativeData">
                <Annotation Term="SAP__capabilities.SearchRestrictions">
                    <Record>
                        <PropertyValue Property="Searchable" Bool="true"/>
                        <PropertyValue Property="UnsupportedExpressions" EnumMember="SAP__capabilities.SearchExpressions/AND SAP__capabilities.SearchExpressions/OR SAP__capabilities.SearchExpressions/NOT SAP__capabilities.SearchExpressions/group SAP__capabilities.SearchExpressions/phrase"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.FilterRestrictions">
                    <Record>
                        <PropertyValue Property="Filterable" Bool="true"/>
                        <PropertyValue Property="FilterExpressionRestrictions">
                            <Collection>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="CreationDateTime"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="LastChangeDateTime"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
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
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/SalesOrder">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/SalesOrderitem">
                <Annotation Term="SAP__common.IsDigitSequence"/>
                <Annotation Term="SAP__common.Label" String="Sales Document Item"/>
                <Annotation Term="SAP__common.Heading" String="Item"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/Material">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Material"/>
                <Annotation Term="SAP__common.QuickInfo" String="Material Number"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType">
                <Annotation Term="SAP__common.Label" String="Materials of Contract"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/ZI_WR_SALESITEM_CONTRACTVH">
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
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/DraftUUID">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft (Technical ID)"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/ProcessingStartDateTime">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft In Process Since"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/DraftIsKeptByUser">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Is Kept By User"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/EnqueueStartDateTime">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Locked Since"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/DraftIsCreatedByMe">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Created By Me"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/DraftIsLastChangedByMe">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Last Changed By Me"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/DraftIsProcessedByMe">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft In Process By Me"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/CreatedByUserDescription">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Created By (Description)"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/LastChangedByUserDescription">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Last Changed By (Description)"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/InProcessByUserDescription">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft In Process By (Description)"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/SDDocumentCategory">
                <Annotation Term="SAP__common.Label" String="SD Document Category"/>
                <Annotation Term="SAP__common.Heading" String="Doc.Cat."/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__EntityControl/Deletable">
                <Annotation Term="SAP__common.Label" String="Dyn. Method Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Method Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Method Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__EntityControl/Updatable">
                <Annotation Term="SAP__common.Label" String="Dyn. Method Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Method Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Method Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__OperationControl/Edit">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__OperationControl/identifyCard">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__OperationControl/NextStep">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__OperationControl/setloadType">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEType/__OperationControl/Submit">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/CreationDateTime">
                <Annotation Term="SAP__common.Label" String="Draft Created On"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/LastChangeDateTime">
                <Annotation Term="SAP__common.Label" String="Draft Last Changed On"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/MaterialText">
                <Annotation Term="SAP__common.Label" String="Material Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/Language">
                <Annotation Term="SAP__common.Label" String="Language Key"/>
                <Annotation Term="SAP__common.Heading" String="Language"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEOperationControl/Edit">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEOperationControl/identifyCard">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEOperationControl/NextStep">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEOperationControl/setloadType">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHBRIDGEOperationControl/Submit">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.EntityControl/Deletable">
                <Annotation Term="SAP__common.Label" String="Dyn. Method Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Method Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Method Property"/>
            </Annotations>
            <Annotations Target="SAP__self.EntityControl/Updatable">
                <Annotation Term="SAP__common.Label" String="Dyn. Method Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Method Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Method Property"/>
            </Annotations>
            <Annotations Target="SAP__self.identifyCard(SAP__self.ZI_WR_WEIGHBRIDGEType)/vbeln">
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.Edit(SAP__self.ZI_WR_WEIGHBRIDGEType)/PreserveChanges">
                <Annotation Term="SAP__common.Label" String="TRUE"/>
                <Annotation Term="SAP__common.QuickInfo" String="Data element for domain BOOLE: TRUE (='X') and FALSE (=' ')"/>
            </Annotations>
            <Annotations Target="SAP__self.setloadType(SAP__self.ZI_WR_WEIGHBRIDGEType)/vbeln">
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
        </Schema>
    </edmx:DataServices>
</edmx:Edmx>



The Controller
sap.ui.define(
    [
        "sap/fe/core/PageController",
        "sap/m/MessageToast",
        "sap/ui/core/Messaging",
        "sap/m/MessageBox"

    ],
    function (PageController, MessageToast, Messaging, MessageBox) {
        'use strict';

        return PageController.extend('com.prologa.zwrweighbrigefinal.ext.main.Main', {
            onInit: function () {
                var parentReturn = PageController.prototype.onInit.apply(this);

                // Messaging
                Messaging.registerObject(this.getView(), true);
                this.getView().setModel(Messaging.getMessageModel(), "message");
                // Wizard
                this.oWizard = this.byId("weighingWizard");
                // Router
                var oRouter = this.getAppComponent().getRouter();
                oRouter.getRoute("ZI_WR_WEIGHBRIDGEMain").attachPatternMatched(this._onObjectMatched, this);

                this._enterWired = false; // guard so we wire Enter only once
                this._isFromScan = true;
                return parentReturn;
            },

             _onObjectMatched: function () {
                const oModel = this.getView().getModel();
                // Bind to the entity set
                const oListBinding = oModel.bindList("/ZI_WR_WEIGHBRIDGE");

                // Create a new (draft) context — do NOT pass unknown props or computed keys
                // SalesDocument is computed, IsActiveEntity is handled by RAP draft, so pass {}
                const oNewContext = oListBinding.create({});

                // Make the new context the view context so your controls bind to it
                this.getView().setBindingContext(oNewContext);

                // Request only properties that exist in metadata
                // (SalesDocument is computed; others are present in the entity type)
                oNewContext.requestProperty([
                    "SalesDocument",
                    "IsActiveEntity",
                    "SalesDocumentType",
                    "SDDocumentCategory"
                ]).catch(function () {
                    // swallow; optional: add your message handling here
                });
            } 

        });
    });


The view

<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" >

      <!-- STEP 1: Identification -->
      <WizardStep id="step1" title="Identification" >
        <HBox id="step1HBox" width="100%" justifyContent="Center">
          <Panel id="step1Panel" class="stepPanel" expandable="false"> <!-- headerText="1. Identification"> -->
            <content>
              <f:Form id="step1Form" editable="true">
                <f:layout><f:ResponsiveGridLayout id="tgrid"/></f:layout>
                <f:formContainers>
                  <f:FormContainer id="step1FormContainer">
                    <f:formElements>
                      <f:FormElement id="step1FormElementContract" label="Please Enter your Contract ID">
                        <f:fields>
                         <Input id="ipContract" value="{SalesDocument}" width="80%" maxLength="10" required="true" placeholder="Scan or enter Contract ID" class="sapUiSizeCompact" change=".onContractChange"/>   
                          <HBox id="step1ScanRow" width="100%" justifyContent="SpaceBetween" class="sapUiMediumMarginTop">
                            <Button id="btnScanCard"
                                    width="12rem"
                                    type="Emphasized"
                                    icon="sap-icon://business-card"
                                    text="Scan Card"
                                    press="onScanCard"/>
                          </HBox>
                        </f:fields>
                      </f:FormElement>
                    </f:formElements>
                  </f:FormContainer>
                </f:formContainers>
              </f:Form>
              <HBox id="step1Buttons" justifyContent="Start" width="100%" class="sapUiMediumMarginTop">
                <Button id="btnStep1Next" text="Next" type="Emphasized" press="onNextStep" visible="false"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 2: Choose Load Type -->
      <WizardStep id="step2" title="Choose Load Type"> <!--  validated="{= ${viewModel>/step2_ok} ? true : false }" -->
        <HBox id="step2HBox" width="100%" justifyContent="Center">
          <Panel id="step2Panel" class="stepPanel" expandable="false"> <!-- headerText="2. Choose Load Type"> -->
            <content>
              <VBox id="step2VBox">
                <!-- <Text id="step2Instruction" text="Position vehicle on weighbridge" class="sapUiSmallMarginBottom"/> -->

                <!-- Big tappable buttons from VH entity -->
                <VBox id="ltContainer"
                      items="{
                        path: '/ZI_WR_SALESITEM_CONTRACTVH',
                        parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                      }">
                  <Button
                    id="ltButton"
                    class="loadTypeBtn"
                    width="100%"
                    text="{= ${Material} + ' - ' + ${MaterialText} }"
                    press=".onChooseLoadType"/>
                </VBox>

                <!-- Show selection -->
                <Text id="step2SelectedText" class="sapUiSmallMarginTop"
                      text="{= ${binding>SalesOrder} ? 'Selected: ' + ${binding>SalesOrder} : ''}"/>

                <HBox id="step2Buttons" justifyContent="SpaceBetween" width="80%" class="sapUiMediumMarginTop">
                  <Button id="btnStep2Back" text="Back" press=".onBackToStep1"/>
                  <Button id="btnStep2Next" text="Next" type="Emphasized" press="onNextStep"/>
                </HBox>
              </VBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

</Wizard>
</content>
</Page>
</mvc:View>

