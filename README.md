<macros:FilterBar id="ServiceWRFilterBar"
    contextPath="_ServiceAssignments"
    metaPath="_ServiceAssignments/@com.sap.vocabularies.UI.v1.SelectionFields" />

<macros:Table id="ServiceWRTable"
    contextPath="_ServiceAssignments"
    metaPath="_ServiceAssignments/@com.sap.vocabularies.UI.v1.LineItem"
    filterBar="ServiceWRFilterBar"
    selectionMode="ForceMulti"
    header="Service WR" />
