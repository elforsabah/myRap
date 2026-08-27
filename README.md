
 
{
  "openapi": "3.0.4",
  "info": {
    "title": "BMS API",
    "description": "",
    "version": "v1"
  },
  "paths": {
    "/api/container/create-order-halle": {
      "post": {
        "tags": [
          "Container-API"
        ],
        "summary": "Erzeugt einen Containerdienstauftrag nach der für den Import entwickelten Logik.",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/InputOrder"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/InputOrder"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/InputOrder"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/InputOrder"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/Int32DataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Int32DataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/Int32DataResponse"
                }
              }
            }
          },
          "400": {
            "description": "Bad Request",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "500": {
            "description": "Internal Server Error"
          }
        }
      }
    },
    "/api/container/depot-data-for-deadline": {
      "get": {
        "tags": [
          "Container-API"
        ],
        "summary": "Gibt die Depot-Containeranzahlen pro Containertyp für einen bestimmten Stichtag zurück.",
        "parameters": [
          {
            "name": "deadline",
            "in": "query",
            "description": "Der Stichtag, für den die Depotdaten abgerufen werden sollen.",
            "schema": {
              "type": "string",
              "format": "date-time"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "text/plain": {
                "schema": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "additionalProperties": { }
                  }
                }
              },
              "application/json": {
                "schema": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "additionalProperties": { }
                  }
                }
              },
              "text/json": {
                "schema": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "additionalProperties": { }
                  }
                }
              }
            }
          },
          "500": {
            "description": "Internal Server Error"
          }
        }
      }
    },
    "/api/user/healthCheck": {
      "get": {
        "tags": [
          "User-API"
        ],
        "responses": {
          "200": {
            "description": "OK"
          }
        }
      }
    },
    "/api/user/authenticate/user": {
      "post": {
        "tags": [
          "User-API"
        ],
        "summary": "Authenticates a user using username and password credentials.",
        "description": "Possible responses:\r\n200 OK, 400 Bad Request, 401 Unauthorized, 500 Internal Server Error.",
        "requestBody": {
          "description": "The authentication request containing username and password.",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UserAuthRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/TokenResponseModelDataResponse"
                }
              }
            }
          },
          "400": {
            "description": "Bad Request",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "401": {
            "description": "Unauthorized",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "500": {
            "description": "Internal Server Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          }
        }
      }
    },
    "/api/user/authenticate/refresh-token": {
      "post": {
        "tags": [
          "User-API"
        ],
        "summary": "Ruft ein neues Tokenpaar mit dem Refresh-Token ab.",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/RefreshRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/RefreshRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/RefreshRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/RefreshRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/TokenResponseModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/TokenResponseModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/TokenResponseModelDataResponse"
                }
              }
            }
          },
          "401": {
            "description": "Unauthorized",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "500": {
            "description": "Internal Server Error",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "ErrorResponse": {
        "type": "object",
        "properties": {
          "error": {
            "$ref": "#/components/schemas/ErrorResponseModel"
          }
        },
        "additionalProperties": false,
        "description": "Eine Fehlerrückgabe."
      },
      "ErrorResponseModel": {
        "type": "object",
        "properties": {
          "message": {
            "type": "string",
            "nullable": true
          },
          "timestamp": {
            "type": "string",
            "format": "date-time"
          }
        },
        "additionalProperties": false
      },
      "InputContact": {
        "required": [
          "city",
          "street",
          "streetNumber",
          "zipCode"
        ],
        "type": "object",
        "properties": {
          "street": {
            "minLength": 1,
            "type": "string",
            "description": "Die Straße des Kontakts."
          },
          "streetNumber": {
            "minLength": 1,
            "type": "string",
            "description": "Die Hausnummer des Kontakts."
          },
          "zipCode": {
            "minLength": 1,
            "type": "string",
            "description": "Die Postleitzahl des Kontakts."
          },
          "city": {
            "minLength": 1,
            "type": "string",
            "description": "Der Ort des Kontakts."
          }
        },
        "additionalProperties": false,
        "description": "Ein Kontakt (Kunde, Erzeuger, Entsorger, Beförderer) mit den grundlegenden Adressinformationen."
      },
      "InputContactWithName": {
        "required": [
          "city",
          "name1",
          "street",
          "streetNumber",
          "zipCode"
        ],
        "type": "object",
        "properties": {
          "name1": {
            "minLength": 1,
            "type": "string",
            "description": "Der erste Name des Kontakts."
          },
          "name2": {
            "type": "string",
            "description": "Der zweite Name des Kontakts (optional).",
            "nullable": true
          },
          "street": {
            "minLength": 1,
            "type": "string",
            "description": "Die Straße des Kontakts."
          },
          "streetNumber": {
            "minLength": 1,
            "type": "string",
            "description": "Die Hausnummer des Kontakts."
          },
          "zipCode": {
            "minLength": 1,
            "type": "string",
            "description": "Die Postleitzahl des Kontakts."
          },
          "city": {
            "minLength": 1,
            "type": "string",
            "description": "Der Ort des Kontakts."
          }
        },
        "additionalProperties": false,
        "description": "Ein Kontakt mit Namen (z.B. Kunde, Erzeuger, Beförderer) und den grundlegenden Adressinformationen."
      },
      "InputContactWithNameAndNumber": {
        "required": [
          "city",
          "name1",
          "number",
          "street",
          "streetNumber",
          "zipCode"
        ],
        "type": "object",
        "properties": {
          "number": {
            "minLength": 1,
            "type": "string",
            "description": "Die Nummer des Kontakts (z.B. Kundennummer, Lieferantennummer, etc.)."
          },
          "name1": {
            "minLength": 1,
            "type": "string",
            "description": "Der erste Name des Kontakts."
          },
          "name2": {
            "type": "string",
            "description": "Der zweite Name des Kontakts (optional).",
            "nullable": true
          },
          "street": {
            "minLength": 1,
            "type": "string",
            "description": "Die Straße des Kontakts."
          },
          "streetNumber": {
            "minLength": 1,
            "type": "string",
            "description": "Die Hausnummer des Kontakts."
          },
          "zipCode": {
            "minLength": 1,
            "type": "string",
            "description": "Die Postleitzahl des Kontakts."
          },
          "city": {
            "minLength": 1,
            "type": "string",
            "description": "Der Ort des Kontakts."
          }
        },
        "additionalProperties": false,
        "description": "Ein Kontakt mit Name und Nummer (z.B. Kunde mit Kundennummer, Erzeuger mit Erzeugernummer, Beförderer mit Beförderernummer) und den grundlegenden Adressinformationen."
      },
      "InputContainer": {
        "required": [
          "containerTypeName",
          "movementType"
        ],
        "type": "object",
        "properties": {
          "containerNumberOld": {
            "type": "string",
            "description": "Containernummer (bei Wechsel und Einzug: alte Containernummer, bei Aufstellung: leer)",
            "nullable": true
          },
          "containerNumberNew": {
            "type": "string",
            "description": "Containernummer (bei Austellung und Wechsel: neue Containernummer, bei Einzug: leer)",
            "nullable": true
          },
          "movementType": {
            "enum": [
              "new",
              "change",
              "collect"
            ],
            "type": "string",
            "description": "Bewegungsart (New = Aufstellung, Change = Wechsel, Collect = Einzug)"
          },
          "customerOwned": {
            "type": "boolean",
            "description": "Gibt an, ob es ein Kundeneigener Container ist, bei dem kein Barcode zum Scannen vorhanden ist."
          },
          "containerTypeName": {
            "minLength": 1,
            "type": "string",
            "description": "Name des Containertyps."
          },
          "ContainerMovementTypeInfo": {
            "type": "string",
            "description": "Zusatzinfo für den Prozess, die in der Auftragsübersicht angezeigt wird. (z.B. \"Containertausch\", \"Leerung\", \"Abholung\")",
            "nullable": true
          },
          "internalRemark": {
            "type": "string",
            "description": "Interne Fahrerbemerkung, die nicht an den Kunden weitergegeben wird.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Container eines Auftrags."
      },
      "InputOrder": {
        "required": [
          "containers",
          "customer",
          "orderNumber",
          "plannedDate",
          "status"
        ],
        "type": "object",
        "properties": {
          "status": {
            "enum": [
              "ok",
              "cancelled"
            ],
            "type": "string",
            "description": "Der Status des Auftrags. (gültige Werte sind OK, Cancelled)"
          },
          "externalSystemID": {
            "type": "string",
            "description": "FremdSystem-TourID für die Rückmeldung gedacht",
            "nullable": true
          },
          "sortNumber": {
            "type": "integer",
            "description": "Sortierreihenfolge der Aufträge. (0, wenn keine Sortierung erforderlich ist)",
            "format": "int32"
          },
          "orderNumber": {
            "minLength": 1,
            "type": "string",
            "description": "Die Auftragsnummer aus dem Quellsystem."
          },
          "orderSheet": {
            "type": "string",
            "description": "Die Nummer des Auftragsscheins.",
            "nullable": true
          },
          "orderSheetType": {
            "enum": [
              "LS",
              "ÜS",
              "BS"
            ],
            "type": "string",
            "description": "Die Art des Auftragsscheins. (gültige Werte sind LS = Lieferschein, ÜS = Übernahmeschein, BS = Begleitschein)",
            "nullable": true
          },
          "customer": {
            "$ref": "#/components/schemas/InputContactWithNameAndNumber"
          },
          "placeOfDelivery": {
            "$ref": "#/components/schemas/InputContact"
          },
          "location": {
            "$ref": "#/components/schemas/InputContact"
          },
          "estimatedDuration": {
            "type": "integer",
            "description": "Die geschätzte Auftragsdauer in Minuten. (0, wenn noch nicht geschätzt)",
            "format": "int32"
          },
          "plannedDate": {
            "type": "string",
            "description": "Das ursprünglich geplante Datum.",
            "format": "date-time"
          },
          "executionTimeFrameStart": {
            "type": "string",
            "description": "Der Beginn des Leistungszeitraums.",
            "format": "date-span",
            "nullable": true
          },
          "executionTimeFrameEnd": {
            "type": "string",
            "description": "Das Ende des Leistungszeitraums.",
            "format": "date-span",
            "nullable": true
          },
          "notes": {
            "type": "string",
            "description": "Die Notizen zum Auftrag.",
            "nullable": true
          },
          "specialNotes": {
            "type": "string",
            "description": "Die besonderen Hinweise zum Auftrag.",
            "nullable": true
          },
          "producer": {
            "$ref": "#/components/schemas/InputContactWithNameAndNumber"
          },
          "recycler": {
            "$ref": "#/components/schemas/InputContactWithName"
          },
          "carrier": {
            "$ref": "#/components/schemas/InputContactWithNameAndNumber"
          },
          "garbageKey": {
            "type": "string",
            "description": "Der Abfallschlüssel, der die Abfallart des Auftrags angibt.",
            "nullable": true
          },
          "garbageName": {
            "type": "string",
            "description": "Die Abfallbezeichnung.",
            "nullable": true
          },
          "collectiveConsignmentNoteNumber": {
            "type": "string",
            "description": "Die Sammelbegleitscheinnummer.",
            "nullable": true
          },
          "team": {
            "type": "string",
            "description": "Die Bezeichnung der Kolonne in BMS.",
            "nullable": true
          },
          "signatureRequired": {
            "type": "boolean",
            "description": "Gibt an, ob eine Unterschrift erforderlich ist."
          },
          "containers": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/InputContainer"
            },
            "description": "Die Container des Auftrags. (mindestens ein Eintrag erforderlich)"
          }
        },
        "additionalProperties": false,
        "description": "Ein Auftrag."
      },
      "Int32DataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "integer",
            "description": "Returns the data of the response.",
            "format": "int32"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "RefreshRequestModel": {
        "required": [
          "refreshToken"
        ],
        "type": "object",
        "properties": {
          "refreshToken": {
            "minLength": 1,
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "TokenResponseModel": {
        "type": "object",
        "properties": {
          "accessToken": {
            "type": "string",
            "nullable": true
          },
          "refreshToken": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "TokenResponseModelDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/TokenResponseModel"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "UserAuthRequestModel": {
        "required": [
          "password",
          "username"
        ],
        "type": "object",
        "properties": {
          "username": {
            "minLength": 1,
            "type": "string"
          },
          "password": {
            "minLength": 1,
            "type": "string",
            "format": "password"
          }
        },
        "additionalProperties": false
      }
    },
    "securitySchemes": {
      "Bearer": {
        "type": "apiKey",
        "name": "Authorization",
        "in": "header"
      }
    }
  },
  "security": [
    { }
  ],
  "tags": [
    {
      "name": "Container-API"
    },
    {
      "name": "User-API"
    }
  ]
}
