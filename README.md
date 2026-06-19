SELECT SINGLE bms_endpoint_url,
              bms_username,
              bms_password,
              active
  FROM ztour_bms_cfg
  WHERE mandt     = @sy-mandt
    AND config_id = 'DEFAULT'
  INTO @DATA(ls_cfg).
