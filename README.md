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

                <!-- ========= ATTACHMENT “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="AttachmentFilterBar"
                    contextPath="/PrintConfiguration"
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                <macros:Table
                    id="AttachmentTable"
                    contextPath="/PrintConfiguration"
                    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                    filterBar="AttachmentFilterBar"
                    selectionMode="Multi"
                    selectionBehavior="Row"
                    header="Attachments" />

                <Toolbar id="tbSpacer1" design="Transparent">
                    <ToolbarSpacer id="tb1" />
                </Toolbar>

                <!-- ========= SERVICE WR “LIST REPORT” ========= -->
                <macros:FilterBar
                    id="ServiceWRFilterBar"
                    contextPath="/ServiceWR"
                    metaPath="@com.sap.vocabularies.UI.v1.SelectionFields" />

                <macros:Table
                    id="ServiceWRTable"
                    contextPath="/ServiceWR"
                    metaPath="@com.sap.vocabularies.UI.v1.LineItem"
                    filterBar="ServiceWRFilterBar"
                    selectionMode="Multi"
                    selectionBehavior="Row"
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
