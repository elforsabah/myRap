            _bindStep2: function () {
                var oVBox = this.byId("step2LtContainer");
                var aItems = oVBox.getItems();
                var oTemplate;
                if (aItems.length > 0) {
                    oTemplate = aItems[0].clone();
                    oVBox.removeAllItems();
                } else {
                    return; // no template
                }
                var sVbeln = this.getView().getBindingContext().getProperty("Vbeln");
                if (sVbeln) {
                    oVBox.bindItems({
                        path: "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sVbeln + "')",
                        parameters: {
                            $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Language",
                            $orderby: "SalesOrder,SalesOrderitem"
                        },
                        template: oTemplate,
                        templateShareable: true
                    });
                }
            },
