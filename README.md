What still needs checking
Do waste approvals actually exist in this client for these EWC codes? This is the one thing that decides scope. If yes, both columns default themselves and the derivation above is not just convenient but consistent with the validation. If no, WDPLANT genuinely cannot be derived — and note your German acceptance criterion only ever promised RE, so defaulting the procedure alone would still meet it. Get this answered before estimating.
Domain /WATP/DWASTEMODE fixed values. If RE really is the only entry, the constant is safe. If there are others, move it to a Z table rather than a literal.
The DEFAULT_AVV field on /WATP/SCSBO_CABAVVD — "Attribute assignment default yes/no". Run a where-used on its data element. There may already be a defaulting mechanism here that nobody switched on.
"Not entered manually" — pre-filling covers the default half. Making the two columns display-only is a change in the waste-list maintenance UI, which is still the one object we haven't identified. F1 → Technical Information on the "Waste Disp. Facility" field in that popup gives you the program in one step. It's a cosmetic follow-up though: AVV_CHECK_SPEC already blocks an active row with either field empty, so leaving them editable carries no data-integrity risk.


<img width="942" height="956" alt="image" src="https://github.com/user-attachments/assets/17634136-0560-482f-816c-b4d444bdf9bd" />
<img width="1342" height="549" alt="image" src="https://github.com/user-attachments/assets/83ea5009-9076-4a9f-82c6-d60b904c7ab0" />
<img width="1658" height="458" alt="image" src="https://github.com/user-attachments/assets/3434845b-3cf8-46bc-9eda-c98f8fc991e3" />
<img width="1619" height="1012" alt="image" src="https://github.com/user-attachments/assets/259c0660-67a1-442f-9855-6809c46b422e" />
*-----------------------------------------------------------------------
*        Modulpool zur Kundenauftragsbearbeitung
*-----------------------------------------------------------------------

         include mv45atop.
         include mv45amac.

enhancement-point sapmv45a_03 spots es_sapmv45a static.
*-----------------------------------------------------------------------
*        U S E R - E X I T S
*        U S E R - E X I T S
*        U S E R - E X I T S
*-----------------------------------------------------------------------
*        INCLUDE MV45ATZZ.            " Data definitions in MV45ATOP
         include mv45aozz.             " User-modules PBO
         include mv45aizz.             " User-modules PAI
         include mv45afza.             " User-forms < 3.0
         include mv45afzb.             " User-forms
         include mv45afzc.             " User-forms < 3.0D
         include mv45afzd.             " User-forms   3.0E
         include mv45afzf.             " User-forms   3.0F
*        include mv45afzg.             " User-forms   3.1G
         include mv45afzh.             " User-forms   4.6B
         include mv45afzz.             " User-forms
         include mv45afz4.             " User-forms   4.0
enhancement-point sapmv45a_01 spots es_sapmv45a static.

*-----------------------------------------------------------------------
*        Tabellen-spezifische Module PBO
*-----------------------------------------------------------------------
         include mv45aoak.             "Kopf
         include mv45aoap.             "Auftragspositionen
         include mv45aoat.             "Texte
         include mv45aoep.             "Einteilungen
         include mv45aosn.             "Änderungsstände
         include mv45aolb.             "Abrufe
         include mv45aoko.             "KOnditionen
         include mv45aoup.             "Positionsstatus

*-----------------------------------------------------------------------
*        Textverarbeitung     Module PBO
*-----------------------------------------------------------------------
         include mv45aotx.
*-----------------------------------------------------------------------
*        PBO - Module alphabetisch geordnet
*-----------------------------------------------------------------------
         include mv45ao01.
         include mv45ao0a.
         include mv45ao0b.
         include mv45ao0c.
         include mv45ao0d.
         include mv45ao0f.
         include mv45ao0g.
         include mv45ao0i.
         include mv45ao0k.
         include mv45ao0m.
         include mv45ao0p.
         include mv45ao0r.
         include mv45ao0s.
         include mv45ao0t.
         include mv45ao0v.
         include mv45ao0w.
         include mv45ao0x.
         include mv45ao0z.
         "{ BeginENHO J3GS_SAPMV45A IS-EC-CEM /SAPCEM/ECO_ETM }
*        ETM-spezifische PBO-Module
         include mj3gv45o.
         "{ End ENHO J3GS_SAPMV45A IS-EC-CEM /SAPCEM/ECO_ETM }
enhancement-point sapmv45a_07 spots es_sapmv45a static.
*-----------------------------------------------------------------------
*        FORM-Routinen alphabetisch geordnet (extern aufgerufen)
*-----------------------------------------------------------------------
         include mv45af0b.
         include mv45af0k.
         include mv45af0t.
         include mv45af1t.
*-----------------------------------------------------------------------
*        FORM-Routinen alphabetisch geordnet
*-----------------------------------------------------------------------
         include mv45af0a.
         include mv45af0c.
         include mv45af0d.
         include mv45af0e.
         include mv45af0f.
         include mv45af0g.
         include mv45af0h.
         include mv45a_sgt_process.

enhancement-point sapmv45a_04 spots es_sapmv45a static.
*$*$-Start: SAPMV45A_04-------------------------------------------------------------------------$*$*
ENHANCEMENT 1  FSH_DET_PROCESS.    "active version
         INCLUDE fsh_detail_process.
         INCLUDE fsh_sd_arun_process.
         INCLUDE fsh_sd_arun_process_02.
         INCLUDE fsh_sd_arun_process_03.
         INCLUDE fsh_sd_arun_process_04.
         INCLUDE fsh_spec_contract.
         INCLUDE fsh_vas_btn_handle_sd.
         INCLUDE fsh_sapmv45a.
         INCLUDE fsh_bapi_cust_master.
         INCLUDE fsh_problem_log_display.
         INCLUDE fsh_season_help.
         INCLUDE fsh_sd_fcode_vred.
         INCLUDE fsh_sd_fcode_vchg.
         INCLUDE fsh_sd_fcode_vdlg.
         INCLUDE fsh_oss_calculation.
         INCLUDE fsh_atprej_so.
         INCLUDE fsh_grids.
         INCLUDE fsh_sd_fcode_vasg.
*--------------------------------------------------------------------*
*        Includes for Partial Quantity Rejection
*--------------------------------------------------------------------*
         INCLUDE fsh_pqr_i01. " PAI Modules
*--------------------------------------------------------------------*
*        Includes for Article Personalization
*--------------------------------------------------------------------*
         INCLUDE fsh_sd_fert_gen.
ENDENHANCEMENT.
*$*$-End:   SAPMV45A_04-------------------------------------------------------------------------$*$*

         include mv45af0i.
         include mv45af0j.
         include mv45af0l.
         include mv45af0m.
         include mv45af0n.
         include mv45af0o.
         include mv45af0p.
         include mv45af0q.
         include mv45af0r.
         include mv45af0s.
         include mv45af0u.
         include mv45af0v.
         include mv45af0w.
         include mv45af0x.
         include mv45af0y.
         include mv45af0z.

*-----------------------------------------------------------------------
*        FORM-Routinen für interne Tabellen
*-----------------------------------------------------------------------

         include mv45affe.             " Feldauswahl
         include mv45afpa.
         include mv45afuv.             " Fehlerprotokoll

*-----------------------------------------------------------------------
*        FORM routines (external)
*-----------------------------------------------------------------------

         include incoterm_location_assignment.
         include incoterm_value_request.

*-----------------------------------------------------------------------
*        Zeitpunkte für Listprocessing
*-----------------------------------------------------------------------

         include mv45afli.

*-----------------------------------------------------------------------
*        Routinen der Nachrichtensteuerung
*-----------------------------------------------------------------------

         include fv61bf00.

*-----------------------------------------------------------------------
*        Routinen der Statistik-Fortschreibung
*-----------------------------------------------------------------------

         include mv45afst.

*-----------------------------------------------------------------------
*        FORM-Routinen für Datenbankzugriffe
*-----------------------------------------------------------------------
         include mv45afdb.
*-----------------------------------------------------------------------
*        Tabellen-spezifische Module PAI
*-----------------------------------------------------------------------
         include mv45aiak.             "Auftragskopf
         include mv45aiap.             "Auftragspositionen
         include mv45aiep.             "Einteilungen
         include mv45aisn.             "Änderungsstände
         include mv45ailb.             "Abrufe
         include mv45aiko.             "Konditionen
*-----------------------------------------------------------------------
*        Textverarbeitung     Module PAI
*-----------------------------------------------------------------------
         include mv45aitx.
*-----------------------------------------------------------------------
*        PAI - Module alphabetisch geordnet
*-----------------------------------------------------------------------
         include mv45ai0a.
         include mv45ai0b.
         include mv45ai0c.
         include mv45ai0d.
         include mv45ai0e.
         include mv45ai0f.
         include mv45ai0g.
         include mv45ai0h.
         include mv45ai0i.
         include mv45ai0k.
         include mv45ai0l.
         include mv45ai0m.
         include mv45ai0o.
         include mv45ai0p.
         include mv45ai0r.
         include mv45ai0s.
         include mv45ai0t.
         include mv45ai0u.
         include mv45ai0v.
         include mv45ai1v.
         include mv45ai0w.
         include mv45ai0x.
         include mv45ai0z.
         include mv45ai_is_enhancement.  "PAI modules industry solutions
         "{ BeginENHO J3GS_SAPMV45A IS-EC-CEM /SAPCEM/ECO_ETM }
*        ETM-spezifische PAI-Module
         include mj3gv45i.
         "{ End ENHO J3GS_SAPMV45A IS-EC-CEM /SAPCEM/ECO_ETM }.
enhancement-point sapmv45a_08 spots es_sapmv45a static.
*-----------------------------------------------------------------------
* Process On Value-Request
*-----------------------------------------------------------------------

         include mv45av0v.
         "{ BeginENHO J3GS_SAPMV45A IS-EC-CEM /SAPCEM/ECO_ETM }
*        ETM-spezifische FORM-Routinen
         include mj3gv45f.
         "{ End ENHO J3GS_SAPMV45A IS-EC-CEM /SAPCEM/ECO_ETM }.
enhancement-point sapmv45a_09 spots es_sapmv45a static.
*-----------------------------------------------------------------------
*        FORM-Routinen für Anschluss an CO-ABC (extern aufgerufen)
*-----------------------------------------------------------------------
         include mv45af_abc_funktionen.

         include mv45ao0p_pric_subscr_set_data.

         include mv45ai0p_pric_subscr_get_data.

         include mv45ai0p_pricing_pass_fcode.

         include mv45ai0p_pricing_fetch_fcode.

         include mv45a_dpbp_subscr_seto01.

         include mv45a_dpbp_subscr_setf01.

         include fith_j_1tpbupl. "2091168/2075763
*IS2ERP IS-OIL START
* Global parameter needed for SAPMV45A 8030
*  DATA venture_data  LIKE  roiu_jva_ep_venture.
*IS2ERP IS-OIL END

*** IS-MP, /SAPMP/GENERAL_01 (Mill Products EhP2) Enhancement Implementation : /SAPMP/GENERAL_SAPMV45A_1 ***
* Mill Configuration & Batch selection
         include /sapmp/conf_batch_sel.
*** IS-MP, /SAPMP/GENERAL_01 (Mill Products EhP2) Enhancement Implementation : /SAPMP/GENERAL_SAPMV45A_1 ***

*** IS-MP, /SAPMP/GENERAL_01 (Mill Products EhP2) ***
* Mill Returns Processing
         include /sapmp/returns_processing.
*** IS-MP, /SAPMP/GENERAL_01 (Mill Products EhP2) ***
* begin APPLICATION COMPONENT: IS-MP-MM SWITCH: /SAPMP/OB_GENERAL SWITCH DESCRIPTION:Original Batch Enh. Mill
*MILL 0005 XFO UC-reine Chargenfindung
         include mill_uc_field_control_pai.
         include mv45af_uc_forms.
*
* end APPLICATION COMPONENT: IS-MP-MM SWITCH: /SAPMP/OB_GENERAL SWITCH DESCRIPTION:Original Batch Enh. Mill

*Begin IS-MP-SD, /SAPMP/FAST_DATA_ENTRY_SD, Fast Data Entry of Char. SD.
         include mill_se_mv45a.
         include mill_se_mv45a_f02.
         include /sapmp/fcode_ce_prop.
         include /sapmp/fcode_ce_gauf.
         include /sapmp/fcode_ce_prot.
         include /sapmp/check_muebs_in_rgf01.
         include /sapmp/mill_se_muebs_listo01.
         include /sapmp/fcode_ce_enh_fde.
*
*End IS-MP-SD, /SAPMP/FAST_DATA_ENTRY_SD, Fast Data Entry of Char. SD.

*Begin IS-MP-NF, /NFM/SD, NFM Processing SD.
         include /nfm/mv45a_f.                 "NE-Formroutinen            "/NFM/
         include /nfm/mv45a_o01.                                           "/NFM/
         include /nfm/mv45a_i01.                                           "/NFM/
*
*End IS-MP-NF, /NFM/SD, NFM Processing SD.
*Begin IS-AD-SPC,  AD_S2K_SPSC , SPEC2000&Spare Parts Stk Calc.
* A&D SPEC2000: Fill texts for AECMA and ATA (MOI and PRI) fields


* ATA
         include mv45aocspcmoit_lesen.
         include mv45af0c_cspcmoit_lesen.
         include mv45aocspcprit_lesen.
         include mv45af0c_cspcprit_lesen.
*End IS-AD-SPC,  AD_S2K_SPSC , SPEC2000&Spare Parts Stk Calc.
enhancement-point sapmv45a_10 spots es_sapmv45a static.
*$*$-Start: SAPMV45A_10-------------------------------------------------------------------------$*$*
ENHANCEMENT 47  OI0_COMMON_SAPMV45A.    "active version
*  IS-Oil Downstream specific includes
         INCLUDE MV45AVOI.  "from MV45AV0V    "SO4K000205 KH  value hlp.
         INCLUDE MV45AIOI.                    "SO3K002178 HJF PAI
         INCLUDE MV45AOOI.                    "SO3K002178 HJF PBO
         INCLUDE MV45AFOI.                    "SO3K002178 HJF forms
         INCLUDE FOIDHNNN.                    "SO4K001323 AWH?
         INCLUDE FOIDINNN.                    "SO4K001323 AWH?
         INCLUDE /ICO/MMO_SAPMV45A IF FOUND.  "SOHK003995 ITW OGSD
ENDENHANCEMENT.
ENHANCEMENT 9  OIJ_SAPMV45A.    "active version
INCLUDE MV45AFOI_TSW.
ENDENHANCEMENT.
*$*$-End:   SAPMV45A_10-------------------------------------------------------------------------$*$*
enhancement-point sapmv45a_05 spots es_sapmv45a static.

         include mv45af0f_fcode_dmc.

         include mv45af0f_fcode_dsd.

         "{ Begin ENHO AD_MPN_SLS_SAPMV45A IS-AD-MPN-SLS AD_MPN_IC }
*A&D MPN
         include adpic_fill_mpn.
         include adpic_fill_mpn_pbo.
         "{ End ENHO AD_MPN_SLS_SAPMV45A IS-AD-MPN-SLS AD_MPN_IC }

enhancement-point sapmv45a_06 spots es_sapmv45a static.
*$*$-Start: SAPMV45A_06-------------------------------------------------------------------------$*$*
ENHANCEMENT 1  ITMF_SAPMV45A_01.    "active version
  INCLUDE ITMF_SAPMV45A_SD_TC.
ENDENHANCEMENT.
*$*$-End:   SAPMV45A_06-------------------------------------------------------------------------$*$*

         include mv45aolo.
         include mv45ailo.

*INCLUDE MV45A_OIL_ESOA_FOR_LORD.

*-----------------------------------------------------------------------
*        Advanced Returns related includes
*-----------------------------------------------------------------------
         include mv45a_msrf01.
         include mv45a_msrf02.
         include mv45a_msrf03.
         include mv45a_msrf04.
         include mv45a_msrf05.
         include mv45a_msri01.
         include mv45a_msro01.
         include mv45a_msrv01.

*-----------------------------------------------------------------------
*        Predefined Price Elements
*-----------------------------------------------------------------------
         include mv45ao_ppe.
         include mv45ai_ppe.

*-----------------------------------------------------------------------
*        Includes for customer full text search
*-----------------------------------------------------------------------
         include mv45aics_handler_class.
         include mv45a_search_customeri01.
         include mv45a_search_customerf01.
         include mv45a_display_alvo01.
         include mv45a_display_alvf01.
         include mv45a_customer_search_exiti01.
         include mv45a_customer_search_ausblo02.
         include mv45a_customer_search_ausblf01.
         include mv45a_cs_set_new_kuagv_kunnf01.

         include mv45a_vbkd_vtref_pruefeni01.

         include mv45a_vbkd_vtref_valuesi01.

         include mv45a_activate_screen_badi_f01.

         include mv45a_customer_head_set_dato01.

         include mv45a_customer_head_pass_fci01.

         include mv45a_customer_head_get_dati01.

*INCLUDE MV45A_F01.

         include mv45a_internal_head_set_dato01.

         include mv45a_customer_head_set_datf01.

         include mv45a_customer_item_set_dato01.

         include mv45a_customer_item_set_datf01.

         include mv45a_customer_item_pass_fci01.

         include mv45a_customer_item_pass_fcf01.

         include mv45a_customer_item_get_dati01.

         include mv45a_customer_item_get_datf01.

         include mv45a_internal_head_pass_fci01.

         include mv45a_internal_head_set_datf01.

         include mv45a_internal_head_pass_fcf01.

         include mv45a_internal_head_get_dati01.

         include mv45a_internal_head_get_datf01.

         include mv45a_internal_item_set_dato01.

         include mv45a_internal_item_set_datf01.

         include mv45a_internal_item_pass_fci01.

         include mv45a_internal_item_pass_fcf01.

         include mv45a_internal_item_get_dati01.

         include mv45a_internal_item_get_datf01.

*-----------------------------------------------------------------------
*        Includes for key user extensibility (Custiom Fields Tab)
*-----------------------------------------------------------------------
         include mv45a_custom_fields_head_seo01.
         include mv45a_custom_fields_head_sef01.
         include mv45a_custom_fields_head_gei01.
         include mv45a_custom_fields_head_gef01.
         include mv45a_custom_fields_item_seo01.
         include mv45a_custom_fields_item_sef01.
         include mv45a_custom_fields_item_gei01.
         include mv45a_custom_fields_item_gef01.
         include mv45a_custom_fields_deactivf01.

         include mv45a_msrf06.

         include mv45a_msrf07.

         include mv45a_msrf08.

         include mv45a_msrf09.

         include mv45a_msrf10.

         include mv45a_msrf11.

         include mv45a_msrf12.

         include mv45a_msrf13.

         include mv45a_msrf14.

         include mv45a_msrf15.

* Includes for TM-ERP status/docflow UI
         include mv45a_tm_handler_class.
         include mv45a_tmf01.
         include mv45a_tm_pbo_4798.

         include mv45a_sp_taggingo01.
         include mv45a_sp_taggingi01.

         include mv45a_sp_taggingf01.
         include mv45a_sp_tagging_reqf01.


         include mv45a_vbkd_sepa_valuesi01.

         include mv45a_vbkd_mndid_valuesi01.

         include mv45ai0v_vbkd_mndid_pruefen.

         include mv45ai0v_vbkd_get_cursor.

         include mv45ao0s_set_pricing_screen.

         include m45a_vbkd_zlsch_valuesi01.

         include mv45af0f_zlsch_funk_ausfuehren.

         include mv45a_farr_migrationo01.

         include mv45a_get_hndovrloc_txt.

         include mv45af01_fill_hndovrloc_txt.

         include mv45a_get_item_hndovrloc_txt.

         include mv45a_handover_date_time_pri01.

         include mv45a_handover_location_check.

         include mv45a_vbap_handoverloc_check.

         include mv45a_type_ahead_102o01.

         include mv45a_type_ahead_4701o01.

         include mv45a_type_ahead_4480o01.

         include mv45a_type_ahead_4900o01.

         include mv45a_gos_displayi01.

*INCLUDE mv45affe_feld_attribute.

         include mv45at99.

*---------------------------------------------------------------------*
*        load of progam                                               *
*---------------------------------------------------------------------*
         load-of-program.
*          Fetch singletons
           go_sls_caller               = cl_sd_sls_va_factory=>get_caller( ).
           go_sls_output_management    = cl_sd_sls_om_factory=>get_output_management( ).
           go_sls_billing_plan         = cl_sd_sls_billing_plan=>so_instance.
           go_sls_event                = cl_sd_sls_event=>so_instance.
           go_tctrl_util               = cl_sd_sls_va_factory=>get_tctrl_util( ).
           go_bopf_trans_mgr_util      = cl_sd_sls_bopf_trans_mgr_util=>so_instance.
           go_sls_trans_mgr            = cl_sd_sls_va_factory=>get_trans_mgr_int( ).
           go_message                  = cl_sd_sls_va_factory=>get_message( ).
           go_sls_mapper               = cl_sd_sls_ext_factory=>get_gfn_mapping_util( ).
           gs_dbsel-cust               = cl_sd_dbsel_cust=>so_instance.
           go_sd_bp_set_deliv_block    = cl_sd_bp_set_deliv_block=>get_instance( ).
           go_sdoc_flex_batch_bill_int = cl_sdoc_flex_batch_bill_int=>get_instance( ).
           go_timestamp_update         = cl_sd_sls_va_factory=>get_timestamp_update( ).

*          Check for switches
           gs_ops_switch_check-ops_sfws_sc_advret1 = cl_ops_switch_check=>ops_sfws_sc_advret1( ).
           gs_ops_switch_check-ops_sfws_sc_advret2 = cl_ops_switch_check=>ops_sfws_sc_advret2( ).
           gs_ops_switch_check-sd_sfws_sc1         = cl_ops_switch_check=>sd_sfws_sc1( ).
           gs_ops_switch_check-sd_sfws_sc2         = cl_ops_switch_check=>sd_sfws_sc2( ).
           gs_ops_switch_check-sd_sfws_sc4         = cl_ops_switch_check=>sd_sfws_sc4( ).
           gs_ops_switch_check-sfsw_segmentation   = cl_ops_switch_check=>sfsw_segmentation( ).
           gs_ops_switch_check-sfsw_segmentation_04 = cl_ops_switch_check=>sfsw_segmentation_04( ).
           gs_ops_switch_check-sfsw_fashion04      = cl_switch_check_fashion=>sfsw_fashion_04( ).

*          Check for Product Compliance Switsches
           gs_product_compliance_switch-is_prod_compl_active = cl_ehfnd_pc_bus_feature_state=>is_prod_compl_active( ).
           gs_product_compliance_switch-is_pma_active        = cl_ehfnd_pc_bus_feature_state=>is_pma_active( ).
           gs_product_compliance_switch-is_dgm_active        = cl_ehfnd_pc_bus_feature_state=>is_dgm_active( ).
           gs_product_compliance_switch-is_sds_active        = cl_ehfnd_pc_bus_feature_state=>is_sds_active( ).

           include mv45a_lcnum_select_switcho01.

           include mv45a_lcnum_select_switchf01.

           include rfm_psst_rule_assignm_01.


           include mv45ai0k_show_lcnum_detail.

           include mv45a_populate_vbpa_from_msi01.

*INCLUDE mv45a_auth_cancel_via_dig_pf01.

*INCLUDE mv45a_auth_cancel_via_dig_pf02.

           include mv45a_hide_rfm_switch_fldo01.

           include rfm_mv45a.

           include mv45a_hide_rfm_switch_fld_lo01.

           include mv45a_internal_sa_rel_set_do01.

           include mv45a_internal_sa_rel_set_df01.

           include mv45a_internal_sa_rel_pass_i01.

           include mv45a_internal_sa_rel_pass_f01.

           include mv45a_internal_sa_rel_get_di01.

           include mv45a_internal_sa_rel_get_df01.

           include mv45affe_fieldproperty_hidden.

           include mv45a_mass_validate_config_o01.

           include mv45a_mass_validate_config_f01.

           include rfm_vas_plant_capability.

           include mv45a_4470_txs_integration.

           include mv45a_tminco_selecto01.

           include mv45af_tminco.

           include mv45a_tminco_checki01.

           include mv45af0c_check_fix_ocsp.

           include mv45a_logbr_lfd_o01 if found. "Local Fiscal Data - PBO Modules
           include mv45a_logbr_lfd_f01 if found. "Local Fiscal Data - Forms

           include rfm_sgt_value_check.

           include mv45af0a_vbak_process_custh.

           include mv45a_rv45a_deliv_addr_ausgo01.
