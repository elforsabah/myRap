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

    // Keep only valid Base64 chars (optional hardening)
    s = s.replace(/[^A-Za-z0-9+/=]/g, "");

    // Pad with '=' to multiple of 4
    var pad = s.length % 4;
    if (pad) s += "=".repeat(4 - pad);

    return { kind: "b64", data: s };
},

_base64ToBlob: function (input, mimeType) {
    var norm = this._normalizeToStdBase64(input);

    if (norm.kind === "u8") {
        return new Blob([norm.data], { type: mimeType || "application/octet-stream" });
    }

    var sB64 = norm.data;

    // Safety check: short, early validation to avoid atob crash
    // (Base64 string should only contain A–Z a–z 0–9 + / and up to two = at end)
    if (!/^[A-Za-z0-9+/]+={0,2}$/.test(sB64)) {
        throw new Error("Input is not valid Base64 after normalization.");
    }

    var byteString = atob(sB64); // now safe
    var len = byteString.length;
    var bytes = new Uint8Array(len);
    for (var i = 0; i < len; i++) bytes[i] = byteString.charCodeAt(i);
    return new Blob([bytes], { type: mimeType || "application/octet-stream" });
},
