Log-dbg.js:497 2025-09-03 21:57:27.024600 Failed to request side effects - Error: HTTP request was not processed because the previous request failed
    at _Requestor-dbg.js:1361:21
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at /resources/sap/ui/core/library-preload.js:2352:12323 sap.ui.model.odata.v4.ODataListBinding
u @ Log-dbg.js:497
(anonymous) @ Log-dbg.js:247
k.reportError @ ODataModel-dbg.js:2507
(anonymous) @ ODataListBinding-dbg.js:4189
Promise.catch
f @ ODataListBinding-dbg.js:4188
y.requestSideEffects @ ODataListBinding-dbg.js:4232
a.requestSideEffectsInternal @ Context-dbg.js:2004
(anonymous) @ Context-dbg.js:1925
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
(anonymous) @ SyncPromise-dbg.js:313
a.requestSideEffects @ Context-dbg.js:1920
t @ SideEffectsServiceFactory.ts:552
z @ facade.ts:2228
S @ facade.ts:1965
t @ facade.ts:2038
(anonymous) @ facade.ts:2054
await in (anonymous)
T @ facade.ts:2052
(anonymous) @ facade.ts:2074
K @ facade.ts:1883
(anonymous) @ facade.ts:530
R @ facade.ts:318
B @ facade.ts:188
await in B
e @ TransactionHelper.ts:1053
t @ EditFlow.ts:2187
await in t
(anonymous) @ ControllerExtension-dbg.js:105
onNextStep @ Main.controller.js:127
(anonymous) @ PageController.ts:108
(anonymous) @ ManagedObject-dbg.js:1216
(anonymous) @ Component-dbg.js:800
ye @ PageController.ts:108
(anonymous) @ Main.controller.js:60
r @ Element-dbg.js:345
(anonymous) @ Element-dbg.js:365
(anonymous) @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234Understand this error
Log-dbg.js:497 2025-09-03 21:57:27.025200 Failed to invoke /ZI_WR_WEIGHINGSESSION(Sessionid=05744661-bb00-1fe0-a2a0-068ff2f2279d,IsActiveEntity=false)/com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...) - State of entity CDS~ZI_WR_WEIGHINGSESSION with key 05744661BB001FE0A2A0068FF2F2279D was already changed (stale If-Match)
Error: Communication error: 412 Precondition Failed
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
W @ facade.ts:1737
S @ facade.ts:1949
t @ facade.ts:2038
(anonymous) @ facade.ts:2054
await in (anonymous)
T @ facade.ts:2052
(anonymous) @ facade.ts:2074
K @ facade.ts:1883
(anonymous) @ facade.ts:530
R @ facade.ts:318
B @ facade.ts:188
await in B
e @ TransactionHelper.ts:1053
t @ EditFlow.ts:2187
await in t
(anonymous) @ ControllerExtension-dbg.js:105
onNextStep @ Main.controller.js:127
(anonymous) @ PageController.ts:108
(anonymous) @ ManagedObject-dbg.js:1216
(anonymous) @ Component-dbg.js:800
ye @ PageController.ts:108
(anonymous) @ Main.controller.js:60
r @ Element-dbg.js:345
(anonymous) @ Element-dbg.js:365
(anonymous) @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234Understand this error
Log-dbg.js:497 2025-09-03 21:57:27.026900 Error while executing action  -  
u @ Log-dbg.js:497
(anonymous) @ Log-dbg.js:247
(anonymous) @ facade.ts:1829
Promise.catch
q @ facade.ts:1828
(anonymous) @ facade.ts:1893
K @ facade.ts:1883
(anonymous) @ facade.ts:530
R @ facade.ts:318
B @ facade.ts:188
await in B
e @ TransactionHelper.ts:1053
t @ EditFlow.ts:2187
await in t
(anonymous) @ ControllerExtension-dbg.js:105
onNextStep @ Main.controller.js:127
(anonymous) @ PageController.ts:108
(anonymous) @ ManagedObject-dbg.js:1216
(anonymous) @ Component-dbg.js:800
ye @ PageController.ts:108
(anonymous) @ Main.controller.js:60
r @ Element-dbg.js:345
(anonymous) @ Element-dbg.js:365
(anonymous) @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234Understand this error
Log-dbg.js:497 2025-09-03 21:57:27.027100 Error while requesting side effects - Error: HTTP request was not processed because the previous request failed  
 Error: HTTP request was not processed because the previous request failed
    at _Requestor-dbg.js:1361:21
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at _Requestor-dbg.js:1422:5Caused by: Error: State of entity CDS~ZI_WR_WEIGHINGSESSION with key 05744661BB001FE0A2A0068FF2F2279D was already changed (stale If-Match)
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at _Requestor-dbg.js:1422:5
