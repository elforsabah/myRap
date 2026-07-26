1. Daher kam der Wunsch auf: Können wir die wichtigsten Daten vor der Verarbeitung prüfen? Aus meiner Sicht wäre das mindestens die Vertragsnummer:

wenn es keine gültige/existierende Vertragsnummer ist (existiert nicht in VBAK oder VBAK-VBTYP <>"G"), dann Fehlermeldung “Vertragsnummer XYZ ist ungültig.” oder so ähnlich.
 

2. Die erzeugten SD-Orders hatten jetzt immer die Verkaufsbelegart “ZLRA” (Lanf Rechnung Allgemein). Eigentlich müsste das SAP SD aber von sich aus immer die Standard-Folgebelegart von der Vertragsart übernehmen. So funktioniert es zumindest manuell. Oder musst du das im Coding händisch festlegen/ermitteln?

Der Vertrag 1200000002 ist zum Beispiel ein “ZKDV” (Dauervertrag Entsorgung) und dessen Folgebelegart wäre “ZLDV” (siehe TVAK-AUARA). 
