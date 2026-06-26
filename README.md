DATA(lv_position_type) = SWITCH string( ls_vbap-pstyv
  WHEN 'ZDVC' THEN 'AWI'        " ← replace with actual BMS enum value
  WHEN 'ZDVN' THEN 'AWI'        " ← same
  ELSE              'AWI' ).    " ← safe default until confirmed
