onScanCard: function () {
  // 1) Put caret in the VBELN field
  const oIp = this.byId("ipContract");
  if (!oIp) { return; }
  oIp.focus();

  // 2) Capture a fast keystroke burst from the scanner
  this._captureScannerBurst().then(function (val) {
    if (!val) { return; }
    // 3) Fill input + bound property, then continue
    oIp.setValue(val);
    const ctx = this.getView().getBindingContext();
    if (ctx) { ctx.setProperty("Vbeln", val); }
    this.onNextStep();                          // reuse your Step-1 flow
  }.bind(this));
},

_captureScannerBurst: function () {
  // Treat sub-35ms intervals as "scanner", stop on Enter or after 1.5s
  return new Promise(function (resolve) {
    let buf = "", last = Date.now();
    const MAX_INTERVAL = 35;      // ms between chars
    const TIMEOUT_MS   = 1500;
    const finish = (v)=>{ detach(); resolve(v && v.trim()); };
    const onKey = (e)=>{
      const now = Date.now();
      if (now - last > MAX_INTERVAL) { buf = ""; } // manual typing → new attempt
      last = now;
      if (e.key === "Enter") { e.preventDefault(); finish(buf); return; }
      if (e.key.length === 1) { buf += e.key; }   // only printable chars
    };
    const detach = ()=>{
      document.removeEventListener("keydown", onKey, true);
      clearTimeout(timer);
    };
    document.addEventListener("keydown", onKey, true);
    const timer = setTimeout(()=>finish(buf), TIMEOUT_MS);
  });
}
