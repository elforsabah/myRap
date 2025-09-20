<!-- Row 2 -->
<f:FormElement id="step3FormElementWeight" label="">
  <f:fields>
    <HBox id="step3HBoxWeight" width="100%" justifyContent="Start" class="bigWeightContainer">
      <Text
        id="step3TextWeight"
        text="{= ${local>/grossWeight} + '\n' + ${local>/teraWeight} }"
        textAlign="Center"
        wrapping="true"
        class="bigWeightNumber"/>
    </HBox>
  </f:fields>
</f:FormElement>
