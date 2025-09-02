**Behavior Deinition**

managed implementation in class zbp_i_wr_weighbridge unique;
strict( 2 );

with draft;

define behavior for ZI_WR_WEIGHBRIDGE  alias WeighingSession
persistent table ZWR_WEIGHBRIDGE
draft table ZWR_WEIGHSESSI_D
lock master
total etag SalesDocument
authorization master ( instance )
etag master SalesDocument

{

// Keys must be readonly in strict managed draft
  field ( numbering : managed  ) SESSIONID; // <-- RAW16 UUID key

  field ( readonly ) SESSIONID;


create;
update;
delete;

draft action Edit;
draft action Activate;
draft action Discard;
draft action Resume;                 // <-- required
draft determine action Prepare;      //<-- required

action NextStep result [1] $self; // server validates & increments Step
action Submit result [1] $self; // final checks; printing trigger optional

action identifyCard parameter ZAE_WR_WEIGHINGSESSION result [1] $self;
action setloadType parameter ZAE_WR_WEIGHINGSESSION  result [1] $self;

//validation validateStep1 on save { field SalesDocument ;} // Identification
//validation validateStep2 on save { field Loadtype; } // Load type
//validation ValidateLoadType on save { field LoadType; } // Calls a method to check against VH
//
//determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math
}

**The Root View**

@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Interface View for weighbridge'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true
define root view entity ZI_WR_WEIGHBRIDGE
  as select from zwr_weighbridge
{
   key sessionid                   as SESSIONID,
       salesdocument               as SalesDocument,
       sddocumentcategory          as SDDocumentCategory,
       salesdocumenttype           as SalesDocumentType

//      SalesDocumentProcessingType as SalesDocumentProcessingType,
//      CreatedByUser               as CreatedByUser,
//      LastChangedByUser           as LastChangedByUser,
//      CreationDate                as CreationDate,
//      CreationTime                as CreationTime,
//      LastChangeDate              as LastChangeDate,
//      LastChangeDateTime          as LastChangeDateTime,
//      LastCustomerContactDate     as LastCustomerContactDate
}
where
      salesdocumenttype = 'ZKM'

**
**The controller****

sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",
    "sap/m/MessageBox",
    "sap/ui/model/json/JSONModel"
], function (PageController, MessageToast, Messaging, MessageBox, JSONModel) {
    "use strict";

    return PageController.extend("com.prologa.zwrweighbrigefinal.ext.main.Main", {
        onInit: function () {
            const parentReturn = PageController.prototype.onInit.apply(this);

            // Messaging
            Messaging.registerObject(this.getView(), true);
            this.getView().setModel(Messaging.getMessageModel(), "message");

            // Local view model (user input etc.)
            this.getView().setModel(new JSONModel({
                contractId: ""     // user types/scans here
            }), "view");

            this.oWizard = this.byId("weighingWizard");

            // Route
            this.getAppComponent().getRouter()
                .getRoute("ZI_WR_WEIGHBRIDGEMain")
                .attachPatternMatched(this._onObjectMatched, this);

            this._enterWired = false;
            this._isFromScan = true;
            return parentReturn;
        },

        _onObjectMatched: function () {
            const oModel = this.getView().getModel();

            // Bind to entity set and create a new draft instance
            const oListBinding = oModel.bindList("/ZI_WR_WEIGHBRIDGE");
            const oCtx = oListBinding.create({});   // no unknown or computed props

            // Make it the view context
            this.getView().setBindingContext(oCtx);

            // Optional: ensure data is loaded (instead of requestProperty([...]))
            oCtx.requestObject().catch(() => { });
        },

        // User pressed Enter / left the field: call bound action identifyCard(vbeln)
        onContractChange: function () {
            const oCtx = this.getView().getBindingContext();
            if (!oCtx) { return; }

            const sVbeln = this.getView().getModel("view").getProperty("/contractId") || "";
            if (!sVbeln) { return; }

            // Call bound action: identifyCard
            oCtx.invokeAction("identifyCard", {
                method: "POST",
                parameters: { vbeln: sVbeln } // matches EDM parameter
            }).then(() => {
                // pull updated properties like SalesDocument back from backend
                return oCtx.requestSideEffects([
                    { $PropertyPath: "SalesDocument" },
                    { $PropertyPath: "SalesDocumentType" },
                    { $PropertyPath: "SDDocumentCategory" }
                ]);
            }).catch((e) => {
                MessageBox.error(e.message || "Action failed");
            });
        },

        // (optional stubs so XML handlers don’t explode even if not used yet)
        onScanCard: function () { },
        onNextStep: function () { },
        onChooseLoadType: function () { },
        onBackToStep1: function () { }
    });
});


**The View**

<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" >

      <!-- STEP 1: Identification -->
      <WizardStep id="step1" title="Identification" >
        <HBox id="step1HBox" width="100%" justifyContent="Center">
          <Panel id="step1Panel" class="stepPanel" expandable="false"> <!-- headerText="1. Identification"> -->
            <content>
              <f:Form id="step1Form" editable="true">
                <f:layout><f:ResponsiveGridLayout id="tgrid"/></f:layout>
                <f:formContainers>
                  <f:FormContainer id="step1FormContainer">
                    <f:formElements>
                      <f:FormElement id="step1FormElementContract" label="Please Enter your Contract ID">
                        <f:fields>
                         <Input id="ipContract" value="{SalesDocument}" width="80%" maxLength="10" required="true" placeholder="Scan or enter Contract ID" class="sapUiSizeCompact" change=".onContractChange"/>   
                          <HBox id="step1ScanRow" width="100%" justifyContent="SpaceBetween" class="sapUiMediumMarginTop">
                            <Button id="btnScanCard"
                                    width="12rem"
                                    type="Emphasized"
                                    icon="sap-icon://business-card"
                                    text="Scan Card"
                                    press="onScanCard"/>
                          </HBox>
                        </f:fields>
                      </f:FormElement>
                    </f:formElements>
                  </f:FormContainer>
                </f:formContainers>
              </f:Form>
              <HBox id="step1Buttons" justifyContent="Start" width="100%" class="sapUiMediumMarginTop">
                <Button id="btnStep1Next" text="Next" type="Emphasized" press="onNextStep" visible="false"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 2: Choose Load Type -->
      <WizardStep id="step2" title="Choose Load Type"> <!--  validated="{= ${viewModel>/step2_ok} ? true : false }" -->
        <HBox id="step2HBox" width="100%" justifyContent="Center">
          <Panel id="step2Panel" class="stepPanel" expandable="false"> <!-- headerText="2. Choose Load Type"> -->
            <content>
              <VBox id="step2VBox">
                <!-- <Text id="step2Instruction" text="Position vehicle on weighbridge" class="sapUiSmallMarginBottom"/> -->

                <!-- Big tappable buttons from VH entity -->
                <VBox id="ltContainer"
                      items="{
                        path: '/ZI_WR_SALESITEM_CONTRACTVH',
                        parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                      }">
                  <Button
                    id="ltButton"
                    class="loadTypeBtn"
                    width="100%"
                    text="{= ${Material} + ' - ' + ${MaterialText} }"
                    press=".onChooseLoadType"/>
                </VBox>

                <!-- Show selection -->
                <Text id="step2SelectedText" class="sapUiSmallMarginTop"
                      text="{= ${binding>SalesOrder} ? 'Selected: ' + ${binding>SalesOrder} : ''}"/>

                <HBox id="step2Buttons" justifyContent="SpaceBetween" width="80%" class="sapUiMediumMarginTop">
                  <Button id="btnStep2Back" text="Back" press=".onBackToStep1"/>
                  <Button id="btnStep2Next" text="Next" type="Emphasized" press="onNextStep"/>
                </HBox>
              </VBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

</Wizard>
</content>
</Page>
</mvc:View>
