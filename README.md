{
  "_version": "1.60.0",
  "sap.app": {
    "id": "com.prologa.zwrweighbrigefinal",
    "type": "application",
    "i18n": "i18n/i18n.properties",
    "applicationVersion": {
      "version": "0.0.1"
    },
    "title": "{{appTitle}}",
    "description": "{{appDescription}}",
    "resources": "resources.json",
    "sourceTemplate": {
      "id": "@sap/generator-fiori:fpm",
      "version": "1.18.7",
      "toolsId": "124bf018-554c-49fc-a88a-6aa5dacbcc8e"
    },
    "dataSources": {
      "annotation": {
        "type": "ODataAnnotation",
        "uri": "annotations/annotation.xml",
        "settings": {
          "localUri": "annotations/annotation.xml"
        }
      },
      "mainService": {
        "uri": "/sap/opu/odata4/sap/zsb_weighbrige_v4/srvd/sap/zsb_wr_weighbrige/0001/",
        "type": "OData",
        "settings": {
          "annotations": [
            "annotation"
          ],
          "localUri": "localService/mainService/metadata.xml",
          "odataVersion": "4.0"
        }
      }
    }
  },
  "sap.ui": {
    "technology": "UI5",
    "icons": {
      "icon": "",
      "favIcon": "",
      "phone": "",
      "phone@2": "",
      "tablet": "",
      "tablet@2": ""
    },
    "deviceTypes": {
      "desktop": true,
      "tablet": true,
      "phone": true
    }
  },
  "sap.ui5": {
    "flexEnabled": true,
    "dependencies": {
      "minUI5Version": "1.127.6",
      "libs": {
        "sap.m": {},
        "sap.ui.core": {},
        "sap.fe.core": {}
      }
    },
    "contentDensities": {
      "compact": true,
      "cozy": true
    },
    "models": {
      "i18n": {
        "type": "sap.ui.model.resource.ResourceModel",
        "settings": {
          "bundleName": "com.prologa.zwrweighbrigefinal.i18n.i18n",
           "supportedLocales": [
            "en",
            "da"
          ],
          "fallbackLocale": "en"
        }
      },
      "": {
        "dataSource": "mainService",
        "preload": true,
        "settings": {
          "operationMode": "Server",
          "autoExpandSelect": true,
          "earlyRequests": true
        }
      },
      "@i18n": {
        "type": "sap.ui.model.resource.ResourceModel",
          "settings": {
          "bundleName": "com.prologa.zwrweighbrige.i18n.i18n",
          "supportedLocales": [
            "en",
            "da"
          ],
          "fallbackLocale": "en"
          }
      }
    },
    "resources": {
      "css": [
        { "uri": "css/style.css" }
        
      ]
    },
    "routing": {
      "config": {},
      "routes": [
        {
          "name": "ZI_WR_R_WEIGHBRIDGEMain",
          "pattern": ":?query:",
          "target": "ZI_WR_R_WEIGHBRIDGEMain"
        }
      ],
      "targets": {
        "ZI_WR_R_WEIGHBRIDGEMain": {
          "type": "Component",
          "id": "ZI_WR_R_WEIGHBRIDGEMain",
          "name": "sap.fe.core.fpm",
          "options": {
            "settings": {
              "navigation": {},
              "contextPath": "/ZI_WR_R_WEIGHBRIDGE",
              "viewName": "com.prologa.zwrweighbrigefinal.ext.main.Main"
            }
          }
        }
      }
    }
  },
  "sap.fiori": {
    "registrationIds": []
  }
}

# This is the resource bundle for com.prologa.zwrweighbrige i18n_da.properties

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




# This is the resource bundle for com.prologa.zwrweighbrige   i18n.properties

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


