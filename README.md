Im Moment läuft das technisch so ab: Neue Services (also Auftragspositionen aus dem IC) landen nur dann auf einer bestehenden Tour, wenn diese Tour gerade im Status IN_PLANNING ist.

Im Code sieht das so aus:

select … from /PLCE/R_PDTOUR
where TOURSTATUS = IN_PLANNING

Heißt konkret: Nur Touren mit dem Status IN_PLANNING gelten als „existierende Tour“. Touren mit anderen Status wie SENT, DISPATCHED, ACTIVE, STARTED oder IN_EXECUTION? Die lässt das System komplett außen vor.

Sobald eine Tour nicht mehr IN_PLANNING ist, tut das System einfach so, als gäbe es diese Tour überhaupt nicht mehr.

Was passiert dann? Die Logik legt eine ganz neue Tour mit einer neuen Tour-ID an und hängt den neuen Service da dran.

Genau das sehen wir dann auch im Planning Cockpit.
