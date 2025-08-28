DATA: lv_timestampl TYPE timestampl VALUE '20250721133408.7780000',
      lv_str        TYPE string,
      lv_timestamp  TYPE timestamp,
      lv_readable   TYPE string.

lv_str = |{ lv_timestampl }|.

lv_str = substring_before( val = lv_str sub = '.' ).  " Or use off=0 len=14 if fixed format

lv_timestamp = lv_str.

lv_readable = |{ lv_timestamp TIMESTAMP = USER }|.
