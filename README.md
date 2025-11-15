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
        <Schema Namespace="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001" Alias="SAP__self">
            <Annotation Term="SAP__core.SchemaVersion" String="1.0.0"/>
            <EntityType Name="ZI_WR_R_WEIGHBRIDGEType">
                <Key>
                    <PropertyRef Name="SalesDocument"/>
                </Key>
                <Property Name="SalesDocument" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Property Name="Korselsnr" Type="Edm.String" Nullable="false" MaxLength="4"/>
                <Property Name="Korselsnrindicator" Type="Edm.String" Nullable="false" MaxLength="4"/>
                <Property Name="PDFBASE64" Type="Edm.String" Nullable="false"/>
                <Property Name="CreatedByUser" Type="Edm.String" Nullable="false" MaxLength="12"/>
                <Property Name="LastChangedByUser" Type="Edm.String" Nullable="false" MaxLength="12"/>
                <Property Name="CreationDate" Type="Edm.Date"/>
                <Property Name="CreationTime" Type="Edm.TimeOfDay" Nullable="false"/>
                <Property Name="LastChangeDate" Type="Edm.Date"/>
                <Property Name="LastChangeDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="__OperationControl" Type="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_R_WEIGHBRIDGEOperationControl"/>
                <Property Name="SAP__Messages" Type="Collection(com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.SAP__Message)" Nullable="false"/>
            </EntityType>
            <EntityType Name="ZI_WR_SALESITEM_CONTRACTVHParameters">
                <Key>
                    <PropertyRef Name="P_SalesOrder"/>
                </Key>
                <Property Name="P_SalesOrder" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <NavigationProperty Name="Set" Type="Collection(com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_SALESITEM_CONTRACTVHType)" Partner="Parameters" ContainsTarget="true"/>
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
                <Property Name="Avvcode" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="Language" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <NavigationProperty Name="Parameters" Type="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_SALESITEM_CONTRACTVHParameters" Nullable="false"/>
            </EntityType>
            <ComplexType Name="ZI_WR_R_WEIGHBRIDGEOperationControl">
                <Property Name="determineWeight" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="identifyCard" Type="Edm.Boolean" Nullable="false"/>
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
            <Action Name="identifyCard" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_R_WEIGHBRIDGEType" Nullable="false"/>
                <Parameter Name="Vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_R_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Action Name="determineWeight" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_R_WEIGHBRIDGEType" Nullable="false"/>
                <Parameter Name="Vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Parameter Name="Loadtype" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_R_WEIGHBRIDGEType" Nullable="false"/>
            </Action>
            <Function Name="printSlip" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_R_WEIGHBRIDGEType" Nullable="false"/>
                <Parameter Name="Vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Parameter Name="Loadtype" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Parameter Name="Weight" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Parameter Name="WeighUnit" Type="Edm.String" Nullable="false" MaxLength="3"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_R_WEIGHBRIDGEType" Nullable="false"/>
            </Function>
            <EntityContainer Name="Container">
                <EntitySet Name="ZI_WR_R_WEIGHBRIDGE" EntityType="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_R_WEIGHBRIDGEType"/>
                <EntitySet Name="ZI_WR_SALESITEM_CONTRACTVH" EntityType="com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.ZI_WR_SALESITEM_CONTRACTVHParameters">
                    <NavigationPropertyBinding Path="Set/Parameters" Target="ZI_WR_SALESITEM_CONTRACTVH"/>
                </EntitySet>
            </EntityContainer>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/SalesDocument">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/Korselsnrindicator">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Sales Document Type"/>
                <Annotation Term="SAP__common.Heading" String="SaTy"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/CreatedByUser">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Created By"/>
                <Annotation Term="SAP__common.QuickInfo" String="Name of Person Responsible for Creating the Object"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/LastChangedByUser">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Last Changed By"/>
                <Annotation Term="SAP__common.Heading" String="Changed By"/>
                <Annotation Term="SAP__common.QuickInfo" String="User Who Last Changed the Business Document"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/__OperationControl">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.identifyCard(SAP__self.ZI_WR_R_WEIGHBRIDGEType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/identifyCard"/>
            </Annotations>
            <Annotations Target="SAP__self.determineWeight(SAP__self.ZI_WR_R_WEIGHBRIDGEType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/determineWeight"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType">
                <Annotation Term="SAP__common.Label" String="Interface View for weighbridge"/>
                <Annotation Term="SAP__common.Messages" Path="SAP__Messages"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/ZI_WR_R_WEIGHBRIDGE">
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
                                    <PropertyValue Property="Property" PropertyPath="PDFBASE64"/>
                                    <PropertyValue Property="AllowedExpressions" String="SearchExpression"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="NonFilterableProperties">
                            <Collection>
                                <PropertyPath>__OperationControl</PropertyPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.SortRestrictions">
                    <Record>
                        <PropertyValue Property="NonSortableProperties">
                            <Collection>
                                <PropertyPath>PDFBASE64</PropertyPath>
                                <PropertyPath>__OperationControl</PropertyPath>
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
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHParameters/P_SalesOrder">
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.QuickInfo" String="Sales Document"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.Container/ZI_WR_SALESITEM_CONTRACTVH">
                <Annotation Term="SAP__capabilities.NavigationRestrictions">
                    <Record>
                        <PropertyValue Property="RestrictedProperties">
                            <Collection>
                                <Record>
                                    <PropertyValue Property="NavigationProperty" NavigationPropertyPath="Set"/>
                                    <PropertyValue Property="InsertRestrictions">
                                        <Record>
                                            <PropertyValue Property="Insertable" Bool="false"/>
                                        </Record>
                                    </PropertyValue>
                                    <PropertyValue Property="UpdateRestrictions">
                                        <Record>
                                            <PropertyValue Property="Updatable" Bool="false"/>
                                        </Record>
                                    </PropertyValue>
                                    <PropertyValue Property="DeleteRestrictions">
                                        <Record>
                                            <PropertyValue Property="Deletable" Bool="false"/>
                                        </Record>
                                    </PropertyValue>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.SearchRestrictions">
                    <Record>
                        <PropertyValue Property="Searchable" Bool="false"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.FilterRestrictions">
                    <Record>
                        <PropertyValue Property="NonFilterableProperties">
                            <Collection>
                                <PropertyPath>P_SalesOrder</PropertyPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.SortRestrictions">
                    <Record>
                        <PropertyValue Property="NonSortableProperties">
                            <Collection>
                                <PropertyPath>P_SalesOrder</PropertyPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.ReadRestrictions">
                    <Record>
                        <PropertyValue Property="Readable" Bool="false"/>
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
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHParameters">
                <Annotation Term="SAP__common.ResultContext"/>
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
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/Avvcode">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="EWC code"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType">
                <Annotation Term="SAP__common.Label" String="Materials of Contract"/>
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
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/Korselsnr">
                <Annotation Term="SAP__common.Label" String="SD Document Category"/>
                <Annotation Term="SAP__common.Heading" String="Doc.Cat."/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/PDFBASE64">
                <Annotation Term="SAP__common.Label" String="String"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/CreationDate">
                <Annotation Term="SAP__common.Label" String="Created On"/>
                <Annotation Term="SAP__common.QuickInfo" String="Record Creation Date"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/CreationTime">
                <Annotation Term="SAP__common.Label" String="Created At"/>
                <Annotation Term="SAP__common.QuickInfo" String="Time at Which Record Was Created"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/LastChangeDate">
                <Annotation Term="SAP__common.Label" String="Changed On"/>
                <Annotation Term="SAP__common.Heading" String="Chngd On"/>
                <Annotation Term="SAP__common.QuickInfo" String="Last Changed On"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/LastChangeDateTime">
                <Annotation Term="SAP__common.Label" String="Time Stamp"/>
                <Annotation Term="SAP__common.QuickInfo" String="UTC Time Stamp in Long Form (YYYYMMDDhhmmssmmmuuun)"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/__OperationControl/determineWeight">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/__OperationControl/identifyCard">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/MaterialText">
                <Annotation Term="SAP__common.Label" String="Material Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/Language">
                <Annotation Term="SAP__common.Label" String="Language Key"/>
                <Annotation Term="SAP__common.Heading" String="Language"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEOperationControl/determineWeight">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEOperationControl/identifyCard">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.printSlip(SAP__self.ZI_WR_R_WEIGHBRIDGEType)/Vbeln">
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.printSlip(SAP__self.ZI_WR_R_WEIGHBRIDGEType)/Loadtype">
                <Annotation Term="SAP__common.Label" String="Material"/>
                <Annotation Term="SAP__common.QuickInfo" String="Material Number"/>
            </Annotations>
            <Annotations Target="SAP__self.identifyCard(SAP__self.ZI_WR_R_WEIGHBRIDGEType)/Vbeln">
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.determineWeight(SAP__self.ZI_WR_R_WEIGHBRIDGEType)/Vbeln">
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.determineWeight(SAP__self.ZI_WR_R_WEIGHBRIDGEType)/Loadtype">
                <Annotation Term="SAP__common.Label" String="Material"/>
                <Annotation Term="SAP__common.QuickInfo" String="Material Number"/>
            </Annotations>
        </Schema>
    </edmx:DataServices>
</edmx:Edmx>
