oResult.requestProperty("SAP__Messages").then(function(aMessages) {
    console.log("Entity Messages:", aMessages);  // Logs array of {code, message, target, ...} objects
}).catch(function(oError) {
    console.error("Error fetching messages:", oError);
});
