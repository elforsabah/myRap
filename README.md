    METHODS remove_obsolete_tour
      IMPORTING iv_touruuid       TYPE /plce/pdtour_uuid
                it_new            TYPE zcl_wr_misc_pd=>tty_attach
      RETURNING VALUE(rv_removed) TYPE i.

    METHODS remove_obsolete_service
      IMPORTING iv_serviceuuid    TYPE /plce/pdservice_uuid
                it_new            TYPE zcl_wr_misc_pd=>tty_attach
      RETURNING VALUE(rv_removed) TYPE i.
