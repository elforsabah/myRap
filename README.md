CLASS zcl_wr_misc_pd DEFINITION
  PUBLIC
  CREATE PRIVATE .

  PUBLIC SECTION.

    TYPES:
      tty_tour TYPE STANDARD TABLE OF /plce/tpdtour WITH EMPTY KEY .
    TYPES:
      tty_srvprtconf TYPE STANDARD TABLE OF zpd_s_srvprtconf WITH EMPTY KEY .
    TYPES:
      tty_tmpl TYPE RANGE OF /plce/pdtour_template .
    TYPES:
        tty_prtconf TYPE STANDARD TABLE OF zc_printconfig WITH EMPTY KEY.
    TYPES:
      BEGIN OF sty_attach,
        serviceuuid TYPE /plce/pdservice_uuid,
        touruuid    TYPE /plce/pdtour_uuid,
        attachment  TYPE /plce/pdattachment,
        mimetype    TYPE /plce/pdmimetype,
        filename    TYPE /plce/filename,
        comments    TYPE /plce/text40,
      END OF sty_attach .
    TYPES:
      tty_attach  TYPE STANDARD TABLE OF sty_attach WITH EMPTY KEY .
    TYPES:
      tty_trprtconf TYPE STANDARD TABLE OF zpd_s_trtmpprtconf WITH EMPTY KEY .
    TYPES:
      BEGIN OF ty_tour_with_attachments,
        tour_uuid     TYPE /plce/pdtour_uuid,
        tour_id       TYPE /plce/pdtour_id,
        tour_template TYPE /plce/pdtour_template,
        attachments   TYPE STANDARD TABLE OF /plcp/cwaprtconf WITH EMPTY KEY,
      END OF ty_tour_with_attachments .

    METHODS get_srv_prtconf
      RETURNING
        VALUE(rt_prtconf) TYPE tty_srvprtconf .
    METHODS get_tr_prtconf
      RETURNING
        VALUE(rt_prtconf) TYPE tty_trprtconf .
    CLASS-METHODS get_instance
      RETURNING
        VALUE(ro_instance) TYPE REF TO zcl_wr_misc_pd .
    METHODS create_tour_attm
      IMPORTING
        !it_tours             TYPE zpd_tour_tt
      RETURNING
        VALUE(rt_attachments) TYPE tty_attach .
    METHODS create_srv_attm
      IMPORTING
        !it_services          TYPE zpd_service_tt
      RETURNING
        VALUE(rt_attachments) TYPE tty_attach .
    METHODS set_srv_prtconf
      IMPORTING
        !iv_servicetype TYPE /plce/pdservice_type
        !iv_material    TYPE /plce/pdtransport_product .
    METHODS set_tr_prtconf
      IMPORTING
        !it_templates TYPE tty_tmpl .
    METHODS build_for_tour
      IMPORTING
        !is_tour       TYPE zr_pdtour
        !it_sel_config TYPE tty_prtconf
      RETURNING
        VALUE(rs_tour) TYPE ty_tour_with_attachments .
    METHODS create_manual_tour_attm
      IMPORTING
        !is_tour_with_conf    TYPE ty_tour_with_attachments
      RETURNING
        VALUE(rt_attachments) TYPE tty_attach .
    METHODS create_manual_service_attm
      IMPORTING
        !it_selected_services TYPE zpd_service_tt
      RETURNING
        VALUE(rt_attachments) TYPE tty_attach .
  PROTECTED SECTION.
  PRIVATE SECTION.

    DATA mt_srvprtconf TYPE tty_srvprtconf .
    DATA mt_trprtconf TYPE tty_trprtconf .
    CLASS-DATA mo_instance TYPE REF TO zcl_wr_misc_pd .

    METHODS create_attachment
      IMPORTING
        !ir_data              TYPE REF TO data
        !ir_config            TYPE REF TO /plcp/cwaprtconf
      RETURNING
        VALUE(rt_attachments) TYPE tty_attach .
    METHODS filter_configs_for_tour
      IMPORTING
        !iv_tour_template TYPE /plce/pdtour_template
        !it_sel_config    TYPE tty_prtconf
      RETURNING
        VALUE(rt_config)  TYPE tty_prtconf .
    METHODS constructor .
ENDCLASS.



CLASS zcl_wr_misc_pd IMPLEMENTATION.


  METHOD create_attachment.

    DATA:
      lt_values           TYPE ewappropvalues,
      lt_printresults     TYPE /plcp/pprint_result,
      ls_attachment       TYPE sty_attach,
      lv_filename         TYPE string,
      lv_fieldname        TYPE string,
      lv_fieldvalue_alpha TYPE string,
      lv_indexstr         TYPE string,
      lv_index            TYPE i,
      lo_structdesc       TYPE REF TO cl_abap_structdescr.

    CLEAR: lt_printresults.

    ASSIGN COMPONENT ir_config->field OF STRUCTURE ir_data->* TO FIELD-SYMBOL(<lfs_data>) ELSE UNASSIGN.
    IF <lfs_data> IS ASSIGNED AND <lfs_data> IS NOT INITIAL.

      lt_printresults = NEW /plcp/cl_print_base( )->print( ir_config = ir_config iv_data = <lfs_data> ). "#EC CI_SEL_NESTED

      LOOP AT lt_printresults REFERENCE INTO DATA(lr_printresult).

        CLEAR: ls_attachment,
               lv_filename,
               lv_fieldname,
               lv_fieldvalue_alpha,
               lv_indexstr.

        "raw data
        ls_attachment-attachment = lr_printresult->pdfdata.
        "mime type
        ls_attachment-mimetype = 'APPLICATION/PDF'.

        "file name
        lv_index = sy-tabix.
        lv_fieldname = ir_config->field.
        REPLACE ALL OCCURRENCES OF PCRE '[^[:alnum:]*$]' IN lv_fieldname WITH '_'.
        CONDENSE lv_fieldname.
        SHIFT lv_fieldname LEFT DELETING LEADING '_'.

        IF ir_config->filenameteemplate IS NOT INITIAL.

          DATA(lo_typedesc) = cl_abap_typedescr=>describe_by_data( EXPORTING p_data = <lfs_data> ).
          IF lo_typedesc->type_kind = cl_abap_typedescr=>typekind_struct1.
            lo_structdesc ?= lo_typedesc.
            DATA(lt_comps) = lo_structdesc->get_components( ).
            LOOP AT lt_comps REFERENCE INTO DATA(lr_comp) WHERE as_include IS INITIAL.
              ASSIGN COMPONENT lr_comp->name OF STRUCTURE <lfs_data> TO FIELD-SYMBOL(<lfs_field>) ELSE UNASSIGN.
              IF <lfs_field> IS ASSIGNED.
                IF lv_fieldvalue_alpha IS INITIAL.
                  lv_fieldvalue_alpha = condense( |{ <lfs_field> ALPHA = OUT }| ).
                ELSE.
                  lv_fieldvalue_alpha = lv_fieldvalue_alpha && '_' && condense( |{ <lfs_field> ALPHA = OUT }| ).
                ENDIF.
              ENDIF.
            ENDLOOP.
          ELSE.
            lv_fieldvalue_alpha = condense( |{ <lfs_data> ALPHA = OUT }| ).
          ENDIF.

          lv_indexstr = condense( CONV string( lv_index ) ).

          lt_values = VALUE #(
          ( name = 'FIELDNAME' value = lv_fieldname )
          ( name = 'FIELDVALUE' value = <lfs_data> )
          ( name = 'FIELDVALUE_ALPHA' value = lv_fieldvalue_alpha )
          ( name = 'INDEX' value = lv_indexstr )
          ).

          lv_filename = cl_eewa_strings=>substitute(
            EXPORTING
              par_text      = ir_config->filenameteemplate
              par_values    = lt_values
           ).

          CONCATENATE lv_filename '.pdf' INTO ls_attachment-filename.

        ELSE.

          lv_filename = lv_fieldname.
          CONCATENATE lv_filename '_' <lfs_data> '.pdf' INTO ls_attachment-filename.

        ENDIF.

        CONDENSE ls_attachment-filename.
        SHIFT ls_attachment-filename LEFT DELETING LEADING '_'.

        APPEND ls_attachment TO rt_attachments.

      ENDLOOP.

    ENDIF.

  ENDMETHOD.


  METHOD constructor.
  ENDMETHOD.


  METHOD get_instance.

    IF mo_instance IS INITIAL.
      mo_instance = NEW zcl_wr_misc_pd( ).
    ENDIF.

    ro_instance = mo_instance.

  ENDMETHOD.


  METHOD create_tour_attm.

    DATA:
      ls_order_head TYPE ewa_order_head,
      lt_templates  TYPE RANGE OF /plce/pdtour_template.

    lt_templates = VALUE #( FOR tour IN it_tours ( sign = 'I' option = 'EQ' low = tour-tour_template ) ).
    SORT lt_templates BY low.
    DELETE ADJACENT DUPLICATES FROM lt_templates COMPARING low.
    set_tr_prtconf( lt_templates ).

    IF mt_trprtconf IS INITIAL. RETURN. ENDIF.

    SELECT head~*
        FROM ewa_order_head  AS head
        INNER JOIN @it_tours AS tours ##ITAB_KEY_IN_SELECT
        ON head~/plcp/pd_tour_id = tours~tour_id
        INTO TABLE @DATA(lt_order_head).

    IF lt_order_head IS INITIAL. RETURN. ENDIF.

    LOOP AT it_tours ASSIGNING FIELD-SYMBOL(<ls_tour>).

      ls_order_head = VALUE #( lt_order_head[ /plcp/pd_tour_id = <ls_tour>-tour_id ] OPTIONAL ).
      IF ls_order_head IS INITIAL. CONTINUE. ENDIF.

      LOOP AT mt_trprtconf REFERENCE INTO DATA(lr_config) WHERE tour_template = <ls_tour>-tour_template.

        DATA(lt_attachments) = create_attachment( ir_data   = REF #( ls_order_head  )
                                                  ir_config = REF #( lr_config->conf ) ).

        rt_attachments = VALUE #( BASE rt_attachments
                                  FOR attachment IN lt_attachments
                                  ( touruuid   = <ls_tour>-tour_uuid
                                    attachment = attachment-attachment
                                    mimetype   = attachment-mimetype
                                    filename   = attachment-filename
                                    comments   = attachment-comments ) ).

      ENDLOOP.

    ENDLOOP.

  ENDMETHOD.


  METHOD create_srv_attm.

    DATA: ls_order_item TYPE ewa_order_object.

    SELECT item~*
      FROM ewa_order_object   AS item
      INNER JOIN @it_services AS services ##ITAB_KEY_IN_SELECT
      ON item~pobjnr = services~reference_int_id
      INTO TABLE @DATA(lt_order_item).

    LOOP AT it_services ASSIGNING FIELD-SYMBOL(<ls_service>).

      ls_order_item = VALUE #( lt_order_item[ pobjnr = <ls_service>-reference_int_id ] OPTIONAL ).
      IF ls_order_item IS INITIAL. CONTINUE. ENDIF.

      set_srv_prtconf( iv_material = <ls_service>-material iv_servicetype = <ls_service>-service_type ).
      IF mt_srvprtconf IS INITIAL. CONTINUE. ENDIF.

      LOOP AT mt_srvprtconf REFERENCE INTO DATA(lr_config) WHERE ( service_type = <ls_service>-service_type AND material = <ls_service>-material ).

        DATA(lt_attachments) = create_attachment( ir_data   = REF #( ls_order_item  )
                                                  ir_config = REF #( lr_config->conf ) ).

        rt_attachments = VALUE #( BASE rt_attachments
                                  FOR attachment IN lt_attachments
                                  ( serviceuuid = <ls_service>-service_uuid
                                    attachment  = attachment-attachment
                                    mimetype    = attachment-mimetype
                                    filename    = attachment-filename
                                    comments    = attachment-comments ) ).

      ENDLOOP.

    ENDLOOP.

  ENDMETHOD.


  METHOD get_srv_prtconf.

    rt_prtconf = mt_srvprtconf.

  ENDMETHOD.


  METHOD get_tr_prtconf.

    rt_prtconf = mt_trprtconf.

  ENDMETHOD.


  METHOD set_srv_prtconf.

    CLEAR mt_srvprtconf.

    SELECT * FROM zwr_pdforms_srv WHERE service_type = @iv_servicetype AND material = @iv_material INTO TABLE @DATA(lt_forms).
    IF sy-subrc <> 0.
      SELECT * FROM zwr_pdforms_srv WHERE service_type = @iv_servicetype AND material IS INITIAL INTO TABLE @lt_forms.
      IF sy-subrc <> 0.
        SELECT * FROM zwr_pdforms_srv WHERE service_type IS INITIAL AND material = @iv_material INTO TABLE @lt_forms.
      ENDIF.
    ENDIF.

    IF lt_forms IS INITIAL. RETURN. ENDIF.

    SELECT *
      FROM zwr_cwaprtconf AS conf
      INNER JOIN @lt_forms AS form
      ON conf~printform = form~printform
      AND conf~formtype = form~formtype
      INTO CORRESPONDING FIELDS OF TABLE @mt_srvprtconf. "#EC CI_NOWHERE

    IF sy-subrc <> 0. RETURN. ENDIF.

  ENDMETHOD.


  METHOD set_tr_prtconf.

    CLEAR mt_trprtconf.

    SELECT tmpl~tour_template,
           conf~* AS conf
      FROM /plce/cpdttmpl AS tmpl
      INNER JOIN zwr_pdforms AS pdf
      ON tmpl~external_tour_template_id = pdf~route
      INNER JOIN zwr_cwaprtconf AS conf
      ON pdf~formtype = conf~formtype
      AND pdf~printform = conf~printform
      INTO CORRESPONDING FIELDS OF TABLE @mt_trprtconf. "#EC CI_NOWHERE

    IF sy-subrc <> 0. RETURN. ENDIF.

  ENDMETHOD.

  METHOD build_for_tour.

    DATA(lt_attachments) = filter_configs_for_tour( iv_tour_template = is_tour-TourTemplate
                                                    it_sel_config    = it_sel_config ).

    IF lt_attachments IS INITIAL. RETURN. ENDIF.

    rs_tour = VALUE ty_tour_with_attachments(
      tour_uuid     = is_tour-TourUuid
      tour_id       = is_tour-TourId
      tour_template = is_tour-TourTemplate
      attachments   = CORRESPONDING #( lt_attachments )
    ).

  ENDMETHOD.

  METHOD filter_configs_for_tour.

    DATA:
     lt_valid_forms_h TYPE HASHED TABLE OF zwr_pdforms WITH UNIQUE KEY formtype printform.

    " Filter user selection using tour template print configuration
    SELECT z~formtype,
           z~printform
      FROM /plce/cpdttmpl AS tmpl
      INNER JOIN zwr_pdforms AS z
        ON tmpl~external_tour_template_id = z~route
      FOR ALL ENTRIES IN @it_sel_config
      WHERE tmpl~tour_template = @iv_tour_template
        AND z~formtype  = @it_sel_config-Formtype
        AND z~printform = @it_sel_config-Printform
      INTO TABLE @DATA(lt_valid_forms).

    IF lt_valid_forms IS INITIAL. RETURN. ENDIF.
    lt_valid_forms_h = CORRESPONDING #( lt_valid_forms ).

    LOOP AT it_sel_config ASSIGNING FIELD-SYMBOL(<ls_conf>).
      IF line_exists(
           lt_valid_forms_h[
             formtype  = <ls_conf>-Formtype
             printform = <ls_conf>-Printform
           ]
         ).
        APPEND <ls_conf> TO rt_config.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

  METHOD create_manual_tour_attm.

    SELECT SINGLE head~*
       FROM ewa_order_head AS head
       WHERE head~/plcp/pd_tour_id = @is_tour_with_conf-tour_id
       INTO @DATA(ls_head).

    IF ls_head IS INITIAL. RETURN. ENDIF.

    LOOP AT is_tour_with_conf-attachments REFERENCE INTO DATA(lr_config).

      DATA(lt_attachments) = create_attachment(
                            ir_data   = REF #( ls_head )
                            ir_config = lr_config ).

      rt_attachments = VALUE #( BASE rt_attachments
                                FOR ls_attachment IN lt_attachments
                                ( touruuid   = is_tour_with_conf-tour_uuid
                                  attachment = ls_attachment-attachment
                                  mimetype   = ls_attachment-mimetype
                                  filename   = ls_attachment-filename
                                  comments   = ls_attachment-comments ) ).

    ENDLOOP.

  ENDMETHOD.

  METHOD create_manual_service_attm.

    IF it_selected_services IS INITIAL. RETURN. ENDIF.

    SELECT ord_items~*,
           services~*
         FROM ewa_order_object   AS ord_items
         INNER JOIN /plce/tpdsrv AS services
         ON ord_items~pobjnr = services~reference_int_id
         FOR ALL ENTRIES IN @it_selected_services  ##ITAB_KEY_IN_SELECT
         WHERE services~service_uuid = @it_selected_services-service_uuid
         INTO TABLE @DATA(lt_order_item).

    LOOP AT it_selected_services ASSIGNING FIELD-SYMBOL(<ls_selected_srv>).

      DATA(ls_order_item) = VALUE #( lt_order_item[ services-service_uuid = <ls_selected_srv>-service_uuid ] OPTIONAL ).
      IF ls_order_item IS INITIAL. CONTINUE. ENDIF.

      set_srv_prtconf( iv_servicetype = <ls_selected_srv>-service_type
                       iv_material = <ls_selected_srv>-material ).

      IF mt_srvprtconf IS INITIAL. CONTINUE. ENDIF.

      LOOP AT mt_srvprtconf REFERENCE INTO DATA(lr_config) WHERE ( service_type = <ls_selected_srv>-service_type AND
                                                                   material = <ls_selected_srv>-material ).

        DATA(lt_attachments) = create_attachment( ir_data   = REF #( ls_order_item-ord_items  )
                                                  ir_config = REF #( lr_config->conf ) ).

        rt_attachments = VALUE #( BASE rt_attachments
                                  FOR attachment IN lt_attachments
                                  ( serviceuuid = <ls_selected_srv>-service_uuid
                                    attachment  = attachment-attachment
                                    mimetype    = attachment-mimetype
                                    filename    = attachment-filename
                                    comments    = attachment-comments ) ).

      ENDLOOP.

    ENDLOOP.

  ENDMETHOD.

ENDCLASS.
