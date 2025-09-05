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
                    // NEW: Manually fetch all messages from the message model
                    var aMessages = Messaging.getMessageModel().getData() || [];
                    // Filter for success messages (assuming the weight message is the latest success)
                    var aSuccesses = aMessages.filter(function (oMsg) {
                        return oMsg.getType() === "Success";
                    });
                    if (aSuccesses.length > 0) {
                        // Take the last success message (or loop if multiple, but likely one per action)
                        var oLatestSuccess = aSuccesses[aSuccesses.length - 1];
                        var sMsgText = oLatestSuccess.getMessage();
                        // Parse the weight (assumes text like "Weight determined: 1000" or similar; adjust regex if needed)
                        var aMatch = sMsgText.match(/(\d+(\.\d+)?)/); // Matches integer or decimal number
                        if (aMatch) {
                            var fWeight = parseFloat(aMatch[0]);
                            oLocalModel.setProperty("/mainWeight", fWeight.toString());
                            // Optional: Show a toast with the parsed weight
                            MessageToast.show("Weight captured from backend: " + fWeight);
                        } else {
                            // Fallback if parsing fails
                            MessageToast.show(sMsgText); // Show original message
                        }
                        // Remove the processed success messages to avoid accumulation or popups
                        Messaging.removeMessages(aSuccesses);
                    } else {
                        MessageToast.show("No success message received from backend.");
                    }
                    // Refresh context for any other updates
                    oContext.refresh();
                }.bind(this)).catch(function (oError) {
                    console.error("Error in determineWeight: ", oError);
                    var sErrorMsg = oError.message || "Failed to determine weight.";
                    MessageToast.show(sErrorMsg);
                    // Optional: Also check for error messages and handle/remove them
                    var aMessages = Messaging.getMessageModel().getData() || [];
                    var aErrors = aMessages.filter(function (oMsg) {
                        return oMsg.getType() === "Error";
                    });
                    if (aErrors.length > 0) {
                        // Example: Show the error message and remove
                        MessageToast.show(aErrors[0].getMessage());
                        Messaging.removeMessages(aErrors);
                    }
                }.bind(this));
            },
