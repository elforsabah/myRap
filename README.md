const sMessageHeader = oResponse.headers["sap-message"];
                            let disableUpload = false;

                            if (sMessageHeader) {
                                const oMessage = JSON.parse(sMessageHeader);

                                // Collect all messages in an array
                                const aAllMessages = [];

                                // Add the main message
                                if (oMessage.message) {
                                    aAllMessages.push(oMessage.message);
                                }

                                // Add any details (bound or extra messages)
                                if (Array.isArray(oMessage.details)) {
                                    oMessage.details.forEach(detail => {
                                        if (detail.message) {
                                            aAllMessages.push(detail.message);
                                        }
                                    });
                                }

                                //  Check for "no upload" message
                                if (aAllMessages.some(msg => msg.toLowerCase().includes("no upload"))) {
                                    // Disable upload button if no results
                                    const oUploadButton = sap.ui.core.Fragment.byId("FileUploadDialog2Fragment", "UploadConfirmButton");
                                    if (oUploadButton) {
                                        oUploadButton.setEnabled(false);
                                    }
                                    disableUpload = true;
                                }
                                // Show all messages in a single MessageBox (or you could use MessageToast or popover)
                                if (!disableUpload) {
                                    ;
                                    MessageBox.success(aAllMessages.join("\n")); // shows in one popup
                                }
                            }

                            resolve(); // allow SmartTable to rebind
                        },
