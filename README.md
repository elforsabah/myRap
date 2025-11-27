<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core"
    xmlns:smartTable="sap.ui.comp.smarttable">

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
                    entitySet="TopEntitySet"          <!-- TODO: replace with real entity set -->
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    useExportToExcel="false"
                    showRowCount="true"
                    header="Top items"
                    enableAutoBinding="true"
                    persistencyKey="TopTable">

                    <smartTable:table>
                        <Table
                            id="InnerTopTable"
                            mode="MultiSelect"
                            growing="true"
                            growingScrollToLoad="true" />
                    </smartTable:table>
                </smartTable:SmartTable>

                <!-- BOTTOM TABLE (yellow area) -->
                <smartTable:SmartTable
                    id="SmartTableBottom"
                    entitySet="BottomEntitySet"       <!-- TODO: replace with real entity set -->
                    tableType="ResponsiveTable"
                    useVariantManagement="false"
                    useExportToExcel="false"
                    showRowCount="true"
                    header="Bottom items"
                    enableAutoBinding="true"
                    persistencyKey="BottomTable">

                    <smartTable:table>
                        <Table
                            id="InnerBottomTable"
                            mode="MultiSelect"
                            growing="true"
                            growingScrollToLoad="true" />
                    </smartTable:table>
                </smartTable:SmartTable>

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
