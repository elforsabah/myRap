// Side effects so Netweight refreshes after Gross/Tare changes
  @Consumption.derivation: { sideEffects: [
    { sourceProperties: ['Grossweight','Tareweight'], targetProperties: ['Netweight'] }
  ] }
