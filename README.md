Fields (match your JSON / action param):

TECHKEY type CHAR32 (or CHAR10 if you use “1”, “2”)

MATNR type MATNR

MENGE type KWMENG (or QUAN with unit reference)

MEINS type MEINS

(That’s enough for your mapping.)

✅ Name suggestion:

Structure: ZLANF_POS_IN_S

2) Create a DDIC table type ZLANF_POS_IN_TT

(SE11 → Data Type → Table Type)

Line type: ZLANF_POS_IN_S

Table type: Standard table

Key: Default key (or TECHKEY)

✅ Name suggestion:

Table Type: ZLANF_POS_IN_TT
