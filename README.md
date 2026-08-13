Anhänge an die Dispo übergeben
Version	Wichtige Änderungen
1	Erstellt 
	
	

Inhaltsverzeichnis
1.	Einleitung/Kundenanforderung	2
2.	Standardprozesse	2
3.	Erweiterungen/Anpassungen	2
4.	Testszenarien/Akzeptanzkriterien	2
5.	Alternativen	3
6.	Aufwand	3

 
1.	Einleitung/Kundenanforderung
In der UST-S.VM.0001-003 „Vertragspositionen verwalten“ AK14 wird beschrieben, dass HWS in der Lage sein möchte, Lagepläne oder Fotos mit einer Vertragsposition zu verknüpfen. Die entsprechenden Dokumente sollen dann im Verlauf der Dispo zur Verfügung stehen und auch in das BMS übertragen werden, damit der Fahrer Zugriff darauf hat. 

2.	Standardprozesse
Am Vertrag im SAP gibt es bereits eine Standardlösung, um Dokumente hochzuladen. Da HWS das d3 als DMS anbinden möchte, muss das Hochladen von Dokumenten entsprechend konfiguriert werden. 
 
Im P&D können zu Services Dokumente angehangen und angezeigt werden. 

3.	Erweiterungen/Anpassungen
Im ersten Schritt muss eine eigene Dokumentenart ZWR_LOGDAT – „Logistikinformationen“ für die Lagepläne bzw. Anfahrtsskizzen geschaffen werden. Diese wird zum einen benötigt, um die Dokumente kategorisiert ins d3 hochzuladen, zum anderen dafür, dass klar ist, welche Dokumente tatsächlich an die Dispo übertragen werden sollen. Denn am Vertrag können beliebige Dokumente hochgeladen werden. 
Die Dokumentenart wird nur pdf-Dateien zulassen. Fotos müssen also vor dem Hochladen in eine entsprechende pdf-Datei umgewandelt werden. 
Weiterhin muss dann eine Übertragung des Dokuments ans P&D stattfinden, wenn ein Auftrag/Service zu einem solchen Vertrag angelegt wird. Es läuft im SAP-System ein Job, der regelmäßig aus den entstandenen Aufträgen aus dem IC/ der EWAORDCREA Services für das P&D macht. Das Programm aus dem Job kann auch manuell über die EWAORDERDOWN angesteuert werden, indem das Formular /PLCP/WA_ORDER_DOWNLOAD genutzt wird. Diese Erzeugung von Services sollte angepasst werden. 
Hier sollte neben der klassischen Serviceanlage ebenfalls der Vertrag geprüft werden. Wenn am Vertrag ein Dokument/ Dokumente der Dokumentenart Z_LOG_DATA hinterlegt sind, müssen diese Dokumente aus dem d3 abgerufen werden (liegen nicht im SAP selbst) und im P&D an den gerade erzeugten Service angehangen werden. 
So sollen die Disponenten in der Lage sein, die Lagepläne, Anfahrtsskizzen oder ähnliches im P&D aufzurufen. 
Es muss geprüft werden, ob das BMS-Anhänge anbietet und ob die BMS-Schnittstelle die Anhänge somit auch dem Fahrer zur Verfügung stellen kann. Für die Fälle, in denen ohne BMS gefahren wird, kann der Disponent die Dokumente öffnen und drucken. (nicht Bestandteil der Umsetzung)
Im P&D liegen die Dokumente vorerst direkt gespeichert. Auf lange Sicht soll ein Löschreport die Dokumente veralteter Services löschen. (nicht Bestandteil der Umsetzung). 

4.	Testszenarien/Akzeptanzkriterien
#	Szenario	Erwartetes Ergebnis
1.	Übers IC einen Vertrag anlegen oder einen vorhandenen Vertrag benutzen und einen Lageplan hochladen über die Funktion „Business Document ablegen“ 	Es gibt eine Option die Datei unter „Logistikinformationen“ abzulegen. Der Lageplan muss im Format pdf vorliegen. Das Dokument wird gesichert im d3 und kann dort auch eingesehen werden. (nicht prüfbar durch Prologa) Der Vertrag kann gesichert werden. 
2.	Aufträge zu dem Vertrag anlegen	Im P&D ist an den Services, die aus den Aufträgen entstanden sind, die Dokumente hinterlegt und einsehbar. 
		

5.	Alternativen
-	Übertragung der Informationen per Mail oder Ausdruck. 
6.	Aufwand
???
