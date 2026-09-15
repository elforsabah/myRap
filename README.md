sap.ui.define([], 
    function () {
	'use strict';

	return {        	
		importIcons: function() {
			try {                
                var oView = this.getEditFlow().getView();
                    oView.setBusy(true);

                const aIconList = sap.ui.core.IconPool.getIconNames().map(IconName => ( 
                        {IconName: IconName, IconUrl: sap.ui.core.IconPool.getIconInfo(IconName)?.uri}
                    ));
				
                //invoking action
                const sNamespace = "com.sap.gateway.srvd.plce.ui_cbaseconfig.v0001.";
                const oModel = this.getModel();
                const oIconOperation = oModel?.bindContext("/IconConfig/" + sNamespace + "ImportIcons(...)");                                         
                oIconOperation.setParameter("dummy", "X"); //dummy - eines der Felder des tiefen Parameters                
                oIconOperation.setParameter("_Icons", aIconList);                
                oIconOperation.execute().then(function() { //ab UI5 Version 1.123.0 stattdessen .invoke() benutzen
                    oView.setBusy(false);
                    oView.getBindingContext().refresh();
                    new sap.m.MessageToast.show(oView.getModel("i18n").getResourceBundle().getText("messageToastIconImportSuccess"));
                }).catch((e) => {
                    sap.m.MessageBox.error(e.message, {
                        title: oView.getModel("i18n").getResourceBundle().getText("messageBoxErrorTitle"),                                      
                        initialFocus: null                                   
                    });
                });                                                 
            } catch (e) {        
                oView.setBusy(false);        
                sap.m.MessageBox.error(e.message, {
                    title: oView.getModel("i18n").getResourceBundle().getText("messageBoxErrorTitle"),                                      
                    initialFocus: null                                   
                });
                console.error(e);
            }            
		},

        isEnabled: function() {
            return !this.getModel("ui").getProperty("/isEditable");            
        }
    }
});
