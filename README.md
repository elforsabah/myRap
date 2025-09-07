// Helper function to convert Base64 to Blob (add this to your controller)
_base64ToBlob: function (base64, mimeType) {
    var byteCharacters = atob(base64);
    var byteNumbers = new Array(byteCharacters.length);
    for (var i = 0; i < byteCharacters.length; i++) {
        byteNumbers[i] = byteCharacters.charCodeAt(i);
    }
    var byteArray = new Uint8Array(byteNumbers);
    return new Blob([byteArray], { type: mimeType });
},

// In onConfirmStep3, after extracting sB64...
if (sB64 && typeof sB64 === "string") {
    try {
        var blob = this._base64ToBlob(sB64, "application/pdf");
        var url = URL.createObjectURL(blob);

        // Open in new tab with iframe (rest of your code remains the same)
        var win = window.open("");
        if (win && win.document) {
            win.document.title = "weighing_slip.pdf";
            win.document.body.style.margin = "0";
            var iframe = win.document.createElement("iframe");
            iframe.style.border = "0";
            iframe.style.width = "100%";
            iframe.style.height = "100%";
            iframe.src = url;
            win.document.body.appendChild(iframe);

            // Optional: Auto-print
            // iframe.onload = function () {
            //     try { iframe.contentWindow.print(); } catch (e) {}
            // };
        } else {
            // Fallback: Download
            var link = document.createElement("a");
            link.href = url;
            link.download = "weighing_slip.pdf";
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }

        // Revoke the URL after use to free memory
        setTimeout(function () {
            URL.revokeObjectURL(url);
        }, 60000); // Adjust timeout as needed

        MessageToast.show("PDF generated.");
    } catch (oError) {
        console.error("Base64 to Blob conversion failed: ", oError);
        MessageToast.show("Failed to process PDF data.");
    }
} else {
    MessageToast.show("No PDF returned by printSlip.");
}
