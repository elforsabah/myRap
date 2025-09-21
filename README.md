<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros"
    xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrigefinal.ext.main.Main">
    <Page id="Main" class="myApp" >
      <customHeader>
    <Toolbar id="headerToolbar">
        <Image id="im1" src="/img/LogoMP.png" alt="Company Logo" width="100px" /> 
        <HBox id="headHBox1" width="100%" justifyContent="Center" class="bigWeightContainer">
        <Text id="txt1" text="Marius Pedersen"  class="headText1" />
        </HBox>
        <ToolbarSpacer id="toole1"/>
          <Button id="btn15" icon="https://upload.wikimedia.org/wikipedia/en/thumb/a/ae/Flag_of_the_United_Kingdom.svg/1280px-Flag_of_the_United_Kingdom.svg.png" 
                press=".onSetEnglish" 
                type="Transparent" 
                tooltip="English" /> <!-- Icon as flag; add text="EN" if desired -->
        <Button id="btn16" icon="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Flag_of_Denmark.svg/1280px-Flag_of_Denmark.svg.png" 
                press=".onSetDanish" 
                type="Transparent" 
                tooltip="Danish" /> <!-- Icon as flag; add text="DK" if desired -->
    </Toolbar>
</customHeader>
<content>
  <Wizard id="weighingWizard" complete="onWizardComplete" showNextButton="false">
   <!-- STEP 1: Identification -->
<WizardStep id="step1" title="{@i18n>step1Title}" validated="true" icon="sap-icon://business-card" >

</WizardStep>   
</Wizard>
</content>
    </Page>
</mvc:View>

# This is the resource bundle for com.prologa.zwrweighbrigefinal

#Texts for manifest.json

#XTIT: Application name
appTitle=Weighbridge Integration 

#YDES: Application description
appDescription=Weighbridge Integration
#XTIT: Custom view title
MainTitle=Main

#XFLD,36:
grossWeight=Gross Weight

#XFLD,33:
tareWeight=Tare Weight

#XFLD,30:
netWeight=Net Weight

#XFLD,30:
sessionId=Session ID

#XFLD,45:
deliveryNumber=Delivery Number

#XFLD,27:
loadType=Load Type

step1Title=Identification
step2Title=Choose Load Type
step3Title=Weighing
step1Label=Please Enter your Contract ID
step1Placeholder=Scan or enter Contract ID
selectedContract=Selected Contract: 
step3Instruction=Please make sure to place the vehicle correctly.
confirmAndPrint=CONFIRM and Print
confirm=CONFIRM
enterContractId=Please enter a Contract ID.
noContext=No session context available.
invalidContract=Invalid Contract. Please try again.
contractValid=Contract is Valid. Step 2 activated
failedLoadTypes=Failed to load load types. Check contract or service.
failedMaterials=Failed to load materials for contract.
missingData=Missing required data: Contract ID, Load Type, or Weight.
weightCaptured=Weight captured: 
noSuccess002=No specific success message (ZWR_WEIGHBRIGE_MESS/002) received from backend.
noSuccess009=No specific success message (ZWR_WEIGHBRIGE_MESS/009) received from backend.
pdfSent=PDF sent to printer.
noPdf=No PDF returned by printSlip.
failedPdf=Failed to process or print PDF.
failedWeight=Failed to determine weight.


# This is the resource bundle for com.prologa.zwrweighbrigefinal

#Texts for manifest.json

#XTIT: Application name
appTitle=Weighbridge Integration 

#YDES: Application description
appDescription=Weighbridge Integration
#XTIT: Custom view title
MainTitle=Main

#XFLD,36:
grossWeight=Gross Weight

#XFLD,33:
tareWeight=Tare Weight

#XFLD,30:
netWeight=Net Weight

#XFLD,30:
sessionId=Session ID

#XFLD,45:
deliveryNumber=Delivery Number

#XFLD,27:
loadType=Load Type

step1Title=Identifikation
step2Title=Vælg lasttype
step3Title=Vejning
step1Label=Indtast venligst din kontrakt-ID
step1Placeholder=Scan eller indtast kontrakt-ID
selectedContract=Valgt kontrakt: 
step3Instruction=Sørg venligst for at placere køretøjet korrekt.
confirmAndPrint=BEKRÆFT og Udskriv
confirm=BEKRÆFT
enterContractId=Indtast venligst en kontrakt-ID.
noContext=Ingen sessionskontekst tilgængelig.
invalidContract=Ugyldig kontrakt. Prøv igen.
contractValid=Kontrakten er gyldig. Trin 2 aktiveret
failedLoadTypes=Kunne ikke indlæse lasttyper. Tjek kontrakt eller service.
failedMaterials=Kunne ikke indlæse materialer til kontrakt.
missingData=Manglende påkrævede data: Kontrakt-ID, lasttype eller vægt.
weightCaptured=Vægt registreret: 
noSuccess002=Ingen specifik succesmeddelelse (ZWR_WEIGHBRIGE_MESS/002) modtaget fra backend.
noSuccess009=Ingen specifik succesmeddelelse (ZWR_WEIGHBRIGE_MESS/009) modtaget fra backend.
pdfSent=PDF sendt til printer.
noPdf=Ingen PDF returneret af printSlip.
failedPdf=Kunne ikke behandle eller udskrive PDF.
failedWeight=Kunne ikke bestemme vægt.


sap.ui.define(
    [
        "sap/fe/core/PageController",
        "sap/m/MessageToast",
        "sap/ui/core/Messaging",
        "sap/ui/core/message/Message",
        "sap/ui/core/library",
        "sap/m/Button",
        "sap/ui/model/json/JSONModel",
        "sap/ui/model/resource/ResourceModel",
        "jquery.sap.global"
    ],
    function (PageController, MessageToast, Messaging, Message, coreLibrary, Button, JSONModel, ResourceModel, jQuery) {
        'use strict';

        return PageController.extend('com.prologa.zwrweighbrigefinal.ext.main.Main', {
            formatter: {
                concatMaterialText: function (sMaterial, sMaterialText) {
                    return (sMaterial || '') + ' - ' + (sMaterialText || '');
                }
            },
            
             onInit: function () {
                PageController.prototype.onInit.apply(this);
                // Set up i18n model with initial language (English)
                var oResourceModel = new ResourceModel({
                    bundleName: "com.prologa.zwrweighbrige.i18n.i18n",
                    bundleLocale: "en"
                });
                this.getView().setModel(oResourceModel, "@i18n");

             },

            onSetEnglish: function () {
                var oResourceModel = new ResourceModel({
                    bundleName: "com.prologa.zwrweighbrige.i18n.i18n",
                    bundleLocale: "en"
                });
                this.getView().setModel(oResourceModel, "@i18n");
            },
            onSetDanish: function () {
                var oResourceModel = new ResourceModel({
                    bundleName: "com.prologa.zwrweighbrige.i18n.i18n",
                    bundleLocale: "da"
                });
                this.getView().setModel(oResourceModel, "@i18n");
            },

        });
    }
);


