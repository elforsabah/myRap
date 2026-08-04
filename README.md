1. Automatisches „Daten aktualisieren" beim Öffnen der App – ja, so bauen wir es
Beim Start der App wird der Abruf automatisch ausgelöst, damit der Disponent sofort den aktuellsten BMS-Stand sieht. Damit das robust bleibt:

Cache in SAP: Die BMS-Daten werden in SAP zwischengespeichert. Beim Öffnen wird dieser Cache aufgefrischt. Fällt das BMS aus, bleibt der letzte Stand sichtbar und die App voll benutzbar – die Sicherheitsbestände lassen sich weiter pflegen. Genau das war die Anforderung.
Frische-Wächter mit Customizing-Schwelle: Der Auto-Refresh ruft das BMS nur, wenn der letzte Stand älter als eine eingestellte Schwelle ist – sonst wird direkt der Cache gezeigt. Das verhindert unnötige BMS-Aufrufe, wenn mehrere Disponenten kurz hintereinander die App öffnen.
Die Schwelle ist Customizing, kein fester Wert im Code (Feld in einer Z-Tabelle). Sie lässt sich jederzeit ohne Programmänderung anpassen:
0 = immer live (jeder Öffnen-Vorgang holt frisch) – falls das BMS die Last verträgt und maximale Aktualität gewünscht ist,
1–2 Min. für hohe Aktualität bei geringer Last,
10–15 Min. möglichst BMS-schonend.
Zusätzlich: manueller Button „Daten aktualisieren" und ein Nacht-Job als Fallback (falls morgens niemand die App öffnet).
Den passenden Startwert legt der Fachbereich fest – er hängt davon ab, wie schnell sich die BMS-Daten real ändern. Da er Customizing ist, kann man ohne Weiteres klein anfangen (z. B. 2 Min.) und später nachjustieren.

2. Depot-Spalten – bewusst nicht fest verdrahtet
Depots sind Stammdaten und ändern sich jederzeit (neu, umbenannt, gelöscht); BMS liefert sie nur als Liste. Feste Spalten würden bei jeder Stammdaten-Änderung eine Coding-Anpassung erzwingen – das vermeiden wir:

Übersicht (List Report): nur stabile Kennzahlen je Behältertyp – Gesamtbestand, Sicherheitsbestand, Differenz, Ampel. Diese Spalten ändern sich nie.
Detail (Object Page): die vollständige Depot-Aufschlüsselung als dynamische Liste, genau wie vom BMS geliefert. Neue oder umbenannte Depots erscheinen automatisch – ohne Codeänderung, ohne Transport.
Falls einzelne, selten wechselnde Depots doch fest in der Übersicht gewünscht sind, geht das als kleine Zusatzoption; der Rest bleibt dynamisch. Stabiler wäre das mit einer technischen Depot-ID statt des Namens – bitte klären, ob BMS eine solche liefert.
