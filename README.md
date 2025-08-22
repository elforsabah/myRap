if (this._isFromScan) {  // Set this flag in onScanCard: this._isFromScan = true;
    // Optional: Custom message or retry logic
}
// ... existing action call ...
.catch(function (oError) {
    MessageBox.error("Invalid Card Scan. Please try again or enter manually.");
    this._isFromScan = false;
}.bind(this));
