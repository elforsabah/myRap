2025-08-17 15:27:29.802800 Failed to update path undefined - Error: Must not change a property before it has been read
    at c.setValue (ODataPropertyBinding-dbg.js:766:22)
    at sap.ui.predefine.o._setBoundValue (PropertyBinding-dbg.js:135:9)
    at sap.ui.predefine.o.setExternalValue (PropertyBinding-dbg.js:249:17)
    at ManagedObjectBindingSupport-dbg.js:347:23
    at SyncPromise-dbg.js:314:14
    at e (SyncPromise-dbg.js:63:4)
    at new r (SyncPromise-dbg.js:230:3)
    at r.then (SyncPromise-dbg.js:313:7)
    at c.updateModelProperty (ManagedObjectBindingSupport-dbg.js:346:34)
    at /test/c.setProperty (https://port8081-workspaces-ws-bhqcv.eu10.applicationstudio.cloud.sap/resources/sap-ui-core.js:398:7973) sap.ui.model.odata.v4.ODataPropertyBinding
g @ Log-dbg.js:499
(anonymous) @ Log-dbg.js:249
T.reportError @ ODataModel-dbg.js:2380
h @ ODataPropertyBinding-dbg.js:751
c.setValue @ ODataPropertyBinding-dbg.js:766
sap.ui.predefine.o._setBoundValue @ PropertyBinding-dbg.js:135
sap.ui.predefine.o.setExternalValue @ PropertyBinding-dbg.js:249
(anonymous) @ ManagedObjectBindingSupport-dbg.js:347
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
(anonymous) @ SyncPromise-dbg.js:313
updateModelProperty @ ManagedObjectBindingSupport-dbg.js:346
(anonymous) @ ManagedObject-dbg.js:1517
(anonymous) @ Element-dbg.js:536
y.setProperty @ InputBase-dbg.js:769
y.setValue @ InputBase-dbg.js:1142
U.setValue @ Input-dbg.js:2577
y.onChange @ InputBase-dbg.js:568
y.onsapenter @ InputBase-dbg.js:651
U.onsapenter @ Input-dbg.js:1497
(anonymous) @ Element-dbg.js:352
(anonymous) @ UIArea-dbg.js:1054
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234Understand this error
SyncPromise-dbg.js:352 Uncaught Error: Must not change a property before it has been read
    at c.setValue (ODataPropertyBinding-dbg.js:766:22)
    at sap.ui.predefine.o._setBoundValue (PropertyBinding-dbg.js:135:9)
    at sap.ui.predefine.o.setExternalValue (PropertyBinding-dbg.js:249:17)
    at ManagedObjectBindingSupport-dbg.js:347:23
    at SyncPromise-dbg.js:314:14
    at e (SyncPromise-dbg.js:63:4)
    at new r (SyncPromise-dbg.js:230:3)
    at r.then (SyncPromise-dbg.js:313:7)
    at c.updateModelProperty (ManagedObjectBindingSupport-dbg.js:346:34)
    at c.setProperty (ManagedObject-dbg.js:1517:8)
c.setValue @ ODataPropertyBinding-dbg.js:766
sap.ui.predefine.o._setBoundValue @ PropertyBinding-dbg.js:135
sap.ui.predefine.o.setExternalValue @ PropertyBinding-dbg.js:249
(anonymous) @ ManagedObjectBindingSupport-dbg.js:347
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
(anonymous) @ SyncPromise-dbg.js:313
updateModelProperty @ ManagedObjectBindingSupport-dbg.js:346
(anonymous) @ ManagedObject-dbg.js:1517
(anonymous) @ Element-dbg.js:536
y.setProperty @ InputBase-dbg.js:769
y.setValue @ InputBase-dbg.js:1142
U.setValue @ Input-dbg.js:2577
y.onChange @ InputBase-dbg.js:568
y.onsapenter @ InputBase-dbg.js:651
U.onsapenter @ Input-dbg.js:1497
(anonymous) @ Element-dbg.js:352
(anonymous) @ UIArea-dbg.js:1054
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234Understand this error
Main.controller.js:18 Uncaught TypeError: oCurrentStep.getId is not a function
    at c.onNextStep (Main.controller.js:18:30)
    at c.fireEvent (EventProvider-dbg.js:241:38)
    at c.fireEvent (Element-dbg.js:683:44)
    at c.firePress (ManagedObjectMetadata-dbg.js:826:49)
    at S.ontap (Button-dbg.js:599:9)
    at c._handleEvent (Element-dbg.js:352:10)
    at constructor._handleEvent (UIArea-dbg.js:1054:15)
    at HTMLDivElement.dispatch (jquery-dbg.js:5430:27)
    at c (jquery-mobile-custom-dbg.js:1907:20)
    at HTMLDivElement.d (jquery-mobile-custom-dbg.js:2030:6)
onNextStep @ Main.controller.js:18
(anonymous) @ EventProvider-dbg.js:241
(anonymous) @ Element-dbg.js:683
(anonymous) @ ManagedObjectMetadata-dbg.js:826
S.ontap @ Button-dbg.js:599
(anonymous) @ Element-dbg.js:352
(anonymous) @ UIArea-dbg.js:1054
dispatch @ jquery-dbg.js:5430
c @ jquery-mobile-custom-dbg.js:1907
d @ jquery-mobile-custom-dbg.js:2030
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234
trigger @ jquery-dbg.js:8823
(anonymous) @ jquery-dbg.js:8901
each @ jquery-dbg.js:385
each @ jquery-dbg.js:207
trigger @ jquery-dbg.js:8900
j @ jquery-mobile-custom-dbg.js:1455
L @ jquery-mobile-custom-dbg.js:1465
dispatch @ jquery-dbg.js:5430
(anonymous) @ jquery-dbg.js:5234Understand this error
