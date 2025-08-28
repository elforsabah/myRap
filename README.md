DATA: lt_parts TYPE TABLE OF string.

IF ls_wr_doc_result-comments IS NOT INITIAL.
  APPEND ls_wr_doc_result-comments TO lt_parts.
ENDIF.

IF ls_wr_doc_result-created_by IS NOT INITIAL.
  APPEND ls_wr_doc_result-created_by TO lt_parts.
ENDIF.

IF ls_wr_doc_result-timestamp IS NOT INITIAL.
  APPEND ls_wr_doc_result-timestamp TO lt_parts.
ENDIF.

DATA(lv_image_data) = concat_lines_of( table = lt_parts sep = `, ` ).
