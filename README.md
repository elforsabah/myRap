Log-dbg.js:499 2025-12-03 06:17:17.954000 Error: resource zpdattachment/changes/changes-bundle.json could not be loaded from ../changes/changes-bundle.json. Check for 'file not found' or parse errors. Reason:  -  
g @ Log-dbg.js:499
t.error @ Log-dbg.js:249
(anonymous) @ LoaderExtensions-dbg.js:350
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
i.loadResource @ LoaderExtensions-dbg.js:344
o @ StaticFileConnector-dbg.js:27
loadFlexData @ StaticFileConnector-dbg.js:70
(anonymous) @ Storage-dbg.js:75
s @ Storage-dbg.js:67
Promise.then
p.loadFlexData @ Storage-dbg.js:156
loadFlexData @ Loader-dbg.js:141
L @ FlexState-dbg.js:357
(anonymous) @ FlexState-dbg.js:456
Promise.then
x.initialize @ FlexState-dbg.js:441
M @ ComponentLifecycleHooks-dbg.js:157
m.instanceCreatedHook @ ComponentLifecycleHooks-dbg.js:227
(anonymous) @ Component-dbg.js:2659
c @ Component-dbg.js:2658
f @ Component-dbg.js:2668
p @ Component-dbg.js:2725
(anonymous) @ Component-dbg.js:2777
b.runWithOwner @ ManagedObject-dbg.js:1213
(anonymous) @ Component-dbg.js:2776
Promise.then
(anonymous) @ Component-dbg.js:2764
Promise.then
z @ Component-dbg.js:2749
j.create @ Component-dbg.js:2455
p @ utils-dbg.js:71
instantiateComponent @ Ui5ComponentLoader-dbg.js:240
(anonymous) @ Ui5ComponentLoader-dbg.js:127
Promise.then
createComponent @ Ui5ComponentLoader-dbg.js:126
(anonymous) @ Application-dbg.js:383
Promise.then
createComponent @ Application-dbg.js:382
(anonymous) @ AppLifeCycle-dbg.js:1076
Promise.then
createComponent @ AppLifeCycle-dbg.js:1075
(anonymous) @ Shell-dbg.controller.js:1201
Promise.then
(anonymous) @ Shell-dbg.controller.js:1200
Promise.then
(anonymous) @ Shell-dbg.controller.js:1149
(anonymous) @ jquery-compat-dbg.js:725
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ Shell-dbg.controller.js:1431
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ NavTargetResolution-dbg.js:460
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
(anonymous) @ jquery-dbg.js:4013
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ NavTargetResolution-dbg.js:136
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ ClientSideTargetResolution-dbg.js:234
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
u @ jquery-dbg.js:3500
fireWith @ jquery-dbg.js:3630
t.resolve @ jquery-compat-dbg.js:764
(anonymous) @ ClientSideTargetResolution-dbg.js:465
Promise.then
(anonymous) @ ClientSideTargetResolution-dbg.js:449
u @ jquery-dbg.js:3500
add @ jquery-dbg.js:3559
(anonymous) @ ClientSideTargetResolution-dbg.js:428
Promise.then
S._resolveSingleMatchingTarget @ ClientSideTargetResolution-dbg.js:426
(anonymous) @ ClientSideTargetResolution-dbg.js:353
u @ jquery-dbg.js:3500
add @ jquery-dbg.js:3559
S._resolveHashFragment @ ClientSideTargetResolution-dbg.js:319
(anonymous) @ ClientSideTargetResolution-dbg.js:232
Promise.then
S.resolveHashFragment @ ClientSideTargetResolution-dbg.js:230
(anonymous) @ NavTargetResolution-dbg.js:129
Promise.then
_resolveHashFragmentClientSideAndFixApplicationType @ NavTargetResolution-dbg.js:128
_resolveHashFragmentClientSide @ NavTargetResolution-dbg.js:112
(anonymous) @ NavTargetResolution-dbg.js:64
_nextResolveHashFragment @ NavTargetResolution-dbg.js:91
_nextResolveHashFragment @ NavTargetResolution-dbg.js:94
_invokeResolveHashChain @ NavTargetResolution-dbg.js:474
(anonymous) @ NavTargetResolution-dbg.js:432
u @ jquery-dbg.js:3500
add @ jquery-dbg.js:3559
resolveHashFragment @ NavTargetResolution-dbg.js:407
(anonymous) @ Shell-dbg.controller.js:1418
Promise.then
_resolveHashFragment @ Shell-dbg.controller.js:1417
_doHashChange @ Shell-dbg.controller.js:1128
doHashChange @ Shell-dbg.controller.js:1063
c.treatHashChanged @ ShellNavigationHashChanger-dbg.js:937
c.initShellNavigation @ ShellNavigationHashChanger-dbg.js:692
init @ ShellNavigation-dbg.js:363
(anonymous) @ Shell-dbg.controller.js:312
(anonymous) @ Shell-dbg.controller.js:333
Promise.then
onInit @ Shell-dbg.controller.js:328
n @ Controller-dbg.js:736
r.fireEvent @ EventProvider-dbg.js:241
y.fireEvent @ Element-dbg.js:683
(anonymous) @ ManagedObjectMetadata-dbg.js:826
b.runWithOwner @ ManagedObject-dbg.js:1213
j.runAsOwner @ Component-dbg.js:762
c @ View-dbg.js:586
(anonymous) @ View-dbg.js:623
Promise.then
d._initCompositeSupport @ View-dbg.js:622
(anonymous) @ ManagedObject-dbg.js:526
constructor @ ManagedObject-dbg.js:513
constructor @ Element-dbg.js:209
constructor @ Control-dbg.js:186
c @ Metadata-dbg.js:467
c @ Metadata-dbg.js:467
I @ View-dbg.js:1312
b @ View-dbg.js:1250
s @ View-dbg.js:1080
b.runWithOwner @ ManagedObject-dbg.js:1213
j.runAsOwner @ Component-dbg.js:762
(anonymous) @ View-dbg.js:1096
Promise.then
d.create @ View-dbg.js:1089
M.createContent @ Renderer-dbg.js:547
(anonymous) @ UIComponent-dbg.js:423
b.runWithPreprocessors @ ManagedObject-dbg.js:1186
(anonymous) @ UIComponent-dbg.js:422
b.runWithOwner @ ManagedObject-dbg.js:1213
j.runAsOwner @ Component-dbg.js:762
p.init @ UIComponent-dbg.js:421
init @ Renderer-dbg.js:275
(anonymous) @ ManagedObject-dbg.js:531
constructor @ ManagedObject-dbg.js:513
constructor @ Component-dbg.js:317
constructor @ UIComponent-dbg.js:85
c @ Metadata-dbg.js:467
(anonymous) @ Container-dbg.js:362
(anonymous) @ ui5loader-dbg.js:2103
Promise.then
Oe @ ui5loader-dbg.js:1853
r @ ui5loader-dbg.js:2099
(anonymous) @ Container-dbg.js:356
c @ Container-dbg.js:355
N.createRenderer @ Container-dbg.js:439
init @ init.js:361
await in init
(anonymous) @ init.js:368
c @ ui5loader-dbg.js:1934
Promise.then
Oe @ ui5loader-dbg.js:1853
De @ ui5loader-dbg.js:1972
(anonymous) @ ui5loader-dbg.js:1340
Ee.process @ ui5loader-dbg.js:1338
r @ ui5loader-dbg.js:1437
script
Ae @ ui5loader-dbg.js:1420
qe @ ui5loader-dbg.js:1681
Oe @ ui5loader-dbg.js:1843
r @ ui5loader-dbg.js:2099
setTimeout
q @ Core-dbg.js:194
$._executeInitialization @ Core-dbg.js:1340
$.init @ Core-dbg.js:1223
(anonymous) @ Core-dbg.js:661
r @ Core-dbg.js:299
X.finishTask @ Core-dbg.js:293
(anonymous) @ Core-dbg.js:684
(anonymous) @ Core-dbg.js:1058
Promise.then
$._boot @ Core-dbg.js:1057
(anonymous) @ Core-dbg.js:683
r @ Core-dbg.js:299
X.finishTask @ Core-dbg.js:293
P.y.get.name @ Core-dbg.js:752
Promise.then
P @ Core-dbg.js:751
boot @ Core-dbg.js:726
(anonymous) @ Object-dbg.js:282
(anonymous) @ sap-ui-core.js:2Understand this error
Log-dbg.js:499 2025-12-03 06:17:20.757800 Error: Error found in Fragment (id: '__fragment2').
XML node: '

                 ========= SERVICE WR “LIST REPORT” ========= -->
                ':
Cannot add text nodes as direct child of an aggregation. For adding text to an aggregation, a surrounding html tag is needed. -  
g @ Log-dbg.js:499
t.error @ Log-dbg.js:249
(anonymous) @ ExtensionAPI.ts:303
Promise.catch
n @ ExtensionAPI.ts:302
manualattachments @ ListReportExt.js:18
(anonymous) @ FPMHelper.ts:48
(anonymous) @ ui5loader-dbg.js:2103
Promise.then
Oe @ ui5loader-dbg.js:1853
r @ ui5loader-dbg.js:2099
(anonymous) @ FPMHelper.ts:45
actionWrapper @ FPMHelper.ts:15
y @ ExpressionParser-dbg.js:412
(anonymous) @ ExpressionParser-dbg.js:735
l @ ExpressionParser-dbg.js:915
l.getExternalValue @ CompositeBinding-dbg.js:316
u @ EventHandlerResolver-dbg.js:349
(anonymous) @ EventHandlerResolver-dbg.js:173
r.fireEvent @ EventProvider-dbg.js:241
y.fireEvent @ Element-dbg.js:683
(anonymous) @ ManagedObjectMetadata-dbg.js:826
S.ontap @ Button-dbg.js:599
y._handleEvent @ Element-dbg.js:352
N._handleEvent @ UIArea-dbg.js:1054
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
L @ jquery-mobile-custom-dbg.js:1465
dispatch @ jquery-dbg.js:5430
y.handle @ jquery-dbg.js:5234Understand this error
XMLTemplateProcessor-dbg.js:1585 Uncaught (in promise) Error: Error found in Fragment (id: '__fragment2').
XML node: '

                 ========= SERVICE WR “LIST REPORT” ========= -->
                ':
Cannot add text nodes as direct child of an aggregation. For adding text to an aggregation, a surrounding html tag is needed.
    at Y (XMLTemplateProcessor-dbg.js:1585:14)
