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

.largeWeighButton {
    font-size: 9rem !important;
}
