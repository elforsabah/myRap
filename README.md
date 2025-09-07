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
                    // Call the printSlip function (note: functions are read-only, so no parameters that modify)
                    var oFunction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.printSlip(...)", oContext);
                    oFunction.setParameter("Vbeln", sContractId);
                    oFunction.setParameter("Loadtype", sLoadType);
                    oFunction.setParameter("Weight", sWeight);
                    oFunction.setParameter("WeighUnit", sWeighUnit);

                    return oFunction.invoke();
                }.bind(this)).then(function () {
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
                    // Retrieve pdfbase64 from the function binding (synchronously after invoke)
                    var sPdfBase64 = oFunction.getValue("pdfbase64");
                    if (sPdfBase64) {
                        this._printPdfFromBase64(sPdfBase64);
                    } else {
                        // Fallback to async if not loaded
                        oFunction.getProperty("pdfbase64").then(function (sPdf) {
                            if (sPdf) {
                                this._printPdfFromBase64(sPdf);
                            } else {
                                MessageToast.show("No PDF base64 returned from backend.");
                            }
                        }.bind(this)).catch(function (oError) {
                            console.error("Error fetching pdfbase64: ", oError);
                            MessageToast.show("Failed to retrieve PDF data.");
                        }.bind(this));
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

            // Helper method to print PDF from base64
            _printPdfFromBase64: function (sBase64) {
                if (!sBase64) {
                    MessageToast.show("No PDF data available to print.");
                    return;
                }
                // Create a data URL for the PDF
                var sPdfDataUrl = "data:application/pdf;base64," + sBase64;
                // Open in a new window and trigger print
                var oPrintWindow = window.open("", "_blank");
                if (oPrintWindow) {
                    oPrintWindow.document.write('<iframe src="' + sPdfDataUrl + '" frameborder="0" style="border:0; top:0px; left:0px; bottom:0px; right:0px; width:100%; height:100%;" allowfullscreen></iframe>');
                    oPrintWindow.document.close();
                    // Wait for iframe to load before printing (optional timeout)
                    setTimeout(function () {
                        oPrintWindow.focus();
                        oPrintWindow.print();
                    }, 1000); // Adjust timeout if needed
                } else {
                    MessageToast.show("Failed to open print window. Please allow pop-ups.");
                }
            },
