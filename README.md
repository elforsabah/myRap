_onObjectMatched: function () {
                var oModel = this.getView().getModel();
                var oListBinding = oModel.bindList("/ZI_WR_WEIGHBRIDGE");
                var oNewContext = oListBinding.create({
                    Sessionid: this.generateUUID()
                });
                this.getView().setBindingContext(oNewContext);
                oNewContext.requestProperty(["Vbeln", "Grossweight", "Grossweightunit", "Sessionid"]).catch(function () { });
            },
