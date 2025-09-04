2025-09-04 05:54:32.159800 Failed to invoke /ZI_WR_WEIGHINGSESSION(Sessionid=05744661-bb00-1fe0-a2a8-5b7e8d5ae79d,IsActiveEntity=false)/com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...) - Contract is Invalid
Error: Communication error: 400 Bad Request
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at /resources/sap/ui/core/library-preload.js:2352:12323 sap.ui.model.odata.v4.ODataContextBinding
u @ Log-dbg.js:497
(anonymous) @ Log-dbg.js:247
k.reportError @ ODataModel-dbg.js:2507
(anonymous) @ ODataContextBinding-dbg.js:277
Promise.then
(anonymous) @ SyncPromise-dbg.js:318
(anonymous) @ SyncPromise-dbg.js:255
l._invoke @ ODataContextBinding-dbg.js:275
l.invoke @ ODataContextBinding-dbg.js:1415
(anonymous) @ Main.controller.js:147
Promise.then
onNextStep @ Main.controller.js:143
(anonymous) @ PageController.ts:108
(anonymous) @ ManagedObject-dbg.js:1216
(anonymous) @ Component-dbg.js:800
ye @ PageController.ts:108
(anonymous) @ Main.controller.js:80
r @ Element-dbg.js:345
(anonymous) @ Element-dbg.js:365
(anonymous) @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234Understand this error
Main.controller.js:201 Error in create or identifyCard:  Error: Contract is Invalid
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at _Requestor-dbg.js:1422:5
(anonymous) @ Main.controller.js:201
Promise.catch
onNextStep @ Main.controller.js:200
(anonymous) @ PageController.ts:108
(anonymous) @ ManagedObject-dbg.js:1216
(anonymous) @ Component-dbg.js:800
ye @ PageController.ts:108
(anonymous) @ Main.controller.js:80
r @ Element-dbg.js:345
(anonymous) @ Element-dbg.js:365
(anonymous) @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234Understand this error
