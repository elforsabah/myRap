{
  "_version": "1.65.0",  // Or your UI5 version
  "sap.app": {
    "id": "com.example.weighingsessionwizard",
    "type": "application",
    "i18n": "i18n/i18n.properties",  // Path to your default English bundle
    "title": "{{appTitle}}",  // References a key from i18n.properties
    "description": "{{appDescription}}",
    "applicationVersion": {
      "version": "1.0.0"
    }
  },
  "sap.ui5": {
    "models": {
      "i18n": {
        "type": "sap.ui.model.resource.ResourceModel",
        "settings": {
          "bundleName": "com.example.weighingsessionwizard.i18n.i18n",  // Your bundle namespace
          "supportedLocales": ["en", "da"],  // English and Danish
          "fallbackLocale": "en"  // Default to English if language not found
        }
      }
    },
    // Other sap.ui5 settings like rootView, dependencies, etc.
  }
}
