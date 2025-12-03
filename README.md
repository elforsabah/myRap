{
  "_version": "1.60.0",
  "sap.app": {
    "id": "zpdattachment",
    "type": "application",
    "i18n": "i18n/i18n.properties",
    "applicationVersion": {
      "version": "0.0.1"
    },
    "title": "{{appTitle}}",
    "description": "{{appDescription}}",
    "resources": "resources.json",
    "sourceTemplate": {
      "id": "@sap/generator-fiori:lrop",
      "version": "1.19.6",
      "toolsId": "789bd532-597f-4b72-9e2d-7d05f9d76347"
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
        "uri": "/sap/opu/odata4/sap/zsb_pdattacments/srvd/sap/zsd_pdattacments/0001/",
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
      "minUI5Version": "1.120.23",
      "libs": {
        "sap.m": {},
        "sap.ui.core": {},
        "sap.fe.templates": {}
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
          "bundleName": "zpdattachment.i18n.i18n"
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
      "css": []
    },
    "routing": {
      "config": {},
      "routes": [
        {
          "pattern": ":?query:",
          "name": "TourList",
          "target": "TourList"
        },
        {
          "pattern": "Tour({key}):?query:",
          "name": "TourObjectPage",
          "target": "TourObjectPage"
        }
      ],
      "targets": {
        "TourList": {
          "type": "Component",
          "id": "TourList",
          "name": "sap.fe.templates.ListReport",
          "options": {
            "settings": {
              "contextPath": "/Tour",
              "variantManagement": "Page",
              "navigation": {
                "Tour": {
                  "detail": {
                    "route": "TourObjectPage"
                  }
                }
              },
              "controlConfiguration": {
                "@com.sap.vocabularies.UI.v1.LineItem": {
                  "tableSettings": {
                    "type": "ResponsiveTable"
                  },
                  "actions": {
                    "ListReportExt": {
                      "press": "zpdattachment.ext.controller.ListReportExt.manualattachments",
                      "visible": true,
                      "enabled": true,
                      "requiresSelection": true,
                      "text": "Generate Documents"
                    }
                  }
                }
              }
            }
          }
        },
        "TourObjectPage": {
          "type": "Component",
          "id": "TourObjectPage",
          "name": "sap.fe.templates.ObjectPage",
          "options": {
            "settings": {
              "editableHeaderContent": false,
              "contextPath": "/Tour"
            }
          }
        }
      }
    }
  },
  "sap.fiori": {
    "registrationIds": [],
    "archeType": "transactional"
  }
}




@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Consumption View (Tour WR)'
@Search.searchable: true
@Metadata.allowExtensions: true
define root view entity ZC_PDTOUR
  provider contract transactional_query
  as projection on ZR_PDTOUR

{
  key TourUuid,
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourWA_VH', element: 'TourId' } }]
      @Search: { defaultSearchElement: true, ranking: #HIGH }
      TourId,

      @EndUserText.label: 'Tour Template'
      @ObjectModel.text.element: ['TourTemplateName']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourTemplateWA_VH', element: 'TourTemplate' } }]
      @Search: { defaultSearchElement: true }
      TourTemplate,
      @EndUserText.label: 'Template Description'
      @Consumption.filter.hidden: true
      @Semantics.text: true
      _TourTemplate._Text.TourTemplateName         as TourTemplateName : localized,
      @UI.hidden: true
      _TourTemplate.ColorTour                      as ColorTour,

      @EndUserText.label: 'Tour Status'
      @ObjectModel.text.element: ['TourStatusText']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourStatus_VH', element: 'TourStatus' } }]
      @Consumption.filter: { multipleSelections: true, selectionType: #SINGLE }
      TourStatus,
      @Semantics.text: true
      @UI.hidden: true
      _PDTourStatusText.Description                as TourStatusText   : localized,
      @UI.hidden: true
      _TourLookup.TourStatusColorValue             as TourStatusColorValue,
      @Consumption.filter.hidden: true
      _PDTourStatusText.IconURL                    as TourStatusIcon   : localized,

      @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
      StartDate                                    as TourStartDate,
      @Consumption.filter.selectionType: #INTERVAL
      EndDate                                      as TourEndDate,
      @Consumption.filter.selectionType: #INTERVAL
      ScheduledDateTimeStart,

      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDResource_VH', element: 'ResourceId'} }]
      MainResourceId,

      @EndUserText.label: 'Work Status'
      @UI.textArrangement: #TEXT_ONLY
      @ObjectModel.text.element: ['WorkStatusText']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDWorkStatus_VH', element: 'Value'} }]
      _TourLookup.WorkStatus,
      @UI.hidden: true
      _TourLookup._WorkStatusText.Description      as WorkStatusText   : localized,
      @Consumption.filter.hidden: true
      _TourLookup._WorkStatusText.IconURL          as WorkStatusIcon   : localized,
      _TourLookup._TourCriticality.TourCriticality as TourCriticality,

      /* Associations */
      _Attachments        : redirected to composition child ZC_PDATTACHMENT,
      _ServiceAssignments : redirected to composition child ZC_PDTOURSERVICEASGMT
}
