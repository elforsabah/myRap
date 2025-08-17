2025-08-18 00:32:42.078600 POST on 'ZI_WR_WEIGHINGSESSION' failed; will be repeated automatically - Value id-1755469961818-49 is not a valid UUID
Error: Communication error: 400 Bad Request
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
Promise.then
r.then @ SyncPromise-dbg.js:318
v @ _Cache-dbg.js:555
(anonymous) @ _Cache-dbg.js:681
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
f.create @ _Cache-dbg.js:679
(anonymous) @ ODataParentBinding-dbg.js:485
Promise.then
r.then @ SyncPromise-dbg.js:318
s.createInCache @ ODataParentBinding-dbg.js:480
y.create @ ODataListBinding-dbg.js:979
_onObjectMatched @ Main.controller.js:24
(anonymous) @ PageController.ts:108
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
ye @ PageController.ts:108
r.fireEvent @ EventProvider-dbg.js:240
(anonymous) @ Route-dbg.js:178
Promise.then
_routeMatched @ Route-dbg.js:134
(anonymous) @ Route-dbg.js:205
execute @ signals-dbg.js:93
dispatch @ signals-dbg.js:384
dispatch @ signals-dbg.js:187
parse @ crossroads-dbg.js:186
parse @ Router-dbg.js:377
initialize @ Router-dbg.js:451
(anonymous) @ AppComponent.ts:417
Promise.then
o @ AppComponent.ts:413
(anonymous) @ AppComponent.ts:440
Promise.then
t @ AppComponent.ts:438
await in t
(anonymous) @ AsyncComponentServiceFactory.ts:50
Promise.then
t @ AsyncComponentServiceFactory.ts:47
constructor @ Service-dbg.js:101
e @ AsyncComponentServiceFactory.ts:8
t @ AsyncComponentServiceFactory.ts:61
(anonymous) @ Component-dbg.js:1235
(anonymous) @ ui5loader-dbg.js:2163
Promise.then
Oe @ ui5loader-dbg.js:1898
r @ ui5loader-dbg.js:2159
(anonymous) @ Component-dbg.js:1220
k.getService @ Component-dbg.js:1218
(anonymous) @ Component-dbg.js:1296
D @ Component-dbg.js:1292
p @ Component-dbg.js:2752
(anonymous) @ Component-dbg.js:2836
m.runWithOwner @ ManagedObject-dbg.js:1216
(anonymous) @ Component-dbg.js:2835
Promise.then
(anonymous) @ Component-dbg.js:2794
Promise.then
z @ Component-dbg.js:2779
k.create @ Component-dbg.js:2531
i @ utils-dbg.js:72
instantiateComponent @ Ui5ComponentLoader-dbg.js:281
(anonymous) @ Ui5ComponentLoader-dbg.js:134
Promise.then
createComponent @ Ui5ComponentLoader-dbg.js:133
(anonymous) @ Application-dbg.js:383
Promise.then
createComponent @ Application-dbg.js:382
(anonymous) @ AppLifeCycle-dbg.js:1047
Promise.then
E.createComponent @ AppLifeCycle-dbg.js:1046
(anonymous) @ Shell-dbg.controller.js:1213
Promise.then
(anonymous) @ Shell-dbg.controller.js:1212
Promise.then
(anonymous) @ Shell-dbg.controller.js:1161
(anonymous) @ jquery-compat-dbg.js:725
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ Shell-dbg.controller.js:1444
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ NavTargetResolutionInternal-dbg.js:465
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
(anonymous) @ jquery-dbg.js:4013
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ NavTargetResolutionInternal-dbg.js:142
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ ClientSideTargetResolution-dbg.js:243
Promise.then
R.resolveHashFragment @ ClientSideTargetResolution-dbg.js:242
(anonymous) @ NavTargetResolutionInternal-dbg.js:135
Promise.then
_resolveHashFragmentClientSideAndFixApplicationType @ NavTargetResolutionInternal-dbg.js:134
_resolveHashFragmentClientSide @ NavTargetResolutionInternal-dbg.js:118
(anonymous) @ NavTargetResolutionInternal-dbg.js:70
_nextResolveHashFragment @ NavTargetResolutionInternal-dbg.js:97
_nextResolveHashFragment @ NavTargetResolutionInternal-dbg.js:100
_invokeResolveHashChain @ NavTargetResolutionInternal-dbg.js:479
(anonymous) @ NavTargetResolutionInternal-dbg.js:437
u @ jquery-dbg.js:3500
add @ jquery-dbg.js:3559
resolveHashFragment @ NavTargetResolutionInternal-dbg.js:412
(anonymous) @ Shell-dbg.controller.js:1433
Promise.then
_resolveHashFragment @ Shell-dbg.controller.js:1432
_doHashChange @ Shell-dbg.controller.js:1138
doHashChange @ Shell-dbg.controller.js:1075
S.treatHashChanged @ ShellNavigationHashChanger-dbg.js:945
S.initShellNavigation @ ShellNavigationHashChanger-dbg.js:701
init @ ShellNavigationInternal-dbg.js:358
(anonymous) @ Shell-dbg.controller.js:314
(anonymous) @ Shell-dbg.controller.js:336
Promise.then
onInit @ Shell-dbg.controller.js:331
n @ Controller-dbg.js:766
r.fireEvent @ EventProvider-dbg.js:240
m.fireEvent @ Element-dbg.js:798
(anonymous) @ ManagedObjectMetadata-dbg.js:826
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
p @ View-dbg.js:594
(anonymous) @ View-dbg.js:631
Promise.then
g._initCompositeSupport @ View-dbg.js:630
(anonymous) @ ManagedObject-dbg.js:528
constructor @ ManagedObject-dbg.js:515
constructor @ Element-dbg.js:218
constructor @ Control-dbg.js:187
c @ Metadata-dbg.js:504
c @ Metadata-dbg.js:504
_ @ View-dbg.js:1334
I @ View-dbg.js:1266
o @ View-dbg.js:1088
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
(anonymous) @ View-dbg.js:1108
Promise.then
g.create @ View-dbg.js:1097
D.createContent @ Renderer-dbg.js:530
(anonymous) @ UIComponent-dbg.js:394
m.runWithPreprocessors @ ManagedObject-dbg.js:1189
(anonymous) @ UIComponent-dbg.js:393
m.runWithOwner @ ManagedObject-dbg.js:1216
k.runAsOwner @ Component-dbg.js:800
p.init @ UIComponent-dbg.js:392
init @ Renderer-dbg.js:299
(anonymous) @ ManagedObject-dbg.js:533
constructor @ ManagedObject-dbg.js:515
constructor @ Component-dbg.js:354
constructor @ UIComponent-dbg.js:89
c @ Metadata-dbg.js:504
(anonymous) @ Container-dbg.js:409
(anonymous) @ ui5loader-dbg.js:2163
Promise.then
Oe @ ui5loader-dbg.js:1898
r @ ui5loader-dbg.js:2159
(anonymous) @ Container-dbg.js:403
c @ Container-dbg.js:402
T.createRenderer @ Container-dbg.js:486
init @ init.js:284
await in init
(anonymous) @ init.js:289
c @ ui5loader-dbg.js:1984
Promise.then
Oe @ ui5loader-dbg.js:1898
Me @ ui5loader-dbg.js:2022
(anonymous) @ ui5loader-dbg.js:1374
process @ ui5loader-dbg.js:1372
r @ ui5loader-dbg.js:1471
script
Le @ ui5loader-dbg.js:1454
Ie @ ui5loader-dbg.js:1718
Oe @ ui5loader-dbg.js:1888
r @ ui5loader-dbg.js:2159
setTimeout
$ @ Core-dbg.js:261
J._executeInitialization @ Core-dbg.js:1414
J.init @ Core-dbg.js:1297
(anonymous) @ Core-dbg.js:733
r @ Core-dbg.js:366
W.finishTask @ Core-dbg.js:360
(anonymous) @ Core-dbg.js:756
(anonymous) @ Core-dbg.js:1128
Promise.then
J._boot @ Core-dbg.js:1127
(anonymous) @ Core-dbg.js:755
r @ Core-dbg.js:366
W.finishTask @ Core-dbg.js:360
P.g.get.name @ Core-dbg.js:824
Promise.then
P @ Core-dbg.js:823
boot @ Core-dbg.js:798
(anonymous) @ Object-dbg.js:282
(anonymous) @ sap-ui-core.js:2Understand this error
