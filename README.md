// 1. Hide the standard SAP field (Hours)
  @UI.hidden: true
  DrivingTime;

  // 2. Show your new custom field (Minutes)
  @UI.lineItem: [{ position: 105, importance: #HIGH }] // Adjust position as needed
  zz_driving_time_min;
