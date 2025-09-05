<f:fields>
  <HBox id="step3HBoxWeight" width="100%" justifyContent="Center" class="bigWeightContainer">
    <Text
      id="step3TextWeight"
      text="{local>/mainWeight} KG"
      textAlign="Center"
      wrapping="true"
      class="bigWeightNumber"/>
  </HBox>
</f:fields>


.bigWeightContainer {
  margin-top: 1rem;
}

.bigWeightNumber {
  font-size: 2.5rem;     /* adjust until it fits */
  font-weight: bold;
  white-space: nowrap;   /* keep number + KG together */
}
