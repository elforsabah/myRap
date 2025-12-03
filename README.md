sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI;        // sap.fe.core.ExtensionAPI
    var oDialog;        // ONE dialog instance
    var oSelectedTourCtx; // Binding context of selected Tour row (instead of just TourId)

    function applyTourFilters() {
        if (!oExtAPI) {
            return;
        }

        // --- Attachments/Print Configs: load all (NO Tour filter) ---
        var oAttachFB = oExtAPI.byId("AttachmentFilterBar");
        if (oAttachFB && oAttachFB.search) {
            // uses whatever default conditions/variant exist
            oAttachFB.search();  // behaves like pressing GO, loads immediately
        }

        // --- Service WR: No manual filter needed; composition handles it ---
        // If FilterBar is present and you want to trigger search (e.g., for additional filters), do it here
        var oServiceFB = oExtAPI.byId("ServiceWRFilterBar");
        if (oServiceFB && oServiceFB.search) {
            oServiceFB.search();  // Optional: triggers immediate load if not auto-loading
        }
    }

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // ExtensionAPI in FE V4

            // 1) Get selected tour context (use first selected row)
            oSelectedTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oSelectedTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }

            // 2) Open dialog and apply bindings/filters
            if (oDialog) {
                oDialog.setBindingContext(oSelectedTourCtx);  // Re-bind for new tour
                applyTourFilters();
                oDialog.open();
                return;
            }

            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);
                oDialog.setBindingContext(oSelectedTourCtx);  // Initial bind
                applyTourFilters();  // Initial load

                // Optional: If timing issues, move applyTourFilters to afterOpen
                oDialog.attachAfterOpen(function () {
                    applyTourFilters();
                });

                oDialog.open();
            });
        },

        onDialogChoose: function () {
            // ... (unchanged)
        },

        onDialogCancel: function () {
            // ... (unchanged)
        }
    };

    return oActionHandlers;
});


<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:macros="sap.fe.macros">

    <Dialog
        id="TwoSmartTablesDialog"
        title="Choose Items"
        stretch="true"
        contentWidth="1200px"
        contentHeight="600px"
        class="sapUiResponsivePadding">

        <content>
            <VBox id="vbMain" width="100%" height="100%" renderType="Div">

                <!-- ========= ATTACHMENTS (UPDATE TO CHILD IF APPLICABLE) ========= -->
                <!-- If this is print configs, keep as-is (absolute path, loads all). -->
                <!-- If actual attachments, change to contextPath="_Attachments" and metaPath to match ZC_PDATTACHMENT annotations. -->
                <macros:FilterBar
                    id="AttachmentFilterBar"
                    contextPath="/PrintConfiguration"  <!-- Change to "_Attachments" if showing tour-specific attachments -->
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                <macros:Table
                    id="AttachmentTable"
                    contextPath="/PrintConfiguration"  <!-- Change to "_Attachments" if showing tour-specific attachments -->
                    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                    filterBar="AttachmentFilterBar"            
                    selectionMode="ForceMulti"    
                    header="Attachments" />  <!-- Or rename header to "Print Configurations" for clarity -->

                <Toolbar id="tbSpacer1" design="Transparent">
                    <ToolbarSpacer id="tb1" />
                </Toolbar>

                <!-- ========= SERVICE WR (USE CHILD NAVIGATION) ========= -->
                <macros:FilterBar
                    id="ServiceWRFilterBar"
                    contextPath="_ServiceAssignments"  <!-- Relative to dialog's binding context (selected tour) -->
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                <macros:Table
                    id="ServiceWRTable"
                    contextPath="_ServiceAssignments"  <!-- Relative to dialog's binding context (selected tour) -->
                    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                    filterBar="ServiceWRFilterBar"
                    selectionMode="ForceMulti"
                    header="Service WR" />

            </VBox>
        </content>

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
