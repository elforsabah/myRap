  @UI.lineItem:       [{ position: 80, label: 'Sicherheitsbestand', importance: #HIGH,
                         criticality: 'SafetyStockCriticality' },
                       { type: #FOR_ACTION, dataAction: 'sicherheitb_aendern',
                         label: 'Sicherheitsbestand ändern' }]
  @UI.identification: [{ position: 40, label: 'Sicherheitsbestand',
                         criticality: 'SafetyStockCriticality' },
                       { type: #FOR_ACTION, dataAction: 'sicherheitb_aendern',
                         label: 'Sicherheitsbestand ändern' }]
  SafetyStock;
