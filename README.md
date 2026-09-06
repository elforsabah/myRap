User Story 

UST-S.AA.0006-005: Nachweispflichtige Stoffe freigeben – AK1 

Abkürzungen, Erläuterungen 

ÜS – Übernahmeschein 

SBS – Sammelbegleitschein (wird im Sammelverfahren genutzt, um mehrere ÜS zu gruppieren, i. Vgl. zum BS steht beim Bereich „Erzeuger“ hier eine fiktive Erzeugernummer, anstatt konkreten Angaben zu einem bestimmten Erzeuger) 

BS – Begleitschein (wird im Einzelnachweisverfahren genutzt) 

SN – Sammelnachweis (darunter befinden sich SBS und ÜS) 

EN – Einzelnachweis (darunter befinden sich BS) 

EAP – Entsorgungsauftragsposition bzw. Auftrag 

Konzept  

Die drei relevanten Scheinarten werden durch folgende Rollen elektronisch signiert: 

BS = ERZ, BEF, ENT 

SBS = ERZ (wird vom BEF signiert), BEF, ENT 

ÜS = keine elektronische Signatur erforderlich! 

Die Anforderung der HWS, ist es, dass ein Auftrag erst tatsächlich vom Fahrer auf dem BMS übergeben wird, wenn die notwendigen elektronischen Signaturen pro Scheinart vorhanden sind.  

Somit soll für den BS und SBS auf eine elektronische Signatur in den 2 Layern (ERZ und BEF) geprüft werden, bevor eine Freigabe der Tour im Planning & Dispatching durch den Disponenten erlaubt ist. Der ÜS benötigt rechtlich keine elektronischen Signaturen also ist er in dieser Anforderung nicht inkludiert. Bei den Aufträgen, wo es einen ÜS gibt, gibt es jedoch immer einen SBS im HWS-System und dieser muss signiert werden.  

Die Konstellation der meisten SBS bei HWS ist so: 

EAP1 -> ÜS1 -> SBS1 

EAP2 -> ÜS2 -> SBS2 

Im seltenen Fall aber auch so: 

EAP1 -> ÜS1 -> SBS1 

EAP2 -> ÜS2 -> SBS1 

Vglw. ist beim BS die Zuordnung direkt zum Auftrag: 

EAP -> BS 

Das Feld NOTEINTNR bzw. NOTENR bekommt nur einen Schein zugeordnet (entweder der ÜS oder BS). Hinter dem ÜS liegt der dazugehörige SBS.  

Wenn man sich einen Schein z.B. via der Transaktion /WATP/ARB_HW_NW - Report Entsorgungsnachweis oder /WATP/ARB_NOTES – Scheine anschaut, sieht man, dass jeweils beim ÜS oder beim SBS die Zuordnung besteht. Z.B. so: 

 <img width="1136" height="333" alt="image" src="https://github.com/user-attachments/assets/cbfaea2f-6d5d-430b-a544-659d096b0079" />


Wie gesagt, beim BS und SBS, aber nicht dem ÜS, sollen die Signaturfelder mittels dieser neuen Entwicklung geprüft werden. 

<img width="1070" height="712" alt="image" src="https://github.com/user-attachments/assets/11bc8ba6-1618-4ad4-a212-e51ff578c2e0" />

 

Interne Scheinnummer = NOTEINTNR 

Externe Scheinnummer = NOTENR 

Scheinart = CERTTYPE 

Beim BS soll auf die Felder Signatur Erzeuger SIGNATUR_ERZ und Signatur Beförderer SIGNATUR_BE1 geprüft werden. Anstatt einer Signatur ist das Feld Quittungsbeleg (QUITTUNG_ERZ, QUITTUNG_BEF) auch akzeptabel. Die Vermutung ist, dass diese Feldnamen beim SBS_DE genauso heißen wie beim BS_DE. (Aktuell liegen in der Transaktion /WATP/ARB_ZKSDOCS - ZKS-Dokumente sowie der Tabelle /WATP/TARBZKSDOC (via SE16N) noch keine SBS). 

In einer sehr ähnlichen Entwicklung (Signaturprüfung für EAP Buchung in EWAWDOC) im TI4 werden die gleichen Felder benutzt in der Prüfung. Siehe #41565 - Schätzung - UST-S.SM.0003-014 AK3 - Signaturprüfung vor Auftragsbuchung - Implementierung W&R - Prologa und #41911 - UST-S.SM.0003-014 AK3 - Signaturprüfung vor Auftragsbuchung - Implementierung W&R - Prologa zur Referenz. 

Die Prüfung erfolgt an dem Moment, wo auf diesen Button geklickt wird: 

<img width="1691" height="427" alt="image" src="https://github.com/user-attachments/assets/77cd7e17-c59f-4178-bffc-225e9a21bd4c" />

 

Sollte eine oder beide Signaturen fehlen beim Klick auf „Tour an BMS freigeben“ im P&D, sollte dem User eine Fehlermeldung angezeigt werden. Diese sollte so etwas wie “Elektronische Signatur auf ext. Scheinnr. X (Scheinart X), EA-ID X, fehlt!" aussagen. Nachdem die Signatur nachgeholt wurde, muss der Disponent erneut auf „Tour an BMS freigeben“ klicken. 

Aufträge die Scheine besitzen aber nicht an das BMS übermittelt werden sind in dieser Entwicklung nicht inbegriffen. 

 

Aufwandsschätzung: ? ET 

System für Tests, Analyse und Entwicklung: TI4 M440 und M442
