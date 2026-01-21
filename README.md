{
  "error" : {
    "code" : "00/001",
    "message" : "Contract 4600001234 not found",
    "@SAP__common.ExceptionCategory" : "Provider_Application_Error",
    "innererror" : {
      "ErrorDetails" : {
        "@SAP__common.Application" : {
          "ComponentId" : "BC-ESI-ESF-GW",
          "ServiceRepository" : "SRVD_A2X",
          "ServiceId" : "ZSB_LANF_BMS_INTERFACE_V4",
          "ServiceVersion" : "0001"
        },
        "@SAP__common.TransactionId" : "F14022DB810C0000E00693874FE409C6",
        "@SAP__common.Timestamp" : "20260121120633.724984",
        "@SAP__common.ErrorResolution" : {
          "Analysis" : "Use ADT feed reader \"SAP Gateway Error Log\" or run transaction /IWFND/ERROR_LOG on SAP Gateway hub system and search for entries with the timestamp above for more details",
          "Note" : "See SAP Note 1797736 for error analysis (https://service.sap.com/sap/support/notes/1797736)"
        }
      }
    }
  }
}
