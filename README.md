Standard-Funktionalität:

Im P&D-Standard besteht ein Service aus beliebig vielen Service Tasks. Diese haben jeweils eine eigene Dauer.
Die Dauer des Service = die Summe der Dauer von den Service Tasks.
Die gesamte Dauer einer Tour = die Summe der Dauer der darauf geplanten Services.
Es gibt zur Tour auch noch die Fahrtzeit (Driving Time). Diese wird ausgerechnet, wenn in der Karte die Tour ausgewählt und die Funktion “Calculate Track” angeklickt wird. Im Augenblick wird die Fahrtzeit aber nicht auf die Gesamt-Dauer der Tour addiert. (Das ist an die Produktentwicklung gemeldet und wahrscheinlich ein Bug. Aber für HWS lassen wir das erstmal so.)
Auf der rechten Seite im Planungscockpit eine Tour selektieren → dann werden die geplanten Services der Tour unten aufgelistet.


<img width="800" height="335" alt="image" src="https://github.com/user-attachments/assets/8038c192-25be-48cf-b715-444884f8fe68" />


Anpassung/Kundenwunsch:

Es soll eine Möglichkeit geschaffen werden, bei verplanten Services manuell eine “Dauer” zu bestimmen. → z.B. Standardfeld änderbar machen oder ein neues Feld “indiv. Dauer”.
Dabei bitte auch prüfen, ob wir (mit wenig Aufwand) auch wahlweise die Dauer in der Einheit Stunden (H) oder Minuten (MIN) eingeben zu können. Falls es deutlich weniger aufwändig ist, können wir auch eine Einheit fest vorgeben. (Kannst du den Unterschied bitte grob einschätzen?)
Die Berechnung der Dauer auf der Tour muss angepasst werden, um die individuellen Zeiten zu berücksichtigen:
Für alle geplanten Services wird die neue individuelle Dauer in die Tour-Dauer summiert, sofern ein Wert manuell eingetragen wurde
Für alle geplanten Services, für die keine individuelle Dauer eingetragen wurde, wird die Dauer aus dem Standardfeld genommen (Standardverhalten)


<img width="800" height="335" alt="image" src="https://github.com/user-attachments/assets/99c1cec5-f2a3-430d-8f85-74b64c56aaf0" />


Hallo Patrick, 

Nach Analyse des Codes und Berücksichtigung aller möglichen Erweiterungen sind folgende Möglichkeiten möglich:

Zu Punkt 1 und 2: 
"Es soll eine Möglichkeit geschaffen werden, bei verplanten Services manuell eine “Dauer” zu bestimmen. → z.B. Standardfeld änderbar machen oder ein neues Feld “indiv. Dauer”.

Man konnte hier einen neues Feld “indiv. Dauer”. zu der Service Assigment hinzufügen und die Einheit in H oder MIN eingeben. Dann kann mein ein neuen Button (z.B Assign Duration ) auf dem Service Assignment hinzufügen mit der man dann die Werte der “indiv. Dauer” ändern kann. 

zu Punkt 3: 
"Die Berechnung der Dauer auf der Tour muss angepasst werden, um die individuellen Zeiten zu berücksichtigen:

Für alle geplanten Services wird die neue individuelle Dauer in die Tour-Dauer summiert, sofern ein Wert manuell eingetragen wurde
Für alle geplanten Services, für die keine individuelle Dauer eingetragen wurde, wird die Dauer aus dem Standardfeld genommen (Standardverhalten)"

 

Ja die Berechnung der Dauer der Tour konnte nach berechnung der  um die individuellen Zeiten aktualiziert werde . 


Um die obige Anforderungen zu erfüllen,  muss Erweiterungen in bestimmte CDS views (/PLCE/R_PDTourServiceAsgmt und auch /PLCE/C_PDMNLTourServiceAsgWR ) vorgenomment werde ABER diese CDS View sind nicht erweitbar. D.h die sind nicht als ‘extensible’ gekennzeichnet und ich finde kein andere Möglichkeit so eine Erweiterung vorzunehmen. 

 
<img width="800" height="640" alt="image" src="https://github.com/user-attachments/assets/cdec3e6e-a987-4cc0-8b24-4877d27bd1b4" />

<img width="800" height="497" alt="image" src="https://github.com/user-attachments/assets/0f7ca128-b65e-4172-ac79-279ce5b6eb43" />


Hi Elvis, danke für die Analyse! Kannst du bitte auch für diesen Punkt bitte schon mal mit Alexandra darüber sprechen. wie aufwändig die Erweiterbarkeit für die Produkt-Entwicklung wahrscheinlich sein wird? Der Kunde wird diese Anpassung (oder etwas ähnliches) absolut zwingend haben wollen. Es ist schwierig, wenn wir da vom Produkt aus komplett handlungsunfähig sind, weil notwendigen CDS-Views not extensible sind. Zur Not muss ich das einfordern.




 ich habe hier noch mit Alexandra geprochen.  

Man konnte ein Feld auf Service an sich hinzufügen und in der Service Assignment konsummiert.  

/PLCE/C_PDMNLTourServiceAsgWR. 


Also Hier mal meine Aufwandschätzung: 

 Zu Punkt 1 & 2 : 
   Ca 5 - 6 PTs konnt reichen. 


Zu Punkt 3 : 

3 bis 4 PTs

Hi Elvis, der Kunde hat nach Diskussionen eine etwas abweichende Idee entwickelt:

Anstatt einer “individuellen Gesamtdauer” pro Service möchte man lieber einen “Auf-/Abschlag” (+/-) pro Service pflegen können.

Hintergrund #1: Man möchte die vorgeschlagene Dauer und Fahrtzeiten aus P&D grundsätzlich verwenden.
Hintergrund #2: Der Ansatz würde dann später auch ermöglichen, dass für bestimmte Kunden oder Behälterstandplätze in der Auftragserfassung dauerhaft gültige Auf-/Abschläge gepflegt werden. Zum Beispiel wenn es bei einem Kunden immer etwas länger dauert. In einer späteren Umsetzung könnten wir solche Angaben dann von der Entsorgungsauftragsposition in den P&D-Service übernehmen. (Nicht Bestandteil jetzt, nur voraus gedacht.)
 

Ich vermute, das ändert am geschätzten Aufwand nicht viel, oder? Die Anpassung der Berechnung der gesamten Tour-Dauer sehe ich sogar simpler als beim ursprünglichen Wunsch: es müssen einfach nur alle Auf- und Abschläge der Services an der Tour-Dauer kumuliert werden.

Siehst du das auch so?

das Pflegen von  einen “Auf-/Abschlag” (+/-) pro Service ist besser.  

Also ich sehe es genau wie du.  

Der Aufwand bleibt eigentlich gleich



