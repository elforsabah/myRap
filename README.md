onConfirmStep3: function () {
        var oContext = this.getView().getBindingContext();
        var oModel = this.getView().getModel();
        var oLocalModel = this.getView().getModel("local");

        if (!oContext) {
            MessageToast.show("No session context available.");
            return;
        }

        var sContractId = oLocalModel.getProperty("/contractId");
        var sLoadType = oLocalModel.getProperty("/loadType");
        var sMainWeight = oLocalModel.getProperty("/mainWeight");

        if (!sContractId || !sLoadType || !sMainWeight) {
            MessageToast.show("Missing required data: Contract ID, Load Type, or Weight.");
            return;
        }

        // Parse mainWeight (assumes format like "345671 KG")
        var aWeightParts = sMainWeight.split(' ');
        var sWeight = aWeightParts[0] || '';
        var sWeighUnit = aWeightParts[1] || 'KG'; // Default to 'KG' if no unit

        // Optionally pad Contract ID if needed (as in onNextStep)
        sContractId = sContractId.padStart(10, '0');

        // Ensure the entity is persisted before calling the action
        oModel.submitBatch("weighingGroup").then(function () {
            console.log("Entity persisted. Invoking printSlip...");
            // Call the printSlip action
            var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.printSlip(...)", oContext);
            oAction.setParameter("Vbeln", sContractId);
            oAction.setParameter("Loadtype", sLoadType);
            oAction.setParameter("Weight", sWeight);
            oAction.setParameter("WeighUnit", sWeighUnit);

            return oAction.invoke();
        }.bind(this)).then(function (oResult) {
            console.log("printSlip invoked successfully.");
            // Handle success: Fetch and process messages (similar to onWeighStep3)
            var aMessages = Messaging.getMessageModel().getData() || [];
            var aSuccesses = aMessages.filter(function (oMsg) {
                return oMsg.getType() === "Success";
            });
            if (aSuccesses.length > 0) {
                MessageToast.show(aSuccesses[0].getMessage());
                Messaging.removeMessages(aSuccesses);
            } else {
                MessageToast.show("Print slip processed successfully.");
            }
            // Refresh context for any updates
            oContext.refresh();
            // Optional: Advance wizard or complete process
            this.oWizard.nextStep(); // Or call onWizardComplete() if this is the final step
        }.bind(this)).catch(function (oError) {
            console.error("Error in printSlip: ", oError);
            var sErrorMsg = oError.message || "Failed to process print slip.";
            MessageToast.show(sErrorMsg);
            // Handle error messages
            var aMessages = Messaging.getMessageModel().getData() || [];
            var aErrors = aMessages.filter(function (oMsg) {
                return oMsg.getType() === "Error";
            });
            if (aErrors.length > 0) {
                MessageToast.show(aErrors[0].getMessage());
                Messaging.removeMessages(aErrors);
            }
        }.bind(this));
    },

