
  "Tour for (date + template) already exists -> inform user and skip this day
  APPEND VALUE #(
    %msg = new_message_with_text(
             severity = if_abap_behv_message=>severity-warning
             text     = |Skipped: Tour already exists for template { <group>-template } on { lv_current_date DATE = USER }.|
           )
  ) TO reported-%other.
