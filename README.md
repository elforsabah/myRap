<img width="507" height="255" alt="image" src="https://github.com/user-attachments/assets/f9430e3a-8e41-4397-8425-b2484c80726f" />


    " -- CHANGED: Output success message containing the Date
                append new_message(
                         id       = 'Z_MSG_CL_SERVICE_EXT'
                         number   = '007' " Create a MSG: 'Success: Tour created for &1'
                         severity = if_abap_behv_message=>severity-success
                         v1       = |{ tours[ 1 ]-TourId }  |
                         v2       = |{ lv_current_date DATE = USER }|
                       ) TO reported-%other.

