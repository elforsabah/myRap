{
  "error" : {
    "code" : "/IWCOR/CX_OD_METHD_NOT_ALLOWED/005056A509B11ED199D8826D151FC0FE",
    "message" : "The specified HTTP method is not allowed for the resource identified by the Data Service Request URI",
    "@SAP__common.ExceptionCategory" : "Client_Error",
    "innererror" : {
      "ErrorDetails" : {
        "@SAP__common.Application" : {
          "ComponentId" : "BC-ESI-ESF-GW",
          "ServiceRepository" : "SRVD_A2X",
          "ServiceId" : "ZSB_LANF_BMS_INTERFACE_V4",
          "ServiceVersion" : "0001"
        },
        "@SAP__common.TransactionId" : "F14022DB810C0000E00693874FE413AB",
        "@SAP__common.Timestamp" : "20260121124907.739255",
        "@SAP__common.ErrorResolution" : {
          "Analysis" : "Use ADT feed reader \"SAP Gateway Error Log\" or run transaction /IWFND/ERROR_LOG on SAP Gateway hub system and search for entries with the timestamp above for more details",
          "Note" : "See SAP Note 1797736 for error analysis (https://service.sap.com/sap/support/notes/1797736)"
        }
      }
    }
  }
}
