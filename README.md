sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast"
], function (PageController, MessageToast) {
    "use strict";
    return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
        onInit: function () {
            PageController.prototype.onInit.apply(this);
            // Initialize wizard
            this.oWizard = this.byId("weighingWizard");

            this._createInitialContext(); 
        },
        _createInitialContext: function () {
            var that = this;
            this.extensionAPI.getEditFlow().createDocument({
                creationMode: "NewPage"  // Or "Inline" depending on your flow
            }).then(function (oNewContext) {
                that.getView().setBindingContext(oNewContext);
                // Pre-request the property to initialize it (avoids "must not change before read")
                oNewContext.requestProperty("Vbeln").catch(function () {
                    // Ignore if not needed; ensures read before any set
                });
            }).catch(function (oError) {
                MessageToast.show("Failed to create session context.");
            });
        },
        onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();
            if (oCurrentStep.getId() === "step1") { // Handle Step 1 validation 
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
                this.extensionAPI.getEditFlow().invokeAction("identifycard", {
                    contexts: [oContext],
                    parameterValues: [{ name: "CardId", value: sContractId }]  // Array of {name, value} objects
                }).then(function () {
                    // On success: Show message and advance
                    MessageToast.show("Contract validated successfully.");
                    this.oWizard.validateStep(oCurrentStep);  // Mark step as valid
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
            this.getView().getModel().setProperty("/GrossWeight", 1000);  // Replace with actual API call if integrated with hardware
        },

        onCaptureFinalWeight: function () {
            // Calculate net weight: Net = Gross - Tare
            var oModel = this.getView().getModel();
            var fGross = oModel.getProperty("/GrossWeight");
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



{
  "_version": "1.60.0",
  "sap.app": {
    "id": "com.prologa.zwrweighbrige",
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
      "version": "1.18.5",
      "toolsId": "b91a0c4c-d512-465f-b1a5-67614404afb7"
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
        "uri": "/sap/opu/odata4/sap/zsb_weighingbrige_v4/srvd/sap/zsb_wr_weighingbrige/0001/",
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
          "bundleName": "com.prologa.zwrweighbrige.i18n.i18n"
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
        "uri": "i18n/i18n.properties"
      }
    },
    "resources": {
      "css": [
        {
          "uri": "css/style.css"
        }
      ]
    },
    "settings": {
      "synchronizationMode": "None",
      "operationMode": "Server",
      "autoExpandSelect": true,
      "groupId": "$direct",
      "annotations": [
        "annotations"
      ]
    },
    "routing": {
      "config": {},
      "routes": [
        {
          "name": "ZI_WR_WEIGHINGSESSIONMain",
          "pattern": ":?query:",
          "target": "ZI_WR_WEIGHINGSESSIONMain"
        }
      ],
      "targets": {
        "ZI_WR_WEIGHINGSESSIONMain": {
          "type": "Component",
          "id": "ZI_WR_WEIGHINGSESSIONMain",
          "name": "sap.fe.core.fpm",
          "options": {
            "settings": {
              "navigation": {},
              "contextPath": "/ZI_WR_WEIGHINGSESSION",
              "viewName": "com.prologa.zwrweighbrige.ext.main.Main"
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

