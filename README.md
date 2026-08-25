SM59 → Create → Connection Type G (HTTP Connection to External Server)

Field	Value
RFC Destination	ZBMS_API
Description	BMS API Testsystem
Technical Settings tab:

Field	Value
Target Host	hws-srv17.hws.vvv.vvv-konzern.net
Service No.	80 (or 443 once you move to HTTPS)
Path Prefix	leave empty
Leave the path prefix empty and keep the full path in code. If you set it here, set_request_uri interacts with it in ways that are easy to get wrong.

Logon & Security tab:

Case A: Basic Authentication, user BmsWebApi, the new password
Case B: leave "No Logon" — the credentials go elsewhere (step 4)
SSL: Active when you switch to HTTPS, with the client PSE that trusts the server certificate
Special Options tab: set a timeout (30000 ms is reasonable) so a hanging BMS doesn't block the dialog process.

Then Verbindungstest. You should get an HTTP response, most likely 404 — that's fine, it proves host, port and network path work.
