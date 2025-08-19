     var sActionPath = "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)";  // Adjust namespace if different
                var oActionBinding = oModel.bindContext(sActionPath, oContext);  // Bind the action to the entity context
                // Set parameters (required for your action)
                oActionBinding.setParameter("vbeln", sContractId);
                //oActionBinding.setParameter("loadtype", "");  // Empty or default value, as before
                // Invoke the action
                oActionBinding.invoke().then(
                    function (oResult) {
                        // Do not clear here; new messages are already in the model from the response
<img width="740" height="230" alt="image" src="https://github.com/user-attachments/assets/4d32b79a-a836-4624-806b-937a2b529d30" />
