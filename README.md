Hi Elvis, das klingt prinzipiell gut!

Nur eine Frage dazu: Kann man “Daten aktualisieren” zusätzlich trotzdem auch beim/nach Start der App auslösen?

 

Zur Erklärung, warum das für die Anwender wichtig wäre:

Der ursprüngliche Ansatz mit Behälter-Lager-Daten aus SAP ist an der Erkenntnis gescheitert, dass die Daten in SAP im Taqesverlauf nicht aktuell sind (v.a. weil Auftragsdaten erst am Abend BMS→SAP übermittelt werden und danach erst noch im SAP manuelle Datenkontrollen und Rückmeldevorgänge durchlaufen werden).

Dass nun also die Daten vom BMS konsumiert werden sollen, liegt an der Aktualität: die meisten Scans von Behälterdaten werden von den BMS-Tablets “zeitnah” an das BMS-Backend übermittelt.

Wenn ein Disponent nun also die App in SAP aufruft, möchte er die aktuellsten Daten von BMS sehen. Es ist sehr gut, wenn die App auch bei Ausfall von BMS benutzbar bleibt (da das wohl oft vorkommt). Vor allem auch, damit darüber die Sicherheitsbestände trotzdem gepflegt werden können.

 

Spalten:

Verstehe ich richtig, dass die angezeigten Spalten für die Behälterdepots fest definiert werden müssen? Die Depots (Lagerorte) sind sowohl im BMS als auch im SAP nur Stammdaten und können jederzeit hinzugefügt, geändert und umbenannt werden. Müsste dann immer das Coding der App angepasst werden? Das wäre schlecht. Könnte man das irgendwie anders lösen?

Vom BMS bekommen wir die Werte auch einfach nur als Liste.
