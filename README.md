Yes — do it in the workbench instead of the running IC.

BSP_WD_CMPWB → component BSP_DLC_FS
Open component BSP_DLC_FS (with enhancement set ZWR_ENHSET, to match what runs)
Views → BSP_DLC_FS/factsheet → tab Configuration
Choose Configuration and pick the existing customer configuration, not New:
Key	Value
Role Key	<DEFAULT>
Component Usage	<DEFAULT>
Object Type	/WATP/EWA_ACCOUNT_FS
Subobject Type	<DEFAULT>
That's the configuration whose context ID is 6A8618F4FC5E92846E22697729E5AD74472EF993 — the one from your F2 popup and from transport S4DK902992.

Edit → select the lower tile → move ZWR_IC_OV_ACCT/Holiday from Available to Assigned → position after Qmel → Save
Same object, same result, and the workbench reads from the database when you open it rather than from a session cache — so this route sidesteps the problem you're hitting entirely.

Take the existing configuration, don't create a new one. A second customer configuration under the same key would compete with the current one and the fact sheet could start resolving to the wrong layout.

If ZWR_IC_OV_ACCT/Holiday is missing in Available here too, then it isn't a cache issue and the row genuinely isn't visible to the framework — at that point check the client on BSPC_DL_FSVASS and run /$SYNC.
