        onWeighStep3: function () {
            var oContext = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            var oLocalModel = this.getView().getModel("local");

            if (!oContext) {
                MessageToast.show("No session context available.");
                return;
            }

            // Ensure the entity is persisted before calling the action
            oModel.submitBatch("weighingGroup").then(function () {
                console.log("Entity persisted. Invoking determineWeight...");
                // Call the determineWeight action
                var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.determineWeight(...)", oContext);
                var sContractId = this.getView().getModel("local").getProperty("/contractId");
                var sLoadType = this.getView().getModel("local").getProperty("/loadType");
                oAction.setParameter("Vbeln", sContractId);
                oAction.setParameter("Loadtype", sLoadType);

                return oAction.invoke();
            }.bind(this)).then(function (oResult) {
                console.log("determineWeight invoked successfully.");
                // Check for success messages in the Messaging model
                var aMessages = Messaging.getMessageModel().getData() || [];
                var oSuccessMessage = aMessages.find(function (oMsg) {
                    return oMsg.getMessageId() === "ZWR_WEIGHBRIGE_MESS" && 
                           oMsg.getMessageNumber() === "002" && 
                           oMsg.getType() === "Success";
                });

                if (oSuccessMessage) {
                    // Extract the weight from the message text (e.g., "Weight determined: 1000 kg")
                    var sMessageText = oSuccessMessage.getMessage();
                    var sWeight = sMessageText.match(/(\d+(\.\d+)?)/)?.[0] || "0"; // Extract number (integer or decimal)
                    oLocalModel.setProperty("/mainWeight", sWeight);
                    MessageToast.show(sMessageText); // Show the full message
                } else {
                    // Fallback if message is not found
                    MessageToast.show("Weight not received from backend.");
                    oLocalModel.setProperty("/mainWeight", "0");
                }

                // Refresh the context to sync other fields (optional, since weight is not persisted)
                oContext.refresh();
                // Advance to the next step
                this.oWizard.validateStep(this.byId("step3"));
                this.oWizard.nextStep();
            }.bind(this)).catch(function (oError) {
                console.error("Error in determineWeight: ", oError);
                var sErrorMsg = oError.message || "Failed to determine weight.";
                MessageToast.show(sErrorMsg);
                oLocalModel.setProperty("/mainWeight", "0");
            });
        },
