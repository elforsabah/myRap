sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast"
], function (PageController, MessageToast) {
    "use strict";
    return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
        onInit: function () {
            var parentReturn = PageController.prototype.onInit.apply(this);

            // Initialize wizard
            this.oWizard = this.byId("weighingWizard");

            // Attach to route pattern matched
            var oRouter = this.getAppComponent().getRouter();
            oRouter.getRoute("ZI_WR_WEIGHINGSESSIONMain").attachPatternMatched(this._onObjectMatched, this);

            return parentReturn;
        },

        generateUUID: function () {
            // Standard UUID v4 generator for backend compatibility
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
                var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
                return v.toString(16);
            });
        },

        _onObjectMatched: function () {
            var oModel = this.getView().getModel();
            var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION");
            var oNewContext = oListBinding.create({
                Sessionid: this.generateUUID()  // Generate valid UUID for Sessionid (adjusted case to match CDS)
            });
            this.getView().setBindingContext(oNewContext);
            // Request all bound properties to initialize and include in queries
            oNewContext.requestProperty(["Vbeln", "Grossweight", "Grossweightunit", "Sessionid"]).catch(function () {
                // Ignore if needed
            });
        },

        onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();  // Returns string ID, e.g., "step1"
            var sStepId = oCurrentStep.split("--").pop(); // Extracts "step1"
            if (sStepId === "step1") { // Compare string directly (fix here)
                var sContractId = this.byId("ip11").getValue(); // Get Contract ID (Vbeln) from input 
                if (!sContractId) {
                    MessageToast.show("Please enter a Contract ID.");
                    return;
                }
                var oContext = this.getView().getBindingContext();  // Get current entity context
                if (!oContext) {
                    MessageToast.show("No session context available.");
                    return;
                }
                // Update the model with the input value (if not already bound)
                oContext.setProperty("Vbeln", sContractId);
                // Invoke the RAP action 'identifycard' with parameter (adjust action name/namespace if needed)
                this.editFlow.invokeAction("identifycard", {
                    contexts: [oContext],
                    parameterValues: [{ name: "vbeln", value: sContractId }]  // Array of {name, value} objects
                }).then(function () {
                    // On success: Show message and advance
                    MessageToast.show("Contract validated successfully.");
                    this.oWizard.validateStep(this.byId(sStepId));  // Pass step object by ID (fix here)
                    this.oWizard.nextStep();
                }.bind(this)).catch(function (oError) {
                    // On error: Show message and stay
                    MessageToast.show("Invalid Contract. Please try again.");
                    // Optional: Log error details - console.error(oError);
                }.bind(this));
            } else {
                // For other steps: Simply advance (add logic as needed)
                this.oWizard.nextStep();
            }
        },

        onStepActivate: function (oEvent) {
            // Custom logic per step, e.g., fetch data from CDS
        },
        validateStep: function (oStep) {
            // Add validation logic, e.g., check required fields
            return true;  // Placeholder
        },

        onCaptureWeight: function () {
            // Simulate weighing: Update model with gross weight
            this.getView().getModel().setProperty("/Grossweight", 1000);  // Adjusted case
        },

        onCaptureFinalWeight: function () {
            // Calculate net weight: Net = Gross - Tare
            var oModel = this.getView().getModel();
            var fGross = oModel.getProperty("/Grossweight");  // Adjusted case
            var fTare = oModel.getProperty("/TareWeight");
            oModel.setProperty("/NetWeight", fGross - fTare);
        },

        onSubmit: function () {
            // Submit to backend via OData create/update
            this.getView().getModel().submitChanges({
                success: function () {
                    MessageToast.show("Weighing session submitted.");
                }
            });
        },

        onPrintSlip: function () {
            // Integrate printing, e.g., via sap.m.Print or external service
        },

        onWizardComplete: function () {
            // Final actions on wizard completion
        }
    });
});

The metadata


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
        <Schema Namespace="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001" Alias="SAP__self">
            <Annotation Term="SAP__core.SchemaVersion" String="1.0.0"/>
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
            <EntityType Name="ZI_WR_WEIGHINGSESSIONType">
                <Key>
                    <PropertyRef Name="Vbeln"/>
                    <PropertyRef Name="Sessionid"/>
                    <PropertyRef Name="IsActiveEntity"/>
                </Key>
                <Property Name="Vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Property Name="Sessionid" Type="Edm.Guid" Nullable="false"/>
                <Property Name="LoadType" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <Property Name="Grossweight" Type="Edm.Decimal" Nullable="false" Precision="15" Scale="3"/>
                <Property Name="Grossweightunit" Type="Edm.String" Nullable="false" MaxLength="3"/>
                <Property Name="Tareweight" Type="Edm.Decimal" Nullable="false" Precision="15" Scale="3"/>
                <Property Name="Tareweightunit" Type="Edm.String" Nullable="false" MaxLength="3"/>
                <Property Name="Netweight" Type="Edm.Decimal" Nullable="false" Precision="15" Scale="3"/>
                <Property Name="Netweightunit" Type="Edm.String" Nullable="false" MaxLength="3"/>
                <Property Name="Step" Type="Edm.Int16" Nullable="false"/>
                <Property Name="Step1Ok" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="Step2Ok" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="IsSummited" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="HasDraftEntity" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="DraftEntityCreationDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="DraftEntityLastChangeDateTime" Type="Edm.DateTimeOffset" Precision="7"/>
                <Property Name="HasActiveEntity" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="IsActiveEntity" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="__EntityControl" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.EntityControl"/>
                <Property Name="__OperationControl" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONOperationControl"/>
                <Property Name="SAP__Messages" Type="Collection(com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.SAP__Message)" Nullable="false"/>
                <NavigationProperty Name="DraftAdministrativeData" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.I_DraftAdministrativeDataType"/>
                <NavigationProperty Name="SiblingEntity" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType"/>
            </EntityType>
            <ComplexType Name="ZI_WR_WEIGHINGSESSIONOperationControl">
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
            <Action Name="setloadType" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
                <Parameter Name="vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Parameter Name="loadtype" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <Action Name="identifyCard" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
                <Parameter Name="vbeln" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Parameter Name="loadtype" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <Action Name="Submit" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <Action Name="NextStep" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <Action Name="Resume" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <Action Name="Discard" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <Action Name="Prepare" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <Action Name="Edit" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
                <Parameter Name="PreserveChanges" Type="Edm.Boolean" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <Action Name="Activate" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType" Nullable="false"/>
            </Action>
            <EntityContainer Name="Container">
                <EntitySet Name="I_DraftAdministrativeData" EntityType="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.I_DraftAdministrativeDataType"/>
                <EntitySet Name="ZI_WR_WEIGHINGSESSION" EntityType="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.ZI_WR_WEIGHINGSESSIONType">
                    <NavigationPropertyBinding Path="DraftAdministrativeData" Target="I_DraftAdministrativeData"/>
                    <NavigationPropertyBinding Path="SiblingEntity" Target="ZI_WR_WEIGHINGSESSION"/>
                </EntitySet>
            </EntityContainer>
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
                        <String>../../../../srvd_f4/sap/i_draftadministrativeuservh/0001;ps='srvd-zsb_wr_weighingbrige-0001';va='com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.et-i_draftadministrativedata.createdbyuser'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Label" String="Draft Created By"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/LastChangedByUser">
                <Annotation Term="SAP__common.Text" Path="LastChangedByUserDescription"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/sap/i_draftadministrativeuservh/0001;ps='srvd-zsb_wr_weighingbrige-0001';va='com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.et-i_draftadministrativedata.lastchangedbyuser'/$metadata</String>
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
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Vbeln">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Sessionid">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Label" String="Session ID"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/LoadType">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/sap/zi_wr_loadtypevh/0001;ps='srvd-zsb_wr_weighingbrige-0001';va='com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.et-zi_wr_weighingsession.loadtype'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__common.Label" String="Internal Adapter: Type of Load"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Grossweight">
                <Annotation Term="SAP__measures.Unit" Path="Grossweightunit"/>
                <Annotation Term="SAP__common.Label" String="Gross weight"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Grossweightunit">
                <Annotation Term="SAP__common.IsUnit"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/sap/i_unitofmeasure/0001;ps='srvd-zsb_wr_weighingbrige-0001';va='com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.et-zi_wr_weighingsession.grossweightunit'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Label" String="Unit of weight"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Tareweight">
                <Annotation Term="SAP__measures.Unit" Path="Tareweightunit"/>
                <Annotation Term="SAP__common.Label" String="Tare weight"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Tareweightunit">
                <Annotation Term="SAP__common.IsUnit"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/sap/i_unitofmeasure/0001;ps='srvd-zsb_wr_weighingbrige-0001';va='com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.et-zi_wr_weighingsession.tareweightunit'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Label" String="Unit of weight"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Netweight">
                <Annotation Term="SAP__measures.Unit" Path="Netweightunit"/>
                <Annotation Term="SAP__common.Label" String="Net weight"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Netweightunit">
                <Annotation Term="SAP__common.IsUnit"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/sap/i_unitofmeasure/0001;ps='srvd-zsb_wr_weighingbrige-0001';va='com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.et-zi_wr_weighingsession.netweightunit'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Label" String="Unit of weight"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/HasDraftEntity">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Has Draft"/>
                <Annotation Term="SAP__common.QuickInfo" String="Draft - Indicator - Has draft document"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/DraftEntityCreationDateTime">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Created On"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/DraftEntityLastChangeDateTime">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Draft Last Changed On"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/HasActiveEntity">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Has active"/>
                <Annotation Term="SAP__common.QuickInfo" String="Draft - Indicator - Has active document"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/IsActiveEntity">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Is active"/>
                <Annotation Term="SAP__common.QuickInfo" String="Draft - Indicator - Is active document"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__EntityControl">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__OperationControl">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/ZI_WR_WEIGHINGSESSION">
                <Annotation Term="SAP__common.DraftRoot">
                    <Record>
                        <PropertyValue Property="DiscardAction" String="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.Discard"/>
                        <PropertyValue Property="PreparationAction" String="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.Prepare"/>
                        <PropertyValue Property="EditAction" String="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.Edit"/>
                        <PropertyValue Property="ActivationAction" String="com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.Activate"/>
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
                                    <PropertyValue Property="Property" PropertyPath="Step1Ok"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleValue"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="Step2Ok"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleValue"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="IsSummited"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleValue"/>
                                </Record>
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
                        <PropertyPath>Grossweight</PropertyPath>
                    </Collection>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.setloadType(SAP__self.ZI_WR_WEIGHINGSESSIONType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/setloadType"/>
            </Annotations>
            <Annotations Target="SAP__self.identifyCard(SAP__self.ZI_WR_WEIGHINGSESSIONType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/identifyCard"/>
            </Annotations>
            <Annotations Target="SAP__self.Submit(SAP__self.ZI_WR_WEIGHINGSESSIONType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/Submit"/>
            </Annotations>
            <Annotations Target="SAP__self.NextStep(SAP__self.ZI_WR_WEIGHINGSESSIONType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/NextStep"/>
            </Annotations>
            <Annotations Target="SAP__self.Edit(SAP__self.ZI_WR_WEIGHINGSESSIONType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/Edit"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType">
                <Annotation Term="SAP__common.Label" String="Weighing Session"/>
                <Annotation Term="SAP__UI.Facets">
                    <Collection>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="Label" String="Step 1 . Identification"/>
                            <PropertyValue Property="ID" String="S1"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#S1"/>
                        </Record>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="Label" String="Step 2 . Load Type"/>
                            <PropertyValue Property="ID" String="S2"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#S2"/>
                        </Record>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="Label" String="Step 3 . Weighing"/>
                            <PropertyValue Property="ID" String="S3 @Consumption.filter:{ selectionType: #SINGLE, multipleSelections: false }"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#S3"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="S1">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Vbeln"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Sessionid"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="S2">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="LoadType"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="S3">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Grossweight"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Grossweightunit"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Tareweight"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Tareweightunit"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Netweight"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Netweightunit"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.SelectionFields">
                    <Collection>
                        <PropertyPath>LoadType</PropertyPath>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Messages" Path="SAP__Messages"/>
            </Annotations>
            <Annotations Target="SAP__self.Container">
                <Annotation Term="SAP__CodeList.UnitsOfMeasure">
                    <Record>
                        <PropertyValue Property="Url" String="../../../../default/iwbep/common/0001/$metadata"/>
                        <PropertyValue Property="CollectionPath" String="UnitsOfMeasure"/>
                    </Record>
                </Annotation>
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
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/CreationDateTime">
                <Annotation Term="SAP__common.Label" String="Draft Created On"/>
            </Annotations>
            <Annotations Target="SAP__self.I_DraftAdministrativeDataType/LastChangeDateTime">
                <Annotation Term="SAP__common.Label" String="Draft Last Changed On"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Step">
                <Annotation Term="SAP__common.Label" String="Step"/>
                <Annotation Term="SAP__common.QuickInfo" String="Session Step"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Step1Ok">
                <Annotation Term="SAP__common.Label" String="TRUE"/>
                <Annotation Term="SAP__common.QuickInfo" String="Data element for domain BOOLE: TRUE (='X') and FALSE (=' ')"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/Step2Ok">
                <Annotation Term="SAP__common.Label" String="TRUE"/>
                <Annotation Term="SAP__common.QuickInfo" String="Data element for domain BOOLE: TRUE (='X') and FALSE (=' ')"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/IsSummited">
                <Annotation Term="SAP__common.Label" String="TRUE"/>
                <Annotation Term="SAP__common.QuickInfo" String="Data element for domain BOOLE: TRUE (='X') and FALSE (=' ')"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__EntityControl/Deletable">
                <Annotation Term="SAP__common.Label" String="Dyn. Method Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Method Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Method Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__EntityControl/Updatable">
                <Annotation Term="SAP__common.Label" String="Dyn. Method Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Method Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Method Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__OperationControl/Edit">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__OperationControl/identifyCard">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__OperationControl/NextStep">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__OperationControl/setloadType">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONType/__OperationControl/Submit">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONOperationControl/Edit">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONOperationControl/identifyCard">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONOperationControl/NextStep">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONOperationControl/setloadType">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSIONOperationControl/Submit">
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
            <Annotations Target="SAP__self.setloadType(SAP__self.ZI_WR_WEIGHINGSESSIONType)/vbeln">
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.setloadType(SAP__self.ZI_WR_WEIGHINGSESSIONType)/loadtype">
                <Annotation Term="SAP__common.Label" String="Internal Adapter: Type of Load"/>
            </Annotations>
            <Annotations Target="SAP__self.identifyCard(SAP__self.ZI_WR_WEIGHINGSESSIONType)/vbeln">
                <Annotation Term="SAP__common.Label" String="Sales Document"/>
                <Annotation Term="SAP__common.Heading" String="Sales Doc."/>
            </Annotations>
            <Annotations Target="SAP__self.identifyCard(SAP__self.ZI_WR_WEIGHINGSESSIONType)/loadtype">
                <Annotation Term="SAP__common.Label" String="Internal Adapter: Type of Load"/>
            </Annotations>
            <Annotations Target="SAP__self.Edit(SAP__self.ZI_WR_WEIGHINGSESSIONType)/PreserveChanges">
                <Annotation Term="SAP__common.Label" String="TRUE"/>
                <Annotation Term="SAP__common.QuickInfo" String="Data element for domain BOOLE: TRUE (='X') and FALSE (=' ')"/>
            </Annotations>
        </Schema>
    </edmx:DataServices>
</edmx:Edmx>

The BDEF

managed implementation in class zbp_i_wr_weighingsession unique;
strict( 2 );

with draft;

define behavior for ZI_WR_WEIGHINGSESSION alias WeighingSession
persistent table zwr_weighsession
draft table ZWR_WEIGHSESSI_D
lock master
total etag Grossweight
authorization master ( instance )
etag master Grossweight

{
//  Keys must be readonly in strict draft
  field (readonly) Vbeln, Sessionid;  // <-- use your exact CDS element names
  field (numbering: managed) Sessionid;
create;
update;
delete;

draft action Edit;
draft action Activate;
draft action Discard;
draft action Resume;                 // <-- required
draft determine action Prepare;      //<-- required

action NextStep result [1] $self; // server validates & increments Step
action Submit result [1] $self; // final checks; printing trigger optional

action identifyCard parameter ZAE_WR_WEIHINGSESSION result [1] $self;
action setloadType parameter ZAE_WR_WEIHINGSESSION  result [1] $self;

validation validateStep1 on save { field Vbeln, Sessionid; } // Identification
validation validateStep2 on save { field Loadtype; } // Load type
validation ValidateLoadType on save { field LoadType; } // Calls a method to check against VH

determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math


}


The Behaviour implementation

CLASS lhc_WeighingSession DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR WeighingSession RESULT result.

    METHODS NextStep FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~NextStep RESULT result.

    METHODS Submit FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~Submit RESULT result.

    METHODS calcNet FOR DETERMINE ON MODIFY
      IMPORTING keys FOR WeighingSession~calcNet.

    METHODS validateStep1 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep1.

    METHODS validateStep2 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep2.
    METHODS identifycard FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~identifycard RESULT result.

    METHODS setloadType FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~setloadType RESULT result.

     METHODS ValidateLoadType FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~ValidateLoadType.

ENDCLASS.

CLASS lhc_WeighingSession IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD NextStep.
  ENDMETHOD.

  METHOD Submit.
  ENDMETHOD.

  METHOD calcNet.
  ENDMETHOD.

  METHOD validateStep1.
  ENDMETHOD.

  METHOD validateStep2.
  ENDMETHOD.

  METHOD ValidateLoadType.
  ENDMETHOD.

***********************************************************************************************************************
* Identification
***********************************************************************************************************************
  METHOD identifycard.
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
     ENTITY WeighingSession
     ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_sessions).

    LOOP AT lt_sessions ASSIGNING FIELD-SYMBOL(<ls_session>).
      " TODO: look up driver/contract by CardId (from keys[ 1 ]-%param-CardId)
      SELECT SINGLE * FROM zwr_weighsession WHERE vbeln = @<ls_session>-Vbeln INTO @DATA(ls_weighsessions).
      IF sy-subrc = 0.
        <ls_session>-step1ok = abap_true.
      ENDIF.

      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession UPDATE FIELDS ( step1ok ) WITH VALUE #( ( %tky = <ls_session>-%tky step1ok = abap_true ) ).
      result = VALUE #( ( %tky = <ls_session>-%tky ) ).
    ENDLOOP.
  ENDMETHOD.

***********************************************************************************************************************
* Selection of Load type
***********************************************************************************************************************
  METHOD setloadType.

   READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
      ENTITY WeighingSession FIELDS ( loadtype ) WITH CORRESPONDING #( keys )
      RESULT DATA(lt_sessions).
    LOOP AT lt_sessions ASSIGNING FIELD-SYMBOL(<ls_session>).
      <ls_session>-loadtype = keys[ sy-tabix ]-%param-LoadType.
      <ls_session>-step2ok  = abap_true.
      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession UPDATE FIELDS ( loadtype step2ok )
        WITH VALUE #( ( %tky = <ls_session>-%tky loadtype = <ls_session>-loadtype step2ok = abap_true ) ).
      result = VALUE #( ( %tky = <ls_session>-%tky ) ).
    ENDLOOP.

  ENDMETHOD.
ENDCLASS.


The CDS View
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Weighing Session'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true
define root view entity ZI_WR_WEIGHINGSESSION
  as select from zwr_weighsession
{
  key vbeln           as Vbeln,
  key sessionid       as Sessionid,
  
      @Consumption.valueHelpDefinition: [{ 
      entity: {
      name: 'ZI_WR_LOADTYPEVH',  
      element: 'LoadType'
      }}]
//      
      loadtype       as LoadType,
      @Semantics.quantity.unitOfMeasure: 'Grossweightunit'
      grossweight     as Grossweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      grossweightunit as Grossweightunit,
      @Semantics.quantity.unitOfMeasure: 'Tareweightunit'
      tareweight      as Tareweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      tareweightunit  as Tareweightunit,
      @Semantics.quantity.unitOfMeasure: 'Netweightunit'
      
      netweight       as Netweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      netweightunit   as Netweightunit,
      step            as Step,
      step1ok        as Step1Ok,
      step2ok        as Step2Ok,
      issummited     as IsSummited
} 


The abstract view used as parameter for the action identifycard

@EndUserText.label: 'Abstract View for Identification'
define abstract entity ZAE_WR_WEIHINGSESSION
  
{
    vbeln : vbeln_va;
    loadtype : load_type;
    
}


