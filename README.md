**The functional requirement**

There will be a new Fiori tile called “List blocked vehicles” within the Fiori page “Planning and Dispatching” 
 
Clicking on the Fiori tile opens a Fiori list application that displays the table “ZTPDMAVEHICLERES”. If the user has the “view” assigned to their Fiori role, this will only display the contents of the table. If the user does not have the “view” restriction, there will be an action called “Delete” in the Fiori list application that deletes the marked entries in the list. When this action is taken, a warning is displayed that reads “Deleting this entry comes with a risk of losing confirmation data from the driver. Do you want to delete this entry?”. This could look like the following mock up: 


**The CDS View**

@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Blocked Vehicles Interface View'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true

define root view entity ZI_PDMA_BLOCKEDVEHICLE as select from ztpdmavehicleres

{

    key vehicleid             as VehicleId,
      driverresourceid      as DriverResourceId,
      //timestamp             as Timestamp,
      @Semantics.user.createdBy: true     
      created_by            as CreatedBy, 
      @Semantics.systemDateTime.createdAt: true     
      created_at            as CreatedAt, 
      @Semantics.user.lastChangedBy: true  
      last_changed_by       as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      last_changed_at       as LastChangedAtwhat    
}


**The  metadata extension**

@Metadata.layer: #CORE
annotate entity ZI_PDMA_BLOCKEDVEHICLE
    with 
{


    @UI.facet: [{ id: 'General', purpose: #STANDARD, type: #IDENTIFICATION_REFERENCE,
    label: 'Vehicle Details',  position: 10  }]
    @UI: { lineItem: [{ position: 10 }], selectionField: [{ position: 10 }] }
    @EndUserText.label: 'Vehicle Id' 
    @UI.identification: [{ position: 10 }]
    VehicleId;
    @UI: { lineItem: [{ position: 20 }], selectionField: [{ position: 20  }] }
    @EndUserText.label: 'Driver Resource Id' 
    @UI.identification: [{ position: 20 }]
    DriverResourceId;
    @EndUserText.label: 'Created By' 
    @UI: { lineItem: [{ position: 30 }] }
    @UI.identification: [{ position: 30 }]
    CreatedBy;
    @EndUserText.label: 'Created At' 
    @UI: { lineItem: [{ position: 40 }] }
    @UI.identification: [{ position: 40 }]
    CreatedAt;
    @EndUserText.label: 'Last Changed By'   
    @UI: { lineItem: [{ position: 50 }] }
    @UI.identification: [{ position: 50 }]
    LastChangedBy;
    @UI.identification: [{ position: 60 }]
    @EndUserText.label: 'Last Changed At'
    @UI: { lineItem: [{ position: 60 }] }
    lastchangedat;
}

**The behavior defination**
managed implementation in class zbp_i_pdma_blockedvehicle unique;
strict ( 2 );

define behavior for ZI_PDMA_BLOCKEDVEHICLE
persistent table ztpdmavehicleres
lock master
authorization master ( instance )
//etag master <field_name>
{
//  create;
//  update;
  delete;

}

**The service Defination**

@EndUserText.label: 'Blocked vehicle Service Definition'
define service ZSB_PDMA_BLOCKEDVEHICLE {
  expose ZI_PDMA_BLOCKEDVEHICLE;
}

**The service binding (Odata Service )**
ZSB_PDMA_BLOCKEDVEHICLE

**Changes made on the UI in Business Application Studio**
# This is the resource bundle for com.prologa.zpdmablockedvehicle

#Texts for manifest.json

#XTIT: Application name
appTitle=Manually throw a driver off a blocked truck

#YDES: Application description
appDescription=Manually throw a driver off a blocked truck

ST_GENERIC_DELETE_SELECTED=Deleting this entry comes with a risk of losing confirmation data from the driver. Do you want to delete this entry?
ST_GENERIC_DELETE_CONFIRMATION=Deleting this entry comes with a risk of losing confirmation data from the driver. Do you want to delete this entry?


