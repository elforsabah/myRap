DATA: lv_timestampl TYPE timestampl VALUE '20250721133408.7780000',
      lv_date       TYPE d,
      lv_time       TYPE t,
      lv_fs         TYPE p LENGTH 7 DECIMALS 7,
      lv_timestamp  TYPE timestamp,
      lv_readable   TYPE string.

CONVERT TIME STAMP lv_timestampl
  TIME ZONE 'UTC'
  INTO DATE lv_date
       TIME lv_time
       FRACTIONAL SECONDS lv_fs.

lv_timestamp = lv_date && lv_time.

lv_readable = |{ lv_timestamp TIMESTAMP = USER }|.
