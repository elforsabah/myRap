<?xml version="1.0"?>
<WebDynProDefinitions version="2">
 <!-- Target BSP-component id (might be used during generation) -->
 <Component id="">
  <!-- Models, Controllers, and Views section only relevant for a designtime repository used for code generation-->
  <Models>
   <!-- Reference to BOL Component Set -->
   <Model id="EWA"/>
  </Models>
  <Controllers>
   <!-- Declaration of custom controllers -->
   <Controller id="BSPWDComponent" type="COMPONENT"/>
  </Controllers>
  <Views>
   <!-- Declaration of views -->
  </Views>
  <!-- Windows, ViewSets, NavigationalLinks, ComponentInterface, and ComponentUsages are parts necessary for runtime repository -->
  <Windows>
   <!-- Declaration of windows -->
   <Window id="EWA_IC_OV_ACCT/BusinessDataEnvironment">
    <InboundPlugs/>
    <OutboundPlugs>
     <OutboundPlug name="ALLOCATECONTAINER"/>
     <OutboundPlug name="PLACECONTAINER"/>
    </OutboundPlugs>
    <EmbeddedView id="EWA_IC_OV_ACCT/VBusinessDataEnvironment" default="TRUE"/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/ObjectDataEnvironment">
    <InboundPlugs/>
    <OutboundPlugs/>
    <EmbeddedView id="EWA_IC_OV_ACCT/VObjectDataEnvironment" default="TRUE"/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/Contracts">
    <EmbeddedView id="EWA_IC_OV_ACCT/VContracts" default="TRUE"/>
    <InboundPlugs/>
    <OutboundPlugs/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/Quotations">
    <EmbeddedView id="EWA_IC_OV_ACCT/VQuotations" default="TRUE"/>
    <InboundPlugs/>
    <OutboundPlugs/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/Invoices">
    <InboundPlugs/>
    <OutboundPlugs/>
    <EmbeddedView id="EWA_IC_OV_ACCT/VInvoices" default="TRUE"/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/Container">
    <EmbeddedView id="EWA_IC_OV_ACCT/VContainers" default="TRUE"/>
    <InboundPlugs/>
    <OutboundPlugs/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/CleaningObjects">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
    <EmbeddedView id="EWA_IC_OV_ACCT/VCleaningObjects" default="TRUE"/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/Cases">
    <InboundPlugs/>
    <OutboundPlugs/>
    <EmbeddedView id="EWA_IC_OV_ACCT/VCases" default="TRUE"/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/Header">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
    <EmbeddedView id="EWA_IC_OV_ACCT/VHeader" default="TRUE"/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/MainWindow" default="TRUE">
    <InboundPlugs>
     <InboundPlug name="DISPLAY" followUpNavigationalLink="MainWindowToFactSheet"/>
    </InboundPlugs>
    <OutboundPlugs/>
    <EmbeddedView id="UFactSheet.BSP_DLC_FS/MainWindow" default="TRUE"/>
    <EmbeddedView id="UServiceAddress.EWA_SERVADDRESS/DAddCleaningObject"/>
    <EmbeddedView id="UServiceAddress.EWA_SERVADDRESS/DAllocateContainer"/>
    <EmbeddedView id="UServiceAddress.EWA_SERVADDRESS/DPlaceContainer"/>
    <EmbeddedView id="UServices.EWA_FREQUENCY/DReoccuringService"/>
    <EmbeddedView id="UServices.EWA_FREQUENCY/WMask"/>
    <EmbeddedView id="UServices.EWA_FREQUENCY/DOneTimeService"/>
    <EmbeddedView id="UServices.EWA_FREQUENCY/DServiceDates"/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/Services">
    <EmbeddedView id="EWA_IC_OV_ACCT/VServices" default="TRUE"/>
    <InboundPlugs/>
    <OutboundPlugs/>
   </Window>
   <Window id="EWA_IC_OV_ACCT/SDInvoice">
    <InboundPlugs/>
    <OutboundPlugs/>
    <EmbeddedView id="EWA_IC_OV_ACCT/VSDInvoices" default="TRUE"/>
   </Window>
   <Window id="ZWR_IC_OV_ACCT/Qmel">
    <InboundPlugs/>
    <OutboundPlugs/>
    <EmbeddedView id="ZWR_IC_OV_ACCT/QmelData" default="TRUE"/>
   </Window>
   <Window id="ZWR_IC_OV_ACCT/Holiday"/>
  </Windows>
  <ViewSets>
   <ViewSet id="EWA_IC_OV_ACCT/OVMain">
    <ViewArea id="OverviewPage">
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/BusinessDataEnvironment" default="TRUE"/>
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/ObjectDataEnvironment"/>
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/Cases"/>
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/CleaningObjects"/>
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/Container"/>
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/Contracts"/>
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/Header"/>
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/Invoices"/>
     <EmbeddedView id="USelf.EWA_IC_OV_ACCT/Quotations"/>
    </ViewArea>
   </ViewSet>
   <ViewSet id="EWA_IC_OV_ACCT/VBusinessDataEnvironment">
    <ViewArea id="CONTENT">
     <EmbeddedView id="BusinessEnvironmentTree.MainWindow" default="TRUE"/>
    </ViewArea>
   </ViewSet>
   <ViewSet id="EWA_IC_OV_ACCT/VObjectDataEnvironment">
    <ViewArea id="CONTENT">
     <EmbeddedView id="ObjectEnvironmentTree.MainWindow" default="TRUE"/>
    </ViewArea>
   </ViewSet>
  </ViewSets>
  <NavigationalLinks>
   <NavigationalLink name="BuildTreeBusinessDataEnvironmentTree">
    <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/BusinessDataEnvironment"/>
    <Targets>
     <Target viewRef="BusinessEnvironmentTree.MainWindow" inboundPlugRef="DEFAULT_SELECT_FIRST"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="MainWindowToFactSheet">
    <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    <Targets>
     <Target inboundPlugRef="DEFAULT" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>
    </Targets>
   </NavigationalLink>
   <!--   <NavigationalLink name="MainWindowToPlaceContainer">
 <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
 <Targets>
                                                        <Target inboundPlugRef="EDIT" viewRef="UServiceAddress.EWA_SERVADDRESS/DPlaceContainer"/>
 </Targets>
                                                                                                                                       </NavigationalLink>
 <NavigationalLink name="MainWindowToAllocateContainer">
                                                                                                   <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
 <Targets>
                                                                                                                                                          <Target inboundPlugRef="EDIT" viewRef="UServiceAddress.EWA_SERVADDRESS/DAllocateContainer"/>
 </Targets>
                                                                 </NavigationalLink>
 <NavigationalLink name="MainWindowToAddCleaningObject">
 <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
                                                                                                                                                                                               <Targets>
 <Target inboundPlugRef="EDIT" viewRef="UServiceAddress.EWA_SERVADDRESS/DAddCleaningObject"/>
                                                                                                            </Targets>
 </NavigationalLink>
 -->
   <NavigationalLink name="MainWindowToPlaceContainer">
    <Source outboundPlugRef="TOPLACECONTAINER" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    <Targets>
     <Target inboundPlugRef="EDIT" viewRef="UServiceAddress.EWA_SERVADDRESS/DPlaceContainer"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="MainWindowToAllocateContainer">
    <Source outboundPlugRef="TOALLOCATECONTAINER" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    <Targets>
     <Target inboundPlugRef="EDIT" viewRef="UServiceAddress.EWA_SERVADDRESS/DAllocateContainer"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="MainWindowToAddCleaningObject">
    <Source outboundPlugRef="TOADDCLEANINGOBJECT" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    <Targets>
     <Target inboundPlugRef="EDIT" viewRef="UServiceAddress.EWA_SERVADDRESS/DAddCleaningObject"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromPlaceContainer">
    <Source outboundPlugRef="DIALOGDONE" viewRef="UServiceAddress.EWA_SERVADDRESS/DPlaceContainer"/>
    <Targets>
     <!--     <Target inboundPlugRef="DEFAULT" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>-->
     <Target inboundPlugRef="FROM_DIALOGDONE" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromAllocateContainer">
    <Source outboundPlugRef="DIALOGDONE" viewRef="UServiceAddress.EWA_SERVADDRESS/DAllocateContainer"/>
    <Targets>
     <!--     <Target inboundPlugRef="DEFAULT" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>-->
     <Target inboundPlugRef="FROM_DIALOGDONE" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromAddCleaningObject">
    <Source outboundPlugRef="DIALOGDONE" viewRef="UServiceAddress.EWA_SERVADDRESS/DAddCleaningObject"/>
    <Targets>
     <!--     <Target inboundPlugRef="DEFAULT" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>-->
     <Target inboundPlugRef="FROM_DIALOGDONE" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromReoccuringService">
    <Source outboundPlugRef="DIALOGDONE" viewRef="UServices.EWA_FREQUENCY/DReoccuringService"/>
    <Targets>
     <!--     <Target inboundPlugRef="DEFAULT" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>-->
     <Target inboundPlugRef="FROM_DIALOGDONE" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromService">
    <Source outboundPlugRef="DIALOGDONE" viewRef="UServices.EWA_FREQUENCY/WMask"/>
    <Targets>
     <!--     <Target inboundPlugRef="DEFAULT" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>-->
     <Target inboundPlugRef="FROM_DIALOGDONE" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="ToOneTimeServiceEdit">
    <Source outboundPlugRef="TOONETIMESERVICEEDIT" viewRef="UServices.EWA_FREQUENCY/WMask"/>
    <Targets>
     <Target inboundPlugRef="EDIT" viewRef="UServices.EWA_FREQUENCY/DOneTimeService"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromOnetimeService">
    <Source outboundPlugRef="DIALOGDONE" viewRef="UServices.EWA_FREQUENCY/DOneTimeService"/>
    <Targets>
     <!--     <Target inboundPlugRef="DEFAULT" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>-->
     <Target inboundPlugRef="FROM_DIALOGDONE" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromFSDefault">
    <Source outboundPlugRef="DEFAULT" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>
    <Targets>
     <Target inboundPlugRef="FROM_FS_DEFAULT" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    </Targets>
   </NavigationalLink>
   <!--   <NavigationalLink name="MainWindowToFactSheetNoRefresh">
                                                                                                                                                                                              <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
 <Targets>
                                                                                                            <Target inboundPlugRef="#INITIAL#" viewRef="UFactSheet.BSP_DLC_FS/MainWindow"/>
                                                                                                                          </Targets>
 </NavigationalLink>
                                                                                                                      -->
   <NavigationalLink name="MainWindowToReoccurringService">
    <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    <Targets>
     <Target inboundPlugRef="NEW" viewRef="UServices.EWA_FREQUENCY/DReoccuringService"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromReoccuringServiceToServiceDatesEdit">
    <Source outboundPlugRef="TOSERVICEDATESEDIT" viewRef="UServices.EWA_FREQUENCY/DReoccuringService"/>
    <Targets>
     <Target inboundPlugRef="EDIT" viewRef="UServices.EWA_FREQUENCY/DServiceDates"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="FromServiceDatesToReoccuringService">
    <Source outboundPlugRef="DIALOGDONE" viewRef="UServices.EWA_FREQUENCY/DServiceDates"/>
    <Targets>
     <Target inboundPlugRef="DEFAULT" viewRef="UServices.EWA_FREQUENCY/DReoccuringService"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="MainWindowToOneTimeService">
    <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    <Targets>
     <Target inboundPlugRef="NEW" viewRef="UServices.EWA_FREQUENCY/DOneTimeService"/>
    </Targets>
   </NavigationalLink>
   <NavigationalLink name="MainWindowToOneTimeServiceExcpH">
    <Source outboundPlugRef="" viewRef="EWA_IC_OV_ACCT/MainWindow"/>
    <Targets>
     <Target inboundPlugRef="NEW_EXCP_HEAD" viewRef="UServices.EWA_FREQUENCY/DOneTimeService"/>
    </Targets>
   </NavigationalLink>
  </NavigationalLinks>
  <ComponentInterface id="">
   <!-- Declaration of Interface views and public parts of the component controller -->
   <InterfaceController>
    <Context/>
    <Events/>
   </InterfaceController>
   <InterfaceView id="EWA_IC_OV_ACCT/BusinessDataEnvironment" mappedToWindow="EWA_IC_OV_ACCT/BusinessDataEnvironment">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/ObjectDataEnvironment" mappedToWindow="EWA_IC_OV_ACCT/ObjectDataEnvironment">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/Contracts" mappedToWindow="EWA_IC_OV_ACCT/Contracts">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/Quotations" mappedToWindow="EWA_IC_OV_ACCT/Quotations">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/Invoices" mappedToWindow="EWA_IC_OV_ACCT/Invoices">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/Container" mappedToWindow="EWA_IC_OV_ACCT/Container">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/CleaningObjects" mappedToWindow="EWA_IC_OV_ACCT/CleaningObjects">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/Cases" mappedToWindow="EWA_IC_OV_ACCT/Cases">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/Header" mappedToWindow="EWA_IC_OV_ACCT/Header">
    <InboundPlugs>
     <InboundPlug name="DEFAULT"/>
     <InboundPlug name="FROM_IC"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/MainWindow" mappedToWindow="EWA_IC_OV_ACCT/MainWindow"/>
   <InterfaceView id="EWA_IC_OV_ACCT/Services" mappedToWindow="EWA_IC_OV_ACCT/Services">
    <InboundPlugs/>
    <OutboundPlugs/>
   </InterfaceView>
   <InterfaceView id="EWA_IC_OV_ACCT/SDInvoice" mappedToWindow="EWA_IC_OV_ACCT/SDInvoice">
    <InboundPlugs>
     <InboundPlug name="FROM_IC"/>
     <InboundPlug name="DEFAULT"/>
    </InboundPlugs>
    <OutboundPlugs/>
   </InterfaceView>
  </ComponentInterface>
  <ComponentUsages>
   <!-- Declaration of the usage of other components -->
   <ComponentUsage id="BusinessEnvironmentTree" usedComponent="IUBOTREE">
    <InterfaceView id="MainWindow">
     <InboundPlugs/>
     <OutboundPlugs/>
    </InterfaceView>
   </ComponentUsage>
   <ComponentUsage id="ObjectEnvironmentTree" usedComponent="IUBOTREE">
    <InterfaceView id="MainWindow"/>
   </ComponentUsage>
   <ComponentUsage id="UServices" usedComponent="EWA_FREQUENCY">
    <InterfaceView id="EWA_FREQUENCY/DOneTimeService"/>
    <InterfaceView id="EWA_FREQUENCY/DReoccuringService">
     <InboundPlugs/>
     <OutboundPlugs/>
    </InterfaceView>
    <InterfaceView id="EWA_FREQUENCY/DServiceDates"/>
    <InterfaceView id="EWA_FREQUENCY/DChangeKeyDate"/>
   </ComponentUsage>
   <ComponentUsage id="UServiceAddress" usedComponent="EWA_SERVADDRESS">
    <InterfaceView id="EWA_SERVADDRESS/DAllocateContainer"/>
    <InterfaceView id="EWA_SERVADDRESS/DAddCleaningObject"/>
    <InterfaceView id="EWA_SERVADDRESS/DPlaceContainer"/>
   </ComponentUsage>
   <ComponentUsage id="UFactSheet" usedComponent="BSP_DLC_FS">
    <InterfaceView id="BSP_DLC_FS/MainWindow">
     <InboundPlugs/>
     <OutboundPlugs/>
    </InterfaceView>
   </ComponentUsage>
  </ComponentUsages>
 </Component>
</WebDynProDefinitions>
