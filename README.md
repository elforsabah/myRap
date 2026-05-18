Report ZZ_TEST_HTTP2

========================================
=== TEST 1: Authentication           ===
========================================
RESULT  : PASSED
Token length :        380
Token prefix : Bearer
Token (50)   : Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJUb2

========================================
=== TEST 0: Get Service Catalogs     ===
========================================
HTTP Status:        200
RESULT  : PASSED — catalogue retrieved

    Catalogue Response Body
{"data":[]}

========================================
=== TEST 2: POST Minimal Order       ===
========================================
JSON length:        760

HTTP Status:        500
RESULT  : FAILED — 500 BMS internal error
ACTION  : Read response body for BMS error detail.

    TEST 2 Response Body
{"error":{"message":"Der Auftrag '12373434' existiert bereits, konnte aber nicht storniert werden. D
er erneute Import wird abgebrochen.","timestamp":"2026-05-18T07:18:06.1798649+02:00"}}


========================================
=== TEST 3: POST Full Order          ===
========================================
JSON length:      1.584

HTTP Status:        200
RESULT  : FAILED — HTTP       200

    TEST 3 Response Body
(empty response body)

    End of Test
