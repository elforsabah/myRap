" Success Message for the UI
        APPEND NEW /plce/cx_pd_exception(
                     severity  = if_abap_behv_message=>severity-success
                     textid    = /plce/cx_pd_exception=>service_assigned " Consider adding a specific Storno success textid
                     serviceid = <ls_service>-ServiceId ) TO reported-%other.
