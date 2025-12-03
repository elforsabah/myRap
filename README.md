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
