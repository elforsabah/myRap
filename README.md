_normalizeToStdBase64: function (val) {
    // If backend already gave binary (Uint8Array/ArrayBuffer), return as-is
    if (val instanceof Uint8Array) return { kind: "u8", data: val };
    if (val && val.buffer instanceof ArrayBuffer) return { kind: "u8", data: new Uint8Array(val) };

    var s = String(val || "").trim();

    // Strip any data URL prefix like "data:application/pdf;base64,..."
    s = s.replace(/^data:[^;]+;base64,/, "");

    // Remove whitespace/newlines
    s = s.replace(/\s+/g, "");

    // Convert base64url -> base64
    s = s.replace(/-/g, "+").replace(/_/g, "/");

    // Pad with '=' to make length % 4 === 0 (without using repeat)
    var pad = s.length % 4;
    if (pad) {
        s += Array(4 - pad + 1).join("=");
    }

    return { kind: "b64", data: s };
},

_base64ToBlob: function (input, mimeType) {
    var norm = this._normalizeToStdBase64(input);

    if (norm.kind === "u8") {
        return new Blob([norm.data], { type: mimeType || "application/octet-stream" });
    }

    var sB64 = norm.data;

    // Quick validation after normalization
    if (!/^[A-Za-z0-9+/]+={0,2}$/.test(sB64)) {
        throw new Error("Input is not valid Base64 after normalization.");
    }

    var byteString = atob(sB64); // safe now
    var len = byteString.length;
    var bytes = new Uint8Array(len);
    for (var i = 0; i < len; i++) bytes[i] = byteString.charCodeAt(i);

    return new Blob([bytes], { type: mimeType || "application/pdf" });
},
