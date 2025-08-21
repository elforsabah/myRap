<mvc:View
  controllerName="your.namespace.controller.Wizard"
  xmlns:mvc="sap.ui.core.mvc"
  xmlns="sap.m"
  xmlns:f="sap.ui.layout.form"
  xmlns:core="sap.ui.core"
  xmlns:macros="sap.fe.macros">

  <VBox width="100%">
    <!-- Optional app header -->
    <Toolbar>
      <Title text="Weighbridge Integration" level="H4"/>
      <ToolbarSpacer/>
    </Toolbar>

    <!-- Wizard -->
    <Wizard id="wiz" validateOnNext="true">

      <!-- STEP 1: Identification -->
      <WizardStep id="step1" title="Identification" validated="{= ${viewModel>/step1_ok} ? true : false }">
        <HBox width="100%" justifyContent="Center">
          <Panel width="36rem" expandable="false" headerText="1. Identification">
            <content>
              <f:Form editable="true">
                <f:layout>
                  <f:ResponsiveGridLayout/>
                </f:layout>
                <f:formContainers>
                  <f:FormContainer>
                    <f:formElements>
                      <f:FormElement label="Please Enter your Contract ID">
                        <f:fields>
                          <!-- Bind to your session entity field -->
                          <Input id="ipContract" value="{binding>vbeln}" width="100%" maxLength="15"/>
                        </f:fields>
                      </f:FormElement>
                    </f:formElements>
                  </f:FormContainer>
                </f:formContainers>
              </f:Form>

              <HBox justifyContent="End" width="100%" class="sapUiSmallMarginTop">
                <Button id="btnStep1Next" text="Next" type="Emphasized" press=".onStep1Next"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 2: Choose Load Type -->
      <WizardStep id="step2" title="Choose Load Type" validated="{= ${viewModel>/step2_ok} ? true : false }">
        <HBox width="100%" justifyContent="Center">
          <Panel width="36rem" expandable="false" headerText="2. Choose Load Type">
            <content>
              <VBox>
                <Text text="Position vehicle on weighbridge" class="sapUiSmallMarginBottom"/>
                <!-- OPTION A: Big tappable buttons from VH entity -->
                <VBox id="ltContainer"
                      items="{
                        path: '/ZI_WR_LOADTYPEVH',
                        parameters: { $select: 'LoadType,LoadTypeText', $orderby: 'LoadType' }
                      }">
                  <Button
                    class="loadTypeBtn"
                    width="100%"
                    text="{= ${LoadType} + ' - ' + ${LoadTypeText} }"
                    press=".onChooseLoadType"/>
                </VBox>

                <!-- Show selection -->
                <Text class="sapUiSmallMarginTop" text="{= ${binding>LoadType} ? 'Selected: ' + ${binding>LoadType} : ''}"/>

                <HBox justifyContent="SpaceBetween" width="100%" class="sapUiSmallMarginTop">
                  <Button text="Back" press=".onBackToStep1"/>
                  <Button text="Next" type="Emphasized" press=".onStep2Next"/>
                </HBox>
              </VBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 3: Weighing -->
      <WizardStep id="step3" title="Weighing" validated="{= ${viewModel>/step3_ok} ? true : false }">
        <HBox width="100%" justifyContent="Center">
          <Panel width="36rem" expandable="false" headerText="3. Weighing">
            <content>
              <f:Form editable="true">
                <f:layout>
                  <f:ResponsiveGridLayout/>
                </f:layout>
                <f:formContainers>
                  <f:FormContainer>
                    <f:formElements>
                      <f:FormElement label="Gross Weight">
                        <f:fields>
                          <Input value="{binding>GrossWeight}" width="100%" type="Number"/>
                        </f:fields>
                      </f:FormElement>
                      <f:FormElement label="Tare Weight">
                        <f:fields>
                          <Input value="{binding>TareWeight}" width="100%" type="Number"/>
                        </f:fields>
                      </f:FormElement>
                      <f:FormElement label="Net Weight">
                        <f:fields>
                          <Input value="{binding>NetWeight}" width="100%" type="Number" editable="false"/>
                        </f:fields>
                      </f:FormElement>
                    </f:formElements>
                  </f:FormContainer>
                </f:formContainers>
              </f:Form>

              <HBox justifyContent="SpaceBetween" width="100%" class="sapUiSmallMarginTop">
                <Button text="Back" press=".onBackToStep2"/>
                <Button text="Finish" type="Accept" press=".onFinish"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

    </Wizard>
  </VBox>
</mvc:View>
