this._createInitialContext();
        },

        _createInitialContext: function () {
            var that = this;
            this.extensionAPI.getEditFlow().createDocument({
                creationMode: "NewPage"  // Or "Inline" depending on your flow
            }).then(function (oNewContext) {
                that.getView().setBindingContext(oNewContext);
                // Pre-request the property to initialize it (avoids "must not change before read")
                oNewContext.requestProperty("Vbeln").catch(function () {
                    // Ignore if not needed; ensures read before any set
                });
            }).catch(function (oError) {
                MessageToast.show("Failed to create session context.");
            });
        },
