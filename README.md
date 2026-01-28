2. Die Code-Analyse (Die Prüfung)
Der Fehler wird in der Methode CHECK_DESTINATION der Klasse /PLCE/CL_BGPROCESSING_HELPER ausgelöst.

Was der Code tut: Er versucht, per READ ENTITIES die Konfiguration für den "Prozesskanal 01" zu lesen (Zeile 3-5).

Das Scheitern: Das Ergebnis ist leer (lprocessingconfig ist initial).

Die Folge: Weil keine Daten gefunden werden, wirft der Code die Ausnahme missing_inbounddest (Zeile 10-16).




Ein Blick in die Datenbanktabelle /PLCE/TBGPCONFIG (bzw. die View /PLCE/R_BGProcessingConfig_S) zeigt, dass die Konfiguration tatsächlich existiert.

Der Befund: Es gibt einen klaren Eintrag für den Verarbeitungskanal 01, der auf die Eingangsverbindung IWFND_BGRFC_DEST zeigt.

Fazit: Die Konfiguration ist technisch korrekt vorhanden. Der Code sollte diese Zeile eigentlich finden.


