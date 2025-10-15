Was ist möglich?

Fiori Elements (List Report / Object Page)

Wenn das Feld bereits im Backend existiert: rein annotation-basiert sichtbar machen und exakt positionieren.

CDS (oder Metadata Extension):

@UI.lineItem: [
  { position: 45, label: 'Neue Spalte', value: NewField, importance: #HIGH }
]


→ Spalte erscheint an definierter Stelle.

Wenn das Feld noch nicht existiert:

Persistentes Feld (DB/Tabelle + CDS + Behavior) – wenn das dauerhaft gespeichert werden soll.

Abgeleitetes Feld (z. B. Berechnung/Mapping) über CDS CASE oder virtuelles Element mit ABAP-Klasse (performant, kein DB-Persist).

SmartTable / UI5 (falls keine FE-App)

XML-View: sap.ui.table.Column hinzufügen, Binding auf das neue Feld; ggf. Formatter.
