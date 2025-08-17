// Disable input initially to prevent sets before read
            this.byId("ip11").setEnabled(false);
            // Create a new draft context if none exists
            this._createInitialContext();
        },

        _createInitialContext: function () {
            var that = this;
            this.extensionAPI.getEditFlow().createDocument({
                creationMode: "NewPage"  // Or "Inline" depending on your flow
            }).then(function (oNewContext) {
                that.getView().setBindingContext(oNewContext);
                // Get the input's value binding and request the value to initialize (read) it
                var oInput = that.byId("ip11");
                var oBinding = oInput.getBinding("value");
                oBinding.requestValue().then(function () {
                    // On successful read (even if null for new entity), enable the input
                    oInput.setEnabled(true);
                }).catch(function (oError) {
                    MessageToast.show("Failed to initialize property.");
                    console.error(oError);
                });
            }).catch(function (oError) {
                MessageToast.show("Failed to create session context.");
            });
        },
