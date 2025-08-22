The goal is to:

Focus the input field when the "Scan Card" button is pressed.
Let the scanner input the Vbeln automatically.
Optionally auto-trigger the next step if the scanner appends a special character (like Enter).

Your existing code already has good foundations: the input field (ipContract), a button (btnScanCard) wired to onScanCard, and Enter key handling (onsapenter) on the input to call onNextStep. We'll build on that.
Step 1: Understand the Hardware and Environment Setup

RFID Scanner Configuration:

Ensure the RFID scanner is connected to the device running the UI5 app (e.g., via USB, Bluetooth, or integrated into the kiosk as shown in the screenshot).
Configure the scanner to output in "keyboard wedge" mode (emulate keystrokes). Most scanners (e.g., from Honeywell, Zebra, or similar) have a configuration sheet or software to set this.

Set the output format to: Scan the RFID tag's data (which should contain the Vbeln), optionally prefixed/suffixed with characters.
Common: Append a "suffix" like Enter (ASCII 13) or Tab (ASCII 9) after the data. This auto-submits the form or triggers events.
Example: If the card's RFID tag encodes "1234567890" as the Vbeln, the scanner should output "1234567890" followed by Enter.


Test this outside the app: Open a text editor on the device, focus it, and scan a card. It should type the Vbeln automatically.
