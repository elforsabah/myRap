1. New table ZWRBEHTYP_BMS (SM30-maintainable, delivery class C, transportable):

Field	Type	Key
SAP_BEH_TYPE	BEHTYPNEU	X
BMS_CONTAINER_TYPE_NO	CHAR20	
BMS_CONTAINER_TYPE_NAME	CHAR60	
2. ZWR_BMS_UPDATE_SERVICE needs an EV_SUBRC export telling the caller whether a row was actually updated, and it must COMMIT WORK (it runs in its own LUW via DESTINATION 'NONE').

3. New messages in Z_MSG_SVR_TOUR_EXT: 023 (Behältertyp nicht gemappt), 024 (Tour nicht lesbar), 025 (Service nicht gefunden), 026 (Bewegungstyp nicht gemappt — replaces the warning at 020), 027 (Auftrag wurde nie freigegeben).
