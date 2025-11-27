sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    // Keep dialog + ExtensionAPI in module scope
    var oDialog;
    var oExtAPI;   // sap.fe.core.ExtensionAPI

    return {
        /**
         * Custom action handler, wired in manifest.json ("press").
         *
         * @param {object} oContext          – FE context (not needed here)
         * @param {sap.ui.model.Context[]} aSelectedContexts – selected rows of the LR table
         */
        manualattachments: function (oContext, aSelectedContexts) {

            // In V4 FE, "this" is the ExtensionAPI, not a controller
            // See sap.fe.core.ExtensionAPI docs (loadFragment, addDependent, getModel, getEditFlow). 
            oExtAPI = this;

            // If dialog already created -> just open
            if (oDialog) {
                oDialog.open();
                return;
            }

            this.loadFragment({
                id: "TwoSmartTablesDialog",
                name: "zsb_attachment.ext.fragments.GenerateDocDialog",
                controller: {
                    onDialogChoose: function () {
                        // get the SmartTables
                        var oSmartTop = oExtAPI.byId("SmartTableTop");
                        var oSmartBottom = oExtAPI.byId("SmartTableBottom");

                        // underlying inner tables created by SmartTable
                        var oTopTable = oSmartTop.getTable();
                        var oBottomTable = oSmartBottom.getTable();

                        function getSelectedObjects(oInnerTable) {
                            if (!oInnerTable) { return []; }

                            var aContexts = [];
                            if (oInnerTable.getSelectedContexts) {
                                // sap.m.Table
                                aContexts = oInnerTable.getSelectedContexts();
                            } else if (oInnerTable.getSelectedIndices) {
                                // sap.ui.table.Table
                                oInnerTable.getSelectedIndices().forEach(function (iIndex) {
                                    var oCtx = oInnerTable.getContextByIndex(iIndex);
                                    if (oCtx) { aContexts.push(oCtx); }
                                });
                            }
                            return aContexts.map(function (oCtx) { return oCtx.getObject(); });
                        }

                        var aTopSelected = getSelectedObjects(oTopTable);
                        var aBottomSelected = getSelectedObjects(oBottomTable);

                        if (!aTopSelected.length && !aBottomSelected.length) {
                            MessageToast.show("Please select at least one row in one of the tables.");
                            return;
                        }

                        // ... your invokeAction logic stays as you have it ...
                    },

                    onDialogCancel: function () {
                        oDialog.close();
                    }
                }
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                // OPTIONAL: force multi-select mode on the inner tables
                var fnSetMultiSelect = function (oSmart) {
                    var fn = function () {
                        var oInner = oSmart.getTable();
                        if (oInner && oInner.setMode) {
                            oInner.setMode("MultiSelect");          // sap.m.Table
                        } else if (oInner && oInner.setSelectionMode) {
                            oInner.setSelectionMode("MultiToggle");  // sap.ui.table.Table
                        }
                    };
                    if (oSmart.getTable()) {
                        fn();
                    } else {
                        oSmart.attachInitialise(fn);
                    }
                };

                fnSetMultiSelect(oExtAPI.byId("SmartTableTop"));
                fnSetMultiSelect(oExtAPI.byId("SmartTableBottom"));

                oDialog.open();
            });

        },

        // you can keep your second handler as-is
        manualattachments2: function (oContext, aSelectedContexts) {
            MessageToast.show("Custom handler invoked 2.");
        }
    };
});


<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:smartTable="sap.ui.comp.smarttable">

    <!-- Popup dialog -->
    <Dialog
        id="TwoSmartTablesDialog"
        title="Choose items"
        stretch="true"
        contentWidth="900px"
        contentHeight="500px"
        class="sapUiResponsivePadding">

        <content>
            <VBox width="100%" height="100%" renderType="Div" id="vb1">

                <!-- TOP TABLE (blue area) -->
                <smartTable:SmartTable
                    id="SmartTableTop"
                    entitySet="Attachment"
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    showRowCount="true"
                    header="Top items"
                    enableAutoBinding="true"
                    persistencyKey="TopTable" />

                <!-- BOTTOM TABLE (yellow area) -->
                <smartTable:SmartTable
                    id="SmartTableBottom"
                    entitySet="ServiceWR"
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    showRowCount="true"
                    header="Bottom items"
                    enableAutoBinding="true"
                    persistencyKey="BottomTable" />

            </VBox>
        </content>

        <!-- "Button A" -->
        <beginButton>
            <Button
                id="btnChoose"
                text="Choose"
                type="Emphasized"
                press=".onDialogChoose" />
        </beginButton>

        <endButton>
            <Button
                id="btnCancel"
                text="Cancel"
                press=".onDialogCancel" />
        </endButton>

    </Dialog>
</core:FragmentDefinition>




2025-11-27 12:55:53.884800 Error: resource zsb_attachment/ext/fragments/GenerateDocDialog.fragment.xml could not be loaded from ../resources/zsb_attachment/ext/fragments/GenerateDocDialog.fragment.xml. Check for 'file not found' or parse errors. Reason:  -  
g @ Log-dbg.js:499
t.error @ Log-dbg.js:249
(anonymous) @ LoaderExtensions-dbg.js:350
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
i.loadResource @ LoaderExtensions-dbg.js:344
sap.ui.predefine.U.loadTemplate @ XMLTemplateProcessor-dbg.js:268
Ee @ CommonUtils.ts:1599
n @ ExtensionAPI.ts:289
manualattachments @ ListReportExt.js:29
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
(anonymous) @ Button-dbg.js:599
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
Log-dbg.js:499 2025-11-27 12:55:53.885000 Error: resource zsb_attachment/ext/fragments/GenerateDocDialog.fragment.xml could not be loaded from ../resources/zsb_attachment/ext/fragments/GenerateDocDialog.fragment.xml. Check for 'file not found' or parse errors. Reason:  -  
g @ Log-dbg.js:499
t.error @ Log-dbg.js:249
(anonymous) @ ExtensionAPI.ts:303
Promise.catch
n @ ExtensionAPI.ts:302
manualattachments @ ListReportExt.js:29
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
(anonymous) @ Button-dbg.js:599
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
LoaderExtensions-dbg.js:331 Uncaught (in promise) Error: resource zsb_attachment/ext/fragments/GenerateDocDialog.fragment.xml could not be loaded from ../resources/zsb_attachment/ext/fragments/GenerateDocDialog.fragment.xml. Check for 'file not found' or parse errors. Reason: 
    at LoaderExtensions-dbg.js:331:19
    at SyncPromise-dbg.js:314:14
    at e (SyncPromise-dbg.js:63:4)
    at new r (SyncPromise-dbg.js:230:3)
    at r.then (SyncPromise-dbg.js:313:7)
    at i.loadResource (LoaderExtensions-dbg.js:323:5)
    at sap.ui.predefine.U.loadTemplate (XMLTemplateProcessor-dbg.js:268:27)
    at Object.Ee [as templateControlFragment] (CommonUtils.ts:1599:25)
    at e.n [as loadFragment] (ExtensionAPI.ts:289:40)
    at e.manualattachments (ListReportExt.js:29:18)
(anonymous) @ LoaderExtensions-dbg.js:331
(anonymous) @ SyncPromise-dbg.js:314
e @ SyncPromise-dbg.js:63
r @ SyncPromise-dbg.js:230
r.then @ SyncPromise-dbg.js:313
i.loadResource @ LoaderExtensions-dbg.js:323
sap.ui.predefine.U.loadTemplate @ XMLTemplateProcessor-dbg.js:268
Ee @ CommonUtils.ts:1599
n @ ExtensionAPI.ts:289
manualattachments @ ListReportExt.js:29
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
(anonymous) @ Button-dbg.js:599
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



