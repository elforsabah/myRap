sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI;   // sap.fe.core.ExtensionAPI
    var oDialog;   // ONE dialog instance

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // in FE V4 this is ExtensionAPI

            if (oDialog) {
                oDialog.open();
                return;
            }

            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers   // .onDialogChoose / .onDialogCancel
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);
                oDialog.open();
            });
        },

        onDialogChoose: function () {
            // Use the IDs from the fragment
            var oTopTable    = oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId("ServiceWRTable");

            function getSelectedObjects(oTable) {
                if (!oTable || !oTable.getSelectedContexts) { return []; }
                var aCtx = oTable.getSelectedContexts() || [];
                return aCtx.map(function (oCtx) { return oCtx.getObject(); });
            }

            var aTopSelected    = getSelectedObjects(oTopTable);
            var aBottomSelected = getSelectedObjects(oBottomTable);

            if (!aTopSelected.length && !aBottomSelected.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            var aAttachmentItems = aTopSelected;
            var aServiceWRItems  = aBottomSelected;

            var sAttachmentJson = JSON.stringify(aAttachmentItems);
            var sServiceWRJson  = JSON.stringify(aServiceWRItems);

            // IMPORTANT: use ExtensionAPI, not this.getView()
            var oModel = oExtAPI.getModel();

            var oActionBinding = oModel.bindContext(
                "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
            );

            oActionBinding.setParameter("AttachmentItemsjson", sAttachmentJson);
            oActionBinding.setParameter("ServiceWRItemsjson",  sServiceWRJson);

            oActionBinding.execute("$auto").then(function (oResultContext) {
                var oReturnedTour = oResultContext.getObject();
                sap.m.MessageToast.show("Documents were generated successfully.");
                oModel.refresh();
            }).catch(function (oError) {
                sap.m.MessageBox.error(oError.message || "Error while generating documents.");
            });

            MessageToast.show(
                "Selected: " + aTopSelected.length + " top, " +
                aBottomSelected.length + " bottom"
            );

            if (oDialog) {
                oDialog.close();
            }
        },

        onDialogCancel: function (oEvent) {
            if (oEvent && oEvent.getSource) {
                var oSource = oEvent.getSource();
                var oParent = oSource.getParent();
                if (oParent && oParent.close) {
                    oParent.close();
                    return;
                }
            }
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});





<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:macros="sap.fe.macros">

    <Dialog
        id="TwoSmartTablesDialog"
        title="Choose Items"
        stretch="true"
        contentWidth="1200px"
        contentHeight="600px"
        class="sapUiResponsivePadding">

        <content>
            <VBox id="vbMain" width="100%" height="100%" renderType="Div">

                <!-- ========= ATTACHMENT “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="AttachmentFilterBar"
                    contextPath="/PrintConfiguration"
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                    <macros:Table
                        id="AttachmentTable"
                        contextPath="/PrintConfiguration"
                        metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                        filterBar="AttachmentFilterBar"
                        type="GridTable"             
                        selectionMode="ForceMulti"    
                        header="Attachments" />

                <Toolbar id="tbSpacer1" design="Transparent">
                    <ToolbarSpacer id="tb1" />
                </Toolbar>

                 ========= SERVICE WR “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="ServiceWRFilterBar"
                    contextPath="/ServiceWR"
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                <macros:Table
                    id="ServiceWRTable"
                    contextPath="/ServiceWR"
                    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                    filterBar="ServiceWRFilterBar"
                    selectionMode="ForceMulti"
                    header="Service WR" />

            </VBox>
        </content>

        <beginButton>
            <Button
                id="btnChoose"
                text="Choose"
                type="Emphasized"
                press=".onDialogChoose" />
        </beginButton>

        <endButton>
            <Button
                id="btnCancel"
                text="Cancel"
                press=".onDialogCancel" />
        </endButton>

    </Dialog>
</core:FragmentDefinition>



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
        <Schema Namespace="com.sap.gateway.srvd.zsd_pdattacments.v0001" Alias="SAP__self">
            <Annotation Term="SAP__core.SchemaVersion" String="1.0.0"/>
            <EntityType Name="PrintConfigurationType">
                <Key>
                    <PropertyRef Name="Field"/>
                    <PropertyRef Name="Printform"/>
                    <PropertyRef Name="Formtype"/>
                </Key>
                <Property Name="Field" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="Printform" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="Formtype" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="ParameterIn" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="Filenameteemplate" Type="Edm.String" Nullable="false" MaxLength="80"/>
            </EntityType>
            <EntityType Name="AttachmentType">
                <Key>
                    <PropertyRef Name="Uuid"/>
                    <PropertyRef Name="AttachmentUuid"/>
                    <PropertyRef Name="TourUUID"/>
                </Key>
                <Property Name="Uuid" Type="Edm.Guid" Nullable="false"/>
                <Property Name="AttachmentUuid" Type="Edm.Guid" Nullable="false"/>
                <Property Name="TourUUID" Type="Edm.Guid" Nullable="false"/>
                <Property Name="TourId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="TourSequence" Type="Edm.String" Nullable="false" MaxLength="5"/>
                <Property Name="ReferenceId" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="Filename" Type="Edm.String" Nullable="false" MaxLength="128"/>
                <Property Name="Mimetype" Type="Edm.String" Nullable="false" MaxLength="128"/>
                <Property Name="Attachment" Type="Edm.Stream"/>
                <Property Name="CreatedBy" Type="Edm.String" Nullable="false" MaxLength="12"/>
                <Property Name="CreatedAt" Type="Edm.DateTimeOffset" Precision="7"/>
                <NavigationProperty Name="_Tour" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourType" Nullable="false" Partner="_Attachments">
                    <ReferentialConstraint Property="TourUUID" ReferencedProperty="TourUuid"/>
                </NavigationProperty>
            </EntityType>
            <EntityType Name="ResourceVHType">
                <Key>
                    <PropertyRef Name="ResourceId"/>
                </Key>
                <Property Name="ResourceId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="SemanticContext" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="Description" Type="Edm.String" Nullable="false" MaxLength="80"/>
            </EntityType>
            <EntityType Name="TourType">
                <Key>
                    <PropertyRef Name="TourUuid"/>
                </Key>
                <Property Name="TourUuid" Type="Edm.Guid" Nullable="false"/>
                <Property Name="TourId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="TourTemplate" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="TourTemplateName" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ColorTour" Type="Edm.String" Nullable="false" MaxLength="7"/>
                <Property Name="TourStatus" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="TourStatusText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="TourStatusColorValue" Type="Edm.Byte" Nullable="false"/>
                <Property Name="TourStatusIcon" Type="Edm.String" Nullable="false" MaxLength="25"/>
                <Property Name="TourStartDate" Type="Edm.Date"/>
                <Property Name="TourEndDate" Type="Edm.Date"/>
                <Property Name="ScheduledDateTimeStart" Type="Edm.DateTimeOffset"/>
                <Property Name="MainResourceId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="WorkStatus" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <Property Name="WorkStatusText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="WorkStatusIcon" Type="Edm.String" Nullable="false" MaxLength="23"/>
                <Property Name="TourCriticality" Type="Edm.Byte" Nullable="false"/>
                <Property Name="__OperationControl" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourOperationControl"/>
                <Property Name="SAP__Messages" Type="Collection(com.sap.gateway.srvd.zsd_pdattacments.v0001.SAP__Message)" Nullable="false"/>
                <NavigationProperty Name="_Attachments" Type="Collection(com.sap.gateway.srvd.zsd_pdattacments.v0001.AttachmentType)" Partner="_Tour">
                    <OnDelete Action="Cascade"/>
                </NavigationProperty>
                <NavigationProperty Name="_ServiceAssignments" Type="Collection(com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceAssignmentType)" Partner="_Tour">
                    <OnDelete Action="Cascade"/>
                </NavigationProperty>
            </EntityType>
            <EntityType Name="TourStatusVHType">
                <Key>
                    <PropertyRef Name="TourStatus"/>
                    <PropertyRef Name="Language"/>
                </Key>
                <Property Name="TourStatus" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Property Name="Language" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="StatusDescription" Type="Edm.String" Nullable="false" MaxLength="60"/>
            </EntityType>
            <EntityType Name="ServiceWRType">
                <Key>
                    <PropertyRef Name="ServiceUUID"/>
                </Key>
                <Property Name="ServiceUUID" Type="Edm.Guid" Nullable="false"/>
                <Property Name="ServiceId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="Profile" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="ServiceType" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="ServiceTypeDescription" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ServiceStatus" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="ServiceStatusText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="ServiceStatusIcon" Type="Edm.String" Nullable="false" MaxLength="25"/>
                <Property Name="ReferenceId" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="ServicePriority" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="PlanningStatusText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="PlanningStatus" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <Property Name="PlanningStatusCriticality" Type="Edm.Byte" Nullable="false"/>
                <Property Name="RequestedDate" Type="Edm.Date"/>
                <Property Name="EarliestDate" Type="Edm.Date"/>
                <Property Name="LatestDate" Type="Edm.Date"/>
                <Property Name="CustomerInfo" Type="Edm.String" Nullable="false" MaxLength="80"/>
                <Property Name="AdditionalText" Type="Edm.String" Nullable="false" MaxLength="80"/>
                <Property Name="FunctionalLocation" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="FullAddress" Type="Edm.String" Nullable="false" MaxLength="220"/>
                <Property Name="TotalDurationUnit" Type="Edm.String" Nullable="false" MaxLength="3"/>
                <Property Name="TotalDuration" Type="Edm.Decimal" Nullable="false" Precision="13" Scale="3"/>
                <Property Name="WorkStatus" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <Property Name="WorkStatusText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="WorkStatusIcon" Type="Edm.String" Nullable="false" MaxLength="23"/>
                <Property Name="ServiceWindow" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="ServiceWindowText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="ServiceWindowStartTime" Type="Edm.TimeOfDay" Nullable="false"/>
                <Property Name="ServiceWindowEndTime" Type="Edm.TimeOfDay" Nullable="false"/>
                <Property Name="TourId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="Material" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="MaterialWithText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="MaterialText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="MaterialWeight" Type="Edm.Decimal" Nullable="false" Precision="13" Scale="3"/>
                <Property Name="MaterialWeightUnit" Type="Edm.String" Nullable="false" MaxLength="3"/>
                <Property Name="MaterialGroup" Type="Edm.String" Nullable="false" MaxLength="9"/>
                <Property Name="MaterialGroupText" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="PlantLocation" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="PlantLocationText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerSourceLocation" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="ContainerSourceAddress" Type="Edm.String" Nullable="false" MaxLength="220"/>
                <Property Name="ContainerFinalLocation" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="ContainerFinalAddress" Type="Edm.String" Nullable="false" MaxLength="220"/>
                <Property Name="ContainerTypeAtLocation" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerTypeAtLocationWiText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerTypeAtLocationText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerAtLocationCount" Type="Edm.Decimal" Nullable="false" Precision="3"/>
                <Property Name="ContainerAtLocationTidnr" Type="Edm.String" Nullable="false" MaxLength="25"/>
                <Property Name="ContainerTypeNew" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerTypeNewWithText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerTypeNewText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerNewCount" Type="Edm.Decimal" Nullable="false" Precision="3"/>
                <Property Name="ContainerNewTidnr" Type="Edm.String" Nullable="false" MaxLength="25"/>
                <Property Name="ServiceFrequency" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="ServiceFrequencyText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="MapTitle" Type="Edm.String" Nullable="false"/>
                <Property Name="MapPopup" Type="Edm.String" Nullable="false"/>
                <Property Name="MapColor" Type="Edm.String" Nullable="false"/>
                <Property Name="MapHighlight" Type="Edm.String" Nullable="false"/>
                <Property Name="MapSymbol" Type="Edm.String" Nullable="false"/>
                <Property Name="service_criticality" Type="Edm.Byte" Nullable="false"/>
                <Property Name="point_of_origin" Type="Edm.String" Nullable="false" MaxLength="22"/>
                <Property Name="point_of_origin_ver" Type="Edm.String" Nullable="false" MaxLength="22"/>
                <Property Name="__OperationControl" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWROperationControl"/>
                <Property Name="SAP__Messages" Type="Collection(com.sap.gateway.srvd.zsd_pdattacments.v0001.SAP__Message)" Nullable="false"/>
            </EntityType>
            <EntityType Name="ServiceAssignmentType">
                <Key>
                    <PropertyRef Name="TourUuid"/>
                    <PropertyRef Name="ServiceUuid"/>
                </Key>
                <Property Name="TourUuid" Type="Edm.Guid" Nullable="false"/>
                <Property Name="ServiceUuid" Type="Edm.Guid" Nullable="false"/>
                <Property Name="TourId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="ServiceId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="ServiceType" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="ServiceTypeText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ServiceStatus" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="ServiceStatusText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="ServiceStatusIcon" Type="Edm.String" Nullable="false" MaxLength="25"/>
                <Property Name="Removed" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="TourSequence" Type="Edm.String" Nullable="false" MaxLength="5"/>
                <Property Name="ReferenceId" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="CustomerInfo" Type="Edm.String" Nullable="false" MaxLength="80"/>
                <Property Name="AdditionalText" Type="Edm.String" Nullable="false" MaxLength="80"/>
                <Property Name="FunctionalLocation" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="FullAddress" Type="Edm.String" Nullable="false" MaxLength="220"/>
                <Property Name="Material" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="MaterialWithText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="MaterialText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="MaterialWeight" Type="Edm.Decimal" Nullable="false" Precision="13" Scale="3"/>
                <Property Name="MaterialWeightUnit" Type="Edm.String" Nullable="false" MaxLength="3"/>
                <Property Name="MaterialGroup" Type="Edm.String" Nullable="false" MaxLength="9"/>
                <Property Name="MaterialGroupText" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="PlantLocation" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="PlantLocationText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerSourceLocation" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="ContainerSourceAddress" Type="Edm.String" Nullable="false" MaxLength="220"/>
                <Property Name="ContainerFinalLocation" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="ContainerFinalAddress" Type="Edm.String" Nullable="false" MaxLength="220"/>
                <Property Name="ContainerTypeAtLocation" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerTypeAtLocationWiText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerTypeAtLocationText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerAtLocationCount" Type="Edm.Decimal" Nullable="false" Precision="3"/>
                <Property Name="ContainerAtLocationTidnr" Type="Edm.String" Nullable="false" MaxLength="25"/>
                <Property Name="ContainerTypeNew" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerTypeNewWithText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerTypeNewText" Type="Edm.String" Nullable="false" MaxLength="40"/>
                <Property Name="ContainerNewCount" Type="Edm.Decimal" Nullable="false" Precision="3"/>
                <Property Name="ContainerNewTidnr" Type="Edm.String" Nullable="false" MaxLength="25"/>
                <Property Name="ServiceFrequency" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="ServiceFrequencyText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="TotalDuration" Type="Edm.Decimal" Nullable="false" Precision="13" Scale="3"/>
                <Property Name="TotalDurationUnit" Type="Edm.String" Nullable="false" MaxLength="3"/>
                <Property Name="WorkStatus" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <Property Name="WorkStatusText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="WorkStatusIcon" Type="Edm.String" Nullable="false" MaxLength="23"/>
                <Property Name="PlanningStatus" Type="Edm.String" Nullable="false" MaxLength="1"/>
                <NavigationProperty Name="_Service" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType"/>
                <NavigationProperty Name="_Tour" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourType" Nullable="false" Partner="_ServiceAssignments">
                    <ReferentialConstraint Property="TourUuid" ReferencedProperty="TourUuid"/>
                </NavigationProperty>
            </EntityType>
            <EntityType Name="TourIdVHType">
                <Key>
                    <PropertyRef Name="TourUUID"/>
                    <PropertyRef Name="WorkArea"/>
                </Key>
                <Property Name="TourUUID" Type="Edm.Guid" Nullable="false"/>
                <Property Name="WorkArea" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="TourId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Property Name="TourTemplate" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="TourStatus" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="TourStatusText" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="StartDate" Type="Edm.Date"/>
                <Property Name="EndDate" Type="Edm.Date"/>
                <Property Name="WorkStatus" Type="Edm.String" Nullable="false" MaxLength="1"/>
            </EntityType>
            <EntityType Name="TourTemplateVHType">
                <Key>
                    <PropertyRef Name="WorkArea"/>
                    <PropertyRef Name="TourTemplate"/>
                </Key>
                <Property Name="WorkArea" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="TourTemplate" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Property Name="TourTemplateName" Type="Edm.String" Nullable="false" MaxLength="40"/>
            </EntityType>
            <EntityType Name="WorkStatusVHType">
                <Key>
                    <PropertyRef Name="Value"/>
                    <PropertyRef Name="Language"/>
                </Key>
                <Property Name="Value" Type="Edm.String" Nullable="false" MaxLength="10"/>
                <Property Name="Language" Type="Edm.String" Nullable="false" MaxLength="2"/>
                <Property Name="Description" Type="Edm.String" Nullable="false" MaxLength="60"/>
                <Property Name="IconURL" Type="Edm.String" Nullable="false" MaxLength="23"/>
            </EntityType>
            <ComplexType Name="TourOperationControl">
                <Property Name="RegenerateDocs" Type="Edm.Boolean" Nullable="false"/>
            </ComplexType>
            <ComplexType Name="ServiceWROperationControl">
                <Property Name="assignTour" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="changeServiceType" Type="Edm.Boolean" Nullable="false"/>
                <Property Name="createTour" Type="Edm.Boolean" Nullable="false"/>
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
            <Action Name="assignTour" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType" Nullable="false"/>
                <Parameter Name="TourId" Type="Edm.String" Nullable="false" MaxLength="20"/>
                <Parameter Name="TourUUID" Type="Edm.Guid" Nullable="true"/>
                <Parameter Name="Sequence" Type="Edm.String" Nullable="false" MaxLength="5"/>
                <Parameter Name="PlaceBefore" Type="Edm.Boolean" Nullable="false"/>
                <ReturnType Type="Collection(com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType)" Nullable="true"/>
            </Action>
            <Action Name="generatedocuments" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="Collection(com.sap.gateway.srvd.zsd_pdattacments.v0001.TourType)" Nullable="false"/>
                <Parameter Name="AttachmentItemsjson" Type="Edm.String" Nullable="false"/>
                <Parameter Name="ServiceWRItemsjson" Type="Edm.String" Nullable="false"/>
                <ReturnType Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourType" Nullable="false"/>
            </Action>
            <Action Name="RegenerateDocs" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourType" Nullable="false"/>
            </Action>
            <Action Name="changeServiceType" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType" Nullable="false"/>
                <Parameter Name="ServiceTypeFrom" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Parameter Name="ServiceTypeTo" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <ReturnType Type="Collection(com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType)" Nullable="true"/>
            </Action>
            <Action Name="createTour" EntitySetPath="_it" IsBound="true">
                <Parameter Name="_it" Type="com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType" Nullable="false"/>
                <Parameter Name="tour_template" Type="Edm.String" Nullable="false" MaxLength="30"/>
                <Parameter Name="start_date" Type="Edm.Date" Nullable="true"/>
                <Parameter Name="created_for_simulation" Type="Edm.Boolean" Nullable="false"/>
                <Parameter Name="without_draft" Type="Edm.Boolean" Nullable="false"/>
                <Parameter Name="Sequence" Type="Edm.String" Nullable="false" MaxLength="5"/>
                <ReturnType Type="Collection(com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType)" Nullable="true"/>
            </Action>
            <EntityContainer Name="Container">
                <EntitySet Name="Attachment" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.AttachmentType">
                    <NavigationPropertyBinding Path="_Tour" Target="Tour"/>
                </EntitySet>
                <EntitySet Name="PrintConfiguration" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.PrintConfigurationType"/>
                <EntitySet Name="ResourceVH" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.ResourceVHType"/>
                <EntitySet Name="ServiceAssignment" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceAssignmentType">
                    <NavigationPropertyBinding Path="_Service" Target="ServiceWR"/>
                    <NavigationPropertyBinding Path="_Tour" Target="Tour"/>
                </EntitySet>
                <EntitySet Name="ServiceWR" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType"/>
                <EntitySet Name="Tour" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourType">
                    <NavigationPropertyBinding Path="_Attachments" Target="Attachment"/>
                    <NavigationPropertyBinding Path="_ServiceAssignments" Target="ServiceAssignment"/>
                </EntitySet>
                <EntitySet Name="TourIdVH" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourIdVHType"/>
                <EntitySet Name="TourStatusVH" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourStatusVHType"/>
                <EntitySet Name="TourTemplateVH" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.TourTemplateVHType"/>
                <EntitySet Name="WorkStatusVH" EntityType="com.sap.gateway.srvd.zsd_pdattacments.v0001.WorkStatusVHType"/>
            </EntityContainer>
            <Annotations Target="SAP__self.PrintConfigurationType/Field">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Field Name"/>
            </Annotations>
            <Annotations Target="SAP__self.PrintConfigurationType/Printform">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Form"/>
                <Annotation Term="SAP__common.Heading" String="Name of PDF-Based Form"/>
                <Annotation Term="SAP__common.QuickInfo" String="PDF-Based Forms: Form Name"/>
            </Annotations>
            <Annotations Target="SAP__self.PrintConfigurationType/Formtype">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Formtype Printform"/>
                <Annotation Term="SAP__common.Heading" String="Formtype of Printforms"/>
                <Annotation Term="SAP__common.QuickInfo" String="Formtype of Printforms"/>
            </Annotations>
            <Annotations Target="SAP__self.PrintConfigurationType/ParameterIn">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Parameter Name"/>
                <Annotation Term="SAP__common.Heading" String="Parameter name"/>
                <Annotation Term="SAP__common.QuickInfo" String="Parameter name"/>
            </Annotations>
            <Annotations Target="SAP__self.PrintConfigurationType">
                <Annotation Term="SAP__common.Label" String="Interface View for ZWR_CWAPRTCONF"/>
                <Annotation Term="SAP__UI.LineItem">
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="Field"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="Printform"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="Formtype"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ParameterIn"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="Filenameteemplate"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.SelectionFields">
                    <Collection>
                        <PropertyPath>Field</PropertyPath>
                    </Collection>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.Container/PrintConfiguration">
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
            <Annotations Target="SAP__self.AttachmentType/Uuid">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Tour UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/AttachmentUuid">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Attachment UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/TourUUID">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Tour UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/TourId">
                <Annotation Term="SAP__common.Label" String="Tour ID"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Tour ID"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Tour Id"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/TourSequence">
                <Annotation Term="SAP__common.IsDigitSequence"/>
                <Annotation Term="SAP__common.Label" String="Sequence Number"/>
                <Annotation Term="SAP__common.QuickInfo" String="Sequence number"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/Filename">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Filename"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/Mimetype">
                <Annotation Term="SAP__core.IsMediaType"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Mimetype"/>
                <Annotation Term="SAP__common.Heading" String="Mimetype for Attachment"/>
                <Annotation Term="SAP__common.QuickInfo" String="Attachment Mimetype"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/Attachment">
                <Annotation Term="SAP__core.ContentDisposition">
                    <Record>
                        <PropertyValue Property="Filename" Path="Filename"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__core.MediaType" Path="Mimetype"/>
                <Annotation Term="SAP__common.Label" String="Attachment"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/CreatedBy">
                <Annotation Term="SAP__common.Label" String="Created By"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="User"/>
                <Annotation Term="SAP__common.QuickInfo" String="User Name"/>
            </Annotations>
            <Annotations Target="SAP__self.AttachmentType/CreatedAt">
                <Annotation Term="SAP__common.Label" String="Created At"/>
                <Annotation Term="SAP__common.Heading" String="Time Stamp"/>
                <Annotation Term="SAP__common.QuickInfo" String="UTC Time Stamp in Long Form (YYYYMMDDhhmmssmmmuuun)"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/Attachment">
                <Annotation Term="SAP__capabilities.NavigationRestrictions">
                    <Record>
                        <PropertyValue Property="RestrictedProperties">
                            <Collection>
                                <Record>
                                    <PropertyValue Property="NavigationProperty" NavigationPropertyPath="_Tour"/>
                                    <PropertyValue Property="InsertRestrictions">
                                        <Record>
                                            <PropertyValue Property="Insertable" Bool="false"/>
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
            <Annotations Target="SAP__self.AttachmentType">
                <Annotation Term="SAP__common.Label" String="Tour Service Attachments"/>
                <Annotation Term="SAP__UI.HeaderInfo">
                    <Record>
                        <PropertyValue Property="TypeName" String="Document"/>
                        <PropertyValue Property="TypeNamePlural" String="Documents"/>
                        <PropertyValue Property="Title">
                            <Record Type="SAP__UI.DataField">
                                <PropertyValue Property="Value" String="">
                                    <Annotation Term="SAP__core.Messages">
                                        <Collection>
                                            <Record>
                                                <PropertyValue Property="code" String="SADL_GW_EXP_VOCAN-001"/>
                                                <PropertyValue Property="message" String="Property 'AttachmentUUID' of annotation 'UI.HEADERINFO.TITLE' in proj./view 'Attachment' not found"/>
                                                <PropertyValue Property="severity" String="error"/>
                                            </Record>
                                        </Collection>
                                    </Annotation>
                                </PropertyValue>
                            </Record>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.LineItem">
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Label" String="Order"/>
                            <PropertyValue Property="Value" Path="ReferenceId"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Label" String="Tour Sequence"/>
                            <PropertyValue Property="Value" Path="TourSequence"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Label" String="Document"/>
                            <PropertyValue Property="Value" Path="Attachment"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="CreatedAt"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="CreatedBy"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAction">
                            <PropertyValue Property="Label" String="Delete"/>
                            <PropertyValue Property="Action" String="">
                                <Annotation Term="SAP__core.Messages">
                                    <Collection>
                                        <Record>
                                            <PropertyValue Property="message" String="ERROR: Mandatory Value for Action not found"/>
                                            <PropertyValue Property="severity" String="error"/>
                                        </Record>
                                    </Collection>
                                </Annotation>
                            </PropertyValue>
                            <PropertyValue Property="InvocationGrouping" EnumMember="SAP__UI.OperationGroupingType/Isolated"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.PresentationVariant">
                    <Record>
                        <PropertyValue Property="SortOrder">
                            <Collection>
                                <Record Type="SAP__common.SortOrderType">
                                    <PropertyValue Property="Property" PropertyPath="CreatedAt"/>
                                    <PropertyValue Property="Descending" Bool="true"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="Visualizations">
                            <Collection>
                                <AnnotationPath>@SAP__UI.LineItem</AnnotationPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="Communication.Contact">
                    <Record/>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.ResourceVHType/ResourceId">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Resource"/>
                <Annotation Term="SAP__common.QuickInfo" String="Resource Id"/>
            </Annotations>
            <Annotations Target="SAP__self.ResourceVHType/SemanticContext">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Semantic Context"/>
                <Annotation Term="SAP__common.QuickInfo" String="Semantical Context"/>
            </Annotations>
            <Annotations Target="SAP__self.ResourceVHType">
                <Annotation Term="SAP__common.Label" String="Main Resource Value Help"/>
                <Annotation Term="SAP__UI.LineItem">
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ResourceId"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="SemanticContext"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="Description"/>
                        </Record>
                    </Collection>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.Container/ResourceVH">
                <Annotation Term="SAP__capabilities.SearchRestrictions">
                    <Record>
                        <PropertyValue Property="Searchable" Bool="true"/>
                        <PropertyValue Property="UnsupportedExpressions" EnumMember="SAP__capabilities.SearchExpressions/AND SAP__capabilities.SearchExpressions/OR SAP__capabilities.SearchExpressions/NOT SAP__capabilities.SearchExpressions/group SAP__capabilities.SearchExpressions/phrase"/>
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
            <Annotations Target="SAP__self.TourType/TourUuid">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Tour UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourId">
                <Annotation Term="SAP__common.Label" String="Tour ID"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdtourwa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-zc_pdtour.tourid'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Tour ID"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Tour Id"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourTemplate">
                <Annotation Term="SAP__common.Label" String="Tour Template"/>
                <Annotation Term="SAP__common.Text" Path="TourTemplateName">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextSeparate"/>
                </Annotation>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdtourtemplatewa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-zc_pdtour.tourtemplate'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Tour Template"/>
                <Annotation Term="SAP__common.QuickInfo" String="Tour Template"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourTemplateName">
                <Annotation Term="SAP__common.Label" String="Template Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Text"/>
                <Annotation Term="SAP__common.QuickInfo" String="Text with 40 characters"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/ColorTour">
                <Annotation Term="SAP__common.Label" String="Color Tour"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Tour Template Color Tour"/>
                <Annotation Term="SAP__common.QuickInfo" String="Tourtemplate Color Tour"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourStatus">
                <Annotation Term="SAP__common.Label" String="Tour Status"/>
                <Annotation Term="SAP__common.Text" Path="TourStatusText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdtourstatus_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-zc_pdtour.tourstatus'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__common.Heading" String="Tour Status"/>
                <Annotation Term="SAP__common.QuickInfo" String="Tour status"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourStatusText">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourStatusColorValue">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Critlty Color Value"/>
                <Annotation Term="SAP__common.Heading" String="Criticality Color Value"/>
                <Annotation Term="SAP__common.QuickInfo" String="Criticality Color Value"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourStatusIcon">
                <Annotation Term="SAP__common.Label" String="Tour Status"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.IsImageURL"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourStartDate">
                <Annotation Term="SAP__common.Label" String="Tour Start Date"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourEndDate">
                <Annotation Term="SAP__common.Label" String="Tour End Date"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/ScheduledDateTimeStart">
                <Annotation Term="SAP__common.Label" String="Scheduled Start"/>
                <Annotation Term="SAP__common.Heading" String="Date Time From"/>
                <Annotation Term="SAP__common.QuickInfo" String="Date/Time From as Timestamp"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/MainResourceId">
                <Annotation Term="SAP__common.Label" String="Main Resource"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdresource_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-zc_pdtour.mainresourceid'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Resource"/>
                <Annotation Term="SAP__common.QuickInfo" String="Resource Id"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/WorkStatus">
                <Annotation Term="SAP__common.Label" String="Work Status"/>
                <Annotation Term="SAP__common.Text" Path="WorkStatusText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsDigitSequence"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdworkstatus_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-zc_pdtour.workstatus'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/WorkStatusText">
                <Annotation Term="SAP__common.Label" String="Work Status Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/WorkStatusIcon">
                <Annotation Term="SAP__common.Label" String="Work Status"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.IsImageURL"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/TourCriticality">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/__OperationControl">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/_Attachments">
                <Annotation Term="SAP__common.Composition"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/Tour">
                <Annotation Term="SAP__capabilities.NavigationRestrictions">
                    <Record>
                        <PropertyValue Property="RestrictedProperties">
                            <Collection>
                                <Record>
                                    <PropertyValue Property="NavigationProperty" NavigationPropertyPath="_Attachments"/>
                                    <PropertyValue Property="InsertRestrictions">
                                        <Record>
                                            <PropertyValue Property="Insertable" Bool="false"/>
                                        </Record>
                                    </PropertyValue>
                                </Record>
                                <Record>
                                    <PropertyValue Property="NavigationProperty" NavigationPropertyPath="_ServiceAssignments"/>
                                    <PropertyValue Property="InsertRestrictions">
                                        <Record>
                                            <PropertyValue Property="Insertable" Bool="false"/>
                                        </Record>
                                    </PropertyValue>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
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
                                    <PropertyValue Property="Property" PropertyPath="TourStatus"/>
                                    <PropertyValue Property="AllowedExpressions" String="MultiValue"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="TourStartDate"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="TourEndDate"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="ScheduledDateTimeStart"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="RequiredProperties">
                            <Collection>
                                <PropertyPath>TourStartDate</PropertyPath>
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
            <Annotations Target="SAP__self.TourType/_ServiceAssignments">
                <Annotation Term="SAP__common.Composition"/>
            </Annotations>
            <Annotations Target="SAP__self.RegenerateDocs(SAP__self.TourType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/RegenerateDocs"/>
                <Annotation Term="SAP__common.SideEffects" Qualifier="SAP__Action_RegenerateDocs">
                    <Record Type="SAP__common.SideEffectsType">
                        <PropertyValue Property="TargetEntities">
                            <Collection>
                                <NavigationPropertyPath>_it/_Attachments</NavigationPropertyPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.TourType">
                <Annotation Term="SAP__common.Label" String="Consumption View (Tour WR)"/>
                <Annotation Term="SAP__UI.Facets">
                    <Collection>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="Label" String="Services"/>
                            <PropertyValue Property="ID" String="SrvLineItemRef"/>
                            <PropertyValue Property="Target" AnnotationPath="_ServiceAssignments/@SAP__UI.LineItem"/>
                        </Record>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="Label" String="Documents"/>
                            <PropertyValue Property="ID" String="DocLineItemRef"/>
                            <PropertyValue Property="Target" AnnotationPath="_Attachments/@SAP__UI.LineItem"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.HeaderInfo">
                    <Record>
                        <PropertyValue Property="TypeName" String="Tour"/>
                        <PropertyValue Property="TypeNamePlural" String="Tours"/>
                        <PropertyValue Property="Title">
                            <Record Type="SAP__UI.DataField">
                                <PropertyValue Property="Value" Path="TourId"/>
                            </Record>
                        </PropertyValue>
                        <PropertyValue Property="Description">
                            <Record Type="SAP__UI.DataField">
                                <PropertyValue Property="Value" Path="TourTemplateName"/>
                            </Record>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.Identification">
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourId"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.LineItem">
                    <Annotation Term="SAP__UI.Criticality" Path="TourCriticality"/>
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourUuid"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourId"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAction">
                            <PropertyValue Property="Label" String="Regenerate Documents"/>
                            <PropertyValue Property="Action" String="com.sap.gateway.srvd.zsd_pdattacments.v0001.RegenerateDocs(com.sap.gateway.srvd.zsd_pdattacments.v0001.TourType)"/>
                            <PropertyValue Property="InvocationGrouping" EnumMember="SAP__UI.OperationGroupingType/ChangeSet"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourTemplate"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="WorkStatusIcon"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                            <Annotation Term="SAP__HTML5.CssDefaults">
                                <Record>
                                    <PropertyValue Property="width" String="5rem"/>
                                </Record>
                            </Annotation>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Criticality" Path="TourStatusColorValue"/>
                            <PropertyValue Property="CriticalityRepresentation" EnumMember="SAP__UI.CriticalityRepresentationType/WithIcon"/>
                            <PropertyValue Property="Value" Path="TourStatus"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourStatusIcon"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                            <Annotation Term="SAP__HTML5.CssDefaults">
                                <Record>
                                    <PropertyValue Property="width" String="5rem"/>
                                </Record>
                            </Annotation>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ScheduledDateTimeStart"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MainResourceId"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourUuid"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourStatusColorValue"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.PresentationVariant" Qualifier="DefaultMap">
                    <Record>
                        <PropertyValue Property="SortOrder">
                            <Collection>
                                <Record Type="SAP__common.SortOrderType">
                                    <PropertyValue Property="Property" PropertyPath="TourId"/>
                                    <PropertyValue Property="Descending" Bool="false"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="Visualizations">
                            <Collection>
                                <AnnotationPath>@SAP__UI.LineItem#DefaultMap</AnnotationPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.PresentationVariant">
                    <Record>
                        <PropertyValue Property="SortOrder">
                            <Collection>
                                <Record Type="SAP__common.SortOrderType">
                                    <PropertyValue Property="Property" PropertyPath="TourId"/>
                                    <PropertyValue Property="Descending" Bool="false"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="Visualizations">
                            <Collection>
                                <AnnotationPath>@SAP__UI.LineItem</AnnotationPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.SelectionFields">
                    <Collection>
                        <PropertyPath>TourStartDate</PropertyPath>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Messages" Path="SAP__Messages"/>
            </Annotations>
            <Annotations Target="SAP__self.TourStatusVHType/TourStatus">
                <Annotation Term="SAP__common.Text" Path="StatusDescription">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Lower Value"/>
                <Annotation Term="SAP__common.Heading" String="Fixed"/>
                <Annotation Term="SAP__common.QuickInfo" String="Values for Domains: Single Value/Lower Limit"/>
            </Annotations>
            <Annotations Target="SAP__self.TourStatusVHType">
                <Annotation Term="SAP__common.Label" String="Tour Status Value Help"/>
                <Annotation Term="SAP__UI.LineItem">
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourStatus"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="Communication.Contact">
                    <Record/>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.Container/TourStatusVH">
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
            <Annotations Target="SAP__self.ServiceWRType/ServiceUUID">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Service UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceId">
                <Annotation Term="SAP__common.Label" String="Service Id"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Service ID"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Service Id"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/Profile">
                <Annotation Term="SAP__common.Label" String="Profile"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdprofilewa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.profile'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__common.Heading" String="PDProfile"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Profile"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceType">
                <Annotation Term="SAP__common.Label" String="Service Type"/>
                <Annotation Term="SAP__common.Text" Path="ServiceTypeDescription">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdservicetypewa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.servicetype'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__common.Heading" String="Service Type"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Type"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceTypeDescription">
                <Annotation Term="SAP__common.Label" String="Service Type Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Text"/>
                <Annotation Term="SAP__common.QuickInfo" String="Text with 40 characters"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceStatus">
                <Annotation Term="SAP__common.Label" String="Service Status"/>
                <Annotation Term="SAP__common.Text" Path="ServiceStatusText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdservicestatus_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.servicestatus'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__common.Heading" String="Service Status"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Status"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceStatusText">
                <Annotation Term="SAP__common.Label" String="Service Status Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceStatusIcon">
                <Annotation Term="SAP__common.Label" String="Service Status"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.IsImageURL"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ReferenceId">
                <Annotation Term="SAP__common.Label" String="Order"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdorderwawr_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.referenceid'/$metadata</String>
                    </Collection>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServicePriority">
                <Annotation Term="SAP__common.Label" String="Service Priority"/>
                <Annotation Term="SAP__common.IsDigitSequence"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdservicepriority_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.servicepriority'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__common.Heading" String="Service Priority"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Priority"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/PlanningStatusText">
                <Annotation Term="SAP__common.Label" String="Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/PlanningStatus">
                <Annotation Term="SAP__common.Label" String="Planning Status"/>
                <Annotation Term="SAP__common.Text" Path="PlanningStatusText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdplanningstatus_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.planningstatus'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__common.FilterDefaultValue" String=""/>
                <Annotation Term="SAP__common.Heading" String="Planning Status"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning Status"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/PlanningStatusCriticality">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Critlty Color Value"/>
                <Annotation Term="SAP__common.Heading" String="Criticality Color Value"/>
                <Annotation Term="SAP__common.QuickInfo" String="Criticality Color Value"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/RequestedDate">
                <Annotation Term="SAP__common.Label" String="Requested Date"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/EarliestDate">
                <Annotation Term="SAP__common.Label" String="Earliest Date"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/LatestDate">
                <Annotation Term="SAP__common.Label" String="Latest Date"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/CustomerInfo">
                <Annotation Term="SAP__common.Label" String="Customer"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdcustomerinfowa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.customerinfo'/$metadata</String>
                    </Collection>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/AdditionalText">
                <Annotation Term="SAP__common.Label" String="Additional Information"/>
                <Annotation Term="SAP__common.Heading" String="Additional Text"/>
                <Annotation Term="SAP__common.QuickInfo" String="Additional Text"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/FunctionalLocation">
                <Annotation Term="SAP__common.Label" String="Customer Location"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdfunclocservicewa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.functionallocation'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Functional Location"/>
                <Annotation Term="SAP__common.QuickInfo" String="Functional Location"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/FullAddress">
                <Annotation Term="SAP__common.Label" String="Address"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdfunclocservicewa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.fulladdress'/$metadata</String>
                    </Collection>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/TotalDurationUnit">
                <Annotation Term="SAP__common.IsUnit"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Base Unit of Measure"/>
                <Annotation Term="SAP__common.Heading" String="BUn"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/TotalDuration">
                <Annotation Term="SAP__common.Label" String="Total Duration"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__measures.Unit" Path="TotalDurationUnit"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Duration"/>
                <Annotation Term="SAP__common.QuickInfo" String="Duration"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/WorkStatus">
                <Annotation Term="SAP__common.Label" String="Work Status"/>
                <Annotation Term="SAP__common.Text" Path="WorkStatusText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsDigitSequence"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdworkstatus_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.workstatus'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/WorkStatusText">
                <Annotation Term="SAP__common.Label" String="Work Status Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/WorkStatusIcon">
                <Annotation Term="SAP__common.Label" String="Work Status"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.IsImageURL"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceWindow">
                <Annotation Term="SAP__common.Label" String="Service Window"/>
                <Annotation Term="SAP__common.Text" Path="ServiceWindowText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdservicewindow_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.servicewindow'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__common.Heading" String="Service Window"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Window"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceWindowText">
                <Annotation Term="SAP__common.Label" String="Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceWindowStartTime">
                <Annotation Term="SAP__common.Label" String="Service Window Start Time"/>
                <Annotation Term="SAP__common.Heading" String="Servcice window start time"/>
                <Annotation Term="SAP__common.QuickInfo" String="Servcice window start time"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceWindowEndTime">
                <Annotation Term="SAP__common.Label" String="Service Window End Time"/>
                <Annotation Term="SAP__common.Heading" String="Service window end time"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service window end time"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/TourId">
                <Annotation Term="SAP__common.Label" String="Tour ID"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdtourwa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.tourid'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Tour ID"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Tour Id"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/Material">
                <Annotation Term="SAP__common.Label" String="Material"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_mdmaterial_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.material'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Transport Product"/>
                <Annotation Term="SAP__common.QuickInfo" String="Transport Product"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MaterialWithText">
                <Annotation Term="SAP__common.Label" String="Material"/>
                <Annotation Term="SAP__common.Text" Path="MaterialText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextLast"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product"/>
                <Annotation Term="SAP__common.QuickInfo" String="Transport Product"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MaterialText">
                <Annotation Term="SAP__common.Label" String="Material Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Product Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MaterialWeight">
                <Annotation Term="SAP__common.Label" String="Material Weight"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__measures.Unit" Path="MaterialWeightUnit"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product Quantity"/>
                <Annotation Term="SAP__common.QuickInfo" String="Transport Product Quantity"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MaterialWeightUnit">
                <Annotation Term="SAP__common.IsUnit"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Base Unit of Measure"/>
                <Annotation Term="SAP__common.Heading" String="BUn"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MaterialGroup">
                <Annotation Term="SAP__common.Label" String="Material Group"/>
                <Annotation Term="SAP__common.Text" Path="MaterialGroupText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextLast"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_materialgroup_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.materialgroup'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Product Group"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Group"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MaterialGroupText">
                <Annotation Term="SAP__common.Label" String="Material Group Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Product Group Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Group Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/PlantLocation">
                <Annotation Term="SAP__common.Label" String="Plant Location"/>
                <Annotation Term="SAP__common.Text" Path="PlantLocationText"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdnocustomerlocwa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.plantlocation'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Functional Location"/>
                <Annotation Term="SAP__common.QuickInfo" String="Functional Location"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/PlantLocationText">
                <Annotation Term="SAP__common.Label" String="Functional Location Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Text"/>
                <Annotation Term="SAP__common.QuickInfo" String="Text with 40 characters"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerSourceLocation">
                <Annotation Term="SAP__common.Label" String="Address - Container Source"/>
                <Annotation Term="SAP__common.Text" Path="ContainerSourceAddress"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdfunclocwa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.containersourcelocation'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Functional Location"/>
                <Annotation Term="SAP__common.QuickInfo" String="Functional Location"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerSourceAddress">
                <Annotation Term="SAP__common.Label" String="Address"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerFinalLocation">
                <Annotation Term="SAP__common.Label" String="Address - Container Final"/>
                <Annotation Term="SAP__common.Text" Path="ContainerFinalAddress"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdfunclocwa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.containerfinallocation'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Functional Location"/>
                <Annotation Term="SAP__common.QuickInfo" String="Functional Location"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerFinalAddress">
                <Annotation Term="SAP__common.Label" String="Address"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerTypeAtLocation">
                <Annotation Term="SAP__common.Label" String="Container Type (at Location)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_mdtransptpackaggt_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.containertypeatlocation'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Transport Product Packaging"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Packaging"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerTypeAtLocationWiText">
                <Annotation Term="SAP__common.Label" String="Container Type (at Location)"/>
                <Annotation Term="SAP__common.Text" Path="ContainerTypeAtLocationText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextLast"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product Packaging"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Packaging"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerTypeAtLocationText">
                <Annotation Term="SAP__common.Label" String="Packaging Type Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Product Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerAtLocationCount">
                <Annotation Term="SAP__common.Label" String="Container Count (at Location)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Quantity"/>
                <Annotation Term="SAP__common.QuickInfo" String="DEC 3"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerAtLocationTidnr">
                <Annotation Term="SAP__common.Label" String="Container ID (at Location)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Technical identification no."/>
                <Annotation Term="SAP__common.QuickInfo" String="Tech. Identification"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerTypeNew">
                <Annotation Term="SAP__common.Label" String="Container Type (New)"/>
                <Annotation Term="SAP__common.Text" Path="ContainerTypeNewText"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_mdtransptpackaggt_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdmnlservicewr.containertypenew'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Transport Product Packaging"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Packaging"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerTypeNewWithText">
                <Annotation Term="SAP__common.Label" String="Container Type (New)"/>
                <Annotation Term="SAP__common.Text" Path="ContainerTypeNewText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextLast"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product Packaging"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Packaging"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerTypeNewText">
                <Annotation Term="SAP__common.Label" String="Packaging Type Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Product Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerNewCount">
                <Annotation Term="SAP__common.Label" String="Container Count (New)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Quantity"/>
                <Annotation Term="SAP__common.QuickInfo" String="DEC 3"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ContainerNewTidnr">
                <Annotation Term="SAP__common.Label" String="Container ID (New)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Technical identification no."/>
                <Annotation Term="SAP__common.QuickInfo" String="Tech. Identification"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceFrequency">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Service Freq"/>
                <Annotation Term="SAP__common.Heading" String="Service Frequency"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Frequency"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/ServiceFrequencyText">
                <Annotation Term="SAP__common.Label" String="Service Frequency"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MapTitle">
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MapPopup">
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MapColor">
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MapHighlight">
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/MapSymbol">
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/service_criticality">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/point_of_origin">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Point Of Origin"/>
                <Annotation Term="SAP__common.QuickInfo" String="Point of origin WDOI"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/point_of_origin_ver">
                <Annotation Term="SAP__common.Label" String="Point of Origin"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Point Of Origin"/>
                <Annotation Term="SAP__common.QuickInfo" String="Point of origin WDOI"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/__OperationControl">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.assignTour(SAP__self.ServiceWRType)/TourId">
                <Annotation Term="SAP__common.Label" String="Tour ID"/>
                <Annotation Term="SAP__common.ValueListForValidation" String=""/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdtour_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.ae-*plce*c_pdmnlservicewr.assigntour.tourid.ServiceWRType'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Tour ID"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Tour Id"/>
            </Annotations>
            <Annotations Target="SAP__self.assignTour(SAP__self.ServiceWRType)/Sequence">
                <Annotation Term="SAP__common.Label" String="Target Sequence"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdtourservice_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.ae-*plce*c_pdmnlservicewr.assigntour.sequence.ServiceWRType'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Sequence Number"/>
                <Annotation Term="SAP__common.QuickInfo" String="Sequence number"/>
            </Annotations>
            <Annotations Target="SAP__self.assignTour(SAP__self.ServiceWRType)/PlaceBefore">
                <Annotation Term="SAP__common.Label" String="Place before"/>
                <Annotation Term="SAP__common.Heading" String="Truth Value"/>
                <Annotation Term="SAP__common.QuickInfo" String="Truth Value: True/False"/>
            </Annotations>
            <Annotations Target="SAP__self.assignTour(SAP__self.ServiceWRType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/assignTour"/>
            </Annotations>
            <Annotations Target="SAP__self.changeServiceType(SAP__self.ServiceWRType)/ServiceTypeFrom">
                <Annotation Term="SAP__common.Label" String="Change from"/>
                <Annotation Term="SAP__UI.ParameterDefaultValue" Path="_it/ServiceType"/>
                <Annotation Term="SAP__common.Heading" String="Service Type"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Type"/>
            </Annotations>
            <Annotations Target="SAP__self.changeServiceType(SAP__self.ServiceWRType)/ServiceTypeTo">
                <Annotation Term="SAP__common.Label" String="Change to"/>
                <Annotation Term="SAP__common.FieldControl" EnumMember="SAP__common.FieldControlType/Mandatory"/>
                <Annotation Term="SAP__common.ValueListForValidation" String=""/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/sap/zzpd_srvtp_map_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.ae-*plce*c_pdmnlservicewr.changeservicetype.servicetypeto.ServiceWRType'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Service Type"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Type"/>
            </Annotations>
            <Annotations Target="SAP__self.changeServiceType(SAP__self.ServiceWRType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/changeServiceType"/>
                <Annotation Term="SAP__common.SideEffects" Qualifier="SAP__Action_changeServiceType">
                    <Record Type="SAP__common.SideEffectsType">
                        <PropertyValue Property="TargetProperties">
                            <Collection>
                                <String>_it/ServiceType</String>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.createTour(SAP__self.ServiceWRType)/tour_template">
                <Annotation Term="SAP__common.Label" String="Template"/>
                <Annotation Term="SAP__common.ValueListForValidation" String=""/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdtourtemplatewa_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.ae-*plce*c_pdmnlservicewr.createtour.tour_template.ServiceWRType'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Heading" String="Tour Template"/>
                <Annotation Term="SAP__common.QuickInfo" String="Tour Template"/>
            </Annotations>
            <Annotations Target="SAP__self.createTour(SAP__self.ServiceWRType)/start_date">
                <Annotation Term="SAP__common.Label" String="Start Date"/>
                <Annotation Term="SAP__common.Heading" String="Date"/>
                <Annotation Term="SAP__common.QuickInfo" String="Date"/>
            </Annotations>
            <Annotations Target="SAP__self.createTour(SAP__self.ServiceWRType)">
                <Annotation Term="SAP__core.OperationAvailable" Path="_it/__OperationControl/createTour"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType">
                <Annotation Term="SAP__common.Label" String="Service WR"/>
                <Annotation Term="SAP__UI.ConnectedFields" Qualifier="cf_containertypeat">
                    <Record>
                        <PropertyValue Property="Data">
                            <Record>
                                <PropertyValue Property="containertype">
                                    <Record Type="SAP__UI.DataField">
                                        <PropertyValue Property="Value" Path="ContainerTypeAtLocationWiText"/>
                                    </Record>
                                </PropertyValue>
                                <PropertyValue Property="containertype_count">
                                    <Record Type="SAP__UI.DataField">
                                        <PropertyValue Property="Value" Path="ContainerAtLocationCount"/>
                                    </Record>
                                </PropertyValue>
                            </Record>
                        </PropertyValue>
                        <PropertyValue Property="Label" String="Container Type At Location"/>
                        <PropertyValue Property="Template" String="{containertype_count} {containertype}"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.ConnectedFields" Qualifier="cf_containertypenew">
                    <Record>
                        <PropertyValue Property="Data">
                            <Record>
                                <PropertyValue Property="containertype">
                                    <Record Type="SAP__UI.DataField">
                                        <PropertyValue Property="Value" Path="ContainerTypeNewWithText"/>
                                    </Record>
                                </PropertyValue>
                                <PropertyValue Property="containertype_count">
                                    <Record Type="SAP__UI.DataField">
                                        <PropertyValue Property="Value" Path="ContainerNewCount"/>
                                    </Record>
                                </PropertyValue>
                            </Record>
                        </PropertyValue>
                        <PropertyValue Property="Label" String="Container Type New"/>
                        <PropertyValue Property="Template" String="{containertype_count} {containertype}"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.ConnectedFields" Qualifier="cf_material">
                    <Record>
                        <PropertyValue Property="Data">
                            <Record>
                                <PropertyValue Property="material">
                                    <Record Type="SAP__UI.DataField">
                                        <PropertyValue Property="Value" Path="MaterialWithText"/>
                                    </Record>
                                </PropertyValue>
                                <PropertyValue Property="material_weight">
                                    <Record Type="SAP__UI.DataField">
                                        <PropertyValue Property="Value" Path="MaterialWeight"/>
                                    </Record>
                                </PropertyValue>
                            </Record>
                        </PropertyValue>
                        <PropertyValue Property="Label" String="Material"/>
                        <PropertyValue Property="Template" String="{material_weight} {material}"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.Facets">
                    <Collection>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="ID" String="F0_CoreDefault"/>
                            <PropertyValue Property="Target" String="UI.FACET$1$">
                                <Annotation Term="SAP__core.Messages">
                                    <Collection>
                                        <Record>
                                            <PropertyValue Property="code" String="SY-530"/>
                                            <PropertyValue Property="message" String="An exception was raised"/>
                                            <PropertyValue Property="severity" String="error"/>
                                        </Record>
                                    </Collection>
                                </Annotation>
                            </PropertyValue>
                        </Record>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="ID" String="F1_CoreDefault"/>
                            <PropertyValue Property="Target" String="UI.FACET$2$">
                                <Annotation Term="SAP__core.Messages">
                                    <Collection>
                                        <Record>
                                            <PropertyValue Property="code" String="SY-530"/>
                                            <PropertyValue Property="message" String="An exception was raised"/>
                                            <PropertyValue Property="severity" String="error"/>
                                        </Record>
                                    </Collection>
                                </Annotation>
                            </PropertyValue>
                        </Record>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="ID" String="F2_CoreDefault"/>
                            <PropertyValue Property="Target" String="UI.FACET$3$">
                                <Annotation Term="SAP__core.Messages">
                                    <Collection>
                                        <Record>
                                            <PropertyValue Property="code" String="SY-530"/>
                                            <PropertyValue Property="message" String="An exception was raised"/>
                                            <PropertyValue Property="severity" String="error"/>
                                        </Record>
                                    </Collection>
                                </Annotation>
                            </PropertyValue>
                        </Record>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="ID" String="F3_CoreDefault"/>
                            <PropertyValue Property="Target" String="UI.FACET$4$">
                                <Annotation Term="SAP__core.Messages">
                                    <Collection>
                                        <Record>
                                            <PropertyValue Property="code" String="SY-530"/>
                                            <PropertyValue Property="message" String="An exception was raised"/>
                                            <PropertyValue Property="severity" String="error"/>
                                        </Record>
                                    </Collection>
                                </Annotation>
                            </PropertyValue>
                        </Record>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="ID" String="F4_CoreDefault"/>
                            <PropertyValue Property="Target" String="UI.FACET$5$">
                                <Annotation Term="SAP__core.Messages">
                                    <Collection>
                                        <Record>
                                            <PropertyValue Property="code" String="SY-530"/>
                                            <PropertyValue Property="message" String="An exception was raised"/>
                                            <PropertyValue Property="severity" String="error"/>
                                        </Record>
                                    </Collection>
                                </Annotation>
                            </PropertyValue>
                        </Record>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="ID" String="F10_WRDefault"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#WR_Default"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="DefaultInformation">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ServiceId"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="AdditionalText"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ServiceType"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="WR_Default">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataFieldForAnnotation">
                                    <PropertyValue Property="Target" AnnotationPath="@SAP__UI.ConnectedFields#cf_containertypeat"/>
                                </Record>
                                <Record Type="SAP__UI.DataFieldForAnnotation">
                                    <PropertyValue Property="Target" AnnotationPath="@SAP__UI.ConnectedFields#cf_containertypenew"/>
                                </Record>
                                <Record Type="SAP__UI.DataFieldForAnnotation">
                                    <PropertyValue Property="Target" AnnotationPath="@SAP__UI.ConnectedFields#cf_material"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="MaterialText"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="lfg_containertypeat">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ContainerTypeAtLocation"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ContainerAtLocationCount"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="lfg_containertypenew">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ContainerTypeNew"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ContainerNewCount"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="lfg_material">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Material"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="MaterialWeight"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.HeaderInfo">
                    <Record>
                        <PropertyValue Property="TypeName" String="Service"/>
                        <PropertyValue Property="TypeNamePlural" String="Services"/>
                        <PropertyValue Property="Title">
                            <Record Type="SAP__UI.DataField">
                                <PropertyValue Property="Value" Path="ServiceId"/>
                            </Record>
                        </PropertyValue>
                        <PropertyValue Property="Description">
                            <Record Type="SAP__UI.DataField">
                                <PropertyValue Property="Value" Path="ServiceTypeDescription"/>
                            </Record>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.LineItem">
                    <Annotation Term="SAP__UI.Criticality" Path="service_criticality"/>
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ServiceUUID"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ReferenceId"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="WorkStatusIcon"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                            <Annotation Term="SAP__HTML5.CssDefaults">
                                <Record>
                                    <PropertyValue Property="width" String="5rem"/>
                                </Record>
                            </Annotation>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ServiceStatusIcon"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                            <Annotation Term="SAP__HTML5.CssDefaults">
                                <Record>
                                    <PropertyValue Property="width" String="5rem"/>
                                </Record>
                            </Annotation>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="CustomerInfo"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="FullAddress"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="point_of_origin"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ServiceType"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAnnotation">
                            <PropertyValue Property="Label" String="ContType At Loc #"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#lfg_containertypeat"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAnnotation">
                            <PropertyValue Property="Label" String="Container Type New #"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#lfg_containertypenew"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAnnotation">
                            <PropertyValue Property="Label" String="Material #"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#lfg_material"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAction">
                            <PropertyValue Property="Label" String="Change ServiceType"/>
                            <PropertyValue Property="IconUrl" String="sap-icon://wrench"/>
                            <PropertyValue Property="Action" String="com.sap.gateway.srvd.zsd_pdattacments.v0001.changeServiceType(com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType)"/>
                            <PropertyValue Property="InvocationGrouping" EnumMember="SAP__UI.OperationGroupingType/ChangeSet"/>
                            <PropertyValue Property="Inline" Bool="true"/>
                            <Annotation Term="SAP__UI.Emphasized"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MaterialGroup"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="RequestedDate"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TotalDuration"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Criticality" Path="PlanningStatusCriticality"/>
                            <PropertyValue Property="Value" Path="PlanningStatus"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourId"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ServiceFrequencyText"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAction">
                            <PropertyValue Property="Label" String="Create Tour"/>
                            <PropertyValue Property="Action" String="com.sap.gateway.srvd.zsd_pdattacments.v0001.createTour(com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType)"/>
                            <PropertyValue Property="InvocationGrouping" EnumMember="SAP__UI.OperationGroupingType/ChangeSet"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAction">
                            <PropertyValue Property="Label" String="Assign Tour"/>
                            <PropertyValue Property="Action" String="com.sap.gateway.srvd.zsd_pdattacments.v0001.assignTour(com.sap.gateway.srvd.zsd_pdattacments.v0001.ServiceWRType)"/>
                            <PropertyValue Property="InvocationGrouping" EnumMember="SAP__UI.OperationGroupingType/ChangeSet"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ServiceUUID"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MaterialWithText"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MaterialWeight"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ContainerTypeAtLocationWiText"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ContainerAtLocationCount"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ContainerTypeNewWithText"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ContainerNewCount"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MapTitle"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MapPopup"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MapColor"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MapHighlight"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MapSymbol"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.PresentationVariant">
                    <Record>
                        <PropertyValue Property="SortOrder">
                            <Collection>
                                <Record Type="SAP__common.SortOrderType">
                                    <PropertyValue Property="Property" PropertyPath="ServiceId"/>
                                    <PropertyValue Property="Descending" Bool="true"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="Visualizations">
                            <Collection>
                                <AnnotationPath>@SAP__UI.LineItem</AnnotationPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.SelectionFields">
                    <Collection>
                        <PropertyPath>RequestedDate</PropertyPath>
                        <PropertyPath>PlanningStatus</PropertyPath>
                        <PropertyPath>MaterialGroup</PropertyPath>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.Messages" Path="SAP__Messages"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/ServiceWR">
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
                                    <PropertyValue Property="Property" PropertyPath="PlanningStatus"/>
                                    <PropertyValue Property="AllowedExpressions" String="MultiValue"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="RequestedDate"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="EarliestDate"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="LatestDate"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleRange"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="ServiceWindowStartTime"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleValue"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="ServiceWindowEndTime"/>
                                    <PropertyValue Property="AllowedExpressions" String="SingleValue"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="MapTitle"/>
                                    <PropertyValue Property="AllowedExpressions" String="SearchExpression"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="MapPopup"/>
                                    <PropertyValue Property="AllowedExpressions" String="SearchExpression"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="MapColor"/>
                                    <PropertyValue Property="AllowedExpressions" String="SearchExpression"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="MapHighlight"/>
                                    <PropertyValue Property="AllowedExpressions" String="SearchExpression"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="MapSymbol"/>
                                    <PropertyValue Property="AllowedExpressions" String="SearchExpression"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="RequiredProperties">
                            <Collection>
                                <PropertyPath>RequestedDate</PropertyPath>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="NonFilterableProperties">
                            <Collection>
                                <PropertyPath>MapTitle</PropertyPath>
                                <PropertyPath>MapPopup</PropertyPath>
                                <PropertyPath>MapColor</PropertyPath>
                                <PropertyPath>MapHighlight</PropertyPath>
                                <PropertyPath>MapSymbol</PropertyPath>
                                <PropertyPath>service_criticality</PropertyPath>
                                <PropertyPath>point_of_origin_ver</PropertyPath>
                                <PropertyPath>__OperationControl</PropertyPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.SortRestrictions">
                    <Record>
                        <PropertyValue Property="NonSortableProperties">
                            <Collection>
                                <PropertyPath>MapTitle</PropertyPath>
                                <PropertyPath>MapPopup</PropertyPath>
                                <PropertyPath>MapColor</PropertyPath>
                                <PropertyPath>MapHighlight</PropertyPath>
                                <PropertyPath>MapSymbol</PropertyPath>
                                <PropertyPath>service_criticality</PropertyPath>
                                <PropertyPath>point_of_origin_ver</PropertyPath>
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
            <Annotations Target="SAP__self.ServiceAssignmentType/TourUuid">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Tour UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceUuid">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Service UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/TourId">
                <Annotation Term="SAP__common.Label" String="Tour ID"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Tour ID"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Tour Id"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceId">
                <Annotation Term="SAP__common.Label" String="Service Id"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Service ID"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Service Id"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceType">
                <Annotation Term="SAP__common.Label" String="Service Type"/>
                <Annotation Term="SAP__common.Text" Path="ServiceTypeText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Service Type"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Type"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceTypeText">
                <Annotation Term="SAP__common.Label" String="Service Type Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Text"/>
                <Annotation Term="SAP__common.QuickInfo" String="Text with 40 characters"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceStatus">
                <Annotation Term="SAP__common.Label" String="Service Status Description"/>
                <Annotation Term="SAP__common.Text" Path="ServiceStatusText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Service Status"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Status"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceStatusText">
                <Annotation Term="SAP__common.Label" String="Service Status Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceStatusIcon">
                <Annotation Term="SAP__common.Label" String="Service Status"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.IsImageURL"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/Removed">
                <Annotation Term="SAP__common.Label" String="Removed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Truth Value"/>
                <Annotation Term="SAP__common.QuickInfo" String="Truth Value: True/False"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/TourSequence">
                <Annotation Term="SAP__common.IsDigitSequence"/>
                <Annotation Term="SAP__common.Label" String="Sequence Number"/>
                <Annotation Term="SAP__common.QuickInfo" String="Sequence number"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ReferenceId">
                <Annotation Term="SAP__common.Label" String="Order"/>
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/CustomerInfo">
                <Annotation Term="SAP__common.Label" String="Customer"/>
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/AdditionalText">
                <Annotation Term="SAP__common.Label" String="Additional Information"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Additional Text"/>
                <Annotation Term="SAP__common.QuickInfo" String="Additional Text"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/FunctionalLocation">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Functional Location"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/FullAddress">
                <Annotation Term="SAP__common.Label" String="Address"/>
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/Material">
                <Annotation Term="SAP__common.Label" String="Material"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product"/>
                <Annotation Term="SAP__common.QuickInfo" String="Transport Product"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/MaterialWithText">
                <Annotation Term="SAP__common.Label" String="Material"/>
                <Annotation Term="SAP__common.Text" Path="MaterialText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextLast"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product"/>
                <Annotation Term="SAP__common.QuickInfo" String="Transport Product"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/MaterialText">
                <Annotation Term="SAP__common.Label" String="Material Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Product Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/MaterialWeight">
                <Annotation Term="SAP__common.Label" String="Material Weight"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__measures.Unit" Path="MaterialWeightUnit"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product Quantity"/>
                <Annotation Term="SAP__common.QuickInfo" String="Transport Product Quantity"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/MaterialWeightUnit">
                <Annotation Term="SAP__common.IsUnit"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Base Unit of Measure"/>
                <Annotation Term="SAP__common.Heading" String="BUn"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/MaterialGroup">
                <Annotation Term="SAP__common.Label" String="Material Group"/>
                <Annotation Term="SAP__common.Text" Path="MaterialGroupText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextLast"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Product Group"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Group"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/MaterialGroupText">
                <Annotation Term="SAP__common.Label" String="Material Group Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Product Group Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Group Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/PlantLocation">
                <Annotation Term="SAP__common.Label" String="Material Plant"/>
                <Annotation Term="SAP__common.Text" Path="PlantLocationText"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Functional Location"/>
                <Annotation Term="SAP__common.QuickInfo" String="Functional Location"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/PlantLocationText">
                <Annotation Term="SAP__common.Label" String="Functional Location Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Text"/>
                <Annotation Term="SAP__common.QuickInfo" String="Text with 40 characters"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerSourceLocation">
                <Annotation Term="SAP__common.Label" String="Container Source Location"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Functional Location"/>
                <Annotation Term="SAP__common.QuickInfo" String="Functional Location"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerSourceAddress">
                <Annotation Term="SAP__common.Label" String="Address - Container Source"/>
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerFinalLocation">
                <Annotation Term="SAP__common.Label" String="Container Final Destination"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Functional Location"/>
                <Annotation Term="SAP__common.QuickInfo" String="Functional Location"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerFinalAddress">
                <Annotation Term="SAP__common.Label" String="Address - Container Final"/>
                <Annotation Term="SAP__core.Computed"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerTypeAtLocation">
                <Annotation Term="SAP__common.Label" String="Container Type (at Location)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product Packaging"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Packaging"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerTypeAtLocationWiText">
                <Annotation Term="SAP__common.Label" String="Container Type (at Location)"/>
                <Annotation Term="SAP__common.Text" Path="ContainerTypeAtLocationText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextLast"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product Packaging"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Packaging"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerTypeAtLocationText">
                <Annotation Term="SAP__common.Label" String="Packaging Type Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Product Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerAtLocationCount">
                <Annotation Term="SAP__common.Label" String="Container Count (at Location)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Quantity"/>
                <Annotation Term="SAP__common.QuickInfo" String="DEC 3"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerAtLocationTidnr">
                <Annotation Term="SAP__common.Label" String="Container ID (at Location)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Technical identification no."/>
                <Annotation Term="SAP__common.QuickInfo" String="Tech. Identification"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerTypeNew">
                <Annotation Term="SAP__common.Label" String="Container Type (New)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product Packaging"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Packaging"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerTypeNewWithText">
                <Annotation Term="SAP__common.Label" String="Container Type (New)"/>
                <Annotation Term="SAP__common.Text" Path="ContainerTypeNewText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextLast"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.HiddenFilter"/>
                <Annotation Term="SAP__common.Heading" String="Transport Product Packaging"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Packaging"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerTypeNewText">
                <Annotation Term="SAP__common.Label" String="Packaging Type Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Product Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Product Description"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerNewCount">
                <Annotation Term="SAP__common.Label" String="Container Count (New)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Quantity"/>
                <Annotation Term="SAP__common.QuickInfo" String="DEC 3"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ContainerNewTidnr">
                <Annotation Term="SAP__common.Label" String="Container ID (New)"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Technical identification no."/>
                <Annotation Term="SAP__common.QuickInfo" String="Tech. Identification"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceFrequency">
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Service Freq"/>
                <Annotation Term="SAP__common.Heading" String="Service Frequency"/>
                <Annotation Term="SAP__common.QuickInfo" String="Service Frequency"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/ServiceFrequencyText">
                <Annotation Term="SAP__common.Label" String="Service Frequency"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/TotalDuration">
                <Annotation Term="SAP__common.Label" String="Total Duration"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__measures.Unit" Path="TotalDurationUnit"/>
                <Annotation Term="SAP__common.Heading" String="Duration"/>
                <Annotation Term="SAP__common.QuickInfo" String="Duration"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/TotalDurationUnit">
                <Annotation Term="SAP__common.IsUnit"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Base Unit of Measure"/>
                <Annotation Term="SAP__common.Heading" String="BUn"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/WorkStatus">
                <Annotation Term="SAP__common.Label" String="Work Status Description"/>
                <Annotation Term="SAP__common.Text" Path="WorkStatusText">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsDigitSequence"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/WorkStatusText">
                <Annotation Term="SAP__common.Label" String="Work Status Description"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/WorkStatusIcon">
                <Annotation Term="SAP__common.Label" String="Work Status"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__UI.IsImageURL"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType/PlanningStatus">
                <Annotation Term="SAP__common.Label" String="Planning Status"/>
                <Annotation Term="SAP__core.Computed"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Planning Status"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning Status"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/ServiceAssignment">
                <Annotation Term="SAP__capabilities.NavigationRestrictions">
                    <Record>
                        <PropertyValue Property="RestrictedProperties">
                            <Collection>
                                <Record>
                                    <PropertyValue Property="NavigationProperty" NavigationPropertyPath="_Service"/>
                                    <PropertyValue Property="InsertRestrictions">
                                        <Record>
                                            <PropertyValue Property="Insertable" Bool="false"/>
                                        </Record>
                                    </PropertyValue>
                                </Record>
                                <Record>
                                    <PropertyValue Property="NavigationProperty" NavigationPropertyPath="_Tour"/>
                                    <PropertyValue Property="InsertRestrictions">
                                        <Record>
                                            <PropertyValue Property="Insertable" Bool="false"/>
                                        </Record>
                                    </PropertyValue>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__capabilities.SearchRestrictions">
                    <Record>
                        <PropertyValue Property="Searchable" Bool="true"/>
                        <PropertyValue Property="UnsupportedExpressions" EnumMember="SAP__capabilities.SearchExpressions/AND SAP__capabilities.SearchExpressions/OR SAP__capabilities.SearchExpressions/NOT SAP__capabilities.SearchExpressions/group SAP__capabilities.SearchExpressions/phrase"/>
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
                <Annotation Term="SAP__capabilities.FilterRestrictions">
                    <Record>
                        <PropertyValue Property="FilterExpressionRestrictions">
                            <Collection>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="MaterialWeight"/>
                                    <PropertyValue Property="AllowedExpressions" String="MultiValue"/>
                                </Record>
                                <Record>
                                    <PropertyValue Property="Property" PropertyPath="TotalDuration"/>
                                    <PropertyValue Property="AllowedExpressions" String="MultiValue"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.ServiceAssignmentType">
                <Annotation Term="SAP__common.Label" String="Tour Service Assignments"/>
                <Annotation Term="SAP__UI.Facets">
                    <Collection>
                        <Record Type="SAP__UI.ReferenceFacet">
                            <PropertyValue Property="ID" String="F0_CoreDefault"/>
                            <PropertyValue Property="Target" String="UI.FACET$1$">
                                <Annotation Term="SAP__core.Messages">
                                    <Collection>
                                        <Record>
                                            <PropertyValue Property="code" String="SY-530"/>
                                            <PropertyValue Property="message" String="An exception was raised"/>
                                            <PropertyValue Property="severity" String="error"/>
                                        </Record>
                                    </Collection>
                                </Annotation>
                            </PropertyValue>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="lfg_containertypeat">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ContainerTypeAtLocation"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ContainerAtLocationCount"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="lfg_containertypenew">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ContainerTypeNew"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="ContainerNewCount"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.FieldGroup" Qualifier="lfg_material">
                    <Record>
                        <PropertyValue Property="Data">
                            <Collection>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="Material"/>
                                </Record>
                                <Record Type="SAP__UI.DataField">
                                    <PropertyValue Property="Value" Path="MaterialWeight"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.HeaderInfo">
                    <Record>
                        <PropertyValue Property="TypeName" String="Service Assignment"/>
                        <PropertyValue Property="TypeNamePlural" String="Service Assignments"/>
                    </Record>
                </Annotation>
                <Annotation Term="SAP__UI.LineItem">
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ServiceUuid"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourSequence"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ReferenceId"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="WorkStatusIcon"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                            <Annotation Term="SAP__HTML5.CssDefaults">
                                <Record>
                                    <PropertyValue Property="width" String="5rem"/>
                                </Record>
                            </Annotation>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ServiceStatusIcon"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                            <Annotation Term="SAP__HTML5.CssDefaults">
                                <Record>
                                    <PropertyValue Property="width" String="5rem"/>
                                </Record>
                            </Annotation>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="CustomerInfo"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="FullAddress"/>
                            <Annotation Term="SAP__UI.Importance" EnumMember="SAP__UI.ImportanceType/High"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ServiceType"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAnnotation">
                            <PropertyValue Property="Label" String="ContType At Loc #"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#lfg_containertypeat"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAnnotation">
                            <PropertyValue Property="Label" String="Container Type New #"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#lfg_containertypenew"/>
                        </Record>
                        <Record Type="SAP__UI.DataFieldForAnnotation">
                            <PropertyValue Property="Label" String="Material #"/>
                            <PropertyValue Property="Target" AnnotationPath="@SAP__UI.FieldGroup#lfg_material"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MaterialGroup"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TotalDuration"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MaterialWithText"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="MaterialWeight"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ContainerTypeAtLocationWiText"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ContainerAtLocationCount"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ContainerTypeNewWithText"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="ContainerNewCount"/>
                            <Annotation Term="SAP__UI.Hidden"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.PresentationVariant">
                    <Record>
                        <PropertyValue Property="SortOrder">
                            <Collection>
                                <Record Type="SAP__common.SortOrderType">
                                    <PropertyValue Property="Property" PropertyPath="TourSequence"/>
                                    <PropertyValue Property="Descending" Bool="false"/>
                                </Record>
                            </Collection>
                        </PropertyValue>
                        <PropertyValue Property="Visualizations">
                            <Collection>
                                <AnnotationPath>@SAP__UI.LineItem</AnnotationPath>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/WorkArea">
                <Annotation Term="SAP__common.ValueListForValidation" String=""/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdworkareauser_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdtourwa_vh.workarea'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Work Area"/>
                <Annotation Term="SAP__common.QuickInfo" String="P&amp;D Work Area"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/TourId">
                <Annotation Term="SAP__common.Label" String="Tour ID"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Tour ID"/>
                <Annotation Term="SAP__common.QuickInfo" String="Planning and Dispatching Tour Id"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/TourTemplate">
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Tour Template"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/TourStatus">
                <Annotation Term="SAP__common.Text" Path="TourStatusText"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Tour Status"/>
                <Annotation Term="SAP__common.QuickInfo" String="Tour status"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/TourStatusText">
                <Annotation Term="SAP__common.Label" String="Tour Status"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/StartDate">
                <Annotation Term="SAP__common.Label" String="Tour Start Date"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/EndDate">
                <Annotation Term="SAP__common.Label" String="Tour End Date"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/WorkStatus">
                <Annotation Term="SAP__common.IsDigitSequence"/>
                <Annotation Term="SAP__UI.Hidden"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType">
                <Annotation Term="SAP__common.Label" String="Tour Value Help"/>
                <Annotation Term="SAP__UI.LineItem">
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourId"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourTemplate"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="TourStatusText"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="StartDate"/>
                        </Record>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="EndDate"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__UI.SelectionFields">
                    <Collection>
                        <PropertyPath>TourId</PropertyPath>
                        <PropertyPath>TourStatusText</PropertyPath>
                        <PropertyPath>TourTemplate</PropertyPath>
                        <PropertyPath>StartDate</PropertyPath>
                    </Collection>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.Container/TourIdVH">
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
                                    <PropertyValue Property="Property" PropertyPath="StartDate"/>
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
            <Annotations Target="SAP__self.TourTemplateVHType/WorkArea">
                <Annotation Term="SAP__common.ValueListForValidation" String=""/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.ValueListReferences">
                    <Collection>
                        <String>../../../../srvd_f4/plce/c_pdworkareauser_vh/0001;ps='srvd-zsd_pdattacments-0001';va='com.sap.gateway.srvd.zsd_pdattacments.v0001.et-*plce*c_pdtourtemplatewa_vh.workarea'/$metadata</String>
                    </Collection>
                </Annotation>
                <Annotation Term="SAP__common.ValueListWithFixedValues"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Work Area"/>
                <Annotation Term="SAP__common.QuickInfo" String="P&amp;D Work Area"/>
            </Annotations>
            <Annotations Target="SAP__self.TourTemplateVHType/TourTemplate">
                <Annotation Term="SAP__common.Label" String="Template"/>
                <Annotation Term="SAP__common.Text" Path="TourTemplateName"/>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Heading" String="Tour Template"/>
                <Annotation Term="SAP__common.QuickInfo" String="Tour Template"/>
            </Annotations>
            <Annotations Target="SAP__self.TourTemplateVHType/TourTemplateName">
                <Annotation Term="SAP__common.Label" String="Description"/>
                <Annotation Term="SAP__common.Heading" String="Text"/>
                <Annotation Term="SAP__common.QuickInfo" String="Text with 40 characters"/>
            </Annotations>
            <Annotations Target="SAP__self.TourTemplateVHType">
                <Annotation Term="SAP__common.Label" String="TourTemplate Value Help"/>
            </Annotations>
            <Annotations Target="SAP__self.Container/TourTemplateVH">
                <Annotation Term="SAP__capabilities.SearchRestrictions">
                    <Record>
                        <PropertyValue Property="Searchable" Bool="true"/>
                        <PropertyValue Property="UnsupportedExpressions" EnumMember="SAP__capabilities.SearchExpressions/AND SAP__capabilities.SearchExpressions/OR SAP__capabilities.SearchExpressions/NOT SAP__capabilities.SearchExpressions/group SAP__capabilities.SearchExpressions/phrase"/>
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
            <Annotations Target="SAP__self.WorkStatusVHType/Value">
                <Annotation Term="SAP__common.Text" Path="Description">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__common.IsUpperCase"/>
                <Annotation Term="SAP__common.Label" String="Lower Value"/>
                <Annotation Term="SAP__common.Heading" String="Fixed"/>
                <Annotation Term="SAP__common.QuickInfo" String="Values for Domains: Single Value/Lower Limit"/>
            </Annotations>
            <Annotations Target="SAP__self.WorkStatusVHType/Description">
                <Annotation Term="SAP__common.Label" String="Work Status Description"/>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Heading" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.WorkStatusVHType/IconURL">
                <Annotation Term="SAP__common.Label" String="Work Status"/>
                <Annotation Term="SAP__common.Text" Path="Description">
                    <Annotation Term="SAP__UI.TextArrangement" EnumMember="SAP__UI.TextArrangementType/TextOnly"/>
                </Annotation>
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__UI.IsImageURL"/>
            </Annotations>
            <Annotations Target="SAP__self.WorkStatusVHType">
                <Annotation Term="SAP__common.Label" String="Work Status Value Help"/>
                <Annotation Term="SAP__UI.LineItem">
                    <Collection>
                        <Record Type="SAP__UI.DataField">
                            <PropertyValue Property="Value" Path="Value"/>
                        </Record>
                    </Collection>
                </Annotation>
                <Annotation Term="Communication.Contact">
                    <Record/>
                </Annotation>
            </Annotations>
            <Annotations Target="SAP__self.Container/WorkStatusVH">
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
            <Annotations Target="SAP__self.createTour(SAP__self.ServiceWRType)/created_for_simulation">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Tour Created For Sim"/>
                <Annotation Term="SAP__common.Heading" String="Tour Created For Simulation"/>
                <Annotation Term="SAP__common.QuickInfo" String="PD Tour Created  for Simulation"/>
            </Annotations>
            <Annotations Target="SAP__self.createTour(SAP__self.ServiceWRType)/without_draft">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Truth Value"/>
                <Annotation Term="SAP__common.QuickInfo" String="Truth Value: True/False"/>
            </Annotations>
            <Annotations Target="SAP__self.createTour(SAP__self.ServiceWRType)/Sequence">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Sequence Number"/>
                <Annotation Term="SAP__common.QuickInfo" String="Sequence number"/>
            </Annotations>
            <Annotations Target="SAP__self.assignTour(SAP__self.ServiceWRType)/TourUUID">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Tour UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.TourStatusVHType/Language">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Lang."/>
                <Annotation Term="SAP__common.Heading" String="Ln"/>
                <Annotation Term="SAP__common.QuickInfo" String="Language Key"/>
            </Annotations>
            <Annotations Target="SAP__self.TourStatusVHType/StatusDescription">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Short Description"/>
                <Annotation Term="SAP__common.QuickInfo" String="Short Text for Fixed Values"/>
            </Annotations>
            <Annotations Target="SAP__self.TourIdVHType/TourUUID">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Tour UUID"/>
            </Annotations>
            <Annotations Target="SAP__self.WorkStatusVHType/Language">
                <Annotation Term="SAP__UI.Hidden"/>
                <Annotation Term="SAP__common.Label" String="Lang."/>
                <Annotation Term="SAP__common.Heading" String="Ln"/>
                <Annotation Term="SAP__common.QuickInfo" String="Language Key"/>
            </Annotations>
            <Annotations Target="SAP__self.PrintConfigurationType/Filenameteemplate">
                <Annotation Term="SAP__common.Label" String="Filename Template"/>
            </Annotations>
            <Annotations Target="SAP__self.ResourceVHType/Description">
                <Annotation Term="SAP__common.Label" String="Description"/>
                <Annotation Term="SAP__common.Heading" String="Description of the Technical User Account"/>
                <Annotation Term="SAP__common.QuickInfo" String="Description of the Technical User Account"/>
            </Annotations>
            <Annotations Target="SAP__self.TourType/__OperationControl/RegenerateDocs">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/__OperationControl/assignTour">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/__OperationControl/changeServiceType">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWRType/__OperationControl/createTour">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.TourOperationControl/RegenerateDocs">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWROperationControl/assignTour">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWROperationControl/changeServiceType">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
            <Annotations Target="SAP__self.ServiceWROperationControl/createTour">
                <Annotation Term="SAP__common.Label" String="Dyn. Action Control"/>
                <Annotation Term="SAP__common.Heading" String="Dynamic Action Control"/>
                <Annotation Term="SAP__common.QuickInfo" String="Dynamic Action Property"/>
            </Annotations>
        </Schema>
    </edmx:DataServices>
</edmx:Edmx>



Log-dbg.js:499 2025-12-03 05:24:44.765800 Error: Error found in Fragment (id: '__fragment2').
XML node: '

                 ========= SERVICE WR “LIST REPORT” ========= -->
                ':
Cannot add text nodes as direct child of an aggregation. For adding text to an aggregation, a surrounding html tag is needed. -  
g @ Log-dbg.js:499Understand this error
XMLTemplateProcessor-dbg.js:1585 Uncaught (in promise) Error: Error found in Fragment (id: '__fragment2').
XML node: '

                 ========= SERVICE WR “LIST REPORT” ========= -->
                ':
Cannot add text nodes as direct child of an aggregation. For adding text to an aggregation, a surrounding html tag is needed.
    at Y (XMLTemplateProcessor-dbg.js:1585:14)Understand this error
Log-dbg.js:499 2025-12-03 05:24:44.992300 Defining the object type ('sap.ui.table.RowSettings') via its string name is deprecated, since it leads to accesses to the global namespace. The object type either stems from an explicitly given 'Type' value or was inferred from the default aggregation type. Please require the respective object type module beforehand. For control development, please also refer to the runtime metadata property 'defaultClass', which allows you to specify a default aggregation class type via constructor reference. -  




