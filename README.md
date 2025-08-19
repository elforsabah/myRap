oActionBinding.invoke().then(
    function(oResult) {
        // Do not clear here; new messages are already in the model from the response

        // Get messages from Messaging
        var aMessages = Messaging.getMessageModel().getData();
        console.log("All Messages:", aMessages);

        if (aMessages.length > 0) {
            var oMsg = aMessages[0];  // Assuming first message; iterate if multiple
            console.log("Message Type:", oMsg.getType(), "Text:", oMsg.getMessage(), "Target:", oMsg.getTarget());

            var oErrorMsg = aMessages.find(function(oMsg) { return oMsg.getType() === "Error"; });
            if (oErrorMsg) {
                MessageBox.error(oErrorMsg.getMessage());
                return;
            }

            // Handle success message (e.g., "Contract is Valid")
            // Example: Show as toast
            MessageToast.show(aMessages[0].getMessage());

            // Continue with your wizard logic
            this.oWizard.validateStep(this.byId("stepId"));  // Adjust as per your app
            this.oWizard.nextStep();
            // Optional: Refresh bindings if side effects occurred
            oContext.refresh();
        } else {
            console.log("No messages Returned.");
        }
    }.bind(this),
    function(oError) {
        // Handle invocation errors (e.g., network or backend failure)
        console.error("Action Invoke Error:", oError);
        MessageToast.show("Invalid Contract. Please try again.");
    }.bind(this)
);
