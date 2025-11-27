<?xml version="1.0" encoding="UTF-8"?>
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
            <VBox
                id="vb1"
                width="100%"
                height="100%"
                renderType="Div">

                <!-- TOP TABLE -->
                <smartTable:SmartTable
                    id="SmartTableTop"
                    entitySet="Attachment"
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    showRowCount="true"
                    header="Top items"
                    enableAutoBinding="true"
                    persistencyKey="TopTable" />

                <!-- BOTTOM TABLE -->
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

        <!-- Button A -->
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
