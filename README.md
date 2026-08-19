
Elvis Forsab
Mein Dashboard
Projekte
Aufgaben
Zeitberichte
Anwesenheit
Kostenträger
Ressourcen(Global)
Gesuchten Text eingeben...
/
To-Do
Aktivitätsfeed1
Implementierung W&R
Überordnungen:
PROLOGA Services›
Projekte›
HWS Hallesche Wasser und Stadtwirtschaft GmbH
Dashboard
Aufgaben
Zeitaufwand
Gantt
Roadmap
Ressourcenmanagement
Wiki
DMS
Dokumente
Dateien
Kalender
Download (395 kB)
Entwicklung #40067 » swagger.html
Patrick DahmsOffline, 04.03.2026 08:55

 
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<meta name="description" content="SwaggerUI" />
		<title>SwaggerUI</title>
		<link rel="stylesheet" href="https://unpkg.com/swagger-ui-dist@5.11.0/swagger-ui.css" />
	</head>
	<body>
		<div id="swagger-ui"></div>
		<script src="https://unpkg.com/swagger-ui-dist@5.11.0/swagger-ui-bundle.js" crossorigin></script>
		<script>
			const spec = {
  "openapi": "3.0.1",
  "info": {
    "title": "BMS API",
    "description": "",
    "version": "v1"
  },
  "paths": {
    "/api/admin/database/update-script/get": {
      "get": {
        "tags": [
          "Admin-API"
        ],
        "summary": "Gibt das Skript zum Aktualisieren der Datenbank zurück.",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/DatabaseScriptResponseModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DatabaseScriptResponseModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/DatabaseScriptResponseModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/bulky-waste-online-form/get-salutations": {
      "get": {
        "tags": [
          "BulkyWasteOnlineForm"
        ],
        "summary": "Get waste bin by id",
        "description": "Get all waste bin information by given ID",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/SelectOptionDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/bulky-waste-online-form/get-streets": {
      "get": {
        "tags": [
          "BulkyWasteOnlineForm"
        ],
        "summary": "Get streets",
        "description": "get all definded streets",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/SelectOptionDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/bulky-waste-online-form/get-housenumbers/{id}": {
      "get": {
        "tags": [
          "BulkyWasteOnlineForm"
        ],
        "summary": "Get housnumber range by AddLocation ID",
        "description": "get a list of housnumbers by location id",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HousenumberSelectOptionDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/bulky-waste-online-form/get-bulky-waste-items": {
      "get": {
        "tags": [
          "BulkyWasteOnlineForm"
        ],
        "summary": "Get housnumber range by AddLocation ID",
        "description": "get a list of housnumbers by location id",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BulkyWasteItemsGroupByOnlineGroupNameDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/bulky-waste-online-form/get-pick-up-dates/{fraction}/{location}": {
      "get": {
        "tags": [
          "BulkyWasteOnlineForm"
        ],
        "summary": "Get pick up dates",
        "description": "get a list of pick up dates",
        "parameters": [
          {
            "name": "fraction",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "location",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/SelectOptionDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/bulky-waste-online-form/create-bulky-waste-contract": {
      "post": {
        "tags": [
          "BulkyWasteOnlineForm"
        ],
        "summary": "Create Bulky Waste Online Contract",
        "description": "Create Bulky Waste Online Contract",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BulkyWasteOnlineContractFormData"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BulkyWasteOnlineContractFormData"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BulkyWasteOnlineContractFormData"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BulkyWasteOnlineContractFormData"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/SelectOptionDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/test": {
      "get": {
        "tags": [
          "Common-API"
        ],
        "summary": "Test",
        "description": "Test route to check if api service work",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/get-taxes": {
      "get": {
        "tags": [
          "Common-API"
        ],
        "summary": "Get taxes",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/TaxDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/get-debitors": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get debitors list",
        "description": "Gets a list of all debitors",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/get-debitor-by-id/{debitorId}": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get debitor by id",
        "description": "Get a debitors by debitor id",
        "parameters": [
          {
            "name": "debitorId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorDTODataResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/check-debitor-exists": {
      "post": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Check if a Debitor already exist",
        "description": "Check if a Debitor already exist filterd by Debitor ID, and Activation Code",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CheckDebitorExists"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CheckDebitorExists"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CheckDebitorExists"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CheckDebitorExists"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorDTODataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              }
            }
          }
        }
      }
    },
    "/api/common/debitor/get-debitor-by-foreign-id/{foreignId}": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get debitor by foreign id",
        "description": "Get debitor by foreign id",
        "parameters": [
          {
            "name": "foreignId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorDTODataResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/get-debitors-by-external-system-id": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get debitors list by external system id",
        "description": "Gets a list of debitors with given external system ID",
        "parameters": [
          {
            "name": "externalSystemID",
            "in": "query",
            "description": "",
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/get-debitors-by-location-id": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get debitors list by location id",
        "description": "Gets a list of debitors by  given location id",
        "parameters": [
          {
            "name": "locationId",
            "in": "query",
            "description": "",
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/get-titles": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get debitor titles list",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorTitleDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/check-debitor-customer-id-relation/{relationId}": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Check Debitor Customer Id Relation",
        "description": "Check if there a relation between customer foreign key and bms debitor id",
        "parameters": [
          {
            "name": "relationId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsDebitorCustomerIdRelationDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
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
    "/api/common/debitor/create-debitor": {
      "post": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Create Debitor",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/create-debitor-egov": {
      "post": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Create Debitor eGov - Special Version for ePortal Basel",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/update-debitor": {
      "patch": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Update Debitor",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/Debitor"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/add-debitor-customer-id-relation": {
      "post": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Add relation between customer foreign key and bms debitor id",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/DebitorCustomerIdRelation"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/DebitorCustomerIdRelation"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/DebitorCustomerIdRelation"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/DebitorCustomerIdRelation"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "409": {
            "description": "Conflict",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
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
          }
        }
      }
    },
    "/api/common/debitor/delete-debitor-customer-id-relation": {
      "delete": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Delete relation between customer id and bms debitor id",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/DebitorCustomerIdRelation"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/DebitorCustomerIdRelation"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/DebitorCustomerIdRelation"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/DebitorCustomerIdRelation"
              }
            }
          }
        },
        "responses": {
          "204": {
            "description": "No Content"
          },
          "500": {
            "description": "Server Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
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
    "/api/common/debitor/check-debitor-state/{debitiorId}": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Check if debitor finaly approved",
        "description": "<br>Check if a Debitor approved, in pending state or blocked\r\n<br>\r\n                    Approved    = Debitor is fully active <br />\r\n                    Pending     = Debitor will be checked <br />\r\n                    Blocked     = Debitor is blocked\r\n                ",
        "parameters": [
          {
            "name": "debitiorId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorStateDTODataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
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
    "/api/common/debitor/get-locations-by-debitor-Id/{debitorId}": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get locations by debitor id",
        "parameters": [
          {
            "name": "debitorId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsLocationBmsDebitorDtoListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/debitor/get-debitor-waste-bins/{externalSystemId}": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get debitor waste bins by external system id",
        "parameters": [
          {
            "name": "externalSystemId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorWasteBinDetailsDtoListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
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
    "/api/common/debitor/get-debitor-waste-bin/{externalSystemId}/{wasteBinId}": {
      "get": {
        "tags": [
          "Common-API (Debitoren)"
        ],
        "summary": "Get debitor waste bins by external system id  and Wastebin id",
        "parameters": [
          {
            "name": "externalSystemId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "wasteBinId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DebitorWasteBinDetailsDtoDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
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
    "/api/common/location/get-active-districts": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get all active districts",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/CityDistrictDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-cities-by-postcode/{postcode}": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get cities by zipcode",
        "parameters": [
          {
            "name": "postcode",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/CityDistrictDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-streets-by-district-id/{districtId}": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get streets by district id",
        "parameters": [
          {
            "name": "districtId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StreetDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-streetpart-housenumber-range/{districtId}/{streetName}": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get streetpart housenumber range",
        "parameters": [
          {
            "name": "districtId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          },
          {
            "name": "streetName",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-waste-bins-by-location-Id/{locationId}": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get waste bins by location id",
        "parameters": [
          {
            "name": "locationId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/LocationWasteBinDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-streets-grouped-by-postcode": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get streets grouped streets group by postcode",
        "description": "This route gets all in BMS defined streets with id grouped by postcode",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StreetGroupByPostcodeDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-all-addresses": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get list of addresses",
        "description": "This route gets all in BMS defined streets with id grouped by poscode",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/AddressDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-address-by-forgin-id/{id}": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get address by Forgin id",
        "description": "This route get address values by forgin id",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/AddressDTODataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-address-by-location-id/{id}": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get address by location id",
        "description": "This route get address values by location id",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/AddressDTODataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-countries": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get countries",
        "description": "This route get all defined countries",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/CountryDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/location/get-country-id/{Iso3166ALPHA2}": {
      "get": {
        "tags": [
          "Common-API (Location)"
        ],
        "summary": "Get Country Id",
        "description": "Get bms contry by ISO 3166 ALPHA 2 Code (eg : DE for Germany)",
        "parameters": [
          {
            "name": "Iso3166ALPHA2",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/CountryDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/order/generate-waste-bin-sale-offer": {
      "post": {
        "tags": [
          "Common-API (Order)"
        ],
        "summary": "Generate Waste Bin Order Offer",
        "description": "Generate a Offer PDF from submited order values",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/WasteBinOrder"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/WasteBinSaleOfferDocumentDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/common/order/add-customer-waste-Bin": {
      "post": {
        "tags": [
          "Common-API (Order)"
        ],
        "summary": "Add already purchased customer waste bins",
        "description": "Add already purchased customer waste bins to BMS",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/WasteBinOrder"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/WasteBinSaleOfferDocumentDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/common/order/order-offer/{TransactionId}": {
      "post": {
        "tags": [
          "Common-API (Order)"
        ],
        "summary": "turn offer into order",
        "parameters": [
          {
            "name": "TransactionId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "400": {
            "description": "Bad Request",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/order/reject-offer/{TransactionId}": {
      "delete": {
        "tags": [
          "Common-API (Order)"
        ],
        "summary": "reject offer",
        "parameters": [
          {
            "name": "TransactionId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/order/get-offer-document/{TransactionId}": {
      "get": {
        "tags": [
          "Common-API (Order)"
        ],
        "summary": "Get offer document",
        "parameters": [
          {
            "name": "TransactionId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/WasteBinSaleOfferDocumentDataResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/common/order/get-invoice-document/{TransactionId}": {
      "get": {
        "tags": [
          "Common-API (Order)"
        ],
        "summary": "Get invoice document",
        "parameters": [
          {
            "name": "TransactionId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/WasteBinSaleBillingDocumentDataResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/container/create": {
      "post": {
        "tags": [
          "Container-API"
        ],
        "summary": "Create container contract",
        "requestBody": {
          "description": "CreateContractModel data",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateContractModel"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/container/update": {
      "put": {
        "tags": [
          "Container-API"
        ],
        "summary": "Update container contract",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateContractModel"
              }
            }
          }
        },
        "responses": {
          "204": {
            "description": "No Content"
          },
          "404": {
            "description": "Not Found",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/container/AddConatinerTypeToContract": {
      "put": {
        "tags": [
          "Container-API"
        ],
        "summary": "Add container type object to contract",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/AddContractServiceContainerTypeModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/AddContractServiceContainerTypeModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/AddContractServiceContainerTypeModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/AddContractServiceContainerTypeModel"
              }
            }
          }
        },
        "responses": {
          "204": {
            "description": "No Content"
          },
          "404": {
            "description": "Not Found",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/container/get-container-service-catalogs": {
      "get": {
        "tags": [
          "Container-API"
        ],
        "summary": "Get container service catalogs",
        "responses": {
          "200": {
            "description": "Success"
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
          "500": {
            "description": "Server Error"
          }
        }
      }
    },
    "/api/container/get-service-catalogs-from-container-contract/{contractId}": {
      "get": {
        "tags": [
          "Container-API"
        ],
        "summary": "Get container service catalogs from container contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsContainerContractObjectDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/container/create-contract-container-object-appointment": {
      "post": {
        "tags": [
          "Container-API"
        ],
        "summary": "",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/ContainerContractAppointment"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ContainerContractAppointment"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/ContainerContractAppointment"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/ContainerContractAppointment"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/container/create-order-halle": {
      "post": {
        "tags": [
          "Container-API"
        ],
        "summary": "Erzeugt einen Containerdienstauftrag nach der für den Opti-AWI-Import entwickelten Logik.",
        "requestBody": {
          "description": "Der anzulegende Auftrag.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/OptiAwiOrder"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/OptiAwiOrder"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/OptiAwiOrder"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/OptiAwiOrder"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/crm/ticket/{ticketId}": {
      "get": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Ruft ein Jsonobjekt zu einem bestimmten Ticket ab",
        "parameters": [
          {
            "name": "ticketId",
            "in": "path",
            "description": "ID des Tickets zu dem die Daten ausgegeben werden sollen",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/crm/get-tickets": {
      "get": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Get all tickets for specified timespan, that are not already exported and the user is authorized to see-",
        "description": "Get tickets",
        "parameters": [
          {
            "name": "numberOfDaysPast",
            "in": "query",
            "description": "Number of Days that are passed before today",
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/crm/ticket/create": {
      "post": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Create a new BMS CRM-Ticket",
        "requestBody": {
          "description": "Data to Create a Ticket",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/crm/ticket/create-citizen-concerns-crm-ticket": {
      "post": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Create Citizen Concerns CRM Ticket",
        "requestBody": {
          "description": "Data to Create a Ticket",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateCitizenConcernsCrmTicket"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateCitizenConcernsCrmTicket"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateCitizenConcernsCrmTicket"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateCitizenConcernsCrmTicket"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
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
          "500": {
            "description": "Server Error",
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
    },
    "/api/crm/get-crm-ticket-online-types": {
      "get": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Get Crm Ticket Types for Online Tickets",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketTypeDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/crm/ticket/comment/add": {
      "post": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Creates ticket step with the given Comment and adds attachments to Ticket with the given ID.",
        "requestBody": {
          "description": "Date for creating commt.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/crm/ticket/status/edit": {
      "post": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Creates ticket step with the given Comment and adds attachments to Ticket with the given ID.",
        "requestBody": {
          "description": "Date for creating commt.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/crm/ticket/finish/doublet": {
      "post": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Markes the ticket as doublet, finishs the ticket and creates a ticket step with a comment given, for given ticket id",
        "requestBody": {
          "description": "Needed ticket data and comment.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/crm/ticket/finish/resolved": {
      "post": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Markes the ticket as resoveld, finishs the ticket and creates a ticket step with a comment given, for given ticket id",
        "requestBody": {
          "description": "Needed ticket data and comment.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/crm/ticket/confirm/export": {
      "post": {
        "tags": [
          "CRM-API"
        ],
        "summary": "Markes the ticket getted by given id as exported and creates a comment as ticket step.",
        "requestBody": {
          "description": "Needed data for comment and setting exported flag",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/damage-type/get-damage-types": {
      "get": {
        "tags": [
          "DamageType"
        ],
        "summary": "Get damage types",
        "description": "Get all damage types",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsDamageTypeDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/emptying-cycle/get-emptying-cycles-validities": {
      "get": {
        "tags": [
          "EmptyingCycle"
        ],
        "summary": "Get emptying cycles validities",
        "description": "Get all emptying cycles validities defined by BMS masterdata",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteBinEmptingCycleValidityDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/emptying-cycle/get-cycles-by-waste-bin-size-and-waste-type/{wasteBinSizeId}/{wasteTypeId}": {
      "get": {
        "tags": [
          "EmptyingCycle"
        ],
        "summary": "Get emptying cycles by waste bin size and waste type",
        "description": "Get all waste bin cycles defined by given waste type id and waste bin size id",
        "parameters": [
          {
            "name": "wasteBinSizeId",
            "in": "path",
            "description": "int wasteBinSizeId",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          },
          {
            "name": "wasteTypeId",
            "in": "path",
            "description": "int wasteTypeId",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/WasteBinSizeWasteTypeCyclesDTODataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/daily-overview/health-check": {
      "get": {
        "tags": [
          "MissionPlanning-API (Daily Overview)"
        ],
        "summary": "Health Check",
        "description": "Checks if controller is healthy",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/daily-overview/get-monitor-configuration/{monitorId}": {
      "get": {
        "tags": [
          "MissionPlanning-API (Daily Overview)"
        ],
        "summary": "Get monitor configuration by id",
        "parameters": [
          {
            "name": "monitorId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DailyOverviewDataListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/daily-overview/create-daily-overview-monitor": {
      "post": {
        "tags": [
          "MissionPlanning-API (Daily Overview)"
        ],
        "summary": "Creates a new daily overview monitor",
        "description": "Creates a new daily overview monitor by the given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateDailyOverviewMonitorDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateDailyOverviewMonitorDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateDailyOverviewMonitorDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateDailyOverviewMonitorDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/deviation/get-deviations": {
      "get": {
        "tags": [
          "MissionPlanning-API (Deviations)"
        ],
        "summary": "Get deviations list",
        "description": "Get deviation data for all current deviations",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/DeviationDataListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/deviation/create-deviation": {
      "post": {
        "tags": [
          "MissionPlanning-API (Deviations)"
        ],
        "summary": "Create deviation",
        "description": "Creates new deviation by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateDeviationDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateDeviationDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateDeviationDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateDeviationDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/deviation/update-deviation": {
      "patch": {
        "tags": [
          "MissionPlanning-API (Deviations)"
        ],
        "summary": "Update deviation",
        "description": "Updates new deviation by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateDeviationDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateDeviationDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateDeviationDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateDeviationDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employeedeviation/get-employee-deviations-by-employee-number": {
      "get": {
        "tags": [
          "MissionPlanning-API (EmployeeDeviations)"
        ],
        "summary": "Get employee deviations list by employee Number",
        "description": "Get employee deviation data for specified employee",
        "parameters": [
          {
            "name": "validFrom",
            "in": "query",
            "schema": {
              "type": "string",
              "format": "date-time"
            }
          },
          {
            "name": "validUntil",
            "in": "query",
            "schema": {
              "type": "string",
              "format": "date-time"
            }
          },
          {
            "name": "employeeNumber",
            "in": "query",
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/EmployeeDeviationDataListListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employeedeviation/get-employee-deviations": {
      "get": {
        "tags": [
          "MissionPlanning-API (EmployeeDeviations)"
        ],
        "summary": "Get employee deviations list",
        "description": "Get employee deviation data for all employees",
        "parameters": [
          {
            "name": "validFrom",
            "in": "query",
            "schema": {
              "type": "string",
              "format": "date-time"
            }
          },
          {
            "name": "validUntil",
            "in": "query",
            "schema": {
              "type": "string",
              "format": "date-time"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/EmployeeDeviationDataListListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employeedeviation/handle-employeedeviation": {
      "post": {
        "tags": [
          "MissionPlanning-API (EmployeeDeviations)"
        ],
        "summary": "Handle employee deviation list",
        "description": "Handels employee deviation list by given JSON object, the following business processes are triggered: <br /><ul><li>create new employee deviation </li><li>updates existing employee deviation by given id </li><li>deletes employee deviation by given id</li></ul>",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/EmployeeDeviationListDTO"
                }
              }
            },
            "application/json": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/EmployeeDeviationListDTO"
                }
              }
            },
            "text/json": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/EmployeeDeviationListDTO"
                }
              }
            },
            "application/*+json": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/EmployeeDeviationListDTO"
                }
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employee/get-employee-by-employee-number": {
      "get": {
        "tags": [
          "MissionPlanning-API (Employees)"
        ],
        "summary": "Get employee by employee number",
        "description": "Get employee data by given employee number",
        "parameters": [
          {
            "name": "employeeNumber",
            "in": "query",
            "description": "employeeNumber",
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/EmployeeDataListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employee/get-employees": {
      "get": {
        "tags": [
          "MissionPlanning-API (Employees)"
        ],
        "summary": "Get employees list",
        "description": "Get employee data for all current employees",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/EmployeeDataListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employee/create-employee": {
      "post": {
        "tags": [
          "MissionPlanning-API (Employees)"
        ],
        "summary": "Create employee",
        "description": "Creates new employee by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateEmployeeDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateEmployeeDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateEmployeeDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateEmployeeDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employee/update-employee": {
      "patch": {
        "tags": [
          "MissionPlanning-API (Employees)"
        ],
        "summary": "Update employee",
        "description": "Updates an existing employee by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateEmployeeDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateEmployeeDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateEmployeeDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateEmployeeDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employee/create-employee-with-sap-btp": {
      "post": {
        "tags": [
          "MissionPlanning-API (Employees)"
        ],
        "summary": "Create employee",
        "description": "Creates new employee by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateEmployeesWithSapBtpDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateEmployeesWithSapBtpDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateEmployeesWithSapBtpDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateEmployeesWithSapBtpDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/employee/change-employee-app-password": {
      "post": {
        "tags": [
          "MissionPlanning-API (Employees)"
        ],
        "summary": "Changes the password of the given employee by common restrictions",
        "description": "Creates new employee by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/EmpoyeeAppPasswordDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/EmpoyeeAppPasswordDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/EmpoyeeAppPasswordDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/EmpoyeeAppPasswordDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/vehicletype/get-vehicletypes": {
      "get": {
        "tags": [
          "MissionPlanning-API (Vehicle Types)"
        ],
        "summary": "Get vehicle types list",
        "description": "Get vehicletypes data for all current vehicletypes",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/VehicleTypeDataListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/vehicletype/get-vehicle-type-by-external-system-id": {
      "get": {
        "tags": [
          "MissionPlanning-API (Vehicle Types)"
        ],
        "summary": "Get vehicle type by external system id",
        "description": "Get vehicle type by the given external system id",
        "parameters": [
          {
            "name": "externalSystemID",
            "in": "query",
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/VehicleTypeDataListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/vehicletype/create-vehicletype": {
      "post": {
        "tags": [
          "MissionPlanning-API (Vehicle Types)"
        ],
        "summary": "Create vehicle type",
        "description": "Creates new vehicletype by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateVehicleTypeDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateVehicleTypeDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateVehicleTypeDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateVehicleTypeDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/vehicletype/update-vehicletype": {
      "patch": {
        "tags": [
          "MissionPlanning-API (Vehicle Types)"
        ],
        "summary": "Update vehicle type",
        "description": "Updates an existing vehicletype by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateVehicleTypeDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateVehicleTypeDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateVehicleTypeDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateVehicleTypeDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/Vehicledeviation/get-vehicle-deviations-by-Vehicle-number/{VehicleNumber}/{validFrom}/{validUntil}": {
      "get": {
        "tags": [
          "MissionPlanning-API (VehicleDeviations)"
        ],
        "summary": "Get vehicle deviation data for a vehicle filterd by Vehicle number",
        "description": "<b>Get vehicle deviation data for a vehicle filterd by Vehicle number</b>\r\n<br />\r\n<br />\r\n                VehicleNumber: unique Vehiclenumber<br />\r\n                validFrom: Start DateTime for between Filter<br />\r\n                validUntil: End DateTime for between Filter<br />",
        "parameters": [
          {
            "name": "VehicleNumber",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "validFrom",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string",
              "format": "date-time"
            }
          },
          {
            "name": "validUntil",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string",
              "format": "date-time"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/VehicleDeviationDataListListDataResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/Vehicledeviation/handle-vehicledeviation": {
      "post": {
        "tags": [
          "MissionPlanning-API (VehicleDeviations)"
        ],
        "summary": "Handle vehicle deviation List",
        "description": "Handels vehicle deviation list by given JSON object, the following business processes are triggered: <br /><ul><li>create new vehicle deviation </li><li>updates existing vehicle deviation by given id </li><li>deletes vehicle deviation by given id</li></ul>",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/VehicleDeviationListDTO"
                }
              }
            },
            "application/json": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/VehicleDeviationListDTO"
                }
              }
            },
            "text/json": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/VehicleDeviationListDTO"
                }
              }
            },
            "application/*+json": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/VehicleDeviationListDTO"
                }
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/vehicle/get-vehicles": {
      "get": {
        "tags": [
          "MissionPlanning-API (Vehicles)"
        ],
        "summary": "Get vehicles list",
        "description": "Get vehicle data for all current vehicles",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/VehicleDataListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/vehicle/get-vehicle-by-vehiclenumber": {
      "get": {
        "tags": [
          "MissionPlanning-API (Vehicles)"
        ],
        "summary": "Get vehicle by vehiclenumber",
        "description": "Get vehicle data for vehicle with the given vehiclenumber",
        "parameters": [
          {
            "name": "vehicleNumber",
            "in": "query",
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/VehicleDataListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/vehicle/create-vehicle": {
      "post": {
        "tags": [
          "MissionPlanning-API (Vehicles)"
        ],
        "summary": "Create vehicle",
        "description": "Creates new vehicle by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateVehicleDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateVehicleDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateVehicleDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateVehicleDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/missionplanning/vehicle/update-vehicle": {
      "patch": {
        "tags": [
          "MissionPlanning-API (Vehicles)"
        ],
        "summary": "Update vehicle",
        "description": "Updates an existing vehicle by given JSON object",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateVehicleDTO"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateVehicleDTO"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateVehicleDTO"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateVehicleDTO"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/pbstacle-level/get-obstacle-levels": {
      "get": {
        "tags": [
          "ObstacleLevel"
        ],
        "summary": "Get obstacle levels",
        "description": "Get all obstacle levels defined by BMS masterdata",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsReasonForChangeDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/reason-for-change/get-reason-for-changes-by-process": {
      "get": {
        "tags": [
          "ReasonForChange"
        ],
        "summary": "Get reason of changes",
        "description": "Get all reason for changes defined by BMS masterdata",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsReasonForChangeDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/reason-for-change/get-reason-for-changes": {
      "get": {
        "tags": [
          "ReasonForChange"
        ],
        "summary": "Get reason of changes by process",
        "description": "Get all reason for changes defined by BMS masterdata, by given process flag",
        "parameters": [
          {
            "name": "bmsProcessFlag",
            "in": "query",
            "description": "Unknown (Process not knowen),  <br />\r\nWasteBinShift (Waste bin shift, for example change of size), <br />\r\nWasteBinDeploy (Set up a new waste bin), <br />\r\nWasteBinCollect (Collect existing waste bin),<br />\r\nWasteBinRegisterDamage (Register damage for waste bin),<br />\r\nWasteBinChangeObstacleLevel (Change the obstacle level for waste bin), <br />\r\nChangeGebOwner (Change the owner of location), <br />\r\nWasteBinChangeEmptyingCount (Change the empting count for waste bin),\r\nWasteBinDebitorSplit (Change the combinationen of debitors that are using the waste bin), <br />\r\nCanceled (Cancel change order),<br />\r\nHousingUnit(Change the housing unit of waste bin),<br />\r\nIndustryUnit (Change the industry unit of waste bin),<br />\r\nChangeGebOwnerStatus (Change the owner status of the location)",
            "schema": {
              "enum": [
                "unknown",
                "wasteBinShift",
                "wasteBinDeploy",
                "wasteBinCollect",
                "wasteBinRegisterDamage",
                "wasteBinChangeObstacleLevel",
                "changeGebOwner",
                "wasteBinChangeEmptyingCount",
                "wasteBinDebitorSplit",
                "canceled",
                "housingUnit",
                "industryUnit",
                "changeGebOwnerStatus"
              ],
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsReasonForChangeDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/signalR-notification/change": {
      "post": {
        "tags": [
          "SiganlR-API"
        ],
        "summary": "Notifies the clients if a change was made concering employees",
        "responses": {
          "200": {
            "description": "Success"
          }
        }
      }
    },
    "/api/bulkywaste/health-check": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/StringDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/contract-order/create": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Erstellt einen kompletten Sperrmülltermin von Vertrag bis Auftrag.",
        "requestBody": {
          "description": "Die Daten für die zu erstellenden Elemente.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BulkyWasteContractOrderCreateRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BulkyWasteContractOrderCreateRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BulkyWasteContractOrderCreateRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BulkyWasteContractOrderCreateRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/ObjectDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ObjectDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/ObjectDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/bulkywaste/contract-order/createByCrmTicket": {
      "post": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Creates BulkywasteOrder by given CRM-Ticket information.",
        "requestBody": {
          "description": "Needed data to create the BulkywasteOrder.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsBulkyWasteContractByCrmRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsBulkyWasteContractByCrmRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsBulkyWasteContractByCrmRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsBulkyWasteContractByCrmRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BulkyWasteContractByCrmTicketResponseModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BulkyWasteContractByCrmTicketResponseModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BulkyWasteContractByCrmTicketResponseModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BulkyWasteContractByCrmTicketResponseModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BulkyWasteContractByCrmTicketResponseModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BulkyWasteContractByCrmTicketResponseModelDataResponse"
                }
              }
            }
          }
        }
      }
    },
    "/api/bulkywaste/create-bulky-waste-contract": {
      "post": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Create bulky waste contract",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateBulkyWasteContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateBulkyWasteContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateBulkyWasteContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateBulkyWasteContractModel"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/Int32DataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-bulky-waste-contract/{contractId}": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get bulky waste contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsBulkyWasteContractDtoDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/update-bulky-waste-contract": {
      "put": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Update bulky waste contract",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateBulkyWasteContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateBulkyWasteContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateBulkyWasteContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateBulkyWasteContractModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BooleanDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/delete-bulky-waste-contract/{contractId}": {
      "delete": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get bulky waste contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "204": {
            "description": "No Content",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BooleanDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/cancel-and-create-new-bulky-waste-contract": {
      "put": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Cancel and create new bulky waste contract",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateBulkyWasteContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateBulkyWasteContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateBulkyWasteContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateBulkyWasteContractModel"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsBulkyWasteContractDtoDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-bulky-waste-items": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get bulky waste items",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsBulkyWasteItemDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-bulky-waste-tours": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get waste collection tours",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteTourDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-bulky-waste-waste-bins": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get waste bins",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteBinDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-bulky-waste-waste-service-catalogs": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get waste service catalogs",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteServiceCatalogDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-waste-bin-transponder-blacklist-validities": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get waste bin transponder blacklist validities",
        "requestBody": {
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/GetWasteBinTransponderBlacklistModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/GetWasteBinTransponderBlacklistModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/GetWasteBinTransponderBlacklistModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/GetWasteBinTransponderBlacklistModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteBinTransponderBlackListValidityDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-service-catalogs-from-bulky-waste-contract/{contractId}": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get bulky waste service catalogs from bulky waste contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteContractObjectDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-bulky-waste-appointments-from-contract/{contractId}": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get bulky waste appointments from contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteContractObjectAppointmentDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/bulkywaste/get-bulky-waste-appointment-price-validities-from-contract/{contractId}": {
      "get": {
        "tags": [
          "Sperrmüll-API"
        ],
        "summary": "Get bulky waste appointment price validities from contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteContractObjectAppointmentPriceValidityDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/street/cleaning/contract/contract-order/createByCrmTicket": {
      "post": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Creates StreetcleaningOrder by given CRM-Ticket information.",
        "requestBody": {
          "description": "Needed data to create the StreetcleaningOrder.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmTicketResponseModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmTicketResponseModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmTicketResponseModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/street/cleaning/contract/create": {
      "post": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Create street cleaning contract",
        "requestBody": {
          "description": "CreateWasteContractModel data",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateContractModel"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/street/cleaning/contract/update": {
      "put": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Update street cleaning contract",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateContractModel"
              }
            }
          }
        },
        "responses": {
          "204": {
            "description": "No Content"
          },
          "404": {
            "description": "Not Found",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/street/cleaning/contract/add/service-catalog-item": {
      "post": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Add a contract service catalog item",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/AddContractServiceCatalogItemModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/AddContractServiceCatalogItemModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/AddContractServiceCatalogItemModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/AddContractServiceCatalogItemModel"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created"
          },
          "404": {
            "description": "Not Found",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProblemDetails"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/streetcleaning/contract-order/createByCrmTicket": {
      "post": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Creates StreetcleaningOrder by given CRM-Ticket information.",
        "requestBody": {
          "description": "Needed data to create the StreetcleaningOrder.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmTicketResponseModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmTicketResponseModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmTicketResponseModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/streetcleaning/get-street-cleaning-service-catalogs": {
      "get": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Get street cleaning service catalogs",
        "responses": {
          "200": {
            "description": "Success"
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
          "500": {
            "description": "Server Error"
          }
        }
      }
    },
    "/api/streetcleaning/get-service-catalogs-from-street-cleaning-contract/{contractId}": {
      "get": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Get service catalogs from street cleaning contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCleaningContractObjectDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/streetcleaning/get-all-street-cleaning-activities": {
      "get": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Get all BmsActivityDto objects.",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsActivityDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/streetcleaning/get-street-cleaning-appointments-from-contract/{contractId}": {
      "get": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Get street cleaning appointments from contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCleaningContractObjectAppointmentDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/streetcleaning/get-street-cleaning-appointment-price-validities-from-contract/{contractId}": {
      "get": {
        "tags": [
          "Strassenreinigung-API"
        ],
        "summary": "Get street cleaning appointment price validities from contract",
        "parameters": [
          {
            "name": "contractId",
            "in": "path",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsCleaningContractObjectAppointmentPriceValidityDtoArrayDataResponse"
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
          "500": {
            "description": "Server Error",
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
    "/api/user/healthCheck": {
      "get": {
        "tags": [
          "User-API"
        ],
        "responses": {
          "200": {
            "description": "Success"
          }
        }
      }
    },
    "/api/user/authenticate/user": {
      "post": {
        "tags": [
          "User-API"
        ],
        "summary": "Authenticates a user.",
        "requestBody": {
          "description": "The data for authentication.",
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
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/TokenResponseModelDataResponse"
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
            "description": "Success",
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
            "description": "Server Error",
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
    },
    "/api/waste-bin-sizes/get-waste-bin-sizes": {
      "get": {
        "tags": [
          "WasteBinSizes"
        ],
        "summary": "Get waste bin sizes",
        "description": "Get all waste bin sizes defined by BMS masterdata",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteBinSizeDTODataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-bin-sizes/get-waste-bin-sizes-by-waste-type/{wasteTypeId}": {
      "get": {
        "tags": [
          "WasteBinSizes"
        ],
        "summary": "Get waste bin sizes by waste type",
        "description": "Get all waste bin sizes defined by given waste type id",
        "parameters": [
          {
            "name": "wasteTypeId",
            "in": "path",
            "description": "",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteBinSizeDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/set-up-waste-bin": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Set up a new waste bin",
        "description": "set up new waste bin by given JSON object, the following business processes are triggered: <br /><ul><li>create exchange order for set up </li><li>assigns location to waste bin </li><li>assigns customer to waste bin </li><li>assigns obstacle levels to waste bin </li><li>assigns indicator to waste bin </li><li>assigns tour to waste bin </li></ul><br /><br /> \r\n Parameterdescription\r\n<table><tr><td>Parametername</td><td>Description</td></tr><tr><td>deadline</td><td>Defines the planned date for setting up the waste bin.</td></tr><tr><td>wasteTypeId</td><td>Describes for which kind of waste the bin should be used (biological waste, plastic waste,...). Possible waste types could be requested on the endpoint \"Get waste types\".</td></tr><tr><td>wasteBinSizeID</td><td>Describes which size the waste the bin has. Possible waste bin sizes could be requested on the endpoint \"Get waste bin sizes\".</td></tr><tr><td>wasteBinEmptyingCycleID</td><td>Describes how often the waste bin should be emptied. Possible waste bin emptying cycles could be requested on the endpoint \"Get emptying cycles validities\".</td></tr><tr><td>reasonForChange</td><td>Describes the reason for changing/setting up the waste bin. Possible reasons for change could be requested on the endpoints \"Get reason of changes / by process\".</td></tr><tr><td>LocationId</td><td>Describes the location for setting up the waste bin. Possible endpoints for requesting the locations could be found under \"Common-API (AddLocation)\".</td></tr><tr><td>remark</td><td>Remark text for describing detailinformation.</td></tr><tr><td>isExtern</td><td>Describes whether the order for setting up the waste bin was external or internal.</td></tr><tr><td>hasExchangeFee</td><td>Describes if there is a fee to pay when the waste bin is setted up.</td></tr><tr><td>isDesiredDate</td><td>Describes if there is an desired date for setting up the waste bin.</td></tr><tr><td>isAutoSetCapNumberActive</td><td>Describes whether the waste bins capnumber should be genereated automatically or not.</td></tr><tr><td>capNumber</td><td>If not setted automatically the cap number could be trasfered in this parameter.</td></tr><tr><td>isLockable</td><td>Describes whether the waste bin is lockable or not.</td></tr><tr><td>lockRemark</td><td>If the waste bin is lockable, details could be transfered in this parameter.</td></tr><tr><td>locationInfo</td><td>If the are some special informations about the location, details could be transfered in this parameter.</td></tr><tr><td>bmsWasteBinLocationFlag</td><td>Decsribes the relation of waste bin and location. For example a waste bin could be related to an location only for loading or supply.</td></tr><tr><td>obstacleLevels</td><td>Describes the obstacle levels of the waste bin. For example if the waste bin must be fetched by the employess. Possible obstacle levels could be requested on the endpoint \"Get obstacle levels\".</td></tr><tr><td>wasteTourIds</td><td>Describes the tours that should be used for empting the waste bin. Possible endpoints for requesting the tours could be found under \"WasteCollection (Tour)\".</td></tr><tr><td>debitors</td><td>Describes the debitors of the waste bin. If not passed, all current owners of the location are added to the waste bin.  Possible endpoints for requesting the locations could be found under \"Common-API (Debitors)\".</td></tr><tr><td>wasteBinIndicatorId</td><td>Describes the indicator for setting up the waste bin. The indicator describes the combination of waste type, size and cycle. Possible indicators could be requested on the endpoint \"Get indicators\".</td></tr></table>",
        "requestBody": {
          "description": "",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/SetUpWasteBin"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/collect-waste-bin": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Collects the given waste bin from given location",
        "description": "collects a waste bin by given JSON object, the following business processes are triggered: <br /><ul><li>create exchange order for collection </li><li>sets the end date for location assignment to waste bin </li><li>sets the end date for customer assignment to waste bin </li><li>sets the end date for obstacle assignment levels to waste bin </li><li>sets the end date for indicator assignment to waste bin </li><li>sets the end date for tour assignment to waste bin </li></ul><br /><br /> \r\n Parameterdescription\r\n<table><tr><td>Parametername</td><td>Description</td></tr><tr><td>wasteBinId</td><td>BMS-Identifier of the waste bin, that should be collected.</td></tr><tr><td>reasonForChange</td><td>Describes the reason for changing/setting up the waste bin. Possible reasons for change could be requested on the endpoints \"Get reason of changes / by process\".</td></tr><tr><td>LocationId</td><td>Describes the location that the waste bin should be collected from.</td></tr><tr><td>deadline</td><td>Defines the planned date for collecting the waste bin.</td></tr><tr><td>remark</td><td>Remark text for describing detailinformation.</td></tr><tr><td>isExtern</td><td>Describes whether the order for collection the waste bin was external or internal.</td></tr><tr><td>hasExchangeFee</td><td>Describes if there is a fee to pay when the waste bin is collected.</td></tr><tr><td>isDesiredDate</td><td>Describes if there is an desired date for collecting the waste bin.</td></tr></table>",
        "requestBody": {
          "description": "",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CollectWasteBin"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/register-damage-for-waste-bin": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Registers a damage for the given waste bin",
        "description": "registers a damage for a waste bin by given JSON object, the following business processes are triggered: <br /><ul><li>create exchange order for damage rigistration </li></ul><br /><br /> \r\n Parameterdescription\r\n<table><tr><td>Parametername</td><td>Description</td></tr><tr><td>wasteBinId</td><td>BMS-Identifier of the waste bin, the damage schould be registrated for.</td></tr><tr><td>reasonForChange</td><td>Describes the reason for changing/setting up the waste bin. Possible reasons for change could be requested on the endpoints \"Get reason of changes / by process\".</td></tr><tr><td>damageTyeId</td><td>Describes the type of damage the waste bin has. Possible damageTypes could be requested on the endpoint \"get demage types\"</td></tr><tr><td>deadline</td><td>Defines the planned date for repairing the waste bin.</td></tr><tr><td>remark</td><td>Remark text for describing detailinformation.</td></tr><tr><td>isExtern</td><td>Describes whether the order for damage registration was external or internal.</td></tr><tr><td>hasExchangeFee</td><td>Describes if there is a fee to pay when the waste bin is repaired.</td></tr><tr><td>isDesiredDate</td><td>Describes if there is an desired date for repairing the waste bin.</td></tr></table>",
        "requestBody": {
          "description": "",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/RegisterDamageForWasteBin"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/change-obstacle-levels-for-waste-bin": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Change obstacle levels for the given waste bin",
        "description": "Changes the obstacle levels for a waste bin by given JSON object, existing obstacle levels where replaced by the new given obstacle levels.<br /> \r\nThe following business processes are triggered: <br /><ul><li>create exchange order for changing the obstacle level </li></ul><br /><br /> \r\n Parameterdescription\r\n<table><tr><td>Parametername</td><td>Description</td></tr><tr><td>wasteBinId</td><td>BMS-Identifier of the waste bin, the obstacle levels should be changed.</td></tr><tr><td>reasonForChange</td><td>Describes the reason for change order. Possible reasons for change could be requested on the endpoints \"Get reason of changes / by process\".</td></tr><tr><td>obstacleLevels</td><td>Describes the obstacle levels of the waste bin. For example if the waste bin must be fetched by the employess. Possible obstacle levels could be requested on the endpoint \"Get obstacle levels\".</td></tr><tr><td>deadline</td><td>Defines the planned date for changeing the waste bins obstacle levels.</td></tr><tr><td>remark</td><td>Remark text for describing detailinformation.</td></tr></table>",
        "requestBody": {
          "description": "",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ChangeObstacleLevelForWasteBin"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/change-emptying-cycle": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Change emptying cycle for the given waste bin",
        "description": "Changes the emptying cycle for a waste bin by given JSON object, existing emptying cycle where replaced by the new given emptying cycle.<br /> \r\nThe following business processes are triggered: <br /><ul><li>create exchange order for changing the emptying cycle </li></ul><br /><br /> \r\n Parameterdescription\r\n<table><tr><td>Parametername</td><td>Description</td></tr><tr><td>wasteBinId</td><td>BMS-Identifier of the waste bin, the emptying cycle should be changed.</td></tr><tr><td>wasteBinEmptyingCycleId</td><td>Possible id´s for change could be requested on the endpoints \"get-emptying-cycles-validities\".</td></tr><tr><td>reasonForChange</td><td>Describes the reason for change order. Possible reasons for change could be requested on the endpoints \"Get reason of changes / by process\".</td></tr><tr><td>deadline</td><td>Defines the planned date for changeing the waste bin emptying cycle.</td></tr><tr><td>remark</td><td>Remark text for describing detailinformation.</td></tr></table>",
        "requestBody": {
          "description": "",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ChangeEmptyingCycle"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/change-location-owner": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Change location owner",
        "description": "Changes waste bin relation of location to new debitor<br /> \r\nThe following business processes are triggered: <br /><ul><li>AddLocation owner shift</li></ul><br /><br /> \r\n Parameterdescription\r\n<table><tr><td>Parametername</td><td>Description</td></tr><tr><td>deadline</td><td>Defines the planned date for changeing the location owner.</td></tr><tr><td>currentDebitorNumber</td><td>The current Debitor Number\".</td></tr><tr><td>newDebitorNumber</td><td>The new Debitor Number\".</td></tr><tr><td>locationId</td><td>the location id which have to change the owner</td></tr><tr><td>reasonForChange</td><td>the reson of change id  \"Get reason of changes / by process\".</td></tr><tr><td>WasteBinUsageGroupID</td><td>Defines the waste bin usage group</td></tr><tr><td>NewBmsDebitorStatusType</td><td>Defines the type of Debitor</td></tr><tr><td>NewBmsDebitorStatusType</td><td>Defines the type of Debitor</td></tr><tr><td>NewPopulationCount</td><td>descripte the poulation inside Building</td></tr></table>",
        "requestBody": {
          "description": "",
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ChangeLocationOwner"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/shift-waste-bin": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Shift waste bin",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ShiftWasteBin"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/lock-transponder": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Lock Transponder",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/LockTransponder"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/unlock-transponder": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Unlock Transponder",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UnlockTransponder"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/change-transponder-validity": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Change Transponder Validity",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UnlockTransponder"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/IActionResultDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/get-transponder-blacklist-reasons": {
      "get": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Get List of possible transponder lock reasons",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/WasteBinTransponderBlackListReasonDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-collection/contract-order/createByCrmTicket": {
      "post": {
        "tags": [
          "WasteCollection"
        ],
        "summary": "Creates WasteCollectionOrder by given CRM-Ticket information.",
        "requestBody": {
          "description": "Needed data to create the WasteCollectionOrder.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsWasteCollectionOrderByCrmTicketRequestModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsWasteCollectionOrderByCrmTicketRequestModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/BmsWasteCollectionOrderByCrmTicketRequestModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/BmsWasteCollectionOrderByCrmTicketRequestModel"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteCollectionOrderByCrmTicketResponseModelListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/create": {
      "post": {
        "tags": [
          "WasteCollectionContract"
        ],
        "summary": "Create waste collection contract",
        "requestBody": {
          "description": "CreateWasteContractModel data",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateWasteContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateWasteContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateWasteContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/CreateWasteContractModel"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/update": {
      "put": {
        "tags": [
          "WasteCollectionContract"
        ],
        "summary": "Update waste collection contract",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWasteContractModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWasteContractModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWasteContractModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWasteContractModel"
              }
            }
          }
        },
        "responses": {
          "204": {
            "description": "No Content"
          },
          "404": {
            "description": "Not Found"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/add/service-catalog-item": {
      "post": {
        "tags": [
          "WasteCollectionContract"
        ],
        "summary": "Add a contract service catalog item",
        "requestBody": {
          "description": "",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/AddWasteContractServiceCatalogItemModel"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/AddWasteContractServiceCatalogItemModel"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/AddWasteContractServiceCatalogItemModel"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/AddWasteContractServiceCatalogItemModel"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created"
          },
          "404": {
            "description": "Not Found"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/add/service-object-singel-appointment": {
      "post": {
        "tags": [
          "WasteCollectionContract"
        ],
        "summary": "Add Contract Single Appointment",
        "requestBody": {
          "description": "ContractServiceObjectSingelAppointment",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/ContractServiceObjectSingelAppointment"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ContractServiceObjectSingelAppointment"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/ContractServiceObjectSingelAppointment"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/ContractServiceObjectSingelAppointment"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created"
          },
          "404": {
            "description": "Not Found"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/add/service-object-cycle-appointment": {
      "post": {
        "tags": [
          "WasteCollectionContract"
        ],
        "summary": "Add Contract Cycle Appointment",
        "requestBody": {
          "description": "ContractServiceObjectSingelAppointment",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/ContractServiceObjectCycleAppointment"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/ContractServiceObjectCycleAppointment"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/ContractServiceObjectCycleAppointment"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/ContractServiceObjectCycleAppointment"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created"
          },
          "404": {
            "description": "Not Found"
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
            "description": "Server Error"
          }
        }
      }
    },
    "/api/waste-type/get-waste-bin/{id}": {
      "get": {
        "tags": [
          "WasteType"
        ],
        "summary": "Get waste type by id",
        "description": "Get all waste type information by given ID",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "description": "waste type id",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/WasteBinDTODataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/waste-type/get-waste-types": {
      "get": {
        "tags": [
          "WasteType"
        ],
        "summary": "Get waste types",
        "description": "Get all waste bin types defined by BMS masterdata\r\nthis ist a test",
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BmsWasteTypeDTOListDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    "/api/wdw/status/update": {
      "post": {
        "tags": [
          "WDW-API"
        ],
        "summary": "Aktualisiert den Status einer WDW-Message.",
        "requestBody": {
          "description": "Die Daten für die zu erstellenden Elemente.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWdwStatusRequestModelIntern"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWdwStatusRequestModelIntern"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWdwStatusRequestModelIntern"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWdwStatusRequestModelIntern"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/UpdateWdwStatusRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/UpdateWdwStatusRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/UpdateWdwStatusRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
    },
    "/api/wdw/picture/update": {
      "post": {
        "tags": [
          "WDW-API"
        ],
        "summary": "Aktualisiert ein von WDW geliefertes Bild.",
        "requestBody": {
          "description": "Die Daten für die zu erstellenden Elemente.",
          "content": {
            "application/json-patch+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWdwStatusRequestModelIntern"
              }
            },
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWdwStatusRequestModelIntern"
              }
            },
            "text/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWdwStatusRequestModelIntern"
              }
            },
            "application/*+json": {
              "schema": {
                "$ref": "#/components/schemas/UpdateWdwStatusRequestModelIntern"
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Success",
            "content": {
              "text/plain": {
                "schema": {
                  "$ref": "#/components/schemas/UpdateWdwStatusRequestModelDataResponse"
                }
              },
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/UpdateWdwStatusRequestModelDataResponse"
                }
              },
              "text/json": {
                "schema": {
                  "$ref": "#/components/schemas/UpdateWdwStatusRequestModelDataResponse"
                }
              }
            }
          },
          "500": {
            "description": "Server Error",
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
      "AddContractServiceCatalogItemModel": {
        "required": [
          "contractId",
          "externalSystemId"
        ],
        "type": "object",
        "properties": {
          "contractId": {
            "type": "integer",
            "format": "int32"
          },
          "externalSystemId": {
            "minLength": 1,
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "AddContractServiceContainerTypeModel": {
        "required": [
          "contractContainerTypeItem",
          "contractId"
        ],
        "type": "object",
        "properties": {
          "contractId": {
            "type": "integer",
            "format": "int32"
          },
          "contractContainerTypeItem": {
            "$ref": "#/components/schemas/ContractContainerTypeItemModel"
          }
        },
        "additionalProperties": false
      },
      "AddWasteContractServiceCatalogItemModel": {
        "required": [
          "contractId",
          "contractServiceCatalogItem"
        ],
        "type": "object",
        "properties": {
          "contractId": {
            "type": "integer",
            "format": "int32"
          },
          "contractServiceCatalogItem": {
            "$ref": "#/components/schemas/ContractServiceCatalogItemModel"
          }
        },
        "additionalProperties": false
      },
      "AddressDTO": {
        "type": "object",
        "properties": {
          "locationId": {
            "type": "integer",
            "format": "int32"
          },
          "foreignId": {
            "type": "string",
            "nullable": true
          },
          "street": {
            "type": "string",
            "nullable": true
          },
          "housnumber": {
            "type": "string",
            "nullable": true
          },
          "city": {
            "type": "string",
            "nullable": true
          },
          "zipcode": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "AddressDTODataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/AddressDTO"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "AddressDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/AddressDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "ApplicationFieldDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "sortNumber": {
            "type": "integer",
            "format": "int32"
          },
          "color": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsActivityDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "isDeleted": {
            "type": "boolean"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "sapKey": {
            "type": "string",
            "nullable": true
          },
          "displayName": {
            "type": "string",
            "nullable": true
          },
          "vehiclePerSapOrder": {
            "type": "boolean"
          },
          "isOnlyOncePerSapOrder": {
            "type": "boolean"
          },
          "isVehicleActivity": {
            "type": "boolean"
          },
          "isVehicleRequired": {
            "type": "boolean"
          },
          "sapOrderNumber": {
            "type": "string",
            "nullable": true
          },
          "bmsCostLocationId": {
            "type": "integer",
            "format": "int32"
          },
          "employeeValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "vehicleValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "bulkyWasteTypeValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsActivityDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsActivityDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsBulkyWasteContractByCrmRequestModel": {
        "type": "object",
        "properties": {
          "bmsCrmTicketID": {
            "type": "integer",
            "format": "int32"
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "deadline": {
            "type": "string",
            "format": "date-time"
          }
        },
        "additionalProperties": false
      },
      "BmsBulkyWasteContractDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "isDeleted": {
            "type": "boolean"
          },
          "bmsCrmTicketID": {
            "type": "integer",
            "format": "int32"
          },
          "emailError": {
            "type": "string",
            "nullable": true
          },
          "emailDate": {
            "type": "string",
            "format": "date-time"
          },
          "printDate": {
            "type": "string",
            "format": "date-time"
          },
          "bmsBulkyWasteOrderMatchPlannedDate": {
            "type": "string",
            "format": "date-time"
          },
          "bmsBulkyWasteOrderPlannedDates": {
            "type": "string",
            "nullable": true
          },
          "furtherItemRemark": {
            "type": "string",
            "nullable": true
          },
          "postcardNumber": {
            "type": "string",
            "nullable": true
          },
          "remark": {
            "type": "string",
            "nullable": true
          },
          "itemInfo": {
            "type": "string",
            "nullable": true
          },
          "locationRemark": {
            "type": "string",
            "nullable": true
          },
          "remarkInternal": {
            "type": "string",
            "nullable": true
          },
          "calendarWeek": {
            "type": "integer",
            "format": "int32"
          },
          "bmsGeoObjectPointReferenceId": {
            "type": "integer",
            "format": "int32"
          },
          "contractAppointmentType": {
            "enum": [
              "none",
              "normal",
              "express",
              "matched"
            ],
            "type": "string"
          },
          "bmsLocationId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsBulkyWasteOnlineContractId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsDebitorId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsBulkyWasteContactTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCorePaymentMethodId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsEventId": {
            "type": "integer",
            "format": "int32"
          },
          "bulkyWasteContractItemValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "bulkyWasteContractServiceTypeValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "bulkyWasteOrderIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "bulkyWasteEmailLogIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsBulkyWasteContractDtoDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/BmsBulkyWasteContractDto"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsBulkyWasteItemDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "isDeleted": {
            "type": "boolean"
          },
          "bmsBulkyWasteTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsBulkyWasteTextBlockId": {
            "type": "integer",
            "format": "int32"
          },
          "isOnlineVisible": {
            "type": "boolean"
          },
          "isFeeRelevant": {
            "type": "boolean"
          },
          "onlineSortNumber": {
            "type": "integer",
            "format": "int32"
          },
          "onlineMaxItem": {
            "type": "integer",
            "format": "int32"
          },
          "onlineGroupOrder": {
            "type": "integer",
            "format": "int32"
          },
          "onlineGroupName": {
            "type": "string",
            "nullable": true
          },
          "isGroupingActivated": {
            "type": "boolean"
          },
          "onlineGroupColor": {
            "type": "string",
            "nullable": true
          },
          "bulkyWasteContractItemValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "bulkyWasteItemPointsValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsBulkyWasteItemDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsBulkyWasteItemDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsCleaningContractObjectAppointmentDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCleaningContractObjectId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsAppointmentType": {
            "enum": [
              "unknown",
              "single",
              "cycle"
            ],
            "type": "string"
          },
          "bmsTimePeriod": {
            "enum": [
              "month",
              "quater",
              "halfYear",
              "year"
            ],
            "type": "string",
            "nullable": true
          },
          "bmsBillingCategory": {
            "enum": [
              "fixedPrice",
              "effort",
              "free"
            ],
            "type": "string",
            "nullable": true
          },
          "timeFrameStart": {
            "type": "string",
            "format": "date-span",
            "nullable": true
          },
          "timeFrameEnd": {
            "type": "string",
            "format": "date-span",
            "nullable": true
          },
          "recurringDateAppointment": {
            "$ref": "#/components/schemas/BmsRecurringDateAppointmentDTO"
          },
          "deadline": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "isSettingBabg": {
            "type": "boolean"
          },
          "estimatedDuration": {
            "type": "number",
            "format": "double",
            "nullable": true
          },
          "sapOrderId": {
            "type": "string",
            "nullable": true
          },
          "cleaningOrderIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "priceValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsCleaningContractObjectAppointmentDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsCleaningContractObjectAppointmentDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsCleaningContractObjectAppointmentPriceValidityDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "validFrom": {
            "type": "string",
            "format": "date-time"
          },
          "validUntil": {
            "type": "string",
            "format": "date-time"
          },
          "estimatedDuration": {
            "type": "number",
            "format": "double",
            "nullable": true
          },
          "price": {
            "type": "number",
            "format": "double",
            "nullable": true
          },
          "unit": {
            "type": "string",
            "nullable": true
          },
          "bmsBillingCategory": {
            "enum": [
              "fixedPrice",
              "effort",
              "free"
            ],
            "type": "string",
            "nullable": true
          },
          "bmsValueAddedTaxId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsActivityId": {
            "type": "integer",
            "format": "int32"
          },
          "cleaningContractObjectAppointmentId": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "BmsCleaningContractObjectAppointmentPriceValidityDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsCleaningContractObjectAppointmentPriceValidityDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsCleaningContractObjectDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCleaningContractId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsPointObjectId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsLineObjectId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsAreaObjectId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsStreetPartId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsLocationId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsStreetCleaningTourId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCleaningServiceCatalogId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsTeamId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsEventId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCleaningTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContractMaintenanceActivityTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContractTechnicalPlaceId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsRecyclerId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsGarbageTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCrmTicketId": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "orderRemark": {
            "type": "string",
            "nullable": true
          },
          "innerOrderNumber": {
            "type": "string",
            "nullable": true
          },
          "innerOrderNumberScale": {
            "type": "string",
            "nullable": true
          },
          "costBillingCircle": {
            "type": "string",
            "nullable": true
          },
          "bmsSapOrderType": {
            "enum": [
              "unknown",
              "singleOrder",
              "standingOrder"
            ],
            "type": "string",
            "nullable": true
          },
          "bmsCleaningContractObjectAppointmentIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsCleaningContractObjectDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsCleaningContractObjectDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsContainerContractObjectAppointmentDto": {
        "type": "object",
        "properties": {
          "bmsContainerProcessType": {
            "enum": [
              "cycle",
              "new",
              "change",
              "emptying",
              "collect"
            ],
            "type": "string"
          },
          "bmsAppointmentType": {
            "enum": [
              "unknown",
              "single",
              "cycle"
            ],
            "type": "string"
          },
          "bmsTimePeriod": {
            "enum": [
              "month",
              "quater",
              "halfYear",
              "year"
            ],
            "type": "string",
            "nullable": true
          },
          "bmsBillingCategory": {
            "enum": [
              "fixedPrice",
              "effort",
              "free"
            ],
            "type": "string",
            "nullable": true
          },
          "cycleStart": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "cycleEnd": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "cycle": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "isWeekDayMonday": {
            "type": "boolean"
          },
          "isWeekDayTuesday": {
            "type": "boolean"
          },
          "isWeekDayWednesday": {
            "type": "boolean"
          },
          "isWeekDayThursday": {
            "type": "boolean"
          },
          "isWeekDayFriday": {
            "type": "boolean"
          },
          "isWeekDaySaturday": {
            "type": "boolean"
          },
          "isWeekDaySunday": {
            "type": "boolean"
          },
          "deadline": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "timeFrameStart": {
            "type": "string",
            "format": "date-span",
            "nullable": true
          },
          "timeFrameEnd": {
            "type": "string",
            "format": "date-span",
            "nullable": true
          },
          "estimatedDuration": {
            "type": "number",
            "format": "double",
            "nullable": true
          },
          "isSettingBabg": {
            "type": "boolean"
          },
          "sapOrderId": {
            "type": "string",
            "nullable": true
          },
          "externalDocumentNumber": {
            "type": "string",
            "nullable": true
          },
          "signatureRequired": {
            "type": "boolean"
          },
          "orderRemark": {
            "type": "string",
            "nullable": true
          },
          "isContainerCustomerOwned": {
            "type": "boolean"
          },
          "bmsContainerOrderIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "bmsContainerContractObjectAppointmentPriceValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsContainerContractObjectDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContainerContractId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContainerServiceCatalogId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsTeamId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsEventId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsLocationId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContainerTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContractMaintenanceActivityTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContractTechnicalPlaceId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsRecyclerId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsGarbageTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "locationRemark": {
            "type": "string",
            "nullable": true
          },
          "orderRemark": {
            "type": "string",
            "nullable": true
          },
          "hasDeliveryTicket": {
            "type": "boolean"
          },
          "bmsSapOrderType": {
            "enum": [
              "unknown",
              "singleOrder",
              "standingOrder"
            ],
            "type": "string",
            "nullable": true
          },
          "appointments": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsContainerContractObjectAppointmentDto"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsContainerContractObjectDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsContainerContractObjectDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsCrmAttachmentModel": {
        "type": "object",
        "properties": {
          "attachmentIdentifier": {
            "type": "string",
            "nullable": true
          },
          "attachmentBytes": {
            "type": "string",
            "nullable": true
          },
          "attachmentName": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsCrmTicketCommentRequestModel": {
        "type": "object",
        "properties": {
          "ticketId": {
            "type": "string",
            "nullable": true
          },
          "comment": {
            "type": "string",
            "nullable": true
          },
          "images": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsCrmAttachmentModel"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsCrmTicketCommentRequestModelDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/BmsCrmTicketCommentRequestModel"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsCrmTicketDTO": {
        "type": "object",
        "properties": {
          "date": {
            "type": "string",
            "nullable": true
          },
          "bmsCrmTicketID": {
            "type": "string",
            "nullable": true
          },
          "foreignId": {
            "type": "string",
            "nullable": true
          },
          "category": {
            "type": "string",
            "nullable": true
          },
          "issue": {
            "type": "string",
            "nullable": true
          },
          "backlink": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "debitor": {
            "type": "string",
            "nullable": true
          },
          "location": {
            "type": "string",
            "nullable": true
          },
          "street": {
            "type": "string",
            "nullable": true
          },
          "images": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsCrmAttachmentModel"
            },
            "nullable": true
          },
          "latitude": {
            "type": "number",
            "format": "double",
            "nullable": true
          },
          "longitude": {
            "type": "number",
            "format": "double",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsCrmTicketDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsCrmTicketDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsCrmTicketRequestModel": {
        "type": "object",
        "properties": {
          "foreignId": {
            "type": "string",
            "nullable": true
          },
          "category": {
            "type": "string",
            "nullable": true
          },
          "backlink": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "images": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsCrmAttachmentModel"
            },
            "nullable": true
          },
          "latitude": {
            "type": "number",
            "format": "double",
            "nullable": true
          },
          "longitude": {
            "type": "number",
            "format": "double",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsCrmTicketRequestModelDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/BmsCrmTicketRequestModel"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsCrmTicketTypeDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsCrmTicketTypeDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsCrmTicketTypeDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsDamageTypeDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "damageType": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsDamageTypeDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsDamageTypeDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsDebitorCustomerIdRelation": {
        "type": "object",
        "properties": {
          "debitorId": {
            "type": "string",
            "nullable": true
          },
          "customerId": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "isDeleted": {
            "type": "boolean"
          },
          "forceDelete": {
            "type": "boolean"
          },
          "creationTime": {
            "type": "string",
            "format": "date-time"
          },
          "createdByUser": {
            "type": "string",
            "nullable": true
          },
          "modificationTime": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "modifiedByUser": {
            "type": "string",
            "nullable": true
          },
          "uniqueIdentifier": {
            "type": "string",
            "format": "uuid",
            "readOnly": true
          },
          "versionTimestamp": {
            "type": "string",
            "format": "byte",
            "nullable": true,
            "readOnly": true
          },
          "skipSettingModifiedByUserOnNextSave": {
            "type": "boolean"
          },
          "id": {
            "type": "integer",
            "format": "int32",
            "readOnly": true
          },
          "isTransient": {
            "type": "boolean",
            "readOnly": true
          }
        },
        "additionalProperties": false
      },
      "BmsDebitorCustomerIdRelationDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/BmsDebitorCustomerIdRelation"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsLocationBmsDebitorDto": {
        "type": "object",
        "properties": {
          "bmsDebitorId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsLocationId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsLocationStreetDisplayName": {
            "type": "string",
            "nullable": true
          },
          "bmsDebitorStatusType": {
            "type": "integer",
            "format": "int32"
          },
          "validFrom": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "validUntil": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsLocationBmsDebitorDtoListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsLocationBmsDebitorDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsReasonForChangeDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsReasonForChangeDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsReasonForChangeDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsRecurringDateAppointmentDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "startDate": {
            "type": "string",
            "format": "date-time"
          },
          "recurrenceEndType": {
            "enum": [
              "none",
              "occurrenceCount",
              "date"
            ],
            "type": "string"
          },
          "occurrenceCount": {
            "type": "integer",
            "format": "int32"
          },
          "endDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "recurrenceType": {
            "enum": [
              "daily",
              "weekly",
              "monthly",
              "yearly"
            ],
            "type": "string"
          },
          "periodicity": {
            "type": "integer",
            "format": "int32"
          },
          "dailyRecurrenceOption": {
            "enum": [
              "everyDay",
              "weekdays"
            ],
            "type": "string"
          },
          "weeklyMonday": {
            "type": "boolean"
          },
          "weeklyTuesday": {
            "type": "boolean"
          },
          "weeklyWednesday": {
            "type": "boolean"
          },
          "weeklyThursday": {
            "type": "boolean"
          },
          "weeklyFriday": {
            "type": "boolean"
          },
          "weeklySaturday": {
            "type": "boolean"
          },
          "weeklySunday": {
            "type": "boolean"
          },
          "monthlyRecurrenceOption": {
            "enum": [
              "everyNDay",
              "everyNWeekOfMonth"
            ],
            "type": "string"
          },
          "dayNumber": {
            "type": "integer",
            "format": "int32"
          },
          "weekOfMonth": {
            "enum": [
              "none",
              "first",
              "second",
              "third",
              "fourth",
              "last"
            ],
            "type": "string"
          },
          "weekDaysOfMonth": {
            "enum": [
              "sunday",
              "monday",
              "tuesday",
              "wednesday",
              "thursday",
              "friday",
              "workDays",
              "saturday",
              "weekendDays",
              "everyDay"
            ],
            "type": "string"
          },
          "yearlyRecurrenceOption": {
            "enum": [
              "dayOfMonth",
              "weekOfMonth"
            ],
            "type": "string"
          },
          "month": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "BmsStreetCleaningContractByCrmRequestModel": {
        "type": "object",
        "properties": {
          "bmsCrmTicketID": {
            "type": "integer",
            "format": "int32"
          },
          "bmsTeamID": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "bmsCrmTicketMeasureID": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "orderRemark": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsStreetCleaningContractByCrmTicketResponseModel": {
        "type": "object",
        "properties": {
          "bmsStreetCleaningContractID": {
            "type": "integer",
            "format": "int32"
          },
          "contractWithOrderCreateResult": {
            "enum": [
              "ok",
              "locationNotFound",
              "debitorNotFound",
              "contactTypeNotFound",
              "contractItemsNotFound",
              "wasteTypeNotFound",
              "districtNotFound",
              "teamNotFound",
              "bmsBulkyWasteItemNotFound",
              "bmsStreetPartNotFound",
              "bmsCrmTicketNotFound",
              "bmsTeamNotFound",
              "cleaningContractNotFound"
            ],
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "BmsStreetCleaningContractByCrmTicketResponseModelDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/BmsStreetCleaningContractByCrmTicketResponseModel"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteBinDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "isDeleted": {
            "type": "boolean"
          },
          "identName": {
            "type": "string",
            "nullable": true
          },
          "lastEmptying": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "cycleAsText": {
            "type": "string",
            "nullable": true
          },
          "capNumber": {
            "type": "string",
            "nullable": true
          },
          "factoryNumber": {
            "type": "string",
            "nullable": true
          },
          "locationInfo": {
            "type": "string",
            "nullable": true
          },
          "remark": {
            "type": "string",
            "nullable": true
          },
          "lockRemark": {
            "type": "string",
            "nullable": true
          },
          "indicator": {
            "type": "string",
            "nullable": true
          },
          "isLockable": {
            "type": "boolean"
          },
          "isService": {
            "type": "boolean"
          },
          "isBusiness": {
            "type": "boolean"
          },
          "sapId": {
            "type": "string",
            "nullable": true
          },
          "sapUser": {
            "type": "string",
            "nullable": true
          },
          "sapCreationTime": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "sapWebServiceCallId": {
            "type": "string",
            "nullable": true
          },
          "wasteTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "wasteBinSizeId": {
            "type": "integer",
            "format": "int32"
          },
          "locationValidities": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "debitorValidities": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "obstacleLevelValidities": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "wasteTourValidities": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "identNameValidities": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "serviceIdentNameValidities": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "oldChangeOrders": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "newChangeOrders": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "emptyingCycleCountValidities": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "indicatorValidities": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteBinDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteBinDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteBinEmptingCycleValidityDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "cycleAsText": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteBinEmptingCycleValidityDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteBinEmptingCycleValidityDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteBinSizeAccessoriesDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "price": {
            "type": "number",
            "format": "double"
          },
          "accessoriesGroup": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteBinSizeDTO": {
        "required": [
          "id",
          "value"
        ],
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "value": {
            "minLength": 1,
            "type": "string"
          },
          "base64Image": {
            "type": "string",
            "nullable": true
          },
          "imageMimeType": {
            "type": "string",
            "nullable": true
          },
          "width": {
            "type": "integer",
            "format": "int32"
          },
          "height": {
            "type": "integer",
            "format": "int32"
          },
          "depth": {
            "type": "integer",
            "format": "int32"
          },
          "size": {
            "type": "integer",
            "format": "int32"
          },
          "price": {
            "type": "number",
            "format": "double"
          },
          "accessories": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteBinSizeAccessoriesDTO"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteBinSizeDTODataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/BmsWasteBinSizeDTO"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteBinSizeDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteBinSizeDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteBinTransponderBlackListValidityDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "bmsWasteBinTransponderBlackListReasonId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsLocationId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsWasteBinId": {
            "type": "integer",
            "format": "int32"
          },
          "identName": {
            "type": "string",
            "nullable": true
          },
          "defaultExportNumber": {
            "type": "string",
            "nullable": true
          },
          "validFrom": {
            "type": "string",
            "format": "date-time"
          },
          "validUntil": {
            "type": "string",
            "format": "date-time"
          }
        },
        "additionalProperties": false
      },
      "BmsWasteBinTransponderBlackListValidityDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteBinTransponderBlackListValidityDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteCollectionOrderByCrmTicketRequestModel": {
        "type": "object",
        "properties": {
          "bmsCrmTicketID": {
            "type": "integer",
            "format": "int32"
          },
          "bmsTeamID": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "bmsWasteContractID": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "bmsCrmTicketMeasureID": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "orderRemark": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteCollectionOrderByCrmTicketResponseModel": {
        "type": "object",
        "properties": {
          "bmsWasteOrderID": {
            "type": "integer",
            "format": "int32"
          },
          "contractWithOrderCreateResult": {
            "enum": [
              "ok",
              "locationNotFound",
              "debitorNotFound",
              "contactTypeNotFound",
              "contractItemsNotFound",
              "wasteTypeNotFound",
              "districtNotFound",
              "teamNotFound",
              "bmsBulkyWasteItemNotFound",
              "bmsStreetPartNotFound",
              "bmsCrmTicketNotFound",
              "bmsTeamNotFound",
              "cleaningContractNotFound"
            ],
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "BmsWasteCollectionOrderByCrmTicketResponseModelListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteCollectionOrderByCrmTicketResponseModel"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteContractObjectAppointmentDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "bmsAppointmentType": {
            "enum": [
              "unknown",
              "single",
              "cycle"
            ],
            "type": "string"
          },
          "cycleStart": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "cycleEnd": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "cycle": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "weekDayMonday": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "weekDayTuesday": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "weekDayWednesday": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "weekDayThursday": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "weekDayFriday": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "weekDaySaturday": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "weekDaySunday": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "deadline": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "sapOrderId": {
            "type": "string",
            "nullable": true
          },
          "bmsTimePeriod": {
            "enum": [
              "month",
              "quater",
              "halfYear",
              "year"
            ],
            "type": "string",
            "nullable": true
          },
          "bmsBillingCategory": {
            "enum": [
              "fixedPrice",
              "effort",
              "free"
            ],
            "type": "string",
            "nullable": true
          },
          "bmsWasteContractObjectId": {
            "type": "integer",
            "format": "int32"
          },
          "isSettingBabg": {
            "type": "boolean"
          }
        },
        "additionalProperties": false
      },
      "BmsWasteContractObjectAppointmentDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteContractObjectAppointmentDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteContractObjectAppointmentPriceValidityDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "validFrom": {
            "type": "string",
            "format": "date-time"
          },
          "validUntil": {
            "type": "string",
            "format": "date-time"
          },
          "price": {
            "type": "number",
            "format": "double",
            "nullable": true
          },
          "unit": {
            "type": "string",
            "nullable": true
          },
          "bmsBillingCategory": {
            "enum": [
              "fixedPrice",
              "effort",
              "free"
            ],
            "type": "string",
            "nullable": true
          },
          "bmsValueAddedTaxId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsActivityId": {
            "type": "integer",
            "format": "int32"
          },
          "estimatedDuration": {
            "type": "number",
            "format": "double",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteContractObjectAppointmentPriceValidityDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteContractObjectAppointmentPriceValidityDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteContractObjectDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "orderRemark": {
            "type": "string",
            "nullable": true
          },
          "bmsWasteContractId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsWasteTourId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsWasteBinId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsWasteServiceCatalogId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsTeamId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContractMaintenanceActivityTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsContractTechnicalPlaceId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsSapOrderType": {
            "enum": [
              "unknown",
              "singleOrder",
              "standingOrder"
            ],
            "type": "string",
            "nullable": true
          },
          "bmsRecyclerId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsGarbageTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "costBillingCircle": {
            "type": "string",
            "nullable": true
          },
          "innerOrderNumber": {
            "type": "string",
            "nullable": true
          },
          "innerOrderNumberScale": {
            "type": "string",
            "nullable": true
          },
          "bmsEventId": {
            "type": "integer",
            "format": "int32"
          },
          "wasteContractObjectAppointments": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteContractObjectAppointmentDto"
            },
            "nullable": true
          },
          "wasteContractObjectAppointmentPriceValidities": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteContractObjectAppointmentPriceValidityDto"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteContractObjectDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteContractObjectDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteServiceCatalogDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "isDeleted": {
            "type": "boolean"
          },
          "contractObjectIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "priceValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteServiceCatalogDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteServiceCatalogDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteTourDto": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "isDeleted": {
            "type": "boolean"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "payloadByVehicle": {
            "type": "number",
            "format": "double",
            "nullable": true
          },
          "approachTime": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "clearanceDriveTime": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "latencyTime": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "returnTime": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "cycle": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "runsPerCycle": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "cycleStartDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "sortNumber": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "inactiveSince": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "bmsWasteTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsWasteTerritoryId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsLiftingSystemId": {
            "type": "integer",
            "format": "int32"
          },
          "changeOrderTourName": {
            "type": "string",
            "nullable": true
          },
          "roadMapColor": {
            "type": "string",
            "nullable": true
          },
          "wasteBinValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "streetPartValidityIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteTourDtoArrayDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteTourDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BmsWasteTypeDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "wasteType": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BmsWasteTypeDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteTypeDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BooleanDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "boolean",
            "description": "Returns the data of the response."
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BulkyWasteContractByCrmTicketResponseModel": {
        "type": "object",
        "properties": {
          "bmsBulkyWasteContractID": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "bmsBulkyWasteOrderID": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "contractWithOrderCreateResult": {
            "enum": [
              "ok",
              "locationNotFound",
              "debitorNotFound",
              "contactTypeNotFound",
              "contractItemsNotFound",
              "wasteTypeNotFound",
              "districtNotFound",
              "teamNotFound",
              "bmsBulkyWasteItemNotFound",
              "bmsStreetPartNotFound",
              "bmsCrmTicketNotFound",
              "bmsTeamNotFound",
              "cleaningContractNotFound"
            ],
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "BulkyWasteContractByCrmTicketResponseModelDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/BulkyWasteContractByCrmTicketResponseModel"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BulkyWasteContractOrderCreateRequestModel": {
        "type": "object",
        "properties": {
          "debitorID": {
            "type": "integer",
            "format": "int32"
          },
          "locationID": {
            "type": "integer",
            "format": "int32"
          },
          "contactTypeID": {
            "type": "integer",
            "format": "int32"
          },
          "contractItems": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/IdAndAmountModel"
            },
            "nullable": true
          },
          "wasteTypeID": {
            "type": "integer",
            "format": "int32"
          },
          "districtID": {
            "type": "integer",
            "format": "int32"
          },
          "teamID": {
            "type": "integer",
            "format": "int32"
          },
          "plannedDate": {
            "type": "string",
            "format": "date-time"
          }
        },
        "additionalProperties": false
      },
      "BulkyWasteItemsGroupByOnlineGroupNameDTO": {
        "type": "object",
        "properties": {
          "section-headline": {
            "type": "string",
            "nullable": true
          },
          "section-type": {
            "type": "string",
            "nullable": true
          },
          "product-list-head": {
            "$ref": "#/components/schemas/ProductListHead"
          },
          "elements": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/Element"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "BulkyWasteItemsGroupByOnlineGroupNameDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BulkyWasteItemsGroupByOnlineGroupNameDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "BulkyWasteOnlineContractFormData": {
        "type": "object",
        "properties": {
          "salution": {
            "type": "string",
            "nullable": true
          },
          "surname": {
            "type": "string",
            "nullable": true
          },
          "lastname": {
            "type": "string",
            "nullable": true
          },
          "phone": {
            "type": "string",
            "nullable": true
          },
          "email": {
            "type": "string",
            "nullable": true
          },
          "email-confirm": {
            "type": "string",
            "nullable": true
          },
          "street": {
            "type": "string",
            "nullable": true
          },
          "housenumber": {
            "type": "string",
            "nullable": true
          },
          "postcode": {
            "type": "string",
            "nullable": true
          },
          "city": {
            "type": "string",
            "nullable": true
          },
          "location-remark": {
            "type": "string",
            "nullable": true
          },
          "location-id": {
            "type": "string",
            "nullable": true
          },
          "pick-up-location-street": {
            "type": "string",
            "nullable": true
          },
          "pick-up-location-housenumber": {
            "type": "string",
            "nullable": true
          },
          "pick-up-location-postcode": {
            "type": "string",
            "nullable": true
          },
          "pick-up-location-city": {
            "type": "string",
            "nullable": true
          },
          "pick-up-location-id": {
            "type": "string",
            "nullable": true
          },
          "pick-up-date": {
            "type": "string",
            "nullable": true
          },
          "items": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/Item"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "ChangeEmptyingCycle": {
        "required": [
          "deadline",
          "reasonForChangeID",
          "remark",
          "wasteBinEmptyingCycleId",
          "wasteTypeId"
        ],
        "type": "object",
        "properties": {
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "wasteTypeId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "wasteBinEmptyingCycleId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "reasonForChangeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "remark": {
            "minLength": 1,
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "ChangeLocationOwner": {
        "required": [
          "currentDebitorNumber",
          "deadline",
          "locationId",
          "NewBmsDebitorStatusType",
          "newDebitorNumber",
          "reasonForChangeID",
          "WasteBinUsageGroupID"
        ],
        "type": "object",
        "properties": {
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "currentDebitorNumber": {
            "minLength": 1,
            "type": "string"
          },
          "newDebitorNumber": {
            "minLength": 1,
            "type": "string"
          },
          "locationId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "reasonForChangeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "WasteBinUsageGroupID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "NewBmsDebitorStatusType": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "NewPopulationCount": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "ChangeObstacleLevelForWasteBin": {
        "required": [
          "deadline",
          "reasonForChangeID",
          "remark",
          "wasteBinId"
        ],
        "type": "object",
        "properties": {
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "wasteBinId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "reasonForChangeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "remark": {
            "minLength": 1,
            "type": "string"
          },
          "obstacleLevels": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CheckDebitorExists": {
        "required": [
          "activationCode",
          "debitorId"
        ],
        "type": "object",
        "properties": {
          "debitorId": {
            "minLength": 1,
            "type": "string"
          },
          "activationCode": {
            "minLength": 1,
            "type": "string"
          },
          "salesTaxIdentificationNumber": {
            "type": "string",
            "nullable": true
          },
          "birthdate": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CityDistrictDTO": {
        "type": "object",
        "properties": {
          "zipCode": {
            "type": "string",
            "nullable": true
          },
          "city": {
            "type": "string",
            "nullable": true
          },
          "cityId": {
            "type": "integer",
            "format": "int32"
          },
          "district": {
            "type": "string",
            "nullable": true
          },
          "districtId": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "CityDistrictDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/CityDistrictDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "CollectWasteBin": {
        "required": [
          "deadline",
          "locationId",
          "reasonForChangeID",
          "wasteBinId"
        ],
        "type": "object",
        "properties": {
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "wasteBinId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "reasonForChangeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "locationId": {
            "type": "integer",
            "format": "int32"
          },
          "remark": {
            "type": "string",
            "nullable": true
          },
          "isExtern": {
            "type": "boolean",
            "default": false
          },
          "hasExchangeFee": {
            "type": "boolean"
          },
          "isDesiredDate": {
            "type": "boolean"
          }
        },
        "additionalProperties": false
      },
      "ContainerContractAppointment": {
        "required": [
          "AppointmentDate",
          "BmsAppointmentType",
          "ContainerObjectId",
          "ContainerProcessType",
          "ContractId"
        ],
        "type": "object",
        "properties": {
          "ContractId": {
            "type": "integer",
            "format": "int32"
          },
          "ContainerObjectId": {
            "type": "integer",
            "format": "int32"
          },
          "AppointmentDate": {
            "type": "string",
            "format": "date-time"
          },
          "ContainerProcessType": {
            "enum": [
              "cycle",
              "new",
              "change",
              "emptying",
              "collect"
            ],
            "type": "string"
          },
          "BmsAppointmentType": {
            "enum": [
              "unknown",
              "single",
              "cycle"
            ],
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "ContractContainerTypeItemModel": {
        "type": "object",
        "properties": {
          "Id": {
            "type": "integer",
            "format": "int32"
          },
          "ExternalSystemId": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "ContractServiceCatalogItemModel": {
        "type": "object",
        "properties": {
          "Id": {
            "type": "integer",
            "format": "int32"
          },
          "ExternalSystemId": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "ContractServiceObjectCycleAppointment": {
        "required": [
          "bmsAppointmentType"
        ],
        "type": "object",
        "properties": {
          "CyleStart": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "CyleEnd": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "IsSettingBabg": {
            "type": "boolean"
          },
          "Monday": {
            "maximum": 9,
            "minimum": 0,
            "type": "integer",
            "format": "int32"
          },
          "Tuesday": {
            "maximum": 9,
            "minimum": 0,
            "type": "integer",
            "format": "int32"
          },
          "Wednesday": {
            "maximum": 9,
            "minimum": 0,
            "type": "integer",
            "format": "int32"
          },
          "Thursday": {
            "maximum": 9,
            "minimum": 0,
            "type": "integer",
            "format": "int32"
          },
          "Friday": {
            "maximum": 9,
            "minimum": 0,
            "type": "integer",
            "format": "int32"
          },
          "´Saturday": {
            "maximum": 9,
            "minimum": 0,
            "type": "integer",
            "format": "int32"
          },
          "´Sunday": {
            "maximum": 9,
            "minimum": 0,
            "type": "integer",
            "format": "int32"
          },
          "ServiceObjectID": {
            "type": "integer",
            "format": "int32"
          },
          "ExternalSystemId": {
            "type": "string",
            "nullable": true
          },
          "bmsAppointmentType": {
            "enum": [
              "unknown",
              "single",
              "cycle"
            ],
            "type": "string"
          },
          "SapOrderId": {
            "type": "string",
            "nullable": true
          },
          "BmsPeriodType": {
            "enum": [
              "month",
              "quater",
              "halfYear",
              "year"
            ],
            "type": "string",
            "nullable": true
          },
          "BmsBillingCategory": {
            "enum": [
              "fixedPrice",
              "effort",
              "free"
            ],
            "type": "string",
            "nullable": true
          },
          "TimeFrameStart": {
            "type": "string",
            "format": "date-span",
            "nullable": true
          },
          "TimeFrameEnd": {
            "type": "string",
            "format": "date-span",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "ContractServiceObjectSingelAppointment": {
        "required": [
          "bmsAppointmentType"
        ],
        "type": "object",
        "properties": {
          "Deadline": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "IsSettingBabg": {
            "type": "boolean"
          },
          "ServiceObjectID": {
            "type": "integer",
            "format": "int32"
          },
          "ExternalSystemId": {
            "type": "string",
            "nullable": true
          },
          "bmsAppointmentType": {
            "enum": [
              "unknown",
              "single",
              "cycle"
            ],
            "type": "string"
          },
          "SapOrderId": {
            "type": "string",
            "nullable": true
          },
          "BmsPeriodType": {
            "enum": [
              "month",
              "quater",
              "halfYear",
              "year"
            ],
            "type": "string",
            "nullable": true
          },
          "BmsBillingCategory": {
            "enum": [
              "fixedPrice",
              "effort",
              "free"
            ],
            "type": "string",
            "nullable": true
          },
          "TimeFrameStart": {
            "type": "string",
            "format": "date-span",
            "nullable": true
          },
          "TimeFrameEnd": {
            "type": "string",
            "format": "date-span",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CountryDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "countryName": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "countryCode": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CountryDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/CountryDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "CreateBulkyWasteContractModel": {
        "required": [
          "ContactTypeId",
          "DebitorId",
          "LocationId"
        ],
        "type": "object",
        "properties": {
          "CalendarWeek": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "ContactTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "LocationId": {
            "type": "integer",
            "format": "int32"
          },
          "DebitorId": {
            "type": "integer",
            "format": "int32"
          },
          "PaymentMethodId": {
            "type": "integer",
            "format": "int32"
          },
          "EventId": {
            "type": "integer",
            "format": "int32"
          },
          "LocationRemark": {
            "type": "string",
            "nullable": true
          },
          "RemarkInternal": {
            "type": "string",
            "nullable": true
          },
          "ItemInfo": {
            "type": "string",
            "nullable": true
          },
          "FurtherItemRemark": {
            "type": "string",
            "nullable": true
          },
          "Remark": {
            "type": "string",
            "nullable": true
          },
          "Items": {
            "type": "object",
            "additionalProperties": {
              "type": "integer",
              "format": "int32",
              "nullable": true
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateCitizenConcernsCrmTicket": {
        "type": "object",
        "properties": {
          "debitorId": {
            "type": "string",
            "nullable": true
          },
          "firstname": {
            "type": "string",
            "nullable": true
          },
          "lastname": {
            "type": "string",
            "nullable": true
          },
          "phone": {
            "type": "string",
            "nullable": true
          },
          "email": {
            "type": "string",
            "nullable": true
          },
          "titel": {
            "type": "string",
            "nullable": true
          },
          "concerns": {
            "type": "string",
            "nullable": true
          },
          "desiredTimeWindow": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "typeOfConcern": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCrmTicketTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCrmTicketEntryTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCrmTicketCategoryId": {
            "type": "integer",
            "format": "int32"
          },
          "bmsCrmTicketIssueId": {
            "type": "integer",
            "format": "int32"
          },
          "reporter": {
            "type": "integer",
            "format": "int32"
          },
          "invoiceNumber": {
            "type": "string",
            "nullable": true
          },
          "locationId": {
            "type": "integer",
            "format": "int32"
          },
          "fractionId": {
            "type": "integer",
            "format": "int32"
          },
          "containerId": {
            "type": "integer",
            "format": "int32"
          },
          "changeoverDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "base64Images": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateContractModel": {
        "required": [
          "Description"
        ],
        "type": "object",
        "properties": {
          "Description": {
            "minLength": 1,
            "type": "string"
          },
          "DebitorNumber": {
            "type": "string",
            "nullable": true
          },
          "DebitorExternalSystemId": {
            "type": "string",
            "nullable": true
          },
          "OrderReferenceNumber": {
            "type": "string",
            "nullable": true
          },
          "InvoiceRemark": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateDailyOverviewMonitorDTO": {
        "required": [
          "deadline",
          "includeWorkSchedules",
          "selectedApplicationFieldIds",
          "showEmployeeFunctions",
          "showEmptyApplicationFields",
          "showEmptyTeams"
        ],
        "type": "object",
        "properties": {
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "selectedApplicationFieldIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            }
          },
          "showEmptyTeams": {
            "type": "boolean"
          },
          "showEmptyApplicationFields": {
            "type": "boolean"
          },
          "showEmployeeFunctions": {
            "type": "boolean"
          },
          "includeWorkSchedules": {
            "type": "boolean"
          }
        },
        "additionalProperties": false
      },
      "CreateDeviationDTO": {
        "required": [
          "name"
        ],
        "type": "object",
        "properties": {
          "name": {
            "minLength": 1,
            "type": "string"
          },
          "description": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateEmployeeDTO": {
        "required": [
          "employeeNumber",
          "firstName",
          "lastName"
        ],
        "type": "object",
        "properties": {
          "employeeNumber": {
            "minLength": 1,
            "type": "string"
          },
          "firstName": {
            "minLength": 1,
            "type": "string"
          },
          "lastName": {
            "minLength": 1,
            "type": "string"
          },
          "salutation": {
            "type": "string",
            "nullable": true
          },
          "entranceDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "exitDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateEmployeeWithSapBtpDTO": {
        "type": "object",
        "properties": {
          "Mitarbeiternummer": {
            "type": "string",
            "nullable": true
          },
          "Anrede": {
            "type": "string",
            "nullable": true
          },
          "Name": {
            "type": "string",
            "nullable": true
          },
          "Vorname": {
            "type": "string",
            "nullable": true
          },
          "Firma/Gesellschaft": {
            "type": "string",
            "nullable": true
          },
          "Geburtstag": {
            "type": "string",
            "nullable": true
          },
          "Eintrittsdatum": {
            "type": "string",
            "nullable": true
          },
          "Austrittsdatum": {
            "type": "string",
            "nullable": true
          },
          "Funktion": {
            "type": "string",
            "nullable": true
          },
          "Urlaubstage": {
            "type": "integer",
            "format": "int32"
          },
          "Organisationseinheit": {
            "type": "string",
            "nullable": true
          },
          "Arbeitszeit": {
            "type": "number",
            "format": "double"
          },
          "Initialen": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateEmployeesWithSapBtpDTO": {
        "type": "object",
        "properties": {
          "employees": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/CreateEmployeeWithSapBtpDTO"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateVehicleDTO": {
        "required": [
          "licensePlate",
          "vehicleNumber",
          "vehicleTypeID"
        ],
        "type": "object",
        "properties": {
          "vehicleNumber": {
            "minLength": 1,
            "type": "string"
          },
          "licensePlate": {
            "minLength": 1,
            "type": "string"
          },
          "vehicleTypeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "vehicleIdentificationNumber": {
            "type": "string",
            "nullable": true
          },
          "makeType": {
            "type": "string",
            "nullable": true
          },
          "annex": {
            "type": "string",
            "nullable": true
          },
          "equipmentNumber": {
            "type": "string",
            "nullable": true
          },
          "driverLicenseClass": {
            "type": "string",
            "nullable": true
          },
          "vehicleLocation": {
            "type": "string",
            "nullable": true
          },
          "buildYear": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "unregisteredDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateVehicleTypeDTO": {
        "required": [
          "name"
        ],
        "type": "object",
        "properties": {
          "name": {
            "minLength": 1,
            "type": "string"
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "externalSystemID": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "CreateWasteContractModel": {
        "required": [
          "Description"
        ],
        "type": "object",
        "properties": {
          "Description": {
            "minLength": 1,
            "type": "string"
          },
          "DebitorNumber": {
            "type": "string",
            "nullable": true
          },
          "DebitorExternalSystemId": {
            "type": "string",
            "nullable": true
          },
          "InvoiceRemark": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DailyOverviewData": {
        "type": "object",
        "properties": {
          "applicationFields": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/ApplicationFieldDTO"
            },
            "nullable": true
          },
          "teams": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/TeamDTO"
            },
            "nullable": true
          },
          "resources": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/TeamResourceDTO"
            },
            "nullable": true
          },
          "newsticker": {
            "type": "string",
            "nullable": true
          },
          "hideJumperSymbol": {
            "type": "boolean"
          },
          "useLargeLetters": {
            "type": "boolean"
          },
          "enableOrderDisplay": {
            "type": "boolean"
          }
        },
        "additionalProperties": false
      },
      "DailyOverviewDataListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DailyOverviewData"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "DataAttributes": {
        "type": "object",
        "properties": {
          "type": {
            "type": "string",
            "nullable": true
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "data-group": {
            "type": "string",
            "nullable": true
          },
          "id": {
            "type": "string",
            "nullable": true
          },
          "min": {
            "type": "string",
            "nullable": true
          },
          "max": {
            "type": "string",
            "nullable": true
          },
          "step": {
            "type": "string",
            "nullable": true
          },
          "value": {
            "type": "string",
            "nullable": true
          },
          "price": {
            "type": "string",
            "nullable": true
          },
          "onkeydown": {
            "type": "string",
            "nullable": true
          },
          "onchange": {
            "type": "string",
            "nullable": true
          },
          "bulky-waste-type": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "DatabaseScriptResponseModel": {
        "type": "object",
        "properties": {
          "script": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DatabaseScriptResponseModelDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/DatabaseScriptResponseModel"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "Debitor": {
        "required": [
          "firstname",
          "lastname"
        ],
        "type": "object",
        "properties": {
          "id": {
            "type": "string",
            "nullable": true
          },
          "debitorType": {
            "enum": [
              "default",
              "isSapCoreDebitor",
              "isSapWasteDebitor",
              "isThirdParty"
            ],
            "type": "string"
          },
          "isIndividualPerson": {
            "type": "boolean"
          },
          "titleId": {
            "type": "integer",
            "format": "int32"
          },
          "firstname": {
            "minLength": 1,
            "type": "string"
          },
          "lastname": {
            "minLength": 1,
            "type": "string"
          },
          "phoneNumber": {
            "type": "string",
            "nullable": true
          },
          "poBox": {
            "type": "string",
            "nullable": true
          },
          "salesTaxIdentificationNumber": {
            "type": "string",
            "nullable": true
          },
          "email": {
            "type": "string",
            "nullable": true
          },
          "information": {
            "type": "string",
            "nullable": true
          },
          "relationId": {
            "type": "string",
            "nullable": true
          },
          "birthdate": {
            "type": "string",
            "nullable": true
          },
          "debitorAddresses": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DebitorAddress"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DebitorAddress": {
        "required": [
          "city",
          "houseNumber",
          "postCode",
          "street"
        ],
        "type": "object",
        "properties": {
          "isDefault": {
            "type": "boolean"
          },
          "isBilling": {
            "type": "boolean"
          },
          "companyName": {
            "type": "string",
            "nullable": true
          },
          "city": {
            "minLength": 1,
            "type": "string"
          },
          "postCode": {
            "minLength": 1,
            "type": "string"
          },
          "street": {
            "minLength": 1,
            "type": "string"
          },
          "houseNumber": {
            "minLength": 1,
            "type": "string"
          },
          "state": {
            "type": "string",
            "nullable": true
          },
          "countryId": {
            "type": "integer",
            "format": "int32"
          },
          "addressSuffix": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DebitorAddressDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "zipCode": {
            "type": "string",
            "nullable": true
          },
          "city": {
            "type": "string",
            "nullable": true
          },
          "street": {
            "type": "string",
            "nullable": true
          },
          "housenumber": {
            "type": "string",
            "nullable": true
          },
          "countryID": {
            "type": "integer",
            "format": "int32"
          },
          "isBilling": {
            "type": "boolean"
          },
          "isDefault": {
            "type": "boolean"
          },
          "addressSuffix": {
            "type": "string",
            "nullable": true
          },
          "companyName": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DebitorCustomerIdRelation": {
        "required": [
          "customerId",
          "debitorId"
        ],
        "type": "object",
        "properties": {
          "debitorId": {
            "minLength": 1,
            "type": "string"
          },
          "customerId": {
            "minLength": 1,
            "type": "string"
          },
          "bmsDebitorType": {
            "enum": [
              "default",
              "isSapCoreDebitor",
              "isSapWasteDebitor",
              "isThirdParty"
            ],
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "DebitorDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "debitorId": {
            "type": "string",
            "nullable": true
          },
          "isIndividualPerson": {
            "type": "boolean"
          },
          "salutationId": {
            "type": "integer",
            "format": "int32"
          },
          "firstname": {
            "type": "string",
            "nullable": true
          },
          "lastname": {
            "type": "string",
            "nullable": true
          },
          "email": {
            "type": "string",
            "nullable": true
          },
          "phoneNumber": {
            "type": "string",
            "nullable": true
          },
          "information": {
            "type": "string",
            "nullable": true
          },
          "poBox": {
            "type": "string",
            "nullable": true
          },
          "birthdate": {
            "type": "string",
            "nullable": true
          },
          "salesTaxIdentificationNumber": {
            "type": "string",
            "nullable": true
          },
          "externalSystemID": {
            "type": "string",
            "nullable": true
          },
          "debitorAddresses": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DebitorAddressDTO"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DebitorDTODataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/DebitorDTO"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "DebitorDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DebitorDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "DebitorStateDTO": {
        "type": "object",
        "properties": {
          "state": {
            "enum": [
              "approved",
              "pending",
              "notFound",
              "blocked"
            ],
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "DebitorStateDTODataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/DebitorStateDTO"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "DebitorTitleDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "title": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DebitorTitleDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DebitorTitleDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "DebitorWasteBinDetailsDto": {
        "type": "object",
        "properties": {
          "WasteBinId": {
            "type": "integer",
            "format": "int32"
          },
          "IdentId": {
            "type": "string",
            "nullable": true
          },
          "LocationId": {
            "type": "integer",
            "format": "int32"
          },
          "StreetName": {
            "type": "string",
            "nullable": true
          },
          "Housenumber": {
            "type": "string",
            "nullable": true
          },
          "City": {
            "type": "string",
            "nullable": true
          },
          "ZipCode": {
            "type": "string",
            "nullable": true
          },
          "FractionId": {
            "type": "integer",
            "format": "int32"
          },
          "Fraction": {
            "type": "string",
            "nullable": true
          },
          "Width": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "Height": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "Depth": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "Size": {
            "type": "integer",
            "format": "int32"
          },
          "SizeName": {
            "type": "string",
            "nullable": true
          },
          "WasteBinSizeImage": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DebitorWasteBinDetailsDtoDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/DebitorWasteBinDetailsDto"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "DebitorWasteBinDetailsDtoListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DebitorWasteBinDetailsDto"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "DeviationData": {
        "type": "object",
        "properties": {
          "bmsDeviationID": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "DeviationDataListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DeviationData"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "DeviationValidityDTO": {
        "required": [
          "deviationId",
          "validFrom",
          "validUntil"
        ],
        "type": "object",
        "properties": {
          "deviationValidityId": {
            "type": "string",
            "nullable": true
          },
          "externalSystemID": {
            "type": "string",
            "nullable": true
          },
          "deviationId": {
            "minLength": 1,
            "type": "string"
          },
          "deviation": {
            "type": "string",
            "nullable": true
          },
          "validFrom": {
            "minLength": 1,
            "type": "string"
          },
          "validUntil": {
            "minLength": 1,
            "type": "string"
          },
          "isDeleted": {
            "type": "boolean"
          }
        },
        "additionalProperties": false
      },
      "DeviationValidityData": {
        "type": "object",
        "properties": {
          "deviationValidityId": {
            "type": "string",
            "nullable": true
          },
          "externalSystemID": {
            "type": "string",
            "nullable": true
          },
          "deviationId": {
            "type": "string",
            "nullable": true
          },
          "deviation": {
            "type": "string",
            "nullable": true
          },
          "validFrom": {
            "type": "string",
            "nullable": true
          },
          "validUntil": {
            "type": "string",
            "nullable": true
          },
          "isDeleted": {
            "type": "boolean"
          }
        },
        "additionalProperties": false
      },
      "Element": {
        "type": "object",
        "properties": {
          "html-tag": {
            "type": "string",
            "nullable": true
          },
          "label": {
            "type": "string",
            "nullable": true
          },
          "additional-information": {
            "type": "string",
            "nullable": true
          },
          "data-attributes": {
            "$ref": "#/components/schemas/DataAttributes"
          }
        },
        "additionalProperties": false
      },
      "EmployeeData": {
        "type": "object",
        "properties": {
          "employeeNumber": {
            "type": "string",
            "nullable": true
          },
          "firstName": {
            "type": "string",
            "nullable": true
          },
          "lastName": {
            "type": "string",
            "nullable": true
          },
          "salutation": {
            "type": "string",
            "nullable": true
          },
          "entranceDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "exitDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "EmployeeDataListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/EmployeeData"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "EmployeeDeviationDataList": {
        "type": "object",
        "properties": {
          "employeeNumber": {
            "type": "string",
            "nullable": true
          },
          "firstName": {
            "type": "string",
            "nullable": true
          },
          "lastName": {
            "type": "string",
            "nullable": true
          },
          "deviations": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DeviationValidityData"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "EmployeeDeviationDataListListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/EmployeeDeviationDataList"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "EmployeeDeviationListDTO": {
        "required": [
          "deviations",
          "employeeNumber"
        ],
        "type": "object",
        "properties": {
          "employeeNumber": {
            "minLength": 1,
            "type": "string"
          },
          "firstName": {
            "type": "string",
            "nullable": true
          },
          "lastName": {
            "type": "string",
            "nullable": true
          },
          "deviations": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DeviationValidityDTO"
            }
          }
        },
        "additionalProperties": false
      },
      "EmpoyeeAppPasswordDTO": {
        "required": [
          "EmployeeId",
          "Password"
        ],
        "type": "object",
        "properties": {
          "EmployeeId": {
            "type": "integer",
            "format": "int32"
          },
          "Password": {
            "minLength": 1,
            "type": "string"
          }
        },
        "additionalProperties": false
      },
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
      "GetWasteBinTransponderBlacklistModel": {
        "type": "object",
        "properties": {
          "Id": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "ExternalSystemId": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "HousenumberSelectOptionDTO": {
        "type": "object",
        "properties": {
          "postcode": {
            "type": "string",
            "nullable": true
          },
          "city": {
            "type": "string",
            "nullable": true
          },
          "location-id": {
            "type": "string",
            "nullable": true
          },
          "value": {
            "type": "string",
            "nullable": true
          },
          "option-text": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "HousenumberSelectOptionDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/HousenumberSelectOptionDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "IActionResult": {
        "type": "object",
        "additionalProperties": false
      },
      "IActionResultDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/IActionResult"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "IdAndAmountModel": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "amount": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
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
      "Item": {
        "type": "object",
        "properties": {
          "id": {
            "type": "string",
            "nullable": true
          },
          "quantity": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "LocationWasteBinDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "wasteType": {
            "type": "string",
            "nullable": true
          },
          "wasteBinSize": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "LocationWasteBinDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/LocationWasteBinDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "LockTransponder": {
        "required": [
          "locationId"
        ],
        "type": "object",
        "properties": {
          "wasteBinId": {
            "type": "integer",
            "format": "int32"
          },
          "locationId": {
            "type": "integer",
            "format": "int32"
          },
          "identName": {
            "type": "string",
            "nullable": true
          },
          "validFrom": {
            "type": "string",
            "nullable": true
          },
          "validUntil": {
            "type": "string",
            "nullable": true
          },
          "externalId": {
            "type": "string",
            "nullable": true
          },
          "blacklistReasonId": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "ObjectDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "OptiAwiContact": {
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
      "OptiAwiContactWithName": {
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
      "OptiAwiContactWithNameAndNumber": {
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
      "OptiAwiContainer": {
        "required": [
          "containerTypeName",
          "containerTypeNumber",
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
              "S",
              "U",
              "H",
              "T",
              "F"
            ],
            "type": "string",
            "description": "Bewegungsart (S = Aufstellung, F = Sofortwechsel, U = Wechsel, T = Wechsel, H = Einzug)"
          },
          "containerTypeName": {
            "minLength": 1,
            "type": "string",
            "description": "Der Typ des Containers."
          },
          "containerTypeNumber": {
            "minLength": 1,
            "type": "string",
            "description": "Die Nummer des Containertyps."
          },
          "customerOwned": {
            "type": "boolean",
            "description": "Gibt an, ob es ein Kundeneigener Container ist, bei dem kein Barcode zum Scannen vorhanden ist."
          }
        },
        "additionalProperties": false,
        "description": "Ein Container eines Auftrags."
      },
      "OptiAwiOrder": {
        "required": [
          "containers",
          "customer",
          "estimatedDuration",
          "executionDate",
          "orderNumber",
          "plannedDate",
          "positions",
          "status"
        ],
        "type": "object",
        "properties": {
          "status": {
            "enum": [
              "OK",
              "GEDRUCKT",
              "ERLEDIGT",
              "STORNIERT"
            ],
            "type": "string",
            "description": "Der Status des Auftrags. (gültige Werte sind OK, GEDRUCKT, ERLEDIGT, STORNIERT)"
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
            "$ref": "#/components/schemas/OptiAwiContactWithNameAndNumber"
          },
          "placeOfDelivery": {
            "$ref": "#/components/schemas/OptiAwiContact"
          },
          "location": {
            "$ref": "#/components/schemas/OptiAwiContact"
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
          "executionDate": {
            "type": "string",
            "description": "Das Datum der Ausführung. (kann vom geplanten Datum abweichen, bspw. wegen Kapazitätsüberlastung bereits beim Anlegen des Auftrages)",
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
          "executionTime": {
            "type": "string",
            "description": "Die erwartete Dauer der Ausführung.",
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
            "$ref": "#/components/schemas/OptiAwiContactWithNameAndNumber"
          },
          "recycler": {
            "$ref": "#/components/schemas/OptiAwiContactWithName"
          },
          "carrier": {
            "$ref": "#/components/schemas/OptiAwiContactWithNameAndNumber"
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
          "contractRelated": {
            "type": "boolean",
            "description": "Gibt an, ob es einen Vertragsbezug gibt."
          },
          "signatureRequired": {
            "type": "boolean",
            "description": "Gibt an, ob eine Unterschrift erforderlich ist."
          },
          "positions": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/OptiAwiPosition"
            },
            "description": "Die Positionen des Auftrags. (kann eine leere Liste sein)"
          },
          "containers": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/OptiAwiContainer"
            },
            "description": "Die Container des Auftrags. (mindestens ein Eintrag erforderlich)"
          }
        },
        "additionalProperties": false,
        "description": "Ein Auftrag."
      },
      "OptiAwiPosition": {
        "required": [
          "itemDescription",
          "itemNumber",
          "itemPrice",
          "positionType",
          "quantity",
          "sortNumber",
          "unit"
        ],
        "type": "object",
        "properties": {
          "sortNumber": {
            "type": "integer",
            "description": "Die Reihenfolge der Positionen (aufsteigend sortiert).",
            "format": "int32"
          },
          "itemNumber": {
            "minLength": 1,
            "type": "string",
            "description": "DIe Artikelnummer."
          },
          "itemDescription": {
            "minLength": 1,
            "type": "string",
            "description": "Die Artikelbezeichnung."
          },
          "quantity": {
            "type": "number",
            "description": "Die Menge des Artikels.",
            "format": "double"
          },
          "unit": {
            "minLength": 1,
            "type": "string",
            "description": "Die Mengeneinheit des Artikels (z.B. t, kg, m³, etc.)."
          },
          "itemPrice": {
            "type": "number",
            "description": "Der Preis für eine Mengeneinheit (netto).",
            "format": "double"
          },
          "positionType": {
            "enum": [
              "P",
              "A",
              "T"
            ],
            "type": "string",
            "description": "Der Typ der Position. (P steht für Positionen mit Artikel, Preis, etc, T und A sind reine Textpositionen (A = belegübergreifend, T = geht nicht in Rechnungsbeleg))"
          }
        },
        "additionalProperties": false,
        "description": "Eine Position eines Auftrags."
      },
      "OrderDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "teamId": {
            "type": "integer",
            "format": "int32"
          },
          "displayText": {
            "type": "string",
            "nullable": true
          },
          "type": {
            "type": "string",
            "nullable": true
          },
          "process": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "ProblemDetails": {
        "type": "object",
        "properties": {
          "type": {
            "type": "string",
            "nullable": true
          },
          "title": {
            "type": "string",
            "nullable": true
          },
          "status": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "detail": {
            "type": "string",
            "nullable": true
          },
          "instance": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": { }
      },
      "ProductListHead": {
        "type": "object",
        "properties": {
          "description": {
            "type": "string",
            "nullable": true
          },
          "quantity": {
            "type": "string",
            "nullable": true
          },
          "single-price": {
            "type": "string",
            "nullable": true
          },
          "subtotal": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
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
      "RegisterDamageForWasteBin": {
        "required": [
          "damageTypeId",
          "deadline",
          "reasonForChangeID",
          "wasteBinId"
        ],
        "type": "object",
        "properties": {
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "wasteBinId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "reasonForChangeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "damageTypeId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "remark": {
            "type": "string",
            "nullable": true
          },
          "isExtern": {
            "type": "boolean",
            "default": false
          },
          "hasExchangeFee": {
            "type": "boolean"
          },
          "isDesiredDate": {
            "type": "boolean"
          }
        },
        "additionalProperties": false
      },
      "SelectOptionDTO": {
        "type": "object",
        "properties": {
          "value": {
            "type": "string",
            "nullable": true
          },
          "option-text": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "SelectOptionDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/SelectOptionDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "SetUpWasteBin": {
        "required": [
          "deadline",
          "locations",
          "reasonForChangeID",
          "wasteBinEmptyingCycleId",
          "wasteBinSizeId",
          "wasteTypeId"
        ],
        "type": "object",
        "properties": {
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "wasteTypeId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "wasteBinSizeId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "wasteBinEmptyingCycleId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "reasonForChangeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "locations": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/WasteBinLocation"
            }
          },
          "remark": {
            "type": "string",
            "nullable": true
          },
          "transponderNr": {
            "type": "string",
            "nullable": true
          },
          "sapId": {
            "type": "string",
            "nullable": true
          },
          "isExtern": {
            "type": "boolean",
            "default": false
          },
          "hasExchangeFee": {
            "type": "boolean"
          },
          "isDesiredDate": {
            "type": "boolean"
          },
          "isAutoSetCapNumberActive": {
            "type": "boolean",
            "default": false
          },
          "capNumber": {
            "type": "string",
            "nullable": true
          },
          "isLockable": {
            "type": "boolean",
            "default": false
          },
          "lockRemark": {
            "type": "string",
            "nullable": true
          },
          "locationInfo": {
            "type": "string",
            "nullable": true
          },
          "debitors": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "bmsWasteBinLocationFlag": {
            "enum": [
              "unknown",
              "defaultLocation",
              "supplyLocation",
              "defaultLocationAndSupplyLocation",
              "loadingLocation",
              "defaultLocationAndLoadingLocation",
              "supplyLocationAndLoadingLocation",
              "defaultLocationAndSupplyLocationAndLoadingLocation"
            ],
            "type": "string"
          },
          "obstacleLevels": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "wasteTourIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "wasteBinIndicatorId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "ShiftWasteBin": {
        "required": [
          "CurrentWasteBinId",
          "deadline",
          "reasonForChangeID",
          "wasteBinEmptyingCycleId",
          "wasteBinSizeId",
          "wasteTypeId"
        ],
        "type": "object",
        "properties": {
          "CurrentWasteBinId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "deadline": {
            "type": "string",
            "format": "date-time"
          },
          "wasteTypeId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "wasteBinSizeId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "wasteBinEmptyingCycleId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "reasonForChangeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "remark": {
            "type": "string",
            "nullable": true
          },
          "isExtern": {
            "type": "boolean",
            "default": false
          },
          "hasExchangeFee": {
            "type": "boolean"
          },
          "isDesiredDate": {
            "type": "boolean"
          },
          "isAutoSetCapNumberActive": {
            "type": "boolean",
            "default": false
          },
          "capNumber": {
            "type": "string",
            "nullable": true
          },
          "isLockable": {
            "type": "boolean",
            "default": false
          },
          "lockRemark": {
            "type": "string",
            "nullable": true
          },
          "locationInfo": {
            "type": "string",
            "nullable": true
          },
          "bmsWasteBinLocationFlag": {
            "enum": [
              "unknown",
              "defaultLocation",
              "supplyLocation",
              "defaultLocationAndSupplyLocation",
              "loadingLocation",
              "defaultLocationAndLoadingLocation",
              "supplyLocationAndLoadingLocation",
              "defaultLocationAndSupplyLocationAndLoadingLocation"
            ],
            "type": "string"
          },
          "obstacleLevels": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "wasteTourIds": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "wasteBinIndicatorId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "StreetDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "streetname": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "StreetDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/StreetDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "StreetGroupByPostcodeDTO": {
        "type": "object",
        "properties": {
          "postcode": {
            "type": "string",
            "nullable": true
          },
          "streets": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/StreetDTO"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "StreetGroupByPostcodeDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/StreetGroupByPostcodeDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "StringDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "string",
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "StringListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "TaxDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "rate": {
            "type": "number",
            "format": "double"
          }
        },
        "additionalProperties": false
      },
      "TaxDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/TaxDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "TeamDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "applicationFieldId": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "color": {
            "type": "string",
            "nullable": true
          },
          "remark": {
            "type": "string",
            "nullable": true
          },
          "sortNumber": {
            "type": "integer",
            "format": "int32"
          },
          "orders": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/OrderDTO"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "TeamResourceDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "teamId": {
            "type": "integer",
            "format": "int32"
          },
          "displayText": {
            "type": "string",
            "nullable": true
          },
          "resourceType": {
            "type": "string",
            "nullable": true
          },
          "sortNumber": {
            "type": "integer",
            "format": "int32"
          },
          "functionColor": {
            "type": "string",
            "nullable": true
          },
          "isAssignedHourly": {
            "type": "boolean"
          },
          "isJumper": {
            "type": "boolean"
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
      "UnlockTransponder": {
        "required": [
          "locationId",
          "wasteBinId"
        ],
        "type": "object",
        "properties": {
          "locationId": {
            "type": "integer",
            "format": "int32"
          },
          "wasteBinId": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "UpdateBulkyWasteContractModel": {
        "required": [
          "ContactTypeId",
          "DebitorId",
          "id",
          "LocationId"
        ],
        "type": "object",
        "properties": {
          "CalendarWeek": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          },
          "id": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "ContactTypeId": {
            "type": "integer",
            "format": "int32"
          },
          "LocationId": {
            "type": "integer",
            "format": "int32"
          },
          "DebitorId": {
            "type": "integer",
            "format": "int32"
          },
          "PaymentMethodId": {
            "type": "integer",
            "format": "int32"
          },
          "EventId": {
            "type": "integer",
            "format": "int32"
          },
          "LocationRemark": {
            "type": "string",
            "nullable": true
          },
          "RemarkInternal": {
            "type": "string",
            "nullable": true
          },
          "ItemInfo": {
            "type": "string",
            "nullable": true
          },
          "FurtherItemRemark": {
            "type": "string",
            "nullable": true
          },
          "Remark": {
            "type": "string",
            "nullable": true
          },
          "Items": {
            "type": "object",
            "additionalProperties": {
              "type": "integer",
              "format": "int32",
              "nullable": true
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "UpdateContractModel": {
        "required": [
          "Description",
          "id"
        ],
        "type": "object",
        "properties": {
          "id": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "Description": {
            "minLength": 1,
            "type": "string"
          },
          "DebitorNumber": {
            "type": "string",
            "nullable": true
          },
          "DebitorExternalSystemId": {
            "type": "string",
            "nullable": true
          },
          "OrderReferenceNumber": {
            "type": "string",
            "nullable": true
          },
          "InvoiceRemark": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "UpdateDeviationDTO": {
        "required": [
          "bmsDeviationID",
          "name"
        ],
        "type": "object",
        "properties": {
          "bmsDeviationID": {
            "minLength": 1,
            "type": "string"
          },
          "name": {
            "minLength": 1,
            "type": "string"
          },
          "description": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "UpdateEmployeeDTO": {
        "required": [
          "employeeNumber",
          "firstName",
          "lastName"
        ],
        "type": "object",
        "properties": {
          "employeeNumber": {
            "minLength": 1,
            "type": "string"
          },
          "firstName": {
            "minLength": 1,
            "type": "string"
          },
          "lastName": {
            "minLength": 1,
            "type": "string"
          },
          "salutation": {
            "type": "string",
            "nullable": true
          },
          "entranceDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "exitDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "UpdateVehicleDTO": {
        "required": [
          "licensePlate",
          "vehicleNumber",
          "vehicleTypeID"
        ],
        "type": "object",
        "properties": {
          "vehicleNumber": {
            "minLength": 1,
            "type": "string"
          },
          "licensePlate": {
            "minLength": 1,
            "type": "string"
          },
          "vehicleTypeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "vehicleIdentificationNumber": {
            "type": "string",
            "nullable": true
          },
          "makeType": {
            "type": "string",
            "nullable": true
          },
          "annex": {
            "type": "string",
            "nullable": true
          },
          "equipmentNumber": {
            "type": "string",
            "nullable": true
          },
          "driverLicenseClass": {
            "type": "string",
            "nullable": true
          },
          "vehicleLocation": {
            "type": "string",
            "nullable": true
          },
          "buildYear": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "unregisteredDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "UpdateVehicleTypeDTO": {
        "required": [
          "bmsVehicleTypeID",
          "name"
        ],
        "type": "object",
        "properties": {
          "name": {
            "minLength": 1,
            "type": "string"
          },
          "bmsVehicleTypeID": {
            "minLength": 1,
            "type": "string"
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "externalSystemID": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "UpdateWasteContractModel": {
        "required": [
          "Description",
          "id"
        ],
        "type": "object",
        "properties": {
          "id": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "Description": {
            "minLength": 1,
            "type": "string"
          },
          "DebitorNumber": {
            "type": "string",
            "nullable": true
          },
          "DebitorExternalSystemId": {
            "type": "string",
            "nullable": true
          },
          "InvoiceRemark": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "UpdateWdwStatusRequestModel": {
        "type": "object",
        "properties": {
          "action": {
            "type": "string",
            "nullable": true
          },
          "comment": {
            "type": "string",
            "nullable": true
          },
          "responsibleid": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "UpdateWdwStatusRequestModelDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/UpdateWdwStatusRequestModel"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "UpdateWdwStatusRequestModelIntern": {
        "type": "object",
        "properties": {
          "wdwId": {
            "type": "string",
            "nullable": true
          },
          "wdwFileId": {
            "type": "string",
            "nullable": true
          },
          "wdwAction": {
            "enum": [
              "deleg",
              "approve",
              "check",
              "solve",
              "unsolved",
              "reject"
            ],
            "type": "string"
          },
          "picture": {
            "type": "string",
            "nullable": true
          },
          "isIntern": {
            "type": "boolean"
          },
          "remark": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
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
      },
      "VehicleData": {
        "type": "object",
        "properties": {
          "vehicleNumber": {
            "type": "string",
            "nullable": true
          },
          "licensePlate": {
            "type": "string",
            "nullable": true
          },
          "vehicleType": {
            "type": "string",
            "nullable": true
          },
          "vehicleIdentificationNumber": {
            "type": "string",
            "nullable": true
          },
          "makeType": {
            "type": "string",
            "nullable": true
          },
          "annex": {
            "type": "string",
            "nullable": true
          },
          "equipmentNumber": {
            "type": "string",
            "nullable": true
          },
          "driverLicenseClass": {
            "type": "string",
            "nullable": true
          },
          "vehicleLocation": {
            "type": "string",
            "nullable": true
          },
          "buildYear": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          },
          "unregisteredDate": {
            "type": "string",
            "format": "date-time",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "VehicleDataListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/VehicleData"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "VehicleDeviationDataList": {
        "type": "object",
        "properties": {
          "vehicleNumber": {
            "type": "string",
            "nullable": true
          },
          "licensePlate": {
            "type": "string",
            "nullable": true
          },
          "deviations": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DeviationValidityData"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "VehicleDeviationDataListListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/VehicleDeviationDataList"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "VehicleDeviationListDTO": {
        "required": [
          "deviations",
          "vehicleNumber"
        ],
        "type": "object",
        "properties": {
          "vehicleNumber": {
            "minLength": 1,
            "type": "string"
          },
          "licensePlate": {
            "type": "string",
            "nullable": true
          },
          "deviations": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/DeviationValidityDTO"
            }
          }
        },
        "additionalProperties": false
      },
      "VehicleTypeData": {
        "type": "object",
        "properties": {
          "bmsVehicleTypeID": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          },
          "externalSystemID": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "VehicleTypeDataListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/VehicleTypeData"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "WasteBinAccessories": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "price": {
            "type": "integer",
            "format": "int32"
          },
          "quantity": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "WasteBinDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          }
        },
        "additionalProperties": false
      },
      "WasteBinDTODataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/WasteBinDTO"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "WasteBinLocation": {
        "required": [
          "locationFlag",
          "locationId"
        ],
        "type": "object",
        "properties": {
          "locationId": {
            "type": "integer",
            "format": "int32"
          },
          "locationFlag": {
            "enum": [
              "unknown",
              "defaultLocation",
              "supplyLocation",
              "defaultLocationAndSupplyLocation",
              "loadingLocation",
              "defaultLocationAndLoadingLocation",
              "supplyLocationAndLoadingLocation",
              "defaultLocationAndSupplyLocationAndLoadingLocation"
            ],
            "type": "string"
          }
        },
        "additionalProperties": false
      },
      "WasteBinOrder": {
        "type": "object",
        "properties": {
          "wasteBinOrderItems": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/WasteBinOrderItem"
            },
            "nullable": true
          },
          "debitorId": {
            "type": "string",
            "nullable": true
          },
          "customerConfirmationEmailAddress": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "WasteBinOrderItem": {
        "required": [
          "locations",
          "reasonForChangeID",
          "wasteBinEmptyingCycleId",
          "wasteBinSizeId",
          "wasteTypeId"
        ],
        "type": "object",
        "properties": {
          "wasteTypeId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "wasteBinSizeId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "wasteBinEmptyingCycleId": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "reasonForChangeID": {
            "maximum": 2147483647,
            "minimum": 1,
            "type": "integer",
            "format": "int32"
          },
          "locations": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/WasteBinLocation"
            }
          },
          "remark": {
            "type": "string",
            "nullable": true
          },
          "isExtern": {
            "type": "boolean",
            "default": false
          },
          "locationInfo": {
            "type": "string",
            "nullable": true
          },
          "debitors": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "obstacleLevels": {
            "type": "array",
            "items": {
              "type": "integer",
              "format": "int32"
            },
            "nullable": true
          },
          "quantity": {
            "type": "integer",
            "format": "int32"
          },
          "singlePrice": {
            "type": "integer",
            "format": "int32"
          },
          "accessories": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/WasteBinAccessories"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "WasteBinSaleBillingDocument": {
        "type": "object",
        "properties": {
          "documentMimeType": {
            "type": "string",
            "nullable": true
          },
          "documentBase64": {
            "type": "string",
            "nullable": true
          },
          "transactionId": {
            "type": "string",
            "nullable": true
          },
          "billingDocumentCreated": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "WasteBinSaleBillingDocumentDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/WasteBinSaleBillingDocument"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "WasteBinSaleOfferDocument": {
        "type": "object",
        "properties": {
          "documentMimeType": {
            "type": "string",
            "nullable": true
          },
          "documentBase64": {
            "type": "string",
            "nullable": true
          },
          "transactionId": {
            "type": "string",
            "nullable": true
          },
          "offerValidity": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "WasteBinSaleOfferDocumentDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/WasteBinSaleOfferDocument"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "WasteBinSizeWasteTypeCyclesDTO": {
        "type": "object",
        "properties": {
          "bmsWasteBinSize": {
            "$ref": "#/components/schemas/BmsWasteBinSizeDTO"
          },
          "bmsWasteType": {
            "$ref": "#/components/schemas/BmsWasteTypeDTO"
          },
          "emptingCycles": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/BmsWasteBinEmptingCycleValidityDTO"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "WasteBinSizeWasteTypeCyclesDTODataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "$ref": "#/components/schemas/WasteBinSizeWasteTypeCyclesDTO"
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
      },
      "WasteBinTransponderBlackListReasonDTO": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "name": {
            "type": "string",
            "nullable": true
          },
          "description": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "WasteBinTransponderBlackListReasonDTOListDataResponse": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/WasteBinTransponderBlackListReasonDTO"
            },
            "description": "Returns the data of the response.",
            "nullable": true
          }
        },
        "additionalProperties": false,
        "description": "Ein Antwortobjekt."
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
    {
      "Bearer": [ ]
    }
  ]
};
			window.onload = () => {
				window.ui = SwaggerUIBundle({
					spec: spec,
					dom_id: '#swagger-ui',
				});
			};
		</script>
	</body>
</html>
