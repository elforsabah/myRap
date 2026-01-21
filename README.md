{
  "error" : {
    "code" : "CX_SXML_PARSE_ERROR",
    "message" : "Error while parsing an XML stream",
    "@SAP__common.ExceptionCategory" : "Client_Error",
    "details" : [
      {
        "code" : "/IWCOR/CX_OD_BAD_REQUEST/005056A509B11ED1B9BF94F386DD82E6",
        "message" : "The Data Services Request could not be understood due to malformed syntax",
        "@SAP__common.Severity" : "error",
        "@SAP__common.numericSeverity" : 4
      },
      {
        "code" : "/IWCOR/CX_OD_EP_PROPERTY_ERROR/005056A509B11ED1BF822D2D0917DA04",
        "message" : "Property 'Menge' at offset '255' has invalid value '1.000'",
        "@SAP__common.Severity" : "error",
        "@SAP__common.numericSeverity" : 4
      }
    ],
    "innererror" : {
      "ErrorDetails" : {
        "@SAP__common.Application" : {
          "ComponentId" : "BC-ESI-ESF-GW",
          "ServiceRepository" : "SRVD_A2X",
          "ServiceId" : "ZSB_LANF_BMS_INTERFACE_V4",
          "ServiceVersion" : "0001"
        },
        "@SAP__common.TransactionId" : "F14022DB810C0000E00693874FE409C6",
        "@SAP__common.Timestamp" : "20260121114705.907948",
        "@SAP__common.ErrorResolution" : {
          "Analysis" : "Use ADT feed reader \"SAP Gateway Error Log\" or run transaction /IWFND/ERROR_LOG on SAP Gateway hub system and search for entries with the timestamp above for more details",
          "Note" : "See SAP Note 1797736 for error analysis (https://service.sap.com/sap/support/notes/1797736)"
        }
      }
    }
  }
}


<img width="1876" height="838" alt="image" src="https://github.com/user-attachments/assets/03759f5f-ed80-4e39-b6cb-81115be0cb66" />
