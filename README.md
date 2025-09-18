Diagnosis
In ABAP for Cloud Development, use of other objects is restricted to objects which

were released as API for Use in Cloud Development or
are in the same software component as the edited object. If the edited object is part of a local software component restricted to ABAP for Cloud Development, the objects of other editable software components are permitted also, if the other software component is restricted to ABAP for Cloud Development as well.
For further details, see ABAP Language Versions.

System Response
If the edited object uses an unpermitted object, this results in a syntax error.

Procedure
Replace the unpermitted Table /PLCE/TPDSRV by a suitable permitted object



        ASSIGN COMPONENT 'POINT_OF_ORIGIN_VER' OF STRUCTURE <ls_calculated> TO <lv_point_of_origin_wdoi>.
        IF sy-subrc = 0.
          <lv_point_of_origin_wdoi> = lv_zzpoint_origin_wdoi.
          UPDATE /PLCE/TPDSRV SET zzpoint_origin_wdoi = @<lv_point_of_origin_wdoi>
          WHERE reference_int_id = @<lv_point_of_origin_wdoi> .
        ENDIF.
