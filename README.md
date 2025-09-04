// ... (existing code up to oContext.refresh())
var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')/Set";  // Add /Set here
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
    try {
        oVBox.bindAggregation("items", {
            path: sPath,
            parameters: {
                $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Language",
                $orderby: "SalesOrder,SalesOrderitem"
            },
            template: oTemplate,
            templateShareable: false,
            events: {
                change: function (oEvent) {
                    if (oEvent.getParameter("reason") === "Rejected") {
                        MessageToast.show("Failed to load load types. Check contract or service.");
                    }
                }
            }
        });
    } catch (oError) {
        console.error("Binding error: ", oError);
        this._setContractInlineError("Failed to load materials for contract.");
        return; // Prevent advancing
    }
} else {
    console.error("step2LtContainer not found");
}
// ... (existing code to advance wizard)
