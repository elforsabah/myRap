.catch(function (oError) {
    console.error("Error in create or identifyCard: ", oError);  // Keep for debugging, or remove if you want to suppress console logs
    var sErrorMsg = oError.message || "Unknown error";  // This captures the backend message like "Contract is Invalid"
    this._setContractInlineError(sErrorMsg);  // Display the exact message inline
});
