1. Automatisches Daten-Update beim Öffnen – so machen wir’s

Sobald die App startet, holen wir automatisch die neuesten BMS-Daten. Der Disponent sieht immer den aktuellen Stand, ohne was drücken zu müssen. Damit das auch zuverlässig funktioniert:

Wir nutzen einen Cache in SAP. Die BMS-Daten lagern da zwischen. Wenn du die App öffnest, aktualisieren wir diesen Cache. Sollte das BMS mal ausfallen, siehst du trotzdem noch die letzten Daten und kannst weiterarbeiten – Sicherheitsbestände lassen sich problemlos pflegen. Genau das wollten wir erreichen.

Frische-Kontrolle mit konfigurierbarem Schwellenwert: Der automatische Refresh fragt das BMS nur ab, wenn die letzten Daten älter sind als der eingestellte Schwellenwert. Ansonsten gibt’s direkt die gecachten Daten. Das spart unnötige BMS-Aufrufe – besonders dann, wenn mehrere Disponenten kurz hintereinander die App öffnen.

Der Schwellenwert liegt im Customizing, nicht im Code (Feld in einer Z-Tabelle). Du kannst ihn jederzeit anpassen, ohne neu programmieren zu müssen:
0 heißt: Immer live – jedes Öffnen holt frische Daten. Ideal, wenn aktuelle Infos gewünscht sind und das BMS die Last aushält.
1–2 Minuten: Sehr aktuell, aber etwas entspannter für’s BMS.
10–15 Minuten: Schont das BMS möglichst, falls wenig Bewegung im Bestand ist.

Außerdem gibt’s einen manuellen Button "Daten aktualisieren" und einen Nacht-Job als Backup – falls morgens niemand die App nutzt.

Den Startwert legt der Fachbereich fest, je nachdem, wie oft sich die BMS-Daten ändern. Da das Customizing ist, kannst du ruhig mit einem kleinen Wert starten (z.B. 2 Minuten) und später anpassen.

2. Depot-Spalten – flexibel statt fest verdrahtet

Depots sind Stammdaten und ständig in Bewegung – neue kommen dazu, andere werden umbenannt oder gelöscht. Das BMS liefert sie nur als Liste, nicht als feste Spalten. Würden wir die Spalten fix kodieren, müsste jedes Mal das Programm angepasst werden. Das vermeiden wir bewusst:

In der Übersicht (List Report) zeigen wir nur stabile Kennzahlen pro Behältertyp – Gesamtbestand, Sicherheitsbestand, Differenz und Ampel. Diese Spalten bleiben immer gleich.

Im Detail (Object Page) gibt’s die komplette Depot-Aufschlüsselung als dynamische Liste – genau wie vom BMS geliefert. Neue oder umbenannte Depots tauchen automatisch auf, ohne dass wir am Code drehen müssen.

Und wenn einzelne, selten wechselnde Depots wirklich fest in die Übersicht sollen, geht das als kleine Zusatzoption. Der Rest bleibt flexibel. Noch robuster wäre das Ganze mit einer technischen Depot-ID statt dem Namen – klärt bitte, ob das BMS so etwas liefert.
