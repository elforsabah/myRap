<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:macros="sap.fe.macros">

    <!-- Popup dialog -->
    <Dialog
        id="TwoSmartTablesDialog"
        title="Choose Items"
        stretch="true"
        contentWidth="1200px"
        contentHeight="600px"
        class="sapUiResponsivePadding">

        <content>
            <VBox id="vbMain" width="100%" height="100%" renderType="Div">

                <!-- ========= ATTACHMENT “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="AttachmentFilterBar"
                    metaPath="/Attachment/@com.sap.vocabularies.UI.v1.SelectionFields"
                    contextPath="/Attachment" />

                <macros:Table
                    id="AttachmentTable"
                    metaPath="/Attachment/@com.sap.vocabularies.UI.v1.LineItem"
                    contextPath="/Attachment"
                    filterBar="AttachmentFilterBar"
                    selectionMode="Multi"
                    header="Attachments" />

                <!-- Spacer between the two areas -->
                <Toolbar id="tbSpacer1" design="Transparent">
                    <ToolbarSpacer />
                </Toolbar>

                <!-- ========= SERVICE WR “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="ServiceWRFilterBar"
                    metaPath="/ServiceWR/@com.sap.vocabularies.UI.v1.SelectionFields"
                    contextPath="/ServiceWR" />

                <macros:Table
                    id="ServiceWRTable"
                    metaPath="/ServiceWR/@com.sap.vocabularies.UI.v1.LineItem"
                    contextPath="/ServiceWR"
                    filterBar="ServiceWRFilterBar"
                    selectionMode="Multi"
                    header="Service WR" />

            </VBox>
        </content>

        <!-- “Button A” -->
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
