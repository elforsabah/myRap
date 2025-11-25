INSERT new_message_with_text(
         severity = if_abap_behv_message=>severity-error
         text     = |Some error text| )
  INTO TABLE reported-%other.
