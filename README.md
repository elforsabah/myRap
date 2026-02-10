METHOD adjusttime.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " 1. Read the Service Tasks associated with this Service
      "    We need to update a task to change the total duration.
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service BY \_ServiceTask
        FIELDS ( ServiceTaskUUID Duration DurationUnit )
        WITH VALUE #( ( %tky = <ls_key>-%tky ) )
        RESULT DATA(lt_tasks)
        FAILED DATA(lt_failed_read).

      IF lt_tasks IS INITIAL.
        " Error: No tasks found. Cannot adjust duration if there are no tasks.
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text( severity = if_abap_behv_message=>severity-error
                                                      text = 'No Service Tasks found to adjust.' )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      " 2. Parse the Input (Adjustment in Minutes)
      DATA(lv_input_str) = CONV string( <ls_key>-%param-ZeitInMin ).
      REPLACE ALL OCCURRENCES OF ',' IN lv_input_str WITH '.'.
      CONDENSE lv_input_str NO-GAPS.

      DATA(lv_adj_min) TYPE decfloat34.
      TRY.
          lv_adj_min = lv_input_str.
        CATCH cx_sy_conversion_no_number.
          CONTINUE.
      ENDTRY.

      " 3. Apply logic to the FIRST task found (Simplest strategy)
      "    We add the +/- adjustment to this task, which automatically updates the Total Sum.
      READ TABLE lt_tasks ASSIGNING FIELD-SYMBOL(<ls_task>) INDEX 1.

      " Convert Adjustment (Minutes) to Task Unit (usually 'H', 'MIN', etc.)
      DATA(lv_adj_in_task_unit) TYPE decfloat34.

      CASE <ls_task>-DurationUnit.
        WHEN 'H' OR 'HR'.
          lv_adj_in_task_unit = lv_adj_min / 60.
        WHEN 'MIN' OR 'M'.
          lv_adj_in_task_unit = lv_adj_min.
        WHEN OTHERS.
          " Default fallback to Hours if unknown
          lv_adj_in_task_unit = lv_adj_min / 60.
      ENDCASE.

      " Calculate New Duration for the Task
      DATA(lv_new_task_duration) = <ls_task>-Duration + lv_adj_in_task_unit.

      " Prevent negative task duration
      IF lv_new_task_duration < 0.
         lv_new_task_duration = 0.
      ENDIF.

      " 4. UPDATE THE SERVICE TASK (Standard RAP Update)
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ServiceTask
        UPDATE FIELDS ( Duration )
        WITH VALUE #( ( %tky     = <ls_task>-%tky
                        Duration = lv_new_task_duration ) )
        FAILED DATA(lt_failed_task)
        REPORTED DATA(lt_reported_task).

      APPEND LINES OF lt_failed_task-servicetask TO failed-servicetask.
      APPEND LINES OF lt_reported_task-servicetask TO reported-servicetask.

      " 5. UPDATE CUSTOM FIELD (Display Label: +45 MIN)
      DATA(lv_display_val) TYPE string.
      IF lv_adj_min > 0.
         lv_display_val = |+{ lv_adj_min } MIN|.
      ELSE.
         lv_display_val = |{ lv_adj_min } MIN|.
      ENDIF.

      " Check/Create Extension if missing
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom FIELDS ( ServiceUUID )
        WITH VALUE #( ( ServiceUUID = <ls_key>-ServiceUUID ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
         MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
           ENTITY Service CREATE BY \_ExtCustom
           AUTO FILL CID WITH VALUE #( ( ServiceUUID = <ls_key>-ServiceUUID ) )
           FAILED DATA(lt_failed_create).
      ENDIF.

      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        UPDATE FIELDS ( zz_timeadjustment )
        WITH VALUE #( ( %key-ServiceUUID = <ls_key>-ServiceUUID
                        zz_timeadjustment = lv_display_val ) )
        FAILED DATA(lt_failed_upd)
        REPORTED DATA(lt_reported_upd).

      " 6. REFRESH UI
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_result).

      result = VALUE #( FOR service IN lt_result ( %tky = service-%tky %param = service ) ).

    ENDLOOP.

  ENDMETHOD.
