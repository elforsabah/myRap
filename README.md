class lhc_serviceassignment definition inheriting from cl_abap_behavior_handler.

  private section.

    methods UnAssign for modify
      importing keys for action ServiceAssignment~UnAssign.
    methods get_instance_features for instance features
      importing keys request requested_features for ServiceAssignment result result.

    methods get_global_authorizations for global authorization
      importing request requested_authorizations for ServiceAssignment result result.
*    methods calc_changed_stamp for determine on save
*      importing keys for ServiceAssignment~calc_changed_stamp.
    methods MoveService for modify
      importing keys for action ServiceAssignment~MoveService result result.
    methods ReplanService for modify
      importing keys for action ServiceAssignment~ReplanService.
    methods assignment_changed for determine on save
      importing keys for ServiceAssignment~assignment_changed.

endclass.

class lhc_serviceassignment implementation.

  method UnAssign.
    data: tasks_param type table for action import /PLCE/R_PDTour\\Tour~UnassignServiceTasks.

    "read all relevant task assignments
    read entities of /PLCE/R_PDTour in local mode
      entity ServiceAssignment
        by \_ServiceTasksAssignments all fields with corresponding #( keys )
        result data(taskassignments).
    delete taskassignments where Removed = abap_true.

    "unassign tasks (per tour)
    loop at taskassignments assigning field-symbol(<task>) group by <task>-TourUUID assigning field-symbol(<tour>).
      modify entities of /PLCE/R_PDTour in local mode entity Tour
        execute UnassignServiceTasks from value #( (
          TourUUID = <tour>
          %param-_servicetasks = value #( for x in group <tour> ( ServiceTaskUUID = x-ServiceTaskUUID ServiceUUID = x-ServiceUUID ) )
        ) )
      failed failed
      reported reported.

      if failed is not initial.
        exit.
      endif.
    endloop.

    read entity /PLCE/R_PDService
      fields ( RequestedDate ) with corresponding #( keys )
      result data(services).

    read entities of /PLCE/R_PDTour in local mode
      entity Tour
        fields ( TourId ) with corresponding #( keys )
        result data(tours).

    " no success message on delete service
    loop at services assigning field-symbol(<service>).
      append new  /plce/cx_pd_exception( textid = /plce/cx_pd_exception=>service_unassigned
                                         severity = if_abap_behv_message=>severity-success
                                         msgv1 = conv #(  |{ services[ key entity ServiceUUID = <service>-ServiceUUID ]-RequestedDate date = user }| )
                                         tour = tours[ key entity touruuid = keys[ ServiceUUID = <service>-ServiceUUID ]-touruuid ]-tourid  ) to reported-%other.
    endloop.
  endmethod.

  method ReplanService.
    data: unassign_service type table for action import /PLCE/R_PDTourServiceAsgmt~Unassign.

    "Unassign Service from Tour
    move-corresponding keys to unassign_service.

    modify entities of /PLCE/R_PDTour in local mode
      entity ServiceAssignment
        execute UnAssign from unassign_service
      failed failed
      reported reported.

    if failed is initial.
      "change service status
      modify entities of /PLCE/R_PDService
        entity Service
          update fields ( ServiceStatus ) with value #( for k in keys
              ( %key-ServiceUUID = k-ServiceUUID ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-in_planning ) )
          create by  \_StatusHistory auto fill cid
            set fields with value #( for k in keys
              ( %tky = corresponding #( k-%tky )
                %target = value #( ( ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-in_planning ) ) ) )
        failed data(failed_service)
        reported data(reported_service).
      if failed_service is not initial.
        failed-serviceassignment = corresponding #( keys ).
        /plce/cl_pd_messagehelper=>collect_reported_service( exporting messages = reported_service changing result = reported-%other ).
      endif.
    endif.
  endmethod.

  method MoveService.

    "read all relevant task assignments
    read entities of /PLCE/R_PDTour in local mode
      entity ServiceAssignment
        by \_ServiceTasksAssignments all fields with corresponding #( keys )
        result data(taskassignments).
    delete taskassignments where Removed = abap_true.

    loop at keys assigning field-symbol(<key>) group by ( TourUUID = <key>-TourUuid Sequence = <key>-%param-Sequence PlaceBefore = <key>-%param-PlaceBefore )  assigning field-symbol(<tour_group>).

      modify entities of /PLCE/R_PDTour in local mode
        entity Tour
          execute SequenceTasks from value #( (
            TourUUID = <tour_group>-touruuid
            %param-SequenceMode = /plce/if_pd_constants=>c_pd_sequencemode-manual
            %param-Sequence = <tour_group>-sequence
            %param-PlaceBefore =  <tour_group>-placebefore
            %param-_servicetasks = value #( for st in taskassignments using key entity where (  TourUUID = <tour_group>-touruuid )  ( ServiceTaskUUID = st-ServiceTaskUUID ServiceUUID = st-ServiceUUID ) )
          ) )
          failed failed
          reported reported.

      if failed is not initial.
        exit.
      endif.

    endloop.

    read entities of /PLCE/R_PDTour in local mode
      entity Tour
        by \_ServiceAssignments
          all fields with corresponding #( keys )
      result data(serviceassignment_result).

    result = value #( for serviceassignment  in serviceassignment_result ( %tky = serviceassignment-%tky %param = serviceassignment ) ).

    modify entities of /PLCE/R_PDTour in local mode
      entity ServiceAssignment
        update fields ( LeadServiceTaskUUID ) with corresponding #( serviceassignment_result )
      failed failed
      reported reported.
  endmethod.

  method get_instance_features.
    read entities of /PLCE/R_PDTour in local mode
      entity Tour
        fields ( TourStatus ) with corresponding #( keys )
        result data(tours)
      entity ServiceAssignment
        fields ( ServiceStatus  ) with corresponding #( keys )
        result data(services).

     result =
      value #( FOR service IN services
                    LET enable_by_status = COND #( WHEN service-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed
                                                     OR service-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed
                                                     OR line_exists( tours[ key entity TourUUID = service-TourUUID TourStatus = /plce/if_pd_constants=>c_pd_tour_status-confirmed ] )
                                                     OR line_exists( tours[ key entity TourUUID = service-TourUUID TourStatus = /plce/if_pd_constants=>c_pd_tour_status-completed ] )
                                                   THEN if_abap_behv=>fc-o-disabled
                                                   ELSE if_abap_behv=>fc-o-enabled )
                    IN ( %tky                = service-%tky
                         %action-UnAssign    = enable_by_status
                         %action-MoveService = enable_by_status )
             ).
  endmethod.

  method get_global_authorizations.
  endmethod.

*  method calc_changed_stamp.
*    get time stamp field data(ts).
*    types ty_uuid type standard table of /plce/pdtour_uuid with key table_line.
*    data(uniquetours) =  value ty_uuid( for groups TourUuid of key in keys group by key-TourUuid without members ( TourUuid ) ).
*
*    modify entities of /PLCE/R_PDTour in local mode entity Tour
*      update fields ( ServiceAssignmentChangedAt )
*        with value #( for ukey in uniquetours ( TourUUID = ukey %is_draft = keys[ TourUuid = ukey ]-%is_draft ServiceAssignmentChangedAt = ts ) )
*      reported data(ltour_reported).
*
*    reported = corresponding #( deep ltour_reported ).
*
*  endmethod.

  method assignment_changed.

    data:
      lkeys          type /plce/ppdsrv_key,
      lqueues           type /plce/ppd_bgp_service_queue,
      service_processor type ref to /plce/cl_bgp_pdservice,
      lbadi             type ref to /plce/pd_confirmation,
      ltbapiret         type bapirettab.

    read entities of /PLCE/R_PDService
      entity Service fields ( ServiceUUID Profile )
      with corresponding #( keys )
      result data(services).

    try.
        loop at services reference into data(lservicegroup) group by lservicegroup->Profile reference into data(lserviceprofile).

          clear:
            lkeys, ltbapiret.

          get badi lbadi filters profile = lserviceprofile->*.

          lkeys = value #( for l in services where ( Profile = lserviceprofile->* ) ( service_uuid = l-ServiceUUID ) ).

          call badi lbadi->get_service_queues
            exporting
              iv_profile = lserviceprofile->*
              it_keys    = lkeys
            changing
              ct_queues  = lqueues
              ct_bapIRET = ltbapiret.

          /plce/cl_base_misc=>check_bapiret(
            exporting
              it_bapiret  = ltbapiret
            changing
              cs_reported = reported
            ).

          loop at lqueues reference into data(lqueueref).
            create object service_processor.
            service_processor->profile = lserviceprofile->*.
            service_processor->service_sequence = value #(
              for lsrv in lqueueref->keys
                for lassgnmt in keys where ( ServiceUUID = lsrv-service_uuid ) ( corresponding #( lassgnmt )
                )
              ).
            service_processor->service_sequence_control-sequence_operational = 'X'.

            /plce/cl_bgprocessing_helper=>schedule(
              i_bgunit = service_processor
              i_queues = lqueueref->queues
              i_associated_entities = value #( for k in lqueueref->keys ( EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = k-service_uuid ) )
            ).
          endloop.

        endloop.

      catch /plce/cx_baseexception into data(lex).
        if lex is instance of /plce/cx_bgp.
          lex->if_abap_behv_message~m_severity = if_abap_behv_message=>severity-warning.
          lex->if_t100_dyn_msg~msgty = if_abap_behv_message=>severity-warning.
        endif.
        insert lex into table reported-%other.
    endtry.

  endmethod.

endclass.

class lhc_taskassignment definition inheriting from cl_abap_behavior_handler.

  private section.
    types _keytab_assignedservices type table for key of /PLCE/R_PDTour\\ServiceAssignment.
    types _failed type response for  failed  /PLCE/R_PDTour.
    types _reported type response for reported  /PLCE/R_PDTour.

    methods AdjustLeadingTask
      importing keys type _keytab_assignedservices.
    methods AdjustTourDuration for determine on save
      importing keys for TaskAssignment~AdjustTourDuration.
    methods AdjustServiceAssignments for determine on modify
      importing keys for TaskAssignment~AdjustServiceAssignments.
    methods Deactivate for modify
      importing keys for action TaskAssignment~Deactivate.
    methods UnassignOnDelete for determine on modify
      importing keys for TaskAssignment~UnassignOnDelete.
    methods ClearGeoRoute for determine on save
      importing keys for TaskAssignment~ClearGeoRoute.
    methods get_global_authorizations for global authorization
      importing request requested_authorizations for TaskAssignment result result.
    methods calc_changed_stamp for determine on save
      importing keys for TaskAssignment~calc_changed_stamp.

endclass.

class lhc_taskassignment implementation.

  method AdjustServiceAssignments.

    data: serviceassignments_tocreate type table for key of /PLCE/R_PDTour\\ServiceAssignment,
          serviceassignments_create   type table for create /PLCE/R_PDTour\_ServiceAssignments,
          serviceassignments_update   type table for update /PLCE/R_PDTour\\ServiceAssignment.

    read entities of /PLCE/R_PDTour in local mode
      entity TaskAssignment
        fields ( Removed ) with corresponding #( keys )
        result final(taskassignments)
        failed final(missingtaskassignments).

    data: taskassignments_active   type table for key of /PLCE/R_PDTour\\TaskAssignment,
          taskassignments_inactive type table for key of /PLCE/R_PDTour\\TaskAssignment.
    "separate active - inactive task assignments
    taskassignments_active = value #( for i in taskassignments where ( Removed is initial ) ( corresponding #( i ) ) ) .
    taskassignments_inactive = value #( for i in taskassignments where ( Removed = abap_true ) ( corresponding #( i ) ) ) .
    taskassignments_inactive = value #( base taskassignments_inactive for i2 in missingtaskassignments-taskassignment ( corresponding #( i2 ) ) ).

    "read all affected service assignments
    "read all tasks assignments for affected service assignments
    read entities of /PLCE/R_PDTour in local mode
      entity ServiceAssignment
        all fields with corresponding #( keys )
          result final(serviceassignments)
        by \_ServiceTasksAssignments
          all fields with corresponding #( keys )
            result final(taskassignments_all).

    "create missing service assignments
    loop at taskassignments_active assigning field-symbol(<taskassignment>) where TaskScope = 'S' group by ( TourUUID = <taskassignment>-TourUUID ServiceUUID = <taskassignment>-ServiceUUID ) assigning field-symbol(<group>).
      assign serviceassignments[ key entity TourUuid = <group>-touruuid ServiceUuid = <group>-serviceuuid ] to field-symbol(<serviceassignment>) else unassign.
      if <serviceassignment> is not assigned. "create new
        append value #( TourUUID = <group>-touruuid ServiceUUID = <group>-serviceuuid  ) to serviceassignments_tocreate.
      elseif <serviceassignment>-Removed = abap_true. "activate
        append value #( TourUUID = <group>-touruuid ServiceUUID = <group>-serviceuuid Removed = abap_false ) to serviceassignments_update.
      endif.
    endloop.

    "deactivate unused service assignments
    loop at taskassignments_inactive assigning <taskassignment> group by ( TourUUID = <taskassignment>-TourUUID ServiceUUID = <taskassignment>-ServiceUUID ) assigning <group>.
      "check if there is any remaining active service task
      assign taskassignments_all[ TourUUID = <group>-touruuid ServiceUUID = <group>-serviceuuid Removed = abap_false ] to field-symbol(<taskassignment_active>) else unassign.
      if <taskassignment_active> is not assigned.   "deactivate
        append value #( TourUUID = <group>-touruuid ServiceUUID = <group>-serviceuuid Removed = abap_true ) to serviceassignments_update.
      endif.
    endloop.

    "write changes
    serviceassignments_create = value #( for groups g of line in serviceassignments_tocreate group by line-TourUuid ( TourUUID = g %target = value #( for x in group g ( ServiceUuid = x-ServiceUuid ) ) ) ).
    modify entities of /PLCE/R_PDTour in local mode
      entity Tour
        create by \_ServiceAssignments auto fill cid
          set fields with serviceassignments_create
      entity ServiceAssignment
        update fields ( Removed TourSequence LeadServiceTaskUUID ) with corresponding #( serviceassignments_update )
      failed data(failed)
      mapped data(mapped)
      reported data(ltour_reported).

    reported = corresponding #( deep ltour_reported ).

    "trigger adjustments
    data: serviceassignments_adjust type  _keytab_assignedservices .
    serviceassignments_adjust = value #( base serviceassignments_tocreate for s in serviceassignments_update ( corresponding #( s ) ) ).

    AdjustLeadingTask( exporting keys = serviceassignments_adjust ) .

  endmethod.


  method AdjustLeadingTask.

    " get all assigned services - service tasks
    read entities of /PLCE/R_PDTour in local mode
      entity ServiceAssignment
        by \_ServiceTasksAssignments fields ( TourSequence Removed ) with corresponding #( keys )
        result data(assignedservicetasks)
      entity ServiceAssignment fields ( LeadServiceTaskUUID )
        with corresponding #( keys )
        result data(assignedservices).
    delete assignedservices where Removed = abap_true.
    delete assignedservicetasks where Removed = abap_true.

    read entities of /PLCE/R_PDService
      entity ServiceTask
        fields ( SequenceNumber IsServiceTaskOptional )
        with corresponding #( assignedservicetasks )
        result data(servicetasks).

    types: begin of _best,
             sericetask_assignment type structure for read result /plce/r_pdtour\\serviceassignment\_servicetasksassignments,
             servicetask           type structure for read result /plce/r_pdservice\\servicetask,
           end of _best.
    data: best                     type _best,
          serviceassignment_update type table for update /plce/r_pdtour\\serviceassignment.

    loop at  assignedservices assigning field-symbol(<assignedservice>).
      clear best.
      if <assignedservice>-LeadServiceTaskUUID is initial "lead task ist empty or invalid
          or not line_exists( assignedservicetasks[ key entity TourUUID = <assignedservice>-TourUuid ServiceUUID = <assignedservice>-ServiceUuid ServiceTaskUUID = <assignedservice>-LeadServiceTaskUUID ] ) .
        "search best fitting task ( least sequence number + not optional
        loop at assignedservicetasks assigning field-symbol(<assignedservicetask>) using key entity where TourUUID = <assignedservice>-TourUuid and ServiceUUID = <assignedservice>-ServiceUuid  .
          assign servicetasks[ key entity ServiceTaskUUID = <assignedservicetask>-ServiceTaskUUID ] to field-symbol(<servicetask>).
          if <servicetask> is assigned and <servicetask>-IsServiceTaskOptional is initial.
            if best is initial.
              best = value #( sericetask_assignment = <assignedservicetask> servicetask = <servicetask>  ).
              continue.
            elseif <servicetask>-SequenceNumber < best-servicetask-SequenceNumber.
              best = value #( sericetask_assignment = <assignedservicetask> servicetask = <servicetask>  ).
            endif.
          endif.
        endloop.
        if best is not initial.
          append value #( %pky = <assignedservice>-%pky LeadServiceTaskUUID = best-sericetask_assignment-ServiceTaskUUID ) to serviceassignment_update.
        endif.

      endif.
    endloop.

    modify entities of /PLCE/R_PDTour in local mode
      entity ServiceAssignment
      update fields ( LeadServiceTaskUUID ) with serviceassignment_update.


  endmethod.



  method AdjustTourDuration.
    read entities of /PLCE/R_PDTour in local mode
      entity Tour fields ( TourUUID ) with corresponding #( keys )
      result data(tours).

    modify entities of /PLCE/R_PDTour in local mode
      entity Tour execute calculateDuration from corresponding #( tours )
      reported data(ltour_reported).

    reported = corresponding #( deep ltour_reported ).
  endmethod.

  method Deactivate.
    modify entities of /PLCE/R_PDTour in local mode
      entity TaskAssignment
        update fields ( Removed ) with value #( for k in keys ( %tky = k-%tky Removed = abap_true ) )
    failed failed
    reported reported
    mapped mapped.
  endmethod.

  method UnassignOnDelete.
    modify entities of /PLCE/R_PDService
      entity ServiceTask
        update fields ( TourUUID )
          with value #( for task in keys where ( ServiceTaskUUID is not initial ) ( ServiceTaskUUID = task-ServiceTaskUUID ) )
    failed data(st_failed)
    reported data(st_reported).

    /plce/cl_base_misc=>check_response(
      exporting
        is_response = st_reported
      changing
        ct_messages = reported-%other
    ).
  endmethod.

  method ClearGeoRoute.
    read entities of /PLCE/R_PDTour in local mode
      entity Tour fields ( TourUUID SessionProcessFlag ) with corresponding #( keys )
      result data(lt_tour)
      entity TaskAssignment fields ( TourUUID TaskScope TourSequence ) with corresponding #( keys )
      result data(lt_taskassignment).

    "Process flow
    delete lt_tour where SessionProcessFlag = /plce/if_pd_constants=>c_process_flow-scenarioconfirm.

    loop at lt_tour into data(ltour).
      data(ltaskassignment) = value #( lt_taskassignment[ TourUUID = ltour-TourUUID TaskScope = 'S' ] optional ).
      if ltaskassignment is initial.
        delete lt_tour.
      endif.
      clear ltaskassignment.
    endloop.

    modify entities of /PLCE/R_PDTour in local mode
      entity Tour execute DeleteGeoRoute from corresponding #( lt_tour )
      reported data(ltour_reported).

    reported = corresponding #( deep ltour_reported ).
  endmethod.

  method get_global_authorizations.
  endmethod.

  method calc_changed_stamp.
    get time stamp field data(ts).
    types ty_uuid type standard table of /plce/pdtour_uuid with key table_line.
    data(uniquetours) =  value ty_uuid( for groups TourUuid of key in keys group by key-TourUuid without members ( TourUuid ) ).

    modify entities of /PLCE/R_PDTour in local mode entity Tour
      update fields ( ServiceAssignmentChangedAt )
        with value #( for ukey in uniquetours ( TourUUID = ukey %is_draft = keys[ TourUuid = ukey ]-%is_draft ServiceAssignmentChangedAt = ts ) )
      reported data(ltour_reported).

    reported = corresponding #( deep ltour_reported ).

  endmethod.

endclass.

class lhc_Assignment_in_PDTour definition inheriting from cl_abap_behavior_handler.
  private section.
    types: _tab_assignedtasks  type table for update /PLCE/R_PDTour\\TaskAssignment,
          _tourtasks type table for read result /plce/r_pdtour\\tour\_tourtasks.

    methods get_global_authorizations for global authorization
      importing request requested_authorizations for Tour~Assignment result result.
    methods UnassignServiceTasks for modify
      importing keys for action Tour~UnassignServiceTasks.
    methods AssignServices for modify
      importing keys for action Tour~AssignServices.
    methods SetSequenceMode for modify
      importing keys for action Tour~SetSequenceMode.
    methods AssignTasks for modify
      importing keys for action Tour~AssignTasks.
    methods SequenceTasks for modify
      importing keys for action Tour~SequenceTasks.
    methods precheck_AssignServices for precheck
      importing keys for action Tour~AssignServices.
    methods ResequenceServiceTasksOfTour for modify
      importing keys for action Tour~ResequenceServiceTasksOfTour.
    methods precheck_ResequenceAllServices for precheck
      importing keys for action Tour~ResequenceAllServices.
    methods ResequenceAllServices for modify
      importing keys for action Tour~ResequenceAllServices.


    "local methods
    methods CorrectTourTasksSequence
      importing touruuid type /plce/pdtour_uuid
                tourtasks type _tourtasks
      changing taskassignments type _tab_assignedtasks.
    methods SpaceTaskSequences
      changing taskassignments type _tab_assignedtasks.



endclass.

class lhc_Assignment_in_PDTour implementation.

  method get_global_authorizations.
  endmethod.


  method AssignTasks.

    "change sequence mode if necessary
    modify entities of /PLCE/R_PDTour in local mode
      entity Tour
        execute SetSequenceMode from value #( for key in keys ( TourUUID = key-TourUUID %param = key-%param-SequenceMode ) )
      failed failed
      reported reported.

    if failed is not initial.
      return.
    endif.

    data: tourtaskassignments_create type table for create /PLCE/R_PDTour\_TaskAssignments,
          tourtaskassignments_update type table for update /PLCE/R_PDTour\\TaskAssignment.

    " each tour separate
    loop at keys assigning field-symbol(<tourkey>).
      check failed is initial.

      clear: tourtaskassignments_create, tourtaskassignments_update.

      "read already assigned and not/other assigned
      data: taskassignments_read type table for read import /PLCE/R_PDTour\\TaskAssignment.
      taskassignments_read = value #( for servicetask in <tourkey>-%param-_servicetasks ( TourUUID = <tourkey>-TourUUID TaskScope = 'S' ServiceTaskUUID = servicetask-ServiceTaskUUID ServiceUUID = servicetask-ServiceUUID ) ).
      taskassignments_read = value #( base taskassignments_read for tourtask in <tourkey>-%param-_tourtasks ( TourUUID = <tourkey>-TourUUID TaskScope = 'T' TourTaskUUID = tourtask-TourTaskUUID ) ).
      read entities of /PLCE/R_PDTour in local mode
        entity TaskAssignment
          all fields with taskassignments_read
            result final(assignments_available)
            failed final(assignments_missing).

      " read all affected service tasks
      read entities of /PLCE/R_PDService entity ServiceTask
        fields ( ServiceTaskUUID ServiceUUID TourUUID Duration DurationUnit  )
        with value #( for i in <tourkey>-%param-_servicetasks ( %tky-ServiceTaskUUID = i-ServiceTaskUUID )  )
      result final(servicetasks) .

      "determine legacy service assignments to delete
      read entities of /PLCE/R_PDTour in local mode entity TaskAssignment
        all fields
          with value #( for st in servicetasks where ( TourUUID is not initial and TourUUID <> <tourkey>-TourUUID ) ( TourUUID = st-TourUUID TaskScope = 'S' ServiceTaskUUID = st-ServiceTaskUUID ServiceUUID = st-ServiceUUID ) )
      result final(tourtaskassignments_delete).

      "prepare missing service assignments for create, tour tasks have assignments by default
      if assignments_missing-taskassignment is not initial.
        tourtaskassignments_create = value #( ( TourUUID = <tourkey>-TourUUID %target = value #(
           for ass in assignments_missing-taskassignment ( TaskScope = ass-TaskScope ServiceTaskUUID = ass-ServiceTaskUUID ServiceUUID = ass-ServiceUUID TourTaskUUID = ass-TourTaskUUID )
          ) ) ).
        loop at tourtaskassignments_create[ 1 ]-%target assigning field-symbol(<tourtaskassignments_create>).
          assign servicetasks[ key entity ServiceTaskUUID = <tourtaskassignments_create>-ServiceTaskUUID ] to field-symbol(<servicetask>).
          if <servicetask> is assigned.
            <tourtaskassignments_create>-Duration = <servicetask>-Duration.
            <tourtaskassignments_create>-DurationUnit = <servicetask>-DurationUnit.
          endif.
        endloop.
      else.
        clear tourtaskassignments_create.
      endif.

      "reactivate disabled task assignments
      tourtaskassignments_update = value #( for ass2 in assignments_available using key entity where ( TourUUID = <tourkey>-TourUUID and Removed = abap_true ) ( %tky = ass2-%tky Removed = abap_false  ) ).

      "create/update task assignments
      modify entities of /PLCE/R_PDTour in local mode
        entity Tour
          create by \_TaskAssignments auto fill cid
            set fields with tourtaskassignments_create
        entity TaskAssignment
          update fields ( Removed ) with tourtaskassignments_update
          execute Deactivate from corresponding #( tourtaskassignments_delete )
        failed failed
        reported reported.

      check failed is initial.

      "update tour in service task
      data servicetask_update type table for update /PLCE/R_PDService\\ServiceTask.
      servicetask_update = value #( for t in assignments_missing-taskassignment where ( TaskScope = 'S' ) ( corresponding #( t ) ) ).
      servicetask_update = value #( base servicetask_update for sa in tourtaskassignments_update ( corresponding #( sa ) ) ).

      modify entities of /PLCE/R_PDService
        entity ServiceTask
          update fields ( TourUUID )
          with value #( for v in servicetask_update ( ServiceTaskUUID = v-ServiceTaskUUID TourUUID = <tourkey>-TourUUID ) )
        failed data(st_failed)
        reported data(st_reported).
      if st_failed is not initial.
        failed-tour = value #( base failed-tour for k in keys ( %tky = k-%tky ) ).
        /plce/cl_pd_messagehelper=>collect_reported_service( exporting messages = st_reported changing result = reported-%other ).
      endif.
      check failed is initial.

      "re-sequence legacy tours
      if tourtaskassignments_delete is not initial.
        SequenceTasks(  exporting
          keys = value #( for groups of line in tourtaskassignments_delete
            where ( TourUUID is not initial )  group by line-TourUUID ( TourUUID = line-TourUUID %param-SequenceMode = <tourkey>-%param-SequenceMode )  ) ).
      endif.
    endloop.
    if failed is not initial.
      return.
    endif.

    "sequence tours with new assignments
    SequenceTasks( exporting keys = corresponding #( keys ) changing failed = failed reported = reported mapped = mapped ).

  endmethod.

  method SequenceTasks.

    "change sequence mode if necessary
    modify entities of /PLCE/R_PDTour in local mode
      entity Tour
        execute SetSequenceMode from value #( for key in keys ( TourUUID = key-TourUUID %param = key-%param-SequenceMode ) )
      failed failed
      reported reported.

    if failed is not initial.
      return.
    endif.

    "read current assignments
    read entities of /PLCE/R_PDTour in local mode entity Tour
      by \_TaskAssignments fields ( Removed TourSequence DateTimeStart Duration DurationUnit )
        with corresponding #( keys )
        result data(assignments_current)
      by \_TourTasks all fields
        with corresponding #( keys )
        result final(tourtasks).
    delete assignments_current where Removed = abap_true.

    data: assignment_update type table for update /PLCE/R_PDTour\\TaskAssignment,
          target_sequence   type /plce/sequence_number.

    " each tour separate
    loop at keys assigning field-symbol(<tourkey>).

      "prepare update table
      assignment_update = value #( for x in assignments_current using key entity where ( TourUUID = <tourkey>-TourUUID ) ( %pky = x-%pky TourSequence = x-TourSequence Duration = x-Duration DurationUnit = x-DurationUnit ) ).

      data assignment_changes type table for read result /PLCE/R_PDTour\\TaskAssignment.

      if <tourkey>-%param-SequenceMode = /plce/if_pd_constants=>c_pd_sequencemode-calculation.
        assignment_changes = value #( for servicetask in <tourkey>-%param-_servicetasks (
          TourUUID = <tourkey>-TourUUID TaskScope = 'S' ServiceUUID = servicetask-ServiceUUID ServiceTaskUUID = servicetask-ServiceTaskUUID
          DateTimeStart = servicetask-StartTime Duration = servicetask-Duration DurationUnit = servicetask-DurationUnit ) ).
        assignment_changes = value #( base assignment_changes for tourtask in <tourkey>-%param-_tourtasks (
          TourUUID = <tourkey>-TourUUID TaskScope = 'T' TourTaskUUID = tourtask-TourTaskUUID
          DateTimeStart = tourtask-StartTime Duration = tourtask-Duration DurationUnit = tourtask-DurationUnit ) ).

        loop at assignment_changes assigning field-symbol(<assignment_change>).
          assign assignment_update[ key id %tky = <assignment_change>-%tky ] to field-symbol(<assignment_update>).
          <assignment_update>-%control-DateTimeStart = cond #( when <assignment_update>-DateTimeStart <> <assignment_change>-DateTimeStart then if_abap_behv=>mk-on ).
          <assignment_update>-DateTimeStart = <assignment_change>-DateTimeStart.
          <assignment_update>-%control-Duration = cond #( when <assignment_update>-Duration <> <assignment_change>-Duration then if_abap_behv=>mk-on ).
          <assignment_update>-Duration = <assignment_change>-Duration.
          <assignment_update>-%control-DurationUnit = cond #( when <assignment_update>-DurationUnit <> <assignment_change>-DurationUnit then if_abap_behv=>mk-on ).
          <assignment_update>-DurationUnit = <assignment_change>-DurationUnit.
        endloop.

        target_sequence = 10 .

        sort assignment_update by DateTimeStart.
        loop at assignment_update assigning <assignment_update>.
          data(new_sequence) = cond #( when <assignment_update>-DateTimeStart is not initial then target_sequence else 0 ).
          <assignment_update>-%control-TourSequence = cond #( when <assignment_update>-TourSequence <> new_sequence then if_abap_behv=>mk-on ).
          <assignment_update>-TourSequence = new_sequence.
          if new_sequence = target_sequence.
            target_sequence = target_sequence + 10 .
          endif.
        endloop.

      endif.
      if <tourkey>-%param-SequenceMode = /plce/if_pd_constants=>c_pd_sequencemode-manual.

        "determine target start sequence
        target_sequence = cond /plce/sequence_number(
          when <tourkey>-%param-PlaceBefore = 'X' and <tourkey>-%param-Sequence < 1
            then 1
          when <tourkey>-%param-PlaceBefore = 'X'
            then <tourkey>-%param-Sequence - 1
          when <tourkey>-%param-PlaceBefore is initial and <tourkey>-%param-Sequence < 1
            then 9999 ##NUMBER_OK
          else <tourkey>-%param-Sequence + 1 ).

        "move temporary to have enough space
        data(countnewtasks) = lines( <tourkey>-%param-_servicetasks ) + lines( <tourkey>-%param-_tourtasks ).
        data(counttourtasks) = lines( tourtasks ).
        data: assignment_update_temp type table for update /PLCE/R_PDTour\\TaskAssignment.
        assignment_update_temp = assignment_update.


        loop at assignment_update_temp assigning field-symbol(<assignment_update_temp>).
          if <assignment_update_temp>-TourSequence is not initial and <assignment_update_temp>-TourSequence > target_sequence.
            <assignment_update_temp>-TourSequence = <assignment_update_temp>-TourSequence + countnewtasks.
          endif.
        endloop.

        "sequence service assignments (all tasks of same service will get the same sequence number)
        loop at <tourkey>-%param-_servicetasks assigning field-symbol(<servicetask>) group by ( ServiceUUID = <servicetask>-ServiceUUID ) assigning field-symbol(<serviceunique>).
          loop at group <serviceunique> assigning <servicetask>." field-symbol(<st>).
            assign assignment_update_temp[ TourUUID = <tourkey>-TourUUID TaskScope = 'S' ServiceUUID = <servicetask>-ServiceUUID ServiceTaskUUID = <servicetask>-ServiceTaskUUID ] to <assignment_update_temp>.
            <assignment_update_temp>-TourSequence = target_sequence.
          endloop.
          target_sequence = target_sequence + 1.
        endloop.

        CorrectTourTasksSequence( exporting touruuid = <tourkey>-TourUUID tourtasks = tourtasks changing taskassignments = assignment_update_temp ).

        "update non temp. update table
        loop at assignment_update_temp assigning <assignment_update_temp>.
          assign assignment_update[ key entity %pky = <assignment_update_temp>-%pky ] to <assignment_update>.
          <assignment_update>-%control-TourSequence = cond #( when <assignment_update>-TourSequence <> <assignment_update_temp>-TourSequence then if_abap_behv=>mk-on ).
          <assignment_update>-TourSequence = <assignment_update_temp>-TourSequence.
        endloop.
      endif.

      delete assignment_update where %control is initial.

      modify  entities of /PLCE/R_PDTour in local mode
        entity TaskAssignment
          update from assignment_update
        failed failed
        reported reported.

      if failed is not initial.
        exit.
      endif.
    endloop.

  endmethod.

  method UnassignServiceTasks.

    "change sequence mode if necessary
    modify entities of /PLCE/R_PDTour in local mode
      entity Tour
        execute SetSequenceMode from value #( for key in keys ( TourUUID = key-TourUUID %param = key-%param-SequenceMode ) )
      failed failed
      reported reported.

    if failed is not initial.
      return.
    endif.

    " each tour separate
    loop at keys assigning field-symbol(<tourkey>).

      modify entities of /PLCE/R_PDTour in local mode
        entity TaskAssignment
          execute Deactivate from value #( for task in <tourkey>-%param-_servicetasks (
            TourUUID = <tourkey>-TourUUID
            TaskScope = 'S'
            ServiceUUID = task-ServiceUUID
            ServiceTaskUUID = task-ServiceTaskUUID
          ) )
        failed failed
        reported reported.

      if failed is not initial.
        exit.
      endif.

      "update tour in service task
      modify entities of /PLCE/R_PDService
        entity ServiceTask
          update fields ( TourUUID )
          with value #( for task in <tourkey>-%param-_servicetasks ( ServiceTaskUUID = task-ServiceTaskUUID  ) )
        failed data(st_failed)
        reported data(st_reported).

      if st_failed is not initial.
        failed-tour = value #( for key in keys ( %tky = key-%tky ) ).
        /plce/cl_pd_messagehelper=>collect_reported_service( exporting messages = st_reported changing result = reported-%other ).
        exit.
      endif.
    endloop.

    if failed is initial.
      SequenceTasks( exporting keys = value #( for key in keys ( TourUUID = key-TourUUID %param-SequenceMode = key-%param-SequenceMode ) ) ) .
    endif.


  endmethod.

  method precheck_AssignServices.
    " precheck for tour constraints in /plce/bp_r_pdservice=>precheck_assigntour
    read entities of /PLCE/R_PDTour in local mode
      entity Tour
        fields ( TourUUID TourId TourStatus ) with corresponding #( keys )
        result final(tours).

    loop at tours assigning field-symbol(<tour>).
      " TODO derive Tour Status from Work Status
      if <tour>-TourStatus = /plce/if_pd_constants=>c_pd_tour_status-completed or <tour>-TourStatus = /plce/if_pd_constants=>c_pd_tour_status-confirmed.
        append value #( %tky = <tour>-%tky
                        %msg = new /plce/cx_pd_exception(
                                 severity = if_abap_behv_message=>severity-error
                                 textid = /plce/cx_pd_exception=>cs_srvc_assgmnt_tour_completed
                                 tour = <tour>-TourId
                               )
                      ) to reported-tour.
        append value #( %tky = <tour>-%tky ) to failed-tour.
      endif.
    endloop.
  endmethod.

  method AssignServices.
    "TODO  precheck/features (nur unplanned/full planned services)

    loop at keys assigning field-symbol(<tourkey>).
      read entities of /PLCE/R_PDService
        entity Service
          "fields ( )
          by \_ServiceTask fields ( PlanningStatus ) "TODO Confirmation/Work Status -> fertiggestellte Tasks sollten nicht mehr verplant werden
            with value #( for i in <tourkey>-%param-_services ( %tky-ServiceUUID  = i )  )
            result data(servicetasks) .

      AssignTasks(
        exporting keys = value #( (
          %tky = <tourkey>-%tky
          %param-SequenceMode = /plce/if_pd_constants=>c_pd_sequencemode-manual
          %param-Sequence = <tourkey>-%param-Sequence
          %param-PlaceBefore = <tourkey>-%param-PlaceBefore
          %param-_servicetasks = corresponding #( servicetasks ) ) )
        changing
          failed = failed
          reported = reported
          mapped = mapped ).

      if failed is not initial.
        exit.
      endif.
    endloop.
  endmethod.


  method SetSequenceMode.
    read entities of /PLCE/R_PDTour in local mode entity Tour
      fields ( SequenceMode ) with corresponding #( keys )
        result final(tours).

    data: tour_update           type table for update /PLCE/R_PDTour,
          taskassignment_update type table for update /PLCE/R_PDTour\\TaskAssignment,
          taskassignment_delete type table for delete /PLCE/R_PDTour\\TaskAssignment,
          tourtask_delete       type table for delete /PLCE/R_PDTour\\TourTask.

    loop at keys assigning field-symbol(<key>).
      data(mode) = <key>-%param.
      check tours[ key id %tky = <key>-%tky ]-SequenceMode <> mode.

      append value #( %tky = <key>-%tky SequenceMode = mode ) to tour_update.

      read entities of /PLCE/R_PDTour in local mode entity Tour
        by \_ServiceAssignments
          fields ( TourSequence ) with corresponding #( keys )
          result data(serviceassignments)
        by \_TaskAssignments
          fields ( DateTimeStart Duration DurationUnit TourSequence ) with corresponding #( keys )
          result data(tasks)
        by \_TourTasks
          fields ( IsCalculated Duration DurationUnit ) with corresponding #( keys )
          result final(tourtasks) .
      delete serviceassignments where Removed = abap_true.
      delete tasks where Removed = abap_true.

      read entities of /PLCE/R_PDService entity Service
        by \_ServiceTask
          fields ( Duration DurationUnit ) with corresponding #( serviceassignments )
          result final(servicetasks).

      if mode = /plce/if_pd_constants=>c_pd_sequencemode-manual.
        loop at tasks using key entity assigning field-symbol(<task>) where  TourUUID = <key>-TourUUID.

          if <task>-TaskScope = 'S'.
            append initial line to taskassignment_update assigning field-symbol(<taskassignment_update>).
            <taskassignment_update>-%tky = <task>-%tky.

            clear <taskassignment_update>-DateTimeStart.
            <taskassignment_update>-TourSequence = value #( serviceassignments[ key entity TourUuid = <task>-TourUUID ServiceUuid = <task>-ServiceUUID ]-TourSequence optional ).

            <taskassignment_update>-%control-DateTimeStart = cond #( when <task>-DateTimeStart <> <taskassignment_update>-DateTimeStart then if_abap_behv=>mk-on ).
            <taskassignment_update>-%control-TourSequence = cond #( when <task>-TourSequence <> <taskassignment_update>-TourSequence then if_abap_behv=>mk-on ).

            assign servicetasks[ key entity ServiceTaskUUID = <taskassignment_update>-ServiceTaskUUID ] to field-symbol(<servicetask>).
            if <servicetask> is assigned.
              <taskassignment_update>-%control-Duration = cond #( when <task>-Duration <> <servicetask>-Duration then if_abap_behv=>mk-on ).
              <taskassignment_update>-Duration = <servicetask>-Duration.
              <taskassignment_update>-%control-DurationUnit = cond #( when <task>-DurationUnit <> <servicetask>-DurationUnit then if_abap_behv=>mk-on ).
              <taskassignment_update>-DurationUnit = <servicetask>-DurationUnit.
            endif.

          elseif <task>-TaskScope = 'T'.
            data(tourtask) = value #( tourtasks[ key entity TourUUID = <key>-TourUUID TourTaskUUID = <task>-TourTaskUUID ] optional ).
            if tourtask-IsCalculated = 'X'.
              append corresponding #( <task> ) to taskassignment_delete.
              append corresponding #( <task> ) to tourtask_delete.
            else.
              append initial line to taskassignment_update assigning <taskassignment_update>.
              <taskassignment_update>-%tky = <task>-%tky.
              clear <taskassignment_update>-TourSequence.
              <taskassignment_update>-%control-TourSequence = cond #( when <task>-TourSequence <> <taskassignment_update>-TourSequence then if_abap_behv=>mk-on  ).

              clear <taskassignment_update>-DateTimeStart.
              <taskassignment_update>-%control-DateTimeStart = cond #( when <task>-DateTimeStart <> <taskassignment_update>-DateTimeStart then if_abap_behv=>mk-on ).

              <taskassignment_update>-%control-Duration = cond #( when <task>-Duration <> tourtask-Duration then if_abap_behv=>mk-on ).
              <taskassignment_update>-Duration = tourtask-Duration.
              <taskassignment_update>-%control-DurationUnit = cond #( when <task>-DurationUnit <> tourtask-DurationUnit then if_abap_behv=>mk-on ).
              <taskassignment_update>-DurationUnit = tourtask-DurationUnit.

            endif.
          endif.
        endloop.
      endif.
    endloop.

    modify entities of /PLCE/R_PDTour in local mode
      entity Tour
        update fields ( SequenceMode ) with tour_update
      entity TaskAssignment
        delete from taskassignment_delete
        update from taskassignment_update
      entity TourTask
        delete from tourtask_delete
    mapped mapped
    failed failed
    reported reported.
  endmethod.

  method ResequenceServiceTasksOfTour.
    "change sequence mode if necessary
    modify entities of /PLCE/R_PDTour in local mode
      entity Tour
        execute SetSequenceMode from value #( for key in keys ( TourUUID = key-TourUUID %param = /plce/if_pd_constants=>c_pd_sequencemode-manual ) )
      failed failed
      reported reported.

    if failed is not initial.
      return.
    endif.

    "read current assignments
    read entities of /PLCE/R_PDTour in local mode entity Tour
      by \_TaskAssignments fields ( Removed TourSequence DateTimeStart Duration DurationUnit )
        with corresponding #( keys )
        result data(assignments_current)
      by \_TourTasks all fields
        with corresponding #( keys )
        result final(tourtasks).
    delete assignments_current where Removed = abap_true.

    data: assignment_update type table for update /PLCE/R_PDTour\\TaskAssignment,
          target_sequence   type /plce/sequence_number.

    " each tour separate
    loop at keys assigning field-symbol(<tourkey>).

      "prepare update table     ToDo: consider subset of services and PlaceAfter
      assignment_update = value #( for x in assignments_current using key entity where ( TourUUID = <tourkey>-TourUUID ) ( %pky = x-%pky TourSequence = x-TourSequence Duration = x-Duration DurationUnit = x-DurationUnit ) ).

      data assignment_changes type table for read result /PLCE/R_PDTour\\TaskAssignment.
      data: assignment_update_temp type table for update /PLCE/R_PDTour\\TaskAssignment.
      assignment_update_temp = assignment_update.

      data: lServiceSequence type /plce/sequence_number.

      "sequence service assignments (all tasks of same service will get the same sequence number)
      loop at assignment_update_temp assigning field-symbol(<assignment_update_temp>).
        clear lServiceSequence.
        if <assignment_update_temp>-ServiceUUID is not initial.
          lServiceSequence = value #( <tourkey>-%param-_servicetasks[ ServiceUUID = <assignment_update_temp>-ServiceUUID ]-NewSequence optional ).
          if lServiceSequence is not initial.
            <assignment_update_temp>-TourSequence = lServiceSequence.
          endif.
        endif.
      endloop.

      CorrectTourTasksSequence( exporting touruuid = <tourkey>-TourUUID tourtasks = tourtasks changing taskassignments = assignment_update_temp ).

      "update non temp. update table
      loop at assignment_update_temp assigning <assignment_update_temp>.
        assign assignment_update[ key entity %pky = <assignment_update_temp>-%pky ] to field-symbol(<assignment_update>).
        <assignment_update>-%control-TourSequence = cond #( when <assignment_update>-TourSequence <> <assignment_update_temp>-TourSequence then if_abap_behv=>mk-on ).
        <assignment_update>-TourSequence = <assignment_update_temp>-TourSequence.
      endloop.

      delete assignment_update where %control is initial.

      modify  entities of /PLCE/R_PDTour in local mode
        entity TaskAssignment
          update from assignment_update
        failed failed
        reported reported.

      if failed is not initial.
        exit.
      endif.
    endloop.
  endmethod.

  method CorrectTourTasksSequence.
    data: target_sequence   type /plce/sequence_number.
      "sort by temp. sequence number and correct to 10 steps size
      SpaceTaskSequences( changing taskassignments = taskassignments ).

      "re- /sequence  tourtasks
      sort taskassignments by TourSequence.
      "check, tour contain at least 1 assigned service tasks
      loop at taskassignments assigning field-symbol(<assignment>) else unassign where TourSequence is not initial and ServiceTaskUUID is not initial .
        exit.
      endloop.
      data(hasservices) = cond #( when <assignment> is assigned then 'X' else '' ).

      "re- sequence pre- and post processings
      loop at tourtasks assigning field-symbol(<tourtask>) where TourUUID = touruuid
          and ( TaskCategory = /plce/if_pd_constants=>c_tour_task_category-preprocessing or TaskCategory = /plce/if_pd_constants=>c_tour_task_category-postprocessing ).
        assign taskassignments[ TourUUID = touruuid TourTaskUUID = <tourtask>-TourTaskUUID ] to <assignment>.
        if <assignment> is assigned.
          if hasservices = 'X'.
            <assignment>-TourSequence = cond #( when <tourtask>-TaskCategory = /plce/if_pd_constants=>c_tour_task_category-preprocessing then 1 else 9999 ) ##NUMBER_OK.
          else.
            clear <assignment>-TourSequence.
          endif.
        endif.
      endloop.

      "re- sequence breaks
      data: duration type /plce/duration.
      clear duration.
      loop at tourtasks assigning <tourtask> where TourUUID = touruuid and TaskCategory = /plce/if_pd_constants=>c_tour_task_category-break.
        assign taskassignments[ TourUUID = touruuid TourTaskUUID = <tourtask>-TourTaskUUID ] to field-symbol(<taskassignments_break>) else unassign.
        if <taskassignments_break> is not assigned.
          continue.
        endif.
        clear <taskassignments_break>-TourSequence.

        loop at taskassignments assigning <assignment> where Duration is not initial and TourSequence is not initial and TourTaskUUID <> <tourtask>-TourTaskUUID.
          data(converter) = cl_uom_conversion=>create(  ).
          data: conv_result type /plce/duration.
          try.
              converter->unit_conversion_simple( exporting input = <assignment>-Duration unit_in = <assignment>-DurationUnit unit_out = <tourtask>-EarliestStartUnit importing output = conv_result ).
              duration = duration + conv_result.
              if duration > <tourtask>-EarliestStart.
                <taskassignments_break>-TourSequence = <assignment>-TourSequence + 1.
                exit.
              endif.
            catch cx_root.
              clear conv_result.
          endtry.
        endloop.
        sort taskassignments by TourSequence descending.
        "no break at end or before post processing
        if taskassignments[ 1 ] = <taskassignments_break> or ( taskassignments[ 1 ]-TourSequence = 9999 and taskassignments[ 2 ] = <taskassignments_break> ).
          clear <taskassignments_break>-TourSequence.
        endif.
      endloop.

      "again:  sort by temp. sequence number and correct to 10 steps size
      SpaceTaskSequences( changing taskassignments = taskassignments ).
  endmethod.

  method SpaceTaskSequences.
    data: target_sequence   type /plce/sequence_number.

      sort taskassignments by TourSequence.
      target_sequence = 10 .
      loop at taskassignments assigning field-symbol(<assignment>) group by ( SequenceNumber = <assignment>-TourSequence )  assigning field-symbol(<group_sequence>).
        data: sequence_used type abap_boolean.
        clear sequence_used.
        loop at group <group_sequence> assigning <assignment>.
          if <assignment>-TourSequence is not initial.
            <assignment>-TourSequence = target_sequence.
            sequence_used = 'X'.
          endif.
        endloop.
        target_sequence = cond #( when sequence_used = 'X' then target_sequence + 10 else target_sequence ).
      endloop.
  endmethod.

  method precheck_ResequenceAllServices.
    types: begin of services,
             serviceuuid type /plce/pdservice_uuid,
             sequence type /plce/sequence_number,
           end of services.

    data: last_sequence type /plce/sequence_number,
          lt_services type table of services.

    loop at keys assigning field-symbol(<key>).
      clear last_sequence.
      move-corresponding <key>-%param-_services to lt_services.
      sort lt_services by Sequence.
      loop at lt_services assigning field-symbol(<lv_service>).
        if last_sequence is initial or last_sequence < <lv_service>-sequence.
          last_sequence = <lv_service>-sequence.
        elseif last_sequence = <lv_service>-Sequence.
          append value #( %tky = <key>-%tky
                          %msg = new /plce/cx_pd_exception(
                                   severity = if_abap_behv_message=>severity-error
                                   textid = /plce/cx_pd_exception=>resequence_different_numbers
                                 )
                        ) to reported-tour.
          append value #( %tky = <key>-%tky ) to failed-tour.
          exit.
        endif.
      endloop.
    endloop.
  endmethod.

  method ResequenceAllServices.
    data: target_sequence type /plce/sequence_number.

    loop at keys assigning field-symbol(<key>).
      "read current assignments
      read entities of /PLCE/R_PDTour in local mode entity Tour
        by \_TaskAssignments fields ( Removed TourSequence DateTimeStart Duration DurationUnit )
          with value #( ( TourUUID = <key>-touruuid ) )
          result data(assignments_current)
        by \_TourTasks all fields
          with value #( ( TourUUID = <key>-touruuid ) )
          result final(tourtasks).
      delete assignments_current where Removed = abap_true.

      data: assignment_update      type table for update /PLCE/R_PDTour\\TaskAssignment,
            assignment_update_temp type table for update /PLCE/R_PDTour\\TaskAssignment,
            move_assignment        type table for update /PLCE/R_PDTour\\TaskAssignment.

      "prepare update table
      assignment_update = value #( for x in assignments_current using key entity where ( TourUUID = <key>-TourUUID ) ( %pky = x-%pky TourSequence = x-TourSequence Duration = x-Duration DurationUnit = x-DurationUnit ) ).
      assignment_update_temp = assignment_update.

      "sequence service assignments (all tasks of same service will get the same sequence number)
      loop at assignment_update_temp assigning field-symbol(<assignment_update_temp>) group by ( ServiceUUID = <assignment_update_temp>-ServiceUUID ) assigning field-symbol(<serviceunique>).
        if line_exists( <key>-%param-_services[ ServiceUUID = <serviceunique>-serviceuuid ] ).
          target_sequence = <key>-%param-_services[ ServiceUUID = <serviceunique>-serviceuuid ]-Sequence.
          target_sequence = cond #( when target_sequence is initial then 1 else target_sequence ).
          "TourSequence already taken and service not in parameter
          if line_exists( assignment_update_temp[ TourSequence = target_sequence TaskScope = 'S' ] )
            and not line_exists(  <key>-%param-_services[ ServiceUUID = assignment_update_temp[ TourSequence = target_sequence TaskScope = 'S' ]-serviceuuid ] ).
            loop at assignment_update_temp assigning field-symbol(<old_assignment>) where TourSequence = target_sequence and TaskScope = 'S'.
              append <old_assignment> to move_assignment.
            endloop.
          endif.
          loop at group <serviceunique> assigning field-symbol(<servicetask>).
            assign assignment_update_temp[ TourUUID = <key>-TourUUID TaskScope = 'S' ServiceUUID = <servicetask>-ServiceUUID ServiceTaskUUID = <servicetask>-ServiceTaskUUID ] to <assignment_update_temp>.
            <assignment_update_temp>-TourSequence = target_sequence.
          endloop.
        endif.
      endloop.

      "move old assignments behind new services
      if move_assignment is not initial.
        sort move_assignment by TourSequence.
        assignment_update_temp = filter #( assignment_update_temp using key entity except in move_assignment where %key = %key ).
        select max( assigned_services~sequence ) from @<key>-%param-_services as assigned_services into @target_sequence ##ITAB_DB_SELECT.
        loop at move_assignment assigning field-symbol(<move_assignment>) group by ( ServiceUUID = <move_assignment>-ServiceUUID ) assigning <serviceunique>.
          target_sequence = target_sequence + 1.
          if target_sequence mod 10 eq 0.
            target_sequence = target_sequence + 1.  " don't assign service to other assignment
          endif.
          loop at group <serviceunique> assigning <servicetask>.
            <servicetask>-TourSequence = target_sequence.
          endloop.
        endloop.
        append lines of move_assignment to assignment_update_temp.
      endif.

      CorrectTourTasksSequence( exporting touruuid = <key>-TourUUID tourtasks = tourtasks changing taskassignments = assignment_update_temp ).

      "update non temp. update table
      loop at assignment_update_temp assigning <assignment_update_temp>.
        assign assignment_update[ key entity %pky = <assignment_update_temp>-%pky ] to field-symbol(<assignment_update>).
        <assignment_update>-%control-TourSequence = cond #( when <assignment_update>-TourSequence <> <assignment_update_temp>-TourSequence then if_abap_behv=>mk-on ).
        <assignment_update>-TourSequence = <assignment_update_temp>-TourSequence.
      endloop.

      delete assignment_update where %control is initial.

      modify  entities of /PLCE/R_PDTour in local mode
        entity TaskAssignment
          update from assignment_update
        failed failed
        reported reported.

      if failed is not initial.
        exit.
      endif.
    endloop.
  endmethod.

endclass.
