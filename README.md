Zielsetzung:

Das Eingabefeld fokussieren, sobald der Button „Scan Card“ gedrückt wird.

Den Scanner die Vbeln automatisch eingeben lassen.

Optional den nächsten Schritt automatisch auslösen, wenn der Scanner ein Sonderzeichen (z. B. Enter) anhängt.

Ihr vorhandener Code hat bereits eine gute Basis: das Eingabefeld (ipContract), ein Button (btnScanCard), der mit onScanCard verknüpft ist, und die Enter-Tastenbehandlung (onsapenter) am Input, um onNextStep aufzurufen. Darauf bauen wir auf.

Schritt 1: Hardware- und Umgebungs-Setup verstehen

RFID-Scanner-Konfiguration:

Stellen Sie sicher, dass der RFID-Scanner mit dem Gerät verbunden ist, auf dem die UI5-App läuft (z. B. über USB, Bluetooth oder in den Kiosk integriert, wie im Screenshot gezeigt).

Konfigurieren Sie den Scanner auf „Keyboard-Wedge“-Modus (Tastaturemulation). Die meisten Scanner (z. B. von Honeywell, Zebra o. ä.) verfügen über ein Konfigurationsblatt oder eine Software dafür.

Legen Sie das Ausgabeformat fest: Der RFID-Tag wird ausgelesen (sollte die Vbeln enthalten) und optional mit Präfix-/Suffix-Zeichen versehen. Üblich ist das Anhängen eines „Suffix“ wie Enter (ASCII 13) oder Tab (ASCII 9) nach den Daten. Das sorgt für ein automatisches Absenden des Formulars bzw. löst Events aus.
Beispiel: Wenn der RFID der Karte „1234567890“ als Vbeln codiert, sollte der Scanner „1234567890“ gefolgt von Enter ausgeben.

Test außerhalb der App:
Öffnen Sie einen Texteditor auf dem Gerät, fokussieren Sie ihn und scannen Sie eine Karte. Die Vbeln sollte automatisch geschrieben werden.
