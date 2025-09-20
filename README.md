<!-- Row 2 -->
<f:FormElement id="step3FormElementWeight" label="">
  <f:fields>
    <HBox id="step3HBoxWeight" width="100%" justifyContent="Center" class="bigWeightContainer">
      <FormattedText
        id="step3TextWeight"
        htmlText="{= ${local>/grossWeight} + '&lt;br/&gt;' + ${local>/teraWeight} }"
        class="bigWeightNumber"/>
    </HBox>
  </f:fields>
</f:FormElement>
