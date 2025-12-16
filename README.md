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

                <!-- ========= ATTACHMENTS (child composition of selected Tour) ========= -->
                <macros:Table
                    id="AttachmentTable"
                    contextPath="/Tour"
                    metaPath="_Attachments/@com.sap.vocabularies.UI.v1.LineItem"
                    selectionMode="ForceMulti"
                    header="Attachments / Documents" />

                <Toolbar design="Transparent">
                    <ToolbarSpacer/>
                </Toolbar>

                <!-- ========= SERVICES (child composition of selected Tour) ========= -->
                <macros:Table
                    id="ServiceWRTable"
                    contextPath="/Tour"
                    metaPath="_ServiceAssignments/@com.sap.vocabularies.UI.v1.LineItem"
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
