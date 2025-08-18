"sap/ui/core/Messaging",  // Import the Messaging module
    "sap/m/MessageBox"        // Optional for displaying messages


// Register the view with Messaging (replaces oMessageManager.registerObject)
            Messaging.registerObject(this.getView(), true);

            // Set the message model (replaces oMessageManager.getMessageModel())
            this.getView().setModel(Messaging.getMessageModel(), "message");

// Clear previous messages (replaces oMessageManager.removeAllMessages())
            Messaging.removeAllMessages();


            // Get messages from Messaging (replaces direct SAP__Messages or oMessageManager)
                var aMessages = Messaging.getMessageModel().getData();
                console.log("All Messages:", aMessages);

                if (aMessages.length > 0) {
                    aMessages.forEach(function (oMsg) {
                        console.log("Message Type:", oMsg.getType(), "Text:", oMsg.getMessage(), "Target:", oMsg.getTarget());
                    });
                    var oErrorMsg = aMessages.find(function (oMsg) { return oMsg.getType() === "Error"; });
                    if (oErrorMsg) {
                        MessageBox.error(oErrorMsg.getMessage());
                        return;
                    }
                } else {
                    console.log("No messages returned.");
                }
