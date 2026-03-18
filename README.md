Bei Bestätigung des Dialogs erfolgt das Storno:
Falls eingeplant, wird der Service in P&D wird aus seiner Tour entfernt. Das entfällt natürlich, falls ein unverplanter Service storniert wird.
Der Service wird aus der Planung entfernt, sodass keine Karteileiche im P&D verbleibt.
Der Storno-Grund (Freitext) wird als Bemerkung zur zugehörigen Entsorgungsauftragsposition (Feld EWA_ORDER_OBJECT-TEXT) übernommen.
Es wird zudem ein fester Rückmeldehinweis (Feld EWA_ORDER_OBJECT-CONFTYPE1) "Storno durch Dispo"* an der EAP gesetzt, sodass ergänzend zum angegebenen Storno-Grund nachvollziehbar ist, dass das Storno auf diesem Weg erfolgt ist.
(* = diesen Rückmeldehinweis werde noch ich in Rücksprache mit Anja im Customizing definieren.)
Die EAP wird negativ zurückgemeldet.
Diese Storno-Verarbeitung wird später in gleicher Weise anderen Stellen auch benötigt (Interaction Center für Kundenservice und evtl. EWAWDOC für Rückmeldung). Wäre gut, wenn wir das Coding dann weiterverwenden/erweitern können.
