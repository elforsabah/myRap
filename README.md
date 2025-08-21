/* Solid green button */
.loadTypeBtn .sapMBtnInner {
  background-color: #309968;     /* base */
  border-color: #309968;
  color: #ffffff;                /* white label */
}

/* Hover (lighter) */
.loadTypeBtn.sapMBtn:hover .sapMBtnInner {
  background-color: #3ab87d;     /* +~8% light */
  border-color: #3ab87d;
}

/* Active/pressed (darker) */
.loadTypeBtn.sapMBtn:active .sapMBtnInner,
.loadTypeBtn.sapMBtnActive .sapMBtnInner {
  background-color: #24724e;     /* -~10% dark */
  border-color: #24724e;
}

/* Disabled (muted) */
.loadTypeBtn.sapMBtnDisabled .sapMBtnInner {
  background-color: #75d4a7;
  border-color: #75d4a7;
  color: #ffffff;
}
