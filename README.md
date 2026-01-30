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
<Property Name="Vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
</ComplexType>
<ComplexType Name="ZI_LANF_POSITION_INPUT">
<Property Name="ContractVbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
<Property Name="Matnr" Type="Edm.String" Nullable="false" MaxLength="40"/>
<Property Name="Menge" Type="Edm.Decimal" Nullable="false" Precision="13" Scale="3"/>
<Property Name="Meins" Type="Edm.String" Nullable="false" MaxLength="3"/>
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
<Parameter Name="Vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
<Parameter Name="Title" Type="Edm.String" Nullable="false" MaxLength="100"/>
<Parameter Name="Url" Type="Edm.String" Nullable="false" MaxLength="132"/>
<ReturnType Type="com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_RESPONSE" Nullable="false"/>
</Action>
<Action Name="CreateLanf" IsBound="true">
<Parameter Name="_it" Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_ROOTType)" Nullable="false"/>
<Parameter Name="ContractVbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
<Parameter Name="DeliveryDate" Type="Edm.Date" Nullable="true"/>
<Parameter Name="CustomerRef" Type="Edm.String" Nullable="false" MaxLength="35"/>
<Parameter Name="Description" Type="Edm.String" Nullable="false" MaxLength="225"/>
<Parameter Name="_Positions" Type="Collection(com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_POSITION_INPUT)" Nullable="false"/>
<ReturnType Type="com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_RESPONSE" Nullable="false"/>
</Action>
<EntityContainer Name="Container">
<EntitySet Name="ZI_LANF_ROOT" EntityType="com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.ZI_LANF_ROOTType"/>
</EntityContainer>
<Annotations Target="SAP__self.ZI_LANF_POSITION_INPUT/Menge">
<Annotation Term="SAP__measures.Unit" Path="Meins"/>
<Annotation Term="SAP__common.Label" String="Target Quantity"/>
<Annotation Term="SAP__common.QuickInfo" String="Target Quantity in Sales Units"/>
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
<Annotation Term="SAP__common.Label" String="Sales Document"/>
<Annotation Term="SAP__common.Heading" String="Sales Doc."/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_POSITION_INPUT/ContractVbeln">
<Annotation Term="SAP__common.Label" String="Sales Document"/>
<Annotation Term="SAP__common.Heading" String="Sales Doc."/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_POSITION_INPUT/Matnr">
<Annotation Term="SAP__common.Label" String="Material"/>
<Annotation Term="SAP__common.QuickInfo" String="Material Number"/>
</Annotations>
<Annotations Target="SAP__self.ZI_LANF_POSITION_INPUT/Meins">
<Annotation Term="SAP__common.Label" String="Base Unit of Measure"/>
<Annotation Term="SAP__common.Heading" String="BUn"/>
</Annotations>
<Annotations Target="SAP__self.SAP__VIRTUAL_CO_TY_2/_Positions/ContractVbeln">
<Annotation Term="SAP__common.Label" String="Sales Document"/>
<Annotation Term="SAP__common.Heading" String="Sales Doc."/>
</Annotations>
<Annotations Target="SAP__self.SAP__VIRTUAL_CO_TY_2/_Positions/Matnr">
<Annotation Term="SAP__common.Label" String="Material"/>
<Annotation Term="SAP__common.QuickInfo" String="Material Number"/>
</Annotations>
<Annotations Target="SAP__self.SAP__VIRTUAL_CO_TY_2/_Positions/Menge">
<Annotation Term="SAP__common.Label" String="Target Quantity"/>
<Annotation Term="SAP__common.QuickInfo" String="Target Quantity in Sales Units"/>
<Annotation Term="SAP__measures.Unit" Path="_Positions/Meins"/>
</Annotations>
<Annotations Target="SAP__self.SAP__VIRTUAL_CO_TY_2/_Positions/Meins">
<Annotation Term="SAP__common.Label" String="Base Unit of Measure"/>
<Annotation Term="SAP__common.Heading" String="BUn"/>
</Annotations>
<Annotations Target="SAP__self.AttachDocument(Collection(SAP__self.ZI_LANF_ROOTType))/Vbeln">
<Annotation Term="SAP__common.Label" String="Sales Document"/>
<Annotation Term="SAP__common.Heading" String="Sales Doc."/>
</Annotations>
<Annotations Target="SAP__self.AttachDocument(Collection(SAP__self.ZI_LANF_ROOTType))/Title">
<Annotation Term="SAP__common.Label" String="Text"/>
<Annotation Term="SAP__common.QuickInfo" String="Text (100 characters)"/>
</Annotations>
<Annotations Target="SAP__self.AttachDocument(Collection(SAP__self.ZI_LANF_ROOTType))/Url">
<Annotation Term="SAP__common.Label" String="URL"/>
<Annotation Term="SAP__common.QuickInfo" String="Uniform resource locator"/>
</Annotations>
<Annotations Target="SAP__self.CreateLanf(Collection(SAP__self.ZI_LANF_ROOTType))/ContractVbeln">
<Annotation Term="SAP__common.Label" String="Sales Document"/>
<Annotation Term="SAP__common.Heading" String="Sales Doc."/>
</Annotations>
<Annotations Target="SAP__self.CreateLanf(Collection(SAP__self.ZI_LANF_ROOTType))/DeliveryDate">
<Annotation Term="SAP__common.Label" String="Purchase order date"/>
<Annotation Term="SAP__common.Heading" String="PO date"/>
</Annotations>
<Annotations Target="SAP__self.CreateLanf(Collection(SAP__self.ZI_LANF_ROOTType))/CustomerRef">
<Annotation Term="SAP__common.Label" String="Customer Reference"/>
</Annotations>
</Schema>
</edmx:DataServices>
</edmx:Edmx>
