<mvc:View
  xmlns:core="sap.ui.core"
  xmlns:mvc="sap.ui.core.mvc"
  xmlns="sap.m"
  xmlns:macros="sap.fe.macros"
  xmlns:html="http://www.w3.org/1999/xhtml"
  xmlns:f="sap.ui.layout.form"
  controllerName="com.prologa.zwrweighbrigefinal.ext.main.Main">

  <Page id="Main" title="{i18n>MainTitle}" class="myApp">
    <content>
      <Wizard id="weighingWizard" complete="onWizardComplete">
        <!-- STEP 1: Identification -->
        <WizardStep id="step1" title="Identification" validated="{= ${viewModel>/step1_ok} ? true : false }">
          <HBox id="step1HBox" width="100%" justifyContent="Center">
            <Panel id="step1Panel" class="stepPanel" expandable="false">
              <content>
                <f:Form id="step1Form" editable="true">
                  <f:layout>
                    <f:ResponsiveGridLayout id="tgrid"/>
                  </f:layout>
                  <f:formContainers>
                    <f:FormContainer id="step1FormContainer">
                      <f:formElements>
                        <f:FormElement id="step1FormElementContract" label="Please Enter your Contract ID">
                          <f:fields>
                            <Input id="ipContract"
                                   value="{Vbeln}"
                                   width="80%"
                                   maxLength="10"
                                   required="true"
                                   placeholder="Scan or enter Contract ID"
                                   class="sapUiSizeCompact"
                                   change=".onContractChange"/>

                            <!--
                            <Input id="ipContract"
                                   value="{ path: '/Vbeln', type: 'sap.ui.model.type.Integer', constraints: { minimum: 1, maximum: 999999 } }"
                                   maxLength="10" required="true"/>
                            -->
                            <!-- <macros:Field id="ipContract" contextPath="/ZI_WR_WEIGHINGSESSION" metaPath="Vbeln" readOnly="false"  /> -->

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
      </Wizard>
    </content>
  </Page>
</mvc:View>
