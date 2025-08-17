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
"odataVersion": "4.0",
"localMetaModel": true
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
"type": "sap.ui.model.odata.v4.ODataModel",
"settings": {
"synchronizationMode": "None",
"operationMode": "Server",
"autoExpandSelect": true,
"groupId": "$direct",
"earlyRequests": true,
"annotations": [
"annotation"
]
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
"sap.fiori": {}
}
