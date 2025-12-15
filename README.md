sap.ui.define([
  "sap/ui/core/mvc/ControllerExtension",
  "sap/ui/model/Filter", "sap/ui/model/FilterOperator"
], function (ControllerExtension, Filter, FilterOperator) {
  "use strict";

  return ControllerExtension.extend("spl.sietssplaudittrail2.ext.controller.ObjectPageExt", {
    onAfterBinding: function () {
      const oView = this.base.getView();
      const oOP   = oView.byId("objectPage");
      const sGuid = oView.getBindingContext()?.getProperty("GUID_SPLAT");
      if (!sGuid) { return; }

      const aFilters = [ new Filter("GUID_SPLAT", FilterOperator.EQ, sGuid) ];

      const prep = (subsecId, tableId) => {
        const oTbl = oView.byId(tableId) || oView.byId(tableId + "::Table");

        // Step 1: wait until FE created the row binding, then SUSPEND it immediately
        const suspendOnce = () => {
          const oRB = oTbl?.getRowBinding?.();
          if (oRB) {
            if (!oRB.isSuspended?.()) { oRB.suspend(); }
            // IMPORTANT: drop $count requests
            oRB.changeParameters({ $$noCount: true });
          } else {
            oTbl.attachEventOnce("modelContextChange", suspendOnce);
          }
        };
        suspendOnce();

        // Step 2: load lazily when the subsection is first entered
        let done = false;
        oOP.attachEvent("subSectionEntered", (e) => {
          if (done) { return; }
          const oSub = e.getParameter("subSection");
          if (oSub && oSub.getId().endsWith(subsecId)) {
            const oRB = oTbl?.getRowBinding?.();
            if (oRB) {
              // apply final filter (GUID) and resume -> single filtered request, no $count
              oRB.filter(aFilters);
              oRB.resume();
            }
            done = true;
          }
        });
      };

      prep("SplEntriesSubSection",   "SplEntriesTable");
      prep("MatchedTermsSubSection", "MatchedTermsTable");
    }
  });
});

<core:FragmentDefinition
    xmlns:core="sap.ui.core"
    xmlns="sap.uxap"
    xmlns:macros="sap.fe.macros">

    <ObjectPageSection
        id="SplEntriesSection"
        title="Matched Sanctioned Party List Entries">

        <subSections>
            <ObjectPageSubSection id="SplEntriesSubSection">
                <blocks>
                    <macros:Table
                        id="SplEntriesTable"
                        contextPath="/xSIETSxI_SPL_AuditTrail"
                        metaPath="_SplEntries/@com.sap.vocabularies.UI.v1.LineItem"
                        enableFullScreen="true"/>
                </blocks>
            </ObjectPageSubSection>
        </subSections>
    </ObjectPageSection>

</core:FragmentDefinition>

<core:FragmentDefinition     xmlns:core="sap.ui.core"
    xmlns="sap.uxap"
    xmlns:macros="sap.fe.macros">


   <ObjectPageSection
        id="MatchedTermsSection"
        title="Matched Terms">

        <subSections>
            <ObjectPageSubSection id="MatchedTermsSubSection">
                <blocks>
                    <macros:Table
                        id="MatchedTermsTable"
                         contextPath="/xSIETSxI_SPL_AuditTrail"
                        metaPath="_MatchedTerms/@com.sap.vocabularies.UI.v1.LineItem"/>
                        <!-- if you used a qualifier: metaPath="@com.sap.vocabularies.UI.v1.LineItem#MatchTerms" -->
                </blocks>
            </ObjectPageSubSection>
        </subSections>
    </ObjectPageSection>

</core:FragmentDefinition>



