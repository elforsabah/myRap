2025-09-04 22:47:05.277000 $batch failed - Error: Communication error: 504 error
    at Object.createError (_Helper-dbg.js:609:15)
    at Object.<anonymous> (_Requestor-dbg.js:2088:24)
    at Object.<anonymous> (jquery-compat-dbg.js:725:30)
    at u (jquery-dbg.js:3500:31)
    at Object.fireWith [as rejectWith] (jquery-dbg.js:3630:7)
    at k (jquery-dbg.js:9913:14)
    at /test/XMLHttpRequest.<anonymous> (https://port8080-workspaces-ws-bhqcv.eu10.applicationstudio.cloud.sap/resources/sap-ui-core.js:1196:86804) sap.ui.model.odata.v4.ODataModel
u @ Log-dbg.js:497
n.error @ Log-dbg.js:247
k.reportError @ ODataModel-dbg.js:2507
(anonymous) @ ODataModel-dbg.js:457
Promise.then
r.then @ SyncPromise-dbg.js:318
r.catch @ SyncPromise-dbg.js:255
k._submitBatch @ ODataModel-dbg.js:456
n @ ODataModel-dbg.js:639
setTimeout
(anonymous) @ ODataModel-dbg.js:655
setTimeout
k.addPrerenderingTask @ ODataModel-dbg.js:654
onCreateGroup @ ODataModel-dbg.js:376
y.getOrCreateBatchQueue @ _Requestor-dbg.js:971
(anonymous) @ _Requestor-dbg.js:1859
y.request @ _Requestor-dbg.js:1858
p @ _Cache-dbg.js:4094
P.post @ _Cache-dbg.js:4165
l.createCacheAndRequest @ ODataContextBinding-dbg.js:685
(anonymous) @ ODataContextBinding-dbg.js:256
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
l._invoke @ ODataContextBinding-dbg.js:236
l.invoke @ ODataContextBinding-dbg.js:1415
(anonymous) @ Main.controller.js:308
Promise.then
onWeighStep3 @ Main.controller.js:299
(anonymous) @ PageController.ts:108
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
ye @ PageController.ts:108
r.fireEvent @ EventProvider-dbg.js:240
m.fireEvent @ Element-dbg.js:798
(anonymous) @ ManagedObjectMetadata-dbg.js:826
S.ontap @ Button-dbg.js:599
m._handleEvent @ Element-dbg.js:361
N._handleEvent @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
c @ jquery-mobile-custom-dbg.js:1907
d @ jquery-mobile-custom-dbg.js:2030
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234
trigger @ jquery-dbg.js:8823
(anonymous) @ jquery-dbg.js:8901
each @ jquery-dbg.js:385
each @ jquery-dbg.js:207
trigger @ jquery-dbg.js:8900
j @ jquery-mobile-custom-dbg.js:1455
H @ jquery-mobile-custom-dbg.js:1560
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234Understand this error
Log-dbg.js:497 2025-09-04 22:47:05.277500 Failed to invoke /ZI_WR_WEIGHINGSESSION(Sessionid=05744661-bb00-1fe0-a2ba-02b4094a279d,IsActiveEntity=false)/com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.determineWeight(...) - Error: HTTP request was not processed because $batch failed
    at /resources/sap/ui/core/library-preload.js:2352:12356 sap.ui.model.odata.v4.ODataContextBinding
u @ Log-dbg.js:497
n.error @ Log-dbg.js:247
k.reportError @ ODataModel-dbg.js:2507
(anonymous) @ ODataContextBinding-dbg.js:277
Promise.then
r.then @ SyncPromise-dbg.js:318
r.catch @ SyncPromise-dbg.js:255
l._invoke @ ODataContextBinding-dbg.js:275
l.invoke @ ODataContextBinding-dbg.js:1415
(anonymous) @ Main.controller.js:308
Promise.then
onWeighStep3 @ Main.controller.js:299
(anonymous) @ PageController.ts:108
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
ye @ PageController.ts:108
r.fireEvent @ EventProvider-dbg.js:240
m.fireEvent @ Element-dbg.js:798
(anonymous) @ ManagedObjectMetadata-dbg.js:826
S.ontap @ Button-dbg.js:599
m._handleEvent @ Element-dbg.js:361
N._handleEvent @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
c @ jquery-mobile-custom-dbg.js:1907
d @ jquery-mobile-custom-dbg.js:2030
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234
trigger @ jquery-dbg.js:8823
(anonymous) @ jquery-dbg.js:8901
each @ jquery-dbg.js:385
each @ jquery-dbg.js:207
trigger @ jquery-dbg.js:8900
j @ jquery-mobile-custom-dbg.js:1455
H @ jquery-mobile-custom-dbg.js:1560
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234Understand this error
Main.controller.js:321 Error in determineWeight:  Error: HTTP request was not processed because $batch failed
    at _Requestor-dbg.js:1424:25Caused by: Error: Communication error: 504 error
    at Object.createError (_Helper-dbg.js:609:15)
    at Object.<anonymous> (_Requestor-dbg.js:2088:24)
    at Object.<anonymous> (jquery-compat-dbg.js:725:30)
    at u (jquery-dbg.js:3500:31)
    at Object.fireWith [as rejectWith] (jquery-dbg.js:3630:7)
    at k (jquery-dbg.js:9913:14)
    at XMLHttpRequest.<anonymous> (jquery-dbg.js:10172:9)
(anonymous) @ Main.controller.js:321
Promise.catch
onWeighStep3 @ Main.controller.js:320
(anonymous) @ PageController.ts:108
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
ye @ PageController.ts:108
r.fireEvent @ EventProvider-dbg.js:240
m.fireEvent @ Element-dbg.js:798
(anonymous) @ ManagedObjectMetadata-dbg.js:826
S.ontap @ Button-dbg.js:599
m._handleEvent @ Element-dbg.js:361
N._handleEvent @ UIArea-dbg.js:1056
dispatch @ jquery-dbg.js:5430
c @ jquery-mobile-custom-dbg.js:1907
d @ jquery-mobile-custom-dbg.js:2030
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234
trigger @ jquery-dbg.js:8823
(anonymous) @ jquery-dbg.js:8901
each @ jquery-dbg.js:385
each @ jquery-dbg.js:207
trigger @ jquery-dbg.js:8900
j @ jquery-mobile-custom-dbg.js:1455
H @ jquery-mobile-custom-dbg.js:1560
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234Understand this error
