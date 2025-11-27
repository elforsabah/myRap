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
                id="vb1"
                width="100%"
                height="100%"
                renderType="Div">

                <!-- TOP TABLE: binds to /Attachment (OData V4 entity set) -->
                <Table
                    id="TopTable"
                    mode="MultiSelect"
                    growing="true"
                    items="{
                        path: '/Attachment'
                    }">

                    <columns>
                        <Column>
                            <Text text="Attachment ID" />
                        </Column>
                        <Column>
                            <Text text="Description" />
                        </Column>
                    </columns>

                    <items>
                        <ColumnListItem>
                            <cells>
                                <!-- TODO: adjust property names to your CDS view -->
                                <Text text="{AttachmentID}" />
                                <Text text="{Description}" />
                            </cells>
                        </ColumnListItem>
                    </items>
                </Table>

                <!-- BOTTOM TABLE: binds to /ServiceWR (OData V4 entity set) -->
                <Table
                    id="BottomTable"
                    mode="MultiSelect"
                    growing="true"
                    items="{
                        path: '/ServiceWR'
                    }">

                    <columns>
                        <Column>
                            <Text text="Service WR ID" />
                        </Column>
                        <Column>
                            <Text text="Description" />
                        </Column>
                    </columns>

                    <items>
                        <ColumnListItem>
                            <cells>
                                <!-- TODO: adjust property names to your CDS view -->
                                <Text text="{ServiceWRID}" />
                                <Text text="{Description}" />
                            </cells>
                        </ColumnListItem>
                    </items>
                </Table>

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

        <!-- Cancel -->
        <endButton>
            <Button
                id="btnCancel"
                text="Cancel"
                press=".onDialogCancel" />
        </endButton>

    </Dialog>
</core:FragmentDefinition>
