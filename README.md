_captureScannerBurst: function () {
  const d = jQuery.Deferred();
  let buf = "", last = Date.now();
  const MAX_INTERVAL = 35, TIMEOUT_MS = 1500;

  const finish = (v)=>{ detach(); d.resolve(v && v.trim()); };
  const onKey = (e)=>{
    const now = Date.now();
    if (now - last > MAX_INTERVAL) { buf = ""; }
    last = now;
    if (e.key === "Enter" || e.key === "Tab") { e.preventDefault(); finish(buf); return; }
    if (e.key.length === 1) { buf += e.key; }
  };
  const detach = ()=>{ document.removeEventListener("keydown", onKey, true); clearTimeout(timer); };
  document.addEventListener("keydown", onKey, true);
  const timer = setTimeout(()=>finish(buf), TIMEOUT_MS);

  return d.promise();
}

