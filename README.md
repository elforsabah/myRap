<!-- Row 2 -->
<f:FormElement id="step3FormElementWeight" label="">
  <f:fields>
    <HBox id="step3HBoxWeight" width="100%" justifyContent="Center" class="bigWeightContainer">
      <FormattedText
        id="step3TextWeight"
        htmlText="{= '&lt;div&gt;' + ${local>/grossWeight} + '&lt;/div&gt;&lt;div&gt;' + ${local>/teraWeight} + '&lt;/div&gt;' }"
        class="bigWeightNumber"/>
    </HBox>
  </f:fields>
</f:FormElement>
