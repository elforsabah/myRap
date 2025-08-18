Main.controller.js:83 Error: Accessed value is not primitive: /ZI_WR_WEIGHINGSESSION(Vbeln='',Sessionid=00000000-0000-0000-0000-000000000000,IsActiveEntity=true)/SAP__Messages
    at Context-dbg.js:711:14
    at SyncPromise-dbg.js:314:14
    at e (SyncPromise-dbg.js:63:4)
    at new r (SyncPromise-dbg.js:230:3)
    at r.then (SyncPromise-dbg.js:313:7)
    at a.fetchPrimitiveValue (Context-dbg.js:706:37)
    at a.getProperty (Context-dbg.js:1016:23)
    at c.<anonymous> (Main.controller.js:73:65)

﻿

  MessageToast.show("Contract validated successfully.");
                        console.log("Entity Data:", oResult.getObject())
                        console.log("Entity Messages:", oResult.getProperty("SAP__Messages").all)
