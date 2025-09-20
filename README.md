_printBase64PdfSilently: function (sB64) {
    // Ensure it's a string (some backends return ArrayBuffer/Uint8Array)
    if (sB64 && sB64.constructor !== String) {
        try { sB64 = atob(btoa(String.fromCharCode.apply(null, new Uint8Array(sB64)))); }
        catch (e) { sB64 = typeof sB64.toString === "function" ? sB64.toString() : String(sB64); }
    }
    // Create a Blob URL (more reliable than giant data-URIs)
    var byteChars = atob(sB64);
    var byteNums = new Array(byteChars.length);
    for (var i = 0; i < byteChars.length; i++) byteNums[i] = byteChars.charCodeAt(i);
    var blob = new Blob([new Uint8Array(byteNums)], { type: "application/pdf" });
    var url = URL.createObjectURL(blob);
    // For testing: Download the PDF instead of printing
    var link = document.createElement('a');
    link.href = url;
    link.download = 'test.pdf';
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    // Cleanup
    setTimeout(function () {
        URL.revokeObjectURL(url);
    }, 1000);
},
