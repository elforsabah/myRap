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

                // Parse "12345 KG" (fallback unit KG)
                var aWeightParts = sMainWeight.trim().split(/\s+/);
                var sWeight = aWeightParts[0] || "";
                var sWeighUnit = aWeightParts[1] || "KG";

                // Pad contract like in step 1
                sContractId = sContractId.padStart(10, "0");

                // Make sure the draft entity is persisted before the action
                oModel.submitBatch("weighingGroup").then(function () {

                    // Bind the action to the same context
                    var oAction = oModel.bindContext(
                        "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.printSlip(...)",
                        oContext
                    );
                    oAction.setParameter("Vbeln", sContractId);
                    oAction.setParameter("Loadtype", sLoadType);
                    oAction.setParameter("Weight", sWeight);
                    oAction.setParameter("WeighUnit", sWeighUnit);

                    // NOTE: invoke()'s promise does NOT pass payload; read it from the binding.
                    return oAction.invoke().then(function () {
                        // Get result payload
                        var oResCtx = oAction.getBoundContext();
                        var oRes = oResCtx && oResCtx.getObject ? oResCtx.getObject() : null;

                        // Be tolerant to server-side casing
                        var sPdfB64 = oRes && (oRes.pdfbase64 || oRes.Pdfbase64 || oRes.PDFBASE64);
                            
                        if (sPdfB64) {
                            // Example 1: open in a new tab
                            var win = window.open("");
                            if (win && win.document) {
                                win.document.write(
                                    '<iframe width="100%" height="100%" ' +
                                    'src="data:application/pdf;base64,' + sPdfB64 + '"></iframe>'
                                );
                            } else {
                                // Example 2: fallback to download
                                var link = document.createElement("a");
                                link.href = "data:application/pdf;base64," + sPdfB64;
                                link.download = "weighing_slip.pdf";
                                link.click();
                            }
                            MessageToast.show("PDF generated.");
                        } else {
                            MessageToast.show("No pdfbase64 returned by printSlip.");
                        }

                        // Handle FE messages (optional)
                        var aMsgs = Messaging.getMessageModel().getData() || [];
                        var aSucc = aMsgs.filter(function (m) { return m.getType() === "Success"; });
                        if (aSucc.length > 0) {
                            MessageToast.show(aSucc[0].getMessage());
                            Messaging.removeMessages(aSucc);
                        }

                        // Refresh & move on
                        oContext.refresh();
                        this.oWizard.nextStep();

                    }.bind(this));
                }.bind(this)).catch(function (oError) {
                    var sErr = (oError && oError.message) || "Failed to process print slip.";
                    MessageToast.show(sErr);

                    // Also surface any error messages from the message model
                    var aMsgs = Messaging.getMessageModel().getData() || [];
                    var aErrs = aMsgs.filter(function (m) { return m.getType() === "Error"; });
                    if (aErrs.length > 0) {
                        MessageToast.show(aErrs[0].getMessage());
                        Messaging.removeMessages(aErrs);
                    }
                }.bind(this));
            },
