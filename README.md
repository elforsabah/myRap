025-09-21 13:03:02.031899 [FUTURE FATAL] The registered Event Listener 'onInit' must not have a return value. - com.prologa.zwrweighbrigefinal.ext.main.Main 
u @ Log-dbg.js:497Understand this error
Main.controller.js:154 Waiting for entity creation...
Log-dbg.js:497 2025-09-21 13:03:24.535199 POST on 'ZI_WR_R_WEIGHBRIDGE' failed; will be repeated automatically - Creating operations are disabled for entity '$SRVD#ZSB_WR_WEIGHBRIGE~ZI_WR_R_WEIGHBRIDGE'
Error: Communication error: 405 Method Not Allowed
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at /resources/sap/ui/core/library-preload.js:2352:12323 sap.ui.model.odata.v4.ODataListBinding
u @ Log-dbg.js:497
n.error @ Log-dbg.js:247
k.reportError @ ODataModel-dbg.js:2507
(anonymous) @ ODataListBinding-dbg.js:982
(anonymous) @ _Cache-dbg.js:626
XMLHttpRequest.send
send @ jquery-dbg.js:10224
ajax @ jquery-dbg.js:9805
jQuery.ajax @ jquery-compat-dbg.js:338
p @ _Requestor-dbg.js:2028
(anonymous) @ _Requestor-dbg.js:2097
y.sendRequest @ _Requestor-dbg.js:2011
y.sendBatch @ _Requestor-dbg.js:1943
y.processBatch @ _Requestor-dbg.js:1420
(anonymous) @ _Requestor-dbg.js:2205
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
y.submitBatch @ _Requestor-dbg.js:2198
k._submitBatch @ ODataModel-dbg.js:456
(anonymous) @ ODataModel-dbg.js:2936
n @ ODataModel-dbg.js:639
setTimeout
(anonymous) @ ODataModel-dbg.js:655
setTimeout
k.addPrerenderingTask @ ODataModel-dbg.js:654
(anonymous) @ ODataModel-dbg.js:2935
k.submitBatch @ ODataModel-dbg.js:2934
onNextStep @ Main.controller.js:152
(anonymous) @ PageController.ts:108
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
ye @ PageController.ts:108
(anonymous) @ Main.controller.js:128
r @ Element-dbg.js:345
m._handleEvent @ Element-dbg.js:365
N._handleEvent @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234Understand this error
