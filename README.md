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
                <Property Name="MasterSalesContract" Type="Edm.String" Nullable="false" MaxLength="10"/>
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
                <Property Name="Korselsnr" Type="Edm.String" Nullable="false" MaxLength="4"/>
                <Property Name="Korselsnrindicator" Type="Edm.String" Nullable="false" MaxLength="4"/>
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
            <Annotations Target="SAP__self.ZI_WR_R_WEIGHBRIDGEType/MasterSalesContract">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Master Contract"/>
                <Annotation Term="SAP__common.Heading" String="Mast.Contr."/>
                <Annotation Term="SAP__common.QuickInfo" String="Master Contract Number"/>
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
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/Korselsnrindicator">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Sales Document Type"/>
                <Annotation Term="SAP__common.Heading" String="SaTy"/>
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
            <Annotations Target="SAP__self.ZI_WR_SALESITEM_CONTRACTVHType/Korselsnr">
                <Annotation Term="SAP__common.Label" String="SD Document Category"/>
                <Annotation Term="SAP__common.Heading" String="Doc.Cat."/>
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



sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",
    "sap/ui/core/message/Message",
    "sap/ui/core/library",
    "sap/m/Button",
    "sap/ui/model/json/JSONModel",
    "sap/ui/model/resource/ResourceModel",
    "sap/ui/model/Filter",
    "sap/ui/model/FilterOperator",
    "jquery.sap.global"
], function (PageController, MessageToast, Messaging, Message, coreLibrary, Button, JSONModel, ResourceModel, Filter, FilterOperator, jQuery) {
    "use strict";
    return PageController.extend("com.prologa.zwrweighbrigefinal.ext.main.Main", {
        formatter: {
            concatMaterialText: function (sMaterial, sMaterialText) {
                return (sMaterial || '') + ' - ' + (sMaterialText || '');
            }
        },
        onInit: function () {
            PageController.prototype.onInit.apply(this);
            // Set up i18n model with initial language (English)
            var oResourceModel = new ResourceModel({
                bundleName: "com.prologa.zwrweighbrigefinal.i18n.i18n",
                bundleLocale: "en"
            });
            this.getView().setModel(oResourceModel, "@i18n");
            // Messaging
            Messaging.registerObject(this.getView(), true);
            this.getView().setModel(Messaging.getMessageModel(), "message");
            // Attach to message model change to intercept and handle messages
            this.oMessageModel = this.getView().getModel("message");
            this.oMessageModel.attachPropertyChange(this._onMessageChange, this);
            // Wizard
            this.oWizard = this.byId("weighingWizard");
            this.oWizard.attachStepActivate(this.onStepActivate, this);
            // Router
            var oRouter = this.getAppComponent().getRouter();
            oRouter.getRoute("ZI_WR_R_WEIGHBRIDGEMain").attachPatternMatched(this._onObjectMatched, this); // Updated route name based on new entity
            this._enterWired = false; // guard so we wire Enter only once
            this._isFromScan = true;
            // Local model for unbound inputs
            var oLocalModel = new JSONModel({
                contractId: "",
                grossWeight: "",
                teraWeight: "",
                loadType: "",
                transaction_state: "",
                korselsnr: ""  // NEW: Added for optional Korselsnr step
            });
            this.getView().setModel(oLocalModel, "local");
        },
        getResourceBundle: function () {
            return this.getView().getModel("@i18n").getResourceBundle();
        },
        onSetEnglish: function () {
            var oResourceModel = new ResourceModel({
                bundleName: "com.prologa.zwrweighbrigefinal.i18n.i18n",
                bundleLocale: "en"
            });
            this.getView().setModel(oResourceModel, "@i18n");
        },
        onSetDanish: function () {
            var oResourceModel = new ResourceModel({
                bundleName: "com.prologa.zwrweighbrigefinal.i18n.i18n",
                bundleLocale: "da"
            });
            this.getView().setModel(oResourceModel, "@i18n");
        },
        _onMessageChange: function (oEvent) {
            var aMessages = this.oMessageModel.getData() || [];
            var aErrors = aMessages.filter(function (oMsg) {
                return oMsg.getType() === "Error";
            });
            if (aErrors.length > 0) {
                // Remove error messages to prevent popup/dialog
                Messaging.removeMessages(aErrors);
                // Set inline error using the message text (if related to contract)
                var sErrorText = aErrors[0].getMessage() || this.getResourceBundle().getText("invalidContract");
                this._setContractInlineError(sErrorText);
            }
            var aSuccesses = aMessages.filter(function (oMsg) {
                return oMsg.getType() === "Success";
            });
            if (aSuccesses.length > 0) {
                // Optionally handle success messages, e.g., show toast and remove
                MessageToast.show(aSuccesses[0].getMessage());
                Messaging.removeMessages(aSuccesses);
            }
        },
        _onObjectMatched: function () {
            this.getView().getModel("local").setProperty("/contractId", ""); // Reset local value
            this.getView().getModel("local").setProperty("/grossWeight", ""); // Reset local value
            this.getView().getModel("local").setProperty("/teraWeight", ""); // Reset local value
            this.getView().getModel("local").setProperty("/loadType", ""); // Reset local value
            this.getView().getModel("local").setProperty("/korselsnr", ""); // NEW: Reset Korselsnr
            // Clear step 2 items binding to reset load types
            var oVBox = this.byId("step2LtContainer");
            if (oVBox) {
                oVBox.unbindAggregation("items");
                oVBox.destroyItems();
            }
            // Reset wizard to step 1
            this.oWizard.setCurrentStep(this.byId("step1"));
            // Clear any inline errors
            this._clearContractInlineError();
            // Hide confirm button
            var oButton = this.byId("step3BtnConfirm");
            if (oButton) {
                oButton.setVisible(false);
            }
            var oButton = this.byId("step3BtnConfirm2");
            if (oButton) {
                oButton.setVisible(true);
            }
        },
        // === ENTER wiring (ADDED) ===
        _setupEnterToNext: function () {
            if (this._enterWired) { return; }
            this._enterWired = true;
            // Step 1: Enter on Contract input -> reuse existing onNextStep
            var oIp = this.byId("step1InputContract");
            if (oIp) {
                oIp.addEventDelegate({
                    onsapenter: function (oEvent) {
                        oEvent.preventDefault();
                        oEvent.stopPropagation();
                        this.onNextStep();
                    }.bind(this)
                }, oIp);
            }
            // NEW: Enter on Korselsnr input -> call onNextFromK
            var oIpK = this.byId("stepKInputKorselsnr");
            if (oIpK) {
                oIpK.addEventDelegate({
                    onsapenter: function (oEvent) {
                        oEvent.preventDefault();
                        oEvent.stopPropagation()
                        this.onNextFromK();
                    }.bind(this)
                }, oIpK);
            }
        },
        onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();
            var sStepId = oCurrentStep.split("--").pop();
            if (sStepId === "step1") {
                var sContractId = this.getView().getModel("local").getProperty("/contractId");
                if (!sContractId) {
                    MessageToast.show(this.getResourceBundle().getText("enterContractId"));
                    return;
                }
                // Pad with leading zeros for internal format
                sContractId = sContractId.padStart(10, '0');
                // Manual bound action call to avoid automatic popups from editFlow
                var oModel = this.getView().getModel();
                // Check if entity exists before invoking action
                var oListBinding = oModel.bindList("/ZI_WR_R_WEIGHBRIDGE", undefined, undefined, new Filter("SalesDocument", FilterOperator.EQ, sContractId), { $$ownRequest: true });
                oListBinding.requestContexts(0, 1).then(function (aContexts) {
                    if (aContexts.length > 0) {
                        // Entity exists, proceed with action
                        console.log("Invoking identifyCard...");
                        var sEntityPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')";
                        var sActionPath = sEntityPath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(...)";
                        var oAction = oModel.bindContext(sActionPath);
                        oAction.setParameter("Vbeln", sContractId);
                        oAction.invoke().then(function (oResult) {
                            console.log("identifyCard invoked successfully."); // Debug: Action succeeded
                            // Get the returned context from action
                            var oContext = oAction.getBoundContext();
                            this.getView().setBindingContext(oContext);
                            // Refresh context to pull server updates
                            oContext.refresh();
                            // Bind step 2 dynamically
                            var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')/Set";
                            var oVBox = this.byId("step2LtContainer");
                            var oTemplate = new Button({
                                text: {
                                    parts: [
                                        { path: 'Avvcode', targetType: 'any' },
                                        { path: 'Material', targetType: 'any' },
                                        { path: 'MaterialText', targetType: 'any' }
                                    ],
                                    formatter: '.formatter.concatMaterialText'
                                },
                                press: [this.onChooseLoadType, this],
                                width: "100%"
                            });
                            oTemplate.addStyleClass("loadTypeBtn");
                            if (oVBox) {
                                try {
                                    oVBox.bindAggregation("items", {
                                        path: sPath,
                                        parameters: {
                                            $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Avvcode,Language,Korselsnrindicator,Korselsnr",
                                            $orderby: "SalesOrder,SalesOrderitem"
                                        },
                                        template: oTemplate,
                                        templateShareable: false,
                                        events: {
                                            change: function (oEvent) {
                                                if (oEvent.getParameter("reason") === "Rejected") {
                                                    MessageToast.show(this.getResourceBundle().getText("failedLoadTypes"));
                                                    this._skipToWeigh();
                                                }
                                            }.bind(this)
                                        }
                                    });
                                    // Attach one-time dataReceived to check if empty
                                    var oBinding = oVBox.getBinding("items");
                                    oBinding.attachEventOnce("dataReceived", function (oEvent) {
                                        if (oBinding.getLength() === 0) {
                                            // No loads, skip step 2
                                            this._skipToWeigh();
                                        }
                                    }.bind(this));
                                } catch (oError) {
                                    console.error("Binding error: ", oError);
                                    this._setContractInlineError(this.getResourceBundle().getText("failedMaterials"));
                                    return; // Prevent advancing
                                }
                            } else {
                                console.error("step2LtContainer not found");
                            }
                            //Manually fetch all messages from the message model
                            var aMessages = Messaging.getMessageModel().getData() || [];
                            // Filter for the specific success message using getCode() (expected format: 'msgid/msgno')
                            var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                                return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/007';
                            });
                            if (aSpecificSuccesses.length > 0) {
                                this.getView().getModel("local").setProperty("/transaction_state", "2. Weighing"); // Reset local value
                            } else {
                                this.getView().getModel("local").setProperty("/transaction_state", "1. Weighing"); // Reset local value
                            }
                            // advance wizard
                            this._clearContractInlineError();
                            this.oWizard.validateStep(this.byId("step1"));
                            MessageToast.show(this.getResourceBundle().getText("contractValid"))
                            this.oWizard.nextStep();
                        }.bind(this)).catch(function (oError) {
                            console.error("Error in identifyCard: ", oError); // Debug: Unified catch for full error details
                            // Handle failure (original logic consolidated into one catch for simplicity)
                            var sErrorMsg = oError.message || "Unknown error"; // This captures the backend message like "Contract is Invalid"
                            this._setContractInlineError(sErrorMsg); // Display the exact message inline
                        }.bind(this));
                    } else {
                        this._setContractInlineError(this.getResourceBundle().getText("contrnotfd"));
                    }
                }.bind(this)).catch(function (oError) {
                    console.error("Entity check error: ", oError);
                    var sErrorMsg = oError.message || "Unknown error";
                    this._setContractInlineError(sErrorMsg);
                }.bind(this));
            }
        },
        _skipToWeigh: function () {
            var oCtx = this.getView().getBindingContext();
            if (oCtx) {
                this.getView().getModel("local").setProperty("/loadType", "");
            }
            this.oWizard.validateStep(this.byId("step2"));
            // REMOVED: onWeighStep3 call (moved to onStepActivate for step3)
            this.oWizard.nextStep();
        },
        onAfterRendering: function () {
            var oIp = this.byId("step1InputContract");
            if (oIp) { oIp.focus(); }
            this._setupEnterToNext(); // wire Enter once
        },
        onChooseLoadType: function (oEvent) {
            const sLoadType = oEvent.getSource().getBindingContext().getProperty("Material");
            const sIndicator = oEvent.getSource().getBindingContext().getProperty("Korselsnrindicator");
            const sKorselsnr = oEvent.getSource().getBindingContext().getProperty("Korselsnr");
            const oSessionCtx = this.getView().getBindingContext();
            if (!oSessionCtx) { return; }
            this.getView().getModel("local").setProperty("/loadType", sLoadType);
            this.oWizard.validateStep(this.byId("step2"));

            // Async check for indicator (fetches if not loaded)
            if (!sIndicator) {
                    // Skip stepK entirely by validating it here and advancing twice
                    this.oWizard.validateStep(this.byId("stepK"));
                    this.oWizard.nextStep(); // To stepK (but it's now complete)
                    this.oWizard.nextStep(); // To step3
                } else {
                    this.oWizard.nextStep(); // To stepK as normal
                }
        },
        onStepActivate: function (oEvent) {
            var oStep = oEvent.getParameter("step");
            if (!oStep) { return; }
            var sId = oStep.getId().split("--").pop();
            if (sId === "stepK") {
                // No skip check here anymore; only focus if we reach this step
                var oIp = this.byId("stepKInputKorselsnr");
                if (oIp) oIp.focus();
            }
            // NEW: Call onWeighStep3 when entering step3
            if (sId === "step3") {
                this.onWeighStep3();
            }
        },
        onWeighStep3: function () {
            var oContext = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            var oLocalModel = this.getView().getModel("local");
            if (!oContext) {
                MessageToast.show(this.getResourceBundle().getText("noContext"));
                return;
            }
            console.log("Invoking determineWeight...");
            // Call the determineWeight action
            var sContractId = oLocalModel.getProperty("/contractId").padStart(10, '0');
            var sActionPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.determineWeight(...)";
            var oAction = oModel.bindContext(sActionPath);
            var sLoadType = oLocalModel.getProperty("/loadType");
            oAction.setParameter("Vbeln", sContractId);
            oAction.setParameter("Loadtype", sLoadType);
            oAction.invoke().then(function (oResult) {
                console.log("determineWeight invoked successfully.");
                // Get the returned context from action
                var oContext = oAction.getBoundContext();
                this.getView().setBindingContext(oContext);
                // Refresh context for any other updates
                oContext.refresh();
                // NEW: Manually fetch all messages from the message model
                var aMessages = Messaging.getMessageModel().getData() || [];
                // Filter for the specific success message using getCode()
                console.log("All Sucess Messages:", aMessages);
                // Grossweight
                var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/002';
                });
                // Handle Gross Weight
                if (aSpecificSuccesses.length > 0) {
                    // Take the first (or last) matching message; assuming one per action
                    var oSpecificMsg = aSpecificSuccesses[0]; // or aSpecificSuccesses[aSpecificSuccesses.length - 1]
                    var sMsgText = oSpecificMsg.getMessage();
                    oLocalModel.setProperty("/grossWeight", sMsgText);
                    // Remove the processed specific success messages to avoid accumulation or popups
                    Messaging.removeMessages(aSpecificSuccesses);
                } else {
                    MessageToast.show(this.getResourceBundle().getText("noSuccess002"));
                }
                // Teraweight
                var aSpecificSuccesses1 = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/009';
                });
                console.log("Mess 009:", aSpecificSuccesses1);
                // Handle TeratWeight
                if (aSpecificSuccesses1.length > 0) {
                    // Take the first (or last) matching message; assuming one per action
                    var oSpecificMsg = aSpecificSuccesses1[0]; // or aSpecificSuccesses[aSpecificSuccesses.length - 1]
                    var sMsgText1 = oSpecificMsg.getMessage();
                    oLocalModel.setProperty("/teraWeight", sMsgText1);
                    // Make the CONFIRM button visible after weight is set
                    var oButton = this.byId("step3BtnConfirm");
                    if (oButton) {
                        oButton.setVisible(true);
                    }
                    var oButton = this.byId("step3BtnConfirm2");
                    if (oButton) {
                        oButton.setVisible(false);
                    }
                    // Remove the processed specific success messages to avoid accumulation or popups
                    Messaging.removeMessages(aSpecificSuccesses1);
                } else {
                    //MessageToast.show("No specific success message (ZWR_WEIGHBRIGE_MESS/009) received from backend.");
                }
                var sGrossweight1 = oLocalModel.getProperty("/grossWeight");
                var steraweight1 = oLocalModel.getProperty("/teraWeight");
                console.log("grossweight:", sGrossweight1);
                console.log("teraweight:", steraweight1);
                MessageToast.show(this.getResourceBundle().getText("weightCaptured") + sGrossweight1 + "\n" + steraweight1, { duration: 5000 });
                jQuery(".sapMMessageToast").addClass("myGreenToast");
            }.bind(this)).catch(function (oError) {
                console.error("Error in determineWeight: ", oError);
                var sErrorMsg = oError.message || this.getResourceBundle().getText("failedWeight");
                MessageToast.show(sErrorMsg);
                // Optional: Also check for error messages and handle/remove them
                var aMessages = Messaging.getMessageModel().getData() || [];
                var aErrors = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Error";
                });
                if (aErrors.length > 0) {
                    // Example: Show the error message and remove
                    MessageToast.show(aErrors[0].getMessage());
                    Messaging.removeMessages(aErrors);
                }
            }.bind(this));
        },
        // NEW: Handler for next from optional Korselsnr step (analogous to onNextStep)
        onNextFromK: function () {
            var sKorselsnr = this.getView().getModel("local").getProperty("/korselsnr");
            if (!sKorselsnr) {
                MessageToast.show(this.getResourceBundle().getText("enterContractId")); // Reuse text or add new i18n
                return;
            }
            // Pad with leading zeros for internal format "Korselsnr"
            sKorselsnr = sKorselsnr.padStart(10, '0');
            // Manual bound action call
            var oModel = this.getView().getModel();
            // Check if entity exists
            var oListBinding = oModel.bindList("/ZI_WR_R_WEIGHBRIDGE", undefined, undefined, new Filter("Korselsnr", FilterOperator.EQ, sKorselsnr), { $$ownRequest: true });
            oListBinding.requestContexts(0, 1).then(function (aContexts) {
                if (aContexts.length > 0) {
                    // Entity exists, get the SalesDocument key
                    var sSalesDocument = aContexts[0].getProperty("SalesDocument");
                    // Proceed with action using the key
                    console.log("Invoking identifyCard for Korselsnr...");
                    var sEntityPath = "/ZI_WR_R_WEIGHBRIDGE('" + sSalesDocument + "')";
                    var sActionPath = sEntityPath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(...)";
                    var oAction = oModel.bindContext(sActionPath);
                    oAction.setParameter("Vbeln", sKorselsnr); // Use Korselsnr as input
                    oAction.invoke().then(function (oResult) {
                        console.log("identifyCard invoked successfully for Korselsnr.");
                        // Get the returned context from action
                        var oContext = oAction.getBoundContext();
                        this.getView().setBindingContext(oContext);
                        // Refresh context
                        oContext.refresh();
                        // NEW: Update local contractId to the SalesDocument (for subsequent actions)
                        this.getView().getModel("local").setProperty("/contractId", sSalesDocument);
                        // Manually fetch messages (similar to step1)
                        var aMessages = Messaging.getMessageModel().getData() || [];
                        var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                            return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/007';
                        });
                        if (aSpecificSuccesses.length > 0) {
                            this.getView().getModel("local").setProperty("/transaction_state", "2. Weighing");
                        } else {
                            this.getView().getModel("local").setProperty("/transaction_state", "1. Weighing");
                        }
                        // Advance wizard
                        this._clearKorselsnrInlineError();
                        this.oWizard.validateStep(this.byId("stepK"));
                        MessageToast.show(this.getResourceBundle().getText("contractValid")); // Reuse or add new
                        this.oWizard.nextStep();
                    }.bind(this)).catch(function (oError) {
                        console.error("Error in identifyCard for Korselsnr: ", oError);
                        var sErrorMsg = oError.message || "Unknown error";
                        this._setKorselsnrInlineError(sErrorMsg);
                    }.bind(this));
                } else {
                    this._setKorselsnrInlineError(this.getResourceBundle().getText("contrnotfd"));
                }
            }.bind(this)).catch(function (oError) {
                console.error("Entity check error for Korselsnr: ", oError);
                var sErrorMsg = oError.message || "Unknown error";
                this._setKorselsnrInlineError(sErrorMsg);
            }.bind(this));
        },
        // NEW: Inline error setters for Korselsnr (analogous to contract)
        _setKorselsnrInlineError: function (sText) {
            var oInput = this.byId("stepKInputKorselsnr");
            if (oInput) {
                oInput.setValueState("Error");
                oInput.setValueStateText(sText);
                oInput.focus();
            }
            // Add field-bound message
            var oCtx = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            if (oCtx && oModel) {
                var sTarget = oCtx.getPath() + "/Korselsnr"; // Use Korselsnr property
                var aAll = Messaging.getMessageModel().getData() || [];
                var aOldForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                if (aOldForField.length) { Messaging.removeMessages(aOldForField); }
                Messaging.addMessages(new Message({
                    message: sText,
                    type: coreLibrary.MessageType.Error,
                    target: sTarget,
                    processor: oModel
                }));
            }
        },
        _clearKorselsnrInlineError: function () {
            var oInput = this.byId("stepKInputKorselsnr");
            if (oInput) {
                oInput.setValueState("None");
                oInput.setValueStateText("");
            }
            var oCtx = this.getView().getBindingContext();
            if (oCtx) {
                var sTarget = oCtx.getPath() + "/Korselsnr"; // Use Korselsnr property
                var aAll = Messaging.getMessageModel().getData() || [];
                var aForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                if (aForField.length) { Messaging.removeMessages(aForField); }
            }
        },
        _normalizeToStdBase64: function (val) {
            // If backend already gave binary (Uint8Array/ArrayBuffer), return as-is
            if (val instanceof Uint8Array) return { kind: "u8", data: val };
            if (val && val.buffer instanceof ArrayBuffer) return { kind: "u8", data: new Uint8Array(val) };
            var s = String(val || "").trim();
            // Strip any data URL prefix like "data:application/pdf;base64,..."
            s = s.replace(/^data:[^;]+;base64,/, "");
            // Remove whitespace/newlines
            s = s.replace(/\s+/g, "");
            // Convert base64url -> base64
            s = s.replace(/-/g, "+").replace(/_/g, "/");
            // Pad with '=' to make length % 4 === 0 (without using repeat)
            var pad = s.length % 4;
            if (pad) {
                s += Array(4 - pad + 1).join("=");
            }
            return { kind: "b64", data: s };
        },
        onConfirmStep3: function () {
            var oContext = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            var oLocalModel = this.getView().getModel("local");
            if (!oContext) {
                MessageToast.show(this.getResourceBundle().getText("noContext"));
                return;
            }
            var sContractId = oLocalModel.getProperty("/contractId");
            var sLoadType = oLocalModel.getProperty("/loadType");
            var sGrossWeight = oLocalModel.getProperty("/grossWeight");
            var sTeraWeight = oLocalModel.getProperty("/teraWeight");
            if (!sContractId || !sLoadType || !sGrossWeight) {
                MessageToast.show(this.getResourceBundle().getText("missingData"));
                return;
            }
            // Parse "12345 KG" (fallback unit KG)
            var aWeightParts = sGrossWeight.trim().split(/\s+/);
            var sWeight = aWeightParts[0] || "";
            var sWeighUnit = aWeightParts[1] || "KG";
            // Pad contract like in step 1
            sContractId = sContractId.padStart(10, "0");
            // Bind the action using entity path
            var sActionPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.printSlip(...)";
            var oAction = oModel.bindContext(sActionPath);
            oAction.setParameter("Vbeln", sContractId);
            oAction.setParameter("Loadtype", sLoadType);
            oAction.setParameter("Weight", sWeight);
            oAction.setParameter("WeighUnit", sWeighUnit);
            // Now call the silent print function
            if (sTeraWeight) { // Print only when there is Tera Weight
                oAction.invoke().then(function () {
                    // Get the returned context from action
                    var oNewContext = oAction.getBoundContext();
                    this.getView().setBindingContext(oNewContext);
                    // Read the result payload from the bound context
                    var oResCtx = oAction.getBoundContext();
                    var oRes = oResCtx && oResCtx.getObject() || null;
                    // Updated to match new metadata (PDFBASE64 uppercase); support variants for robustness
                    var sB64 = oRes && (oRes.PDFBASE64 || oRes.pdfbase64 || oRes.Pdfbase64);
                    if (typeof sB64 === "string") {
                        sB64 = String(sB64);
                    }
                    if (sB64 && typeof sB64 === "string") {
                        try {
                            // Normalize the base64 string first (using your existing _normalizeToStdBase64 for robustness)
                            var norm = this._normalizeToStdBase64(sB64);
                            if (norm.kind === "b64") {
                                sB64 = norm.data; // Use the normalized base64 string
                            } else if (norm.kind === "u8") {
                                // If it's already binary, convert to base64 for _printBase64PdfSilently (which expects base64)
                                sB64 = btoa(String.fromCharCode.apply(null, norm.data));
                            }
                            this._printBase64PdfSilently(sB64); // Print only when there is Teraweight
                            MessageToast.show(this.getResourceBundle().getText("pdfSent"));
                        } catch (oError) {
                            console.error("Base64 processing or printing failed: ", oError);
                            MessageToast.show(this.getResourceBundle().getText("failedPdf"));
                        }
                    } else {
                        MessageToast.show(this.getResourceBundle().getText("noPdf"));
                    }
                    // Optional: surface server messages (Success)
                    var aMsgs = Messaging.getMessageModel().getData() || [];
                    console.log("All Messages 3", aMsgs);
                    var aSucc = aMsgs.filter(function (m) {
                        return m.getType && m.getType() === "Success";
                    });
                    if (aSucc.length > 0) {
                        MessageToast.show(aSucc[0].getMessage());
                        Messaging.removeMessages(aSucc);
                    }
                    // Refresh & reset to step 1 instead of next step
                    oNewContext.refresh();
                    this._onObjectMatched();
                }.bind(this)).catch(function (oError) {
                    var sErr = (oError && oError.message) || this.getResourceBundle().getText("failedPdf");
                    MessageToast.show(sErr);
                    // Optional: surface server messages (Error)
                    var aMsgs = Messaging.getMessageModel().getData() || [];
                    var aErrs = aMsgs.filter(function (m) {
                        return m.getType && m.getType() === "Error";
                    });
                    if (aErrs.length > 0) {
                        MessageToast.show(aErrs[0].getMessage());
                        Messaging.removeMessages(aErrs);
                    }
                }.bind(this));
            } else {
                // Refresh & reset to step 1 instead of next step
                oContext.refresh();
                this._onObjectMatched();
            }
        },
        /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
        //// Printing in Kiosk Mode for Chrome 
        /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
        _printBase64PdfSilently: function (sB64) {
            // Ensure it's a string (some backends return ArrayBuffer/Uint8Array)
            if (sB64 && sB64.constructor !== String) {
                try { sB64 = atob(btoa(String.fromCharCode.apply(null, new Uint8Array(sB64)))); }
                catch (e) { sB64 = typeof sB64.toString === "function" ? sB64.toString() : String(sB64); }
            }
            // Create a Blob URL (more reliable than giant data-URIs)
            var byteChars = atob(sB64);
            var byteNums = new Array(byteChars.length);
            for (var i = 0; i < byteChars.length; i++) byteNums[i] = byteChars.charCodeAt(i);
            var blob = new Blob([new Uint8Array(byteNums)], { type: "application/pdf" });
            var url = URL.createObjectURL(blob);
            // Hidden iframe
            var iframe = document.createElement("iframe");
            iframe.style.position = "absolute";
            iframe.style.left = "-10000px";
            iframe.style.top = "-10000px";
            iframe.style.width = "0px";
            iframe.style.height = "0px";
            iframe.style.border = "0";
            iframe.style.visibility = "hidden";
            iframe.onload = function () {
                // Important: call print on the iframe's *contentWindow*
                setTimeout(function () {
                    try {
                        iframe.contentWindow.focus();
                        iframe.contentWindow.print(); // dialog-less if Chrome has --kiosk-printing
                    } finally {
                        // Cleanup after a short delay to avoid killing the print job
                        setTimeout(function () {
                            URL.revokeObjectURL(url);
                            iframe.parentNode && iframe.parentNode.removeChild(iframe);
                        }, 1000);
                    }
                }, 200); // give the PDF plugin a moment to render
            };
            iframe.src = url;
            document.body.appendChild(iframe);
        },
        _setContractInlineError: function (sText) {
            // Inline on the input
            var oInput = this.byId("step1InputContract");
            if (oInput) {
                oInput.setValueState("Error");
                oInput.setValueStateText(sText);
                oInput.focus();
            }
            // Optional: also add a field-bound message (shows in FE message popover / keeps state on rebind)
            var oCtx = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            if (oCtx && oModel) {
                var sTarget = oCtx.getPath() + "/SalesDocument"; // Updated to SalesDocument
                var aAll = Messaging.getMessageModel().getData() || [];
                var aOldForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                if (aOldForField.length) { Messaging.removeMessages(aOldForField); }
                Messaging.addMessages(new Message({
                    message: sText,
                    type: coreLibrary.MessageType.Error,
                    target: sTarget,
                    processor: oModel
                }));
            }
        },
        _clearContractInlineError: function () {
            var oInput = this.byId("step1InputContract");
            if (oInput) {
                oInput.setValueState("None");
                oInput.setValueStateText("");
            }
            var oCtx = this.getView().getBindingContext();
            if (oCtx) {
                var sTarget = oCtx.getPath() + "/SalesDocument"; // Updated to SalesDocument
                var aAll = Messaging.getMessageModel().getData() || [];
                var aForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                if (aForField.length) { Messaging.removeMessages(aForField); }
            }
        },
        onCancel: function () {
            // Reset the wizard and clear data
            this._onObjectMatched();
        }
    });
});
