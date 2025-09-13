Contents
Header Information
What happened?
Error analysis
Information on where terminated
Source Code Extract
Active Calls/Events
Header Information
Short Text 	Two internal tables are not compatible or convertible.
Runtime Error 	OBJECTS_TABLES_NOT_COMPATIBLE
Program 	ZCL_WR_SERVICE_EXTEND_CALC====CP
Date/Time 	13.09.2025 07:00:13 (System)
User 	PROLOGAEFO (Elvis Forsab)
Client 	110
Host 	vhmuzs4dci_S4D_00
What happened?
Error in the ABAP application program.

The current ABAP program "ZCL_WR_SERVICE_EXTEND_CALC====CP" had to be terminated because it found a
statement that could not be executed.
Error analysis
You attempted to copy two data objects to each other or to compare them
with each other.
This is not possible here because the internal tables concerned are not
compatible or convertible.
Information on where terminated
The termination occurred in ABAP program or include "ZCL_WR_SERVICE_EXTEND_CALC====CP", in "IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE". The
main program was "SAPMHTTP".

In the source code, the termination point is in line 2 of include "ZCL_WR_SERVICE_EXTEND_CALC====CM001".
include "ZCL_WR_SERVICE_EXTEND_CALC====CM001".
Source Code Extract

