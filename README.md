<?xml version="1.0" encoding="UTF-8"?>
<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core">

    <!-- Popup dialog -->
    <Dialog
        id="TwoSmartTablesDialog"
        title="Choose items"
        stretch="true"
        contentWidth="900px"
        contentHeight="500px"
        class="sapUiResponsivePadding">

        <content>
            <VBox
                id="VBoxWrapper"
                width="100%"
                height="100%"
                renderType="Div">

                <!-- TOP TABLE: /Attachment -->
                <Table
                    id="TopTable"
                    mode="MultiSelect"
                    growing="true"
                    growingScrollToLoad="true"
                    items="{
                        path: '/Attachment'
                    }">

                    <columns>
                        <Column id="TopColAttachmentId">
                            <Text id="TopColAttachmentIdHeader" text="Attachment ID" />
                        </Column>
                        <Column id="TopColAttachmentDesc">
                            <Text id="TopColAttachmentDescHeader" text="Description" />
                        </Column>
                    </columns>

                    <items>
                        <ColumnListItem id="TopRowTemplate">
                            <cells>
                                <!-- TODO: adapt property names -->
                                <Text id="TopCellAttachmentId" text="{AttachmentID}" />
                                <Text id="TopCellAttachmentDesc" text="{Description}" />
                            </cells>
                        </ColumnListItem>
                    </items>
                </Table>

                <!-- BOTTOM TABLE: /ServiceWR -->
                <Table
                    id="BottomTable"
                    mode="MultiSelect"
                    growing="true"
                    growingScrollToLoad="true"
                    items="{
                        path: '/ServiceWR'
                    }">

                    <columns>
                        <Column id="BottomColServiceId">
                            <Text id="BottomColServiceIdHeader" text="Service WR ID" />
                        </Column>
                        <Column id="BottomColServiceDesc">
                            <Text id="BottomColServiceDescHeader" text="Description" />
                        </Column>
                    </columns>

                    <items>
                        <ColumnListItem id="BottomRowTemplate">
                            <cells>
                                <!-- TODO: adapt property names -->
                                <Text id="BottomCellServiceId" text="{ServiceWRID}" />
                                <Text id="BottomCellServiceDesc" text="{Description}" />
                            </cells>
                        </ColumnListItem>
                    </items>
                </Table>

            </VBox>
        </content>

        <!-- "Button A" -->
        <beginButton>
            <Button
                id="BtnChoose"
                text="Choose"
                type="Emphasized"
                press=".onDialogChoose" />
        </beginButton>

        <!-- Cancel -->
        <endButton>
            <Button
                id="BtnCancel"
                text="Cancel"
                press=".onDialogCancel" />
        </endButton>

    </Dialog>
</core:FragmentDefinition>
