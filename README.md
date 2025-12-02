DATA lt_servers TYPE TABLE OF msxxlist.

CALL FUNCTION 'TH_SERVER_LIST'
  TABLES
    list = lt_servers.

LOOP AT lt_servers INTO DATA(ls_server).
  WRITE: / ls_server-host, ls_server-name, ls_server-instno.
ENDLOOP.
