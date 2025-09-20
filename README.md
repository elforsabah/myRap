<!-- Row 2 -->
<f:FormElement id="step3FormElementWeight" label="">
  <f:fields>
    <HBox id="step3HBoxWeight" width="100%" justifyContent="Center" class="bigWeightContainer">
      <VBox id="step3VBoxWeights" class="tightWeights">
        <Text
          id="step3TextWeight"
          text="{local>/grossWeight}"
          textAlign="Center"
          wrapping="true"
          class="bigWeightNumber"/>
        <Text
          id="step3TextWeight2"
          text="{local>/teraWeight}"
          textAlign="Center"
          wrapping="true"
          class="bigWeightNumber"/>
      </VBox>
    </HBox>
  </f:fields>
</f:FormElement>
