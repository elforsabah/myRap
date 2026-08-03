What to open next, in this order
SE11 → /WATP/SCSSDATF (and the table behind CL_GETCONFIG, inside /WATP/CL_BO_CSCABNAVV_BASE). This gives you the full setter registry — whether a function already exists for facility/procedure, and which characteristics they target. Single most informative check.
SE11 → /WATP/SCSBO_CABAVVD — confirm the exact field names for facility and procedure. A hint from the source: /WATP/TCSCABNAVV has a field CABN_AVVWDPLANT, so the facility is likely modelled as a waste disposal plant, which would explain 67 and 7 being different per row.
SE16 → /WATP/TCSCABAVVD filtered on key 0000001042 — see whether 67/RE are stored rows. They will be; this tells you the popup writes there and confirms the defaulting target.
SE24 → /WATP/CL_BO_CSCABNAVV — the method that builds/initialises the AVV detail table. That is where a new row would get RE and a derived plant.
SPRO / SM34 on /WATP/ views around CS_CABNAVV — this add-on is heavily Customizing-driven (implementation switches via /WATP/CL_BO_IMPLEMENTATION=>GETISACTIVE, a config record, a function registry). A default-value Customizing entry may already exist, which would make this a config task with no development.
Ranking
Given how much of this add-on is table-driven, I'd bet on step 5 or step 1 producing the answer. Only if both come back empty does it become a BAdI/implicit enhancement in /WATP/CL_BO_CSCABNAVV — and since /WATP/ is a partner namespace, check SE18 filtered on /WATP/* before assuming you need an access key.

Send me /WATP/SCSBO_CABAVVD's field list and the contents of the setter registry, and I can tell you whether this is Customizing, a second setter function, or an enhancement.
