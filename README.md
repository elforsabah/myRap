user: zwr_weighbrige $ ui5 build --all
INFO: Using local @ui5/cli installation

info ProjectBuilder Preparing build for project com.prologa.zwrweighbrige
info ProjectBuilder   Target directory: ./dist
info Project 1 of 1: ❯ Building application project com.prologa.zwrweighbrige...
info com.prologa.zwrweighbrige › Running task escapeNonAsciiCharacters...
info com.prologa.zwrweighbrige › Running task replaceCopyright...
info com.prologa.zwrweighbrige › Running task replaceVersion...
info com.prologa.zwrweighbrige › Running task minify...
info com.prologa.zwrweighbrige › Running task enhanceManifest...
warn builder:processors:manifestEnhancer /resources/com/prologa/zwrweighbrige/manifest.json: Generated supported locales ('') for bundle 'i18n/i18n.properties' do not contain default fallback locale 'en'. Either provide a properties file for 'en' or configure another available fallbackLocale
warn builder:processors:manifestEnhancer /resources/com/prologa/zwrweighbrige/manifest.json: Generated supported locales ('') for bundle 'i18n/i18n.properties' do not contain default fallback locale 'en'. Either provide a properties file for 'en' or configure another available fallbackLocale
warn builder:processors:manifestEnhancer /resources/com/prologa/zwrweighbrige/manifest.json: Generated supported locales ('') for bundle 'i18n/i18n.properties' do not contain default fallback locale 'en'. Either provide a properties file for 'en' or configure another available fallbackLocale
info com.prologa.zwrweighbrige › Running task generateFlexChangesBundle...
info com.prologa.zwrweighbrige › Running task generateComponentPreload...
info ProjectBuilder Build succeeded in 349 ms
info ProjectBuilder Executing cleanup tasks...
user: zwr_weighbrige $ npx fiori deploy
(node:5081) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)

Confirmation is required to deploy the app:

    Application Name: ZWR_WEIGHBRIDGE
    Package:  ZWR_CLEAN_WB
    Transport Request: S4DK902108
    Destination: bas-s4d-100
    SCP: false
    Target System SAPUI5 version: 1.120.23
    
Target system's SAPUI5 version is lower than the local minUI5Version. Testing locally with different Run Configurations recommended https://help.sap.com/viewer/17d50220bcd848aa854c9c182d65b699/Latest/en-US/09171c8bc3a64ec7848f0ef31770a793.html

✔ Start deployment (Y/n)?

 … yes
info abap-deploy-task ZWR_WEIGHBRIDGE Creating archive with UI5 build result.
info abap-deploy-task ZWR_WEIGHBRIDGE Archive created.
info abap-deploy-task ZWR_WEIGHBRIDGE Starting to deploy.
info abap-deploy-task ZWR_WEIGHBRIDGE ZWR_WEIGHBRIDGE found on target system: false
error abap-deploy-task ZWR_WEIGHBRIDGE Request failed with status code 400
error abap-deploy-task ZWR_WEIGHBRIDGE SAPUI5 application ZWR_WEIGHBRIDGE was not uploaded or registered successfully
error abap-deploy-task ZWR_WEIGHBRIDGE SAPUI5 application ZWR_WEIGHBRIDGE was not uploaded or registered successfully
info abap-deploy-task ZWR_WEIGHBRIDGE Please copy/paste this URL in a browser for more details:
info abap-deploy-task ZWR_WEIGHBRIDGE (Note: You will need to replace the host in the URL with the internal host, if your destination is configured using an On-Premise SAP Cloud Connector)
info abap-deploy-task ZWR_WEIGHBRIDGE http://s4d-bas.sap.mariuspedersen.dk:44300/sap/opu/odata/iwbep/message_text/
info abap-deploy-task ZWR_WEIGHBRIDGE ***** Upload of SAPUI5 App or Library from ZIP-Archive into SAPUI5 ABAP Repository *****
info abap-deploy-task ZWR_WEIGHBRIDGE 
info abap-deploy-task ZWR_WEIGHBRIDGE Running in regular mode, brief log
info abap-deploy-task ZWR_WEIGHBRIDGE 
info abap-deploy-task ZWR_WEIGHBRIDGE 16 Files found in Archive.
info abap-deploy-task ZWR_WEIGHBRIDGE 
info abap-deploy-task ZWR_WEIGHBRIDGE * Parameters *
info abap-deploy-task ZWR_WEIGHBRIDGE 
info abap-deploy-task ZWR_WEIGHBRIDGE 
info abap-deploy-task ZWR_WEIGHBRIDGE A BAdI implementation for /UI5/BADI_REPOSITORY_LOAD is active: Operation parameters may have been adjusted.
info abap-deploy-task ZWR_WEIGHBRIDGE 
info abap-deploy-task ZWR_WEIGHBRIDGE The name of the SAPUI5 repository "ZWR_WEIGHBRIDGE" has been determined from the corresponding import parameter.
info abap-deploy-task ZWR_WEIGHBRIDGE The binary files are identified using the standard settings.
info abap-deploy-task ZWR_WEIGHBRIDGE The text files are identified using the standard settings.
info abap-deploy-task ZWR_WEIGHBRIDGE The files and folders to be ignored are determined from the built-in standard settings.
info abap-deploy-task ZWR_WEIGHBRIDGE The description of the SAPUI5 repository has been determined from the corresponding import parameter.
info abap-deploy-task ZWR_WEIGHBRIDGE The package of the SAPUI5 Application " ZWR_CLEAN_WB" has been determined from the corresponding import parameter.
info abap-deploy-task ZWR_WEIGHBRIDGE Transport Request "S4DK902108" has been determined from the corresponding import parameter.
info abap-deploy-task ZWR_WEIGHBRIDGE The external Code Page Name "UTF8" has been determined from the corresponding import parameter.
info abap-deploy-task ZWR_WEIGHBRIDGE The acceptance of Unix style end of line markers in text files has been determined from the corresponding import parameter.
info abap-deploy-task ZWR_WEIGHBRIDGE Unix style end of line markers in text files get accepted.
info abap-deploy-task ZWR_WEIGHBRIDGE The delta mode has been turned on.
info abap-deploy-task ZWR_WEIGHBRIDGE Running in safe mode
info abap-deploy-task ZWR_WEIGHBRIDGE 
info abap-deploy-task ZWR_WEIGHBRIDGE * Creating new SAPUI5 ABAP repository ZWR_WEIGHBRIDGE *
info abap-deploy-task ZWR_WEIGHBRIDGE * Creating new SAPUI5 ABAP repository ZWR_WEIGHBRIDGE *
error abap-deploy-task ZWR_WEIGHBRIDGE Package ZWR_CLEAN_WB does not exist
error abap-deploy-task ZWR_WEIGHBRIDGE Upload canceled: SAPUI5 ABAP repository has not been created (successfully).
error abap-deploy-task ZWR_WEIGHBRIDGE SAPUI5 application ZWR_WEIGHBRIDGE was not uploaded or registered successfully
info abap-deploy-task ZWR_WEIGHBRIDGE Please copy/paste this URL in a browser for more details:
info abap-deploy-task ZWR_WEIGHBRIDGE (Note: You will need to replace the host in the URL with the internal host, if your destination is configured using an On-Premise SAP Cloud Connector)
info abap-deploy-task ZWR_WEIGHBRIDGE http://s4d-bas.sap.mariuspedersen.dk:44300/sap/opu/odata/iwbep/message_text/
info abap-deploy-task ZWR_WEIGHBRIDGE 
info abap-deploy-task ZWR_WEIGHBRIDGE * Done *
error abap-deploy-task ZWR_WEIGHBRIDGE SAP_Transaction: For backend administrators: use ADT feed reader "SAP Gateway Error Log" or run transaction /IWFND/ERROR_LOG on SAP Gateway hub system and search for entries with the timestamp above for more details
error abap-deploy-task ZWR_WEIGHBRIDGE SAP_Note: See SAP Note 1797736 for error analysis (https://service.sap.com/sap/support/notes/1797736)
error abap-deploy-task ZWR_WEIGHBRIDGE Deployment has failed.
error abap-deploy-task ZWR_WEIGHBRIDGE Change logging level to debug your issue
error abap-deploy-task ZWR_WEIGHBRIDGE  (see examples https://github.com/SAP/open-ux-tools/tree/main/packages/deploy-tooling#configuration-with-logging-enabled)
error builder:custom deploy-to-abap Request failed with status code 400
Command deploy failed with error : Request failed with status code 400
