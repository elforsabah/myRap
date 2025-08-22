Log-dbg.js:497 2025-08-22 10:29:36.303399 Failed to request side effects - Error: HTTP request was not processed because the previous request failed
    at _Requestor-dbg.js:1361:21
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at /resources/sap/ui/co…v4.ODataListBinding
Log-dbg.js:497 2025-08-22 10:29:36.304000 Failed to invoke /ZI_WR_WEIGHINGSESSION(Vbeln='',Sessionid=05744661-bb00-1fe0-9fe6-43b187c62798,IsActiveEntity=false)/com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...) - Contract is not valid
Error: Communication error: 400 Bad Request
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at /resources/sap/ui/co…ODataContextBinding

Log-dbg.js:497 2025-08-22 10:29:36.305600 Error while executing action  -  
Log-dbg.js:497 2025-08-22 10:29:36.305800 Error while requesting side effects - Error: HTTP request was not processed because the previous request failed  
 Error: HTTP request was not processed because the previous request failed
    at _Requestor-dbg.js:1361:21
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at _Requestor-dbg.js:1422:5
Caused by: Error: Contract is not valid
    at Object.createError (_Helper-dbg.js:609:15)
    at _Requestor-dbg.js:1371:23
    at Array.forEach (<anonymous>)
    at a (_Requestor-dbg.js:1353:15)
    at _Requestor-dbg.js:1422:5
Main.controller.js:165 Action Error: Error: Error in EditFlow.invokeAction:actionExecutionFailed
    at n.t [as invokeAction] (EditFlow.ts:2300:11)
