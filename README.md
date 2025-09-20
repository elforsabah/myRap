.myApp .myCustomFormElement .sapMLabel {
    font-size: 1.9rem !important;
    font-weight: bold !important;
    color: red !important;
}

/* Responsive panel for tablet */
.stepPanel {
    width: 90% !important;     /* take most of the tablet width */
    max-width: 40rem !important;          /* but don’t get too wide */
    margin: 1rem !important;              /* breathing space */
}

/* Bigger touch buttons */
.loadTypeBtn {
    margin: 0.5rem 0 !important;
    padding: 1.5rem 1rem !important;     
    border: 1px solid #d0d5db !important;
    border-radius: 0.5rem !important;
    text-align: left !important;
    font-size: 1.1rem !important;
    font-weight: 600 !important;
    color: #218154 !important;;
}

/* Solid green button */
.loadTypeBtn .sapMBtnInner {
    background-color: #1f8e9c;     /* base */
    border-color: #1f6d8b;
    color: #ffffff;                /* white label */
}

/* Hover (lighter) */
.loadTypeBtn.sapMBtn:hover .sapMBtnInner {
    background-color: #3ab87d;     /* +~8% light */
    border-color: #1b9fa3;
}

/* Active/pressed (darker) */
.loadTypeBtn.sapMBtn:active .sapMBtnInner,
.loadTypeBtn.sapMBtnActive .sapMBtnInner {
    background-color: #0f693f;     /* -~10% dark */
    border-color: #2c77a1;
}

/* Disabled (muted) */
.loadTypeBtn.sapMBtnDisabled .sapMBtnInner {
    background-color: #75d4a7;
    border-color: #2e89a5;
    color: #ffffff;
}

.sapMWizardProgressNavStepIcon {
    color: rgb(47, 226, 12) !important; /* Replace with your desired color, e.g., #FF0000 or rgb(255, 0, 0) */
}

/* Bigger touch buttons for largeWeighButton */
.largeWeighButton {
    margin: 0.5rem 0 !important;
    padding: 1.5rem 1rem !important;     
    border: 1px solid #d0d5db !important;
    border-radius: 0.5rem !important;
    text-align: left !important;
    font-size: 9rem !important;
    font-weight: 600 !important;
    color: #218154 !important;
}

/* Solid green button for largeWeighButton */
.largeWeighButton .sapMBtnInner {
    background-color: #1f8e9c;     /* base */
    border-color: #1f6d8b;
    color: #ffffff;                /* white label */
}

/* Hover (lighter) for largeWeighButton */
.largeWeighButton.sapMBtn:hover .sapMBtnInner {
    background-color: #3ab87d;     /* +~8% light */
    border-color: #1b9fa3;
}

/* Active/pressed (darker) for largeWeighButton */
.largeWeighButton.sapMBtn:active .sapMBtnInner,
.largeWeighButton.sapMBtnActive .sapMBtnInner {
    background-color: #0f693f;     /* -~10% dark */
    border-color: #2c77a1;
}

/* Disabled (muted) for largeWeighButton */
.largeWeighButton.sapMBtnDisabled .sapMBtnInner {
    background-color: #75d4a7;
    border-color: #2e89a5;
    color: #ffffff;
}

.sapMMessageToast.myGreenToast {
    color: #4F8A10 !important;
    background-color: #DFF2BF !important;
    border: 1px solid #4F8A10 !important;
    border-left: 7px solid #4F8A10 !important;
}

.veryBigNumber .sapMObjectNumberText {
    font-size: 2rem !important; /* Makes it very very big; rem is relative to root font size */
    font-weight: bold !important; /* Ensures boldness, though emphasized="true" already handles this */
    color: #000000;
}


.bigWeightContainer {
  margin-top: 3rem;
}

.bigWeightNumber {
  font-size: 1.5rem;     /* adjust until it fits */
  font-weight: bold;
  white-space: nowrap;   /* keep number + KG together */
}
 
