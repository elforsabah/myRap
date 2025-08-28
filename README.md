DATA(lv_image_data) = ``.

IF condense( val = ls_wr_doc_result-comments ) IS NOT INITIAL.
  lv_image_data = ls_wr_doc_result-comments.
ENDIF.

IF condense( val = ls_wr_doc_result-created_by ) IS NOT INITIAL.
  lv_image_data = COND string(
                     WHEN lv_image_data IS INITIAL
                     THEN ls_wr_doc_result-created_by
                     ELSE |{ lv_image_data }, { ls_wr_doc_result-created_by }| ).
ENDIF.

IF condense( val = ls_wr_doc_result-timestamp ) IS NOT INITIAL.
  lv_image_data = COND string(
                     WHEN lv_image_data IS INITIAL
                     THEN ls_wr_doc_result-timestamp
                     ELSE |{ lv_image_data }, { ls_wr_doc_result-timestamp }| ).
ENDIF.
