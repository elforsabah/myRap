Design
The design introduces a new Fiori tile 'List blocked vehicles' under the 'Planning and Dispatching' page. This tile opens a Fiori Elements List Report application based on CDS view `ZI_PDMA_BLOCKEDVEHICLE`. The application displays entries from table `ZTPDMAVEHICLERES`. Authorization logic ensures users either have view-only access or the ability to delete entries with confirmation.
Technical Documentation
The application was developed to meet the requirement of listing blocked vehicles. The CDS View `ZI_PDMA_BLOCKEDVEHICLE` exposes vehicle data from table `ZTPDMAVEHICLERES`. Metadata extensions enhance the UI with labels and line items. Behavior definition restricts operations to delete-only. The service definition `ZSB_PDMA_BLOCKEDVEHICLE` makes the data available via OData and is bound for consumption in the Fiori List Report application.
Code Inspector Results
No Code Inspector findings were reported for this development.
Post import Steps
1. Activate CDS View `ZI_PDMA_BLOCKEDVEHICLE`.
2. Activate Behavior Definition and Implementation.
3. Activate Service Definition `ZSB_PDMA_BLOCKEDVEHICLE` and bind it to OData.
4. Deploy the Fiori Elements List Report application in BAS.
5. Assign appropriate Fiori role including the new tile 'List blocked vehicles'.
Unit Test Procedure
1. Open the Fiori Launchpad and navigate to the 'Planning and Dispatching' page.
2. Select the tile 'List blocked vehicles'.
3. Verify entries from table `ZTPDMAVEHICLERES` are displayed.
4. Ensure users with 'view-only' restriction cannot delete entries.
5. Verify that authorized users can delete entries after confirmation.
6. Check that deleted entries are removed from both UI and database.
