// Bind step 2 dynamically
var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')";
var oVBox = this.byId("step2LtContainer");
var oTemplate = new Button({
    text: {
        parts: [
            { path: 'Material', targetType: 'any' },
            { path: 'MaterialText', targetType: 'any' }
        ],
        formatter: '.formatter.concatMaterialText'
    },
    press: [this.onChooseLoadType, this],
    width: "100%"
});
oTemplate.addStyleClass("loadTypeBtn");
if (oVBox) {
    oVBox.bindAggregation("items", {  // Changed from bindItems to bindAggregation("items")
        path: sPath,
        parameters: {
            $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Language",
            $orderby: "SalesOrder,SalesOrderitem"
        },
        template: oTemplate,
        templateShareable: false
    });
} else {
    console.error("step2LtContainer not found");  // Debug: If control is missing
}
// advance wizard
this._clearContractInlineError();
this.oWizard.validateStep(this.byId("step1"));
this.oWizard.nextStep();
