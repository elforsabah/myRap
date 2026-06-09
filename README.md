Stand (nach meinem Verständnis):

Authorization (Anmelden + Token empfangen + dann POST mit Token) erfolgreich.
erste Aufrufe für den Endpunkt /api/container/create-order-halle mit Beispiel-JSON erfolgreich.
Einbauen der Methoden in Funktion “an BMS freigeben” ist in Arbeit.
 

Und damit als nächstes offen:

Einbauen der Methoden in Funktion “an BMS freigeben” abschließen
Können wir dabei temporär für unsere internen Tests die Aufrufe mit Payload und BMS-Response protokollieren? (Z-Tabelle o.ä.)
Vorbereiten Funktion “BMS Storno”
Für ganze Tour und auch einzelne Services.
Auch aus anderen Prozessen im SAP heraus (z.B. Kundenservice / Interaction Center) werden wir technisch eine Methode benötigen, um schon an BMS gesendete Services zu stornieren.
Wie funktioniert es? → gleicher Endpunkt und im Feld “status” den Wert “STORNIERT” setzen. → Aufruf für jeden Service einzeln!
(Ich kläre, ob “status”=STORNIERT" und die “orderNumber” für das Storno im BMS ausreicht oder ob wir dabei nochmal alle Daten senden müssen.)
Vorbereiten/Einbauen Anzeige “BMS-Status” in P&D-Planungscockpit.
In Tour-Liste für die ganze Tour.
In Service-Liste für jeden Service.
Hintergrund: Wir übergeben an den BMS-Endpunkt mit jedem Aufruf/JSON nur genau 1 Service. Das heißt bei “Tour an BMS freigeben” wird der Endpunkt für jeden verplanten Service der Tour einzeln nacheinander aufgerufen. Wir müssen uns für jeden einzelnen Service also den Status speichern, ob dieser an BMS freigegeben ist. Es kann passieren, dass manche Services einer Tour erfolgreich übermittelt werden (=freigegeben), aber andere einen Fehler verursachen und damit noch nicht freigegeben sind.
Eine genaue Status-Folge für die Services wird aktuell mit BMS abgestimmt.
(ungefähr so: initial/leer → freigegeben → begonnen → beendet)
Auf Tour-Ebene können wir im SAP den Status von den zugehörigen Services ableiten.
Iterative Tests und Detail-Arbeiten für die einzelnen Felder des Payloads (mit ggf. Mappings und Erweiterungen um neue Felder, sobald diese am BMS-Endpunkt verfügbar
