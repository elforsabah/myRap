"sap.ui5": {
  "services": {
    "ShellUIService": {
      "factoryName": "sap.ushell.ui5service.ShellUIService"
    }
  }
}
Step 2: Use the Service in Your App's Component.js
In the init method of your app's Component (or another appropriate lifecycle method like onInit), get the service instance and call setHeaderVisible(false) to hide the shell header:
javascriptsap.ui.define([
  "sap/ui/core/UIComponent"
], function (UIComponent) {
  "use strict";

  return UIComponent.extend("your.app.Component", {
    metadata: {
      manifest: "json"
    },

    init: function () {
      UIComponent.prototype.init.apply(this, arguments);

      // Get the ShellUIService and hide the header
      this.getService("ShellUIService").then(function (oShellService) {
        oShellService.setHeaderVisible(false);
      }).catch(function (oError) {
        console.error("Error getting ShellUIService:", oError);
      });
    }
  });
});
Additional Notes

This approach assumes your app is launched within the Fiori Launchpad. If running standalone, the service won't be available.
The service is asynchronous, so use then to handle the promise.
If you need to toggle visibility based on app states or views, call setHeaderVisible in a view controller or router event handler.
For global shell customizations (not app-specific), create a shell plugin using sap.ushell.services.Extension to extend the launchpad behavior. Refer to the SAP UI5 SDK documentation for plugin development examples.
If this is for a specific tile, you can also configure the target mapping in the Fiori Launchpad Designer (/n/ui2/flpd_cust) by adding the parameter sap-ushell-config=headerless in the URL parameters section for headerless mode.
64 web pages2.7s
