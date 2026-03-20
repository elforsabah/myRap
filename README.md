FUNCTION Z_WR_DELAYED_HARD_DELETE.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(IV_SERVICEUUID) TYPE  /PLCE/PDSERVICE_UUID
*"----------------------------------------------------------------------

  " 1. Wait for 3 seconds. 
  " This allows the Fiori UI to finish its OData GET request peacefully.
  WAIT UP TO 3 SECONDS.

  " 2. The Nuclear Option: Physically delete the record from the DB
  DELETE FROM /plce/tpdsrv WHERE service_uuid = iv_serviceuuid.
  
  " Note: If you need to delete the Service Tasks too, add them here:
  DELETE FROM /plce/tpdsrvtsk WHERE service_uuid = iv_serviceuuid.

  " 3. Commit the deletion
  COMMIT WORK.

ENDFUNCTION.
