Log-dbg.js:497 2025-09-03 18:09:33.925399 Failed to update path /ZI_WR_WEIGHINGSESSION(Sessionid=05744661-bb00-1fe0-a29c-0a6da3e9e79c,IsActiveEntity=false)/Vbeln - Edit not possible: locking of entity ZI_WR_WEIGHINGSESSION failed
Error: Communication error: 423 REASON_423
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at /resources/sap/ui/core/library-preload.js:2352:12323 sap.ui.model.odata.v4.Context
u @ Log-dbg.js:497
n.error @ Log-dbg.js:247
k.reportError @ ODataModel-dbg.js:2507
g @ Context-dbg.js:546
(anonymous) @ _Cache-dbg.js:2237
Promise.then
r.then @ SyncPromise-dbg.js:318
k @ _Cache-dbg.js:2203
(anonymous) @ _Cache-dbg.js:2336
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
f.update @ _Cache-dbg.js:2132
(anonymous) @ Context-dbg.js:590
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
(anonymous) @ Context-dbg.js:530
(anonymous) @ ODataBinding-dbg.js:1581
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
h.withCache @ ODataBinding-dbg.js:1577
a.withCache @ Context-dbg.js:2445
a.doSetProperty @ Context-dbg.js:528
l.setValue @ ODataPropertyBinding-dbg.js:806
(anonymous) @ PropertyBinding-dbg.js:136
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
i._setBoundValue @ PropertyBinding-dbg.js:130
i._setExternalValue @ PropertyBinding-dbg.js:286
i.setExternalValue @ PropertyBinding-dbg.js:253
(anonymous) @ ManagedObjectBindingSupport-dbg.js:328
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
updateModelProperty @ ManagedObjectBindingSupport-dbg.js:327
m.setProperty @ ManagedObject-dbg.js:1520
m.setProperty @ Element-dbg.js:548
m.setProperty @ InputBase-dbg.js:791
m.setValue @ InputBase-dbg.js:1208
U.setValue @ Input-dbg.js:2600
m.onChange @ InputBase-dbg.js:580
m.onsapenter @ InputBase-dbg.js:673
U.onsapenter @ Input-dbg.js:1507
m._handleEvent @ Element-dbg.js:361
N._handleEvent @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234Understand this error
Log-dbg.js:497 2025-09-03 18:09:33.927000 Failed to update path /ZI_WR_WEIGHINGSESSION(Sessionid=05744661-bb00-1fe0-a29c-0a6da3e9e79c,IsActiveEntity=false)/Vbeln - Edit not possible: locking of entity ZI_WR_WEIGHINGSESSION failed
Error: Communication error: 423 REASON_423
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at /resources/sap/ui/core/library-preload.js:2352:12323 sap.ui.model.odata.v4.Context
u @ Log-dbg.js:497
n.error @ Log-dbg.js:247
k.reportError @ ODataModel-dbg.js:2507
(anonymous) @ Context-dbg.js:2288
Promise.catch
a.setProperty @ Context-dbg.js:2284
onNextStep @ Main.controller.js:128
(anonymous) @ PageController.ts:108
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
ye @ PageController.ts:108
(anonymous) @ Main.controller.js:60
r @ Element-dbg.js:345
m._handleEvent @ Element-dbg.js:365
N._handleEvent @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234Understand this error
_Helper-dbg.js:609 Uncaught (in promise) Error: Edit not possible: locking of entity ZI_WR_WEIGHINGSESSION failed
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at _Requestor-dbg.js:1422:5
