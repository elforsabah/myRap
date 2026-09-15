sap.ui.define(['sap/ui/core/mvc/ControllerExtension'], function (ControllerExtension) {
	'use strict';

	return ControllerExtension.extend('com.prologa.plce.base.plcebaseconfiguration.ext.controller.ListExtension', {
		// this section allows to extend lifecycle hooks or hooks provided by Fiori elements
		override: {
			/**
             * Called when a controller is instantiated and its View controls (if available) are already created.
             * Can be used to modify the View before it is displayed, to bind event handlers and do other one-time initialization.
             * @memberOf com.prologa.plce.base.plcebaseconfiguration.ext.controller.ListExtension
             */
			onInit: function () {
				// you can access the Fiori elements extensionAPI via this.base.getExtensionAPI
				var oModel = this.base.getExtensionAPI().getModel();
			},
			routing: {
                onBeforeNavigation: function(oContextInfo) {
                    //var oLineContextData = oContextInfo.sourceBindingContext,
                    var sContextPath = oContextInfo.bindingContext.getPath(),
                        oNav = this.base.getExtensionAPI().intentBasedNavigation,
                        oRouting = this.base.getExtensionAPI().routing;
                    // for Name Trailer navigate to TrailerList
                    switch (true) {
						case /'MAP'/.test(sContextPath): 
	                        oRouting.navigateToRoute("Map_SObjectPage", {
    	                        "key" : "1"
        	                 });
							 break;
						case /'REMOTECONFIGURATION'/.test(sContextPath): 
	                        oRouting.navigateToRoute("RemoteConfig_SObjectPage", {
    	                        "key" : "1"
        	                 });
							 break;
						case /'BGPROCESSINGCONFIG'/.test(sContextPath): 
							oRouting.navigateToRoute("BPProcessingConfig_SObjectPage", {
								"key" : "1"
						 	});
						 	break;
						case /'PROFILE'/.test(sContextPath): 
	                        oRouting.navigateToRoute("Profile_SObjectPage", {
    	                        "key" : "1"
        	                 });
							 break;
						case /'REFERENCEPRODUCT'/.test(sContextPath): 
	                        oRouting.navigateToRoute("CharcRefProduct_SDetail", {
    	                        "key" : "1"
        	                 });
							 break;
						case /'PROFILEPRODUCTGROUP'/.test(sContextPath): 
	                        oRouting.navigateToRoute("ProfileProductGrp_SObjectPage", {
    	                        "key" : "1"
        	                 });
							 break;
						case /'SEMANTICCONTEXT'/.test(sContextPath): 
	                        oRouting.navigateToRoute("SemanticContext_SObjectPage", {
    	                        "key" : "1"
        	                 });
							 break;
						//case /'CHARACTERISTICCONFIGURATION'/.test(sContextPath): 
	                    //    oRouting.navigateToRoute("CharcConfig_SObjectPage", {
    	                //        "key" : "1"
        	            //     });
						//	 break;
						case /'SEMANTICCONFIGURATION'/.test(sContextPath): 
	                        oRouting.navigateToRoute("SemanticConfig_SList", {
        	                 });
							 break;
						case /'SEMANTICTEMPLATE'/.test(sContextPath): 
	                        oRouting.navigateToRoute("SemanticTemplate_SList", {
        	                 });
							 break;
						case /'PRODUCTCHARACTERISTIC'/.test(sContextPath): 
	                        oRouting.navigateToRoute("SemanticCharcPrd_SObjectPage", {
    	                        "key" : "1"
        	                 });
							 break;
						case /'BACKENDCAPABILITIES'/.test(sContextPath): 
							oRouting.navigateToRoute("BckndCapability_SObjectPage", {
								"key" : "1"
							});
							break;
						case /'ICON'/.test(sContextPath): 
							oRouting.navigateToRoute("IconConfig_SObjectPage", {
								"key" : "1"
							});
							break;
						default :
    	                    // return false to trigger the default internal navigation
	                        return false;
                    }
                    // return true is necessary to prevent further default navigation
                    return true;
                }
            }
		}
	});

<img width="1308" height="679" alt="image" src="https://github.com/user-attachments/assets/d45cb4c2-6425-4b00-a1e4-0ea625ea44f5" />


	ST_UMLEER
});
