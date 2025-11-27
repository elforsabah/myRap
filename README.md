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
            <VBox width="100%" height="100%" renderType="Div">

                <!-- TOP TABLE (blue area) -->
                <smartTable:SmartTable
                    id="SmartTableTop"
                    entitySet="Attachment"
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    useExportToExcel="false"
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
                    useExportToExcel="false"
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
            <VBox width="100%" height="100%" renderType="Div">

                <!-- TOP TABLE (blue area) -->
                <smartTable:SmartTable
                    id="SmartTableTop"
                    entitySet="Attachment"
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    useExportToExcel="false"
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
                    useExportToExcel="false"
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


