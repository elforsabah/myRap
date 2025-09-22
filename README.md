DATA(lv_input)  = 'WE00000000000000002802'.
DATA(lv_suffix) = lv_input+2.   "skip the first 'WE'

SHIFT lv_suffix LEFT DELETING LEADING '0'.

WRITE: / lv_suffix.  "2802
