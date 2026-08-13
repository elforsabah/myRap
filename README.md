Ein kleines vorgeschaltetes Z-Programm sucht zuerst nach neuen oder geänderten Aufträgen und übergibt nur diese an das Standardprogramm.

Warum funktioniert das ohne Anpassung? Im Selektionsbild gibt es schon die Select-Option ORDER. Die steht im Moment leer da, kann aber ganz einfach eine Liste von Auftragsnummern aufnehmen. Das Standardprogramm braucht also kein neues Feld – es muss nur wissen, welche Aufträge übermittelt werden sollen:

SUBMIT rewaorder_download
  USING SELECTION-SET 'Z_NEXT_7_DAYS'   " Formular, Zeitraum, Druckparameter … alles bleibt wie bisher
  WITH  order IN lr_order               " Nur bei ORDER wird eingeschränkt: nur diese Auftragsnummern
  AND RETURN.

SAP-Standard bleibt unberührt, kein Upgrade-Risiko.

Dazu gibt’s noch eine Merktabelle mit einer Zeile pro Auftrag: Auftragsnummer, Zeit der letzten erfolgreichen Übertragung und Änderungsstand. Die Regel ist simpel: Ein Auftrag wird gesendet, wenn es noch keinen Eintrag gibt oder sich am Auftrag seitdem etwas geändert hat.
