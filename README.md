<!-- ATTACHMENTS / PRINT CONFIGS (global) -->
<macros:FilterBar id="AttachmentFilterBar" contextPath="/PrintConfiguration" ... />
<macros:Table id="AttachmentTable" contextPath="/PrintConfiguration" ... />

<!-- SERVICE WR – absolute but filtered by selected TourId -->
<macros:FilterBar
    id="ServiceWRFilterBar"
    contextPath="/ServiceWR"
    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

<macros:Table
    id="ServiceWRTable"
    contextPath="/ServiceWR"
    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
    filterBar="ServiceWRFilterBar"
    selectionMode="ForceMulti"
    header="Service WR"
    binding="{ 
        path: '/ServiceWR',
        parameters: {
            $$filter: 'TourId eq ''' + ${/TourId} + ''''
        }
    }" />


    manualattachments: function (oContext, aSelectedContexts) {
    oExtAPI = this;

    const oTourCtx = aSelectedContexts?.[0];
    if (!oTourCtx) {
        MessageToast.show("Please select a tour first.");
        return;
    }

    const sTourId = oTourCtx.getProperty("TourId");

    if (oDialog) {
        // re-bind the ServiceWR table with the new TourId
        const oTable = oExtAPI.byId("ServiceWRTable");
        oTable.getBinding("rows")?.filter(new sap.ui.model.Filter("TourId", "EQ", sTourId));

        const oAttachFB = oExtAPI.byId("AttachmentFilterBar");
        oAttachFB?.search(); // load print configs

        oDialog.open();
        return;
    }

    oExtAPI.loadFragment({
        name: "zpdattachment.ext.fragments.GenerateDocDialog",
        controller: oActionHandlers
    }).then(function (oLoadedDialog) => {
        oDialog = oLoadedDialog;
        oExtAPI.addDependent(oDialog);

        // bind the table with filter for the selected tour
        const oTable = oExtAPI.byId("ServiceWRTable");
        oTable.bindRows({
            path: "/ServiceWR",
            filters: [new sap.ui.model.Filter("TourId", "EQ", sTourId)]
        });

        // load print configurations immediately
        oExtAPI.byId("AttachmentFilterBar").search();

        oDialog.open();
    });
}
