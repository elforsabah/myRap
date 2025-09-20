/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            //// Printing in Kiosk Mode for Chrome
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
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
                // Hidden iframe
                var iframe = document.createElement("iframe");
                iframe.style.position = "absolute";
                iframe.style.left = "-10000px";
                iframe.style.top = "-10000px";
                iframe.style.width = "0px";
                iframe.style.height = "0px";
                iframe.style.border = "0";
                iframe.style.visibility = "hidden";
                iframe.onload = function () {
                    // Important: call print on the iframe's *contentWindow*
                    setTimeout(function () {
                        try {
                            iframe.contentWindow.focus();
                            iframe.contentWindow.print(); // dialog-less if Chrome has --kiosk-printing
                        } finally {
                            // Cleanup after a short delay to avoid killing the print job
                            setTimeout(function () {
                                URL.revokeObjectURL(url);
                                iframe.parentNode && iframe.parentNode.removeChild(iframe);
                            }, 1000);
                        }
                    }, 200); // give the PDF plugin a moment to render
                };
                iframe.src = url;
                document.body.appendChild(iframe);
            },
