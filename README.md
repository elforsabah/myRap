Ergänzung - Mapping für BMS-Feld “movementType” (unter “containers”):

Hierfür bitte die existierende Mappingtabelle ZWRVWAALAPRCTP (ursprünglich entwickelt von Heiko) um eine Spalte für “BMS-MovementType” erweitern.
Ziel: für jede W&R-Leistungsart soll ein BMS-MovementType zugeordnet werden. So kann für die Schnittstelle das JSON-Feld für jeden Service/EAP anhand der Leistungsart ermittelt werden.
Im SAP: EWA_ORDER_OBJECT-SERVICE_TYPE = /PLCE/TPDSRV-ACTION  (davon wird der P&D-ServiceType abgeleitet, das ist also alles eine 1:1-Zuordnung.)
