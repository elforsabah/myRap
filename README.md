                    
                    var sGrossweight1 = oLocalModel.getProperty("/grossWeight");
                    var steraweight1 = oLocalModel.getProperty("/teraWeight");
                    
                    console.log("grossweight:", sGrossweight1 );
                    console.log("teraweight:", steraweight1 );

                    MessageToast.show("Weight captured: " + sGrossweight1, + steraweight1, { duration: 50000 });
