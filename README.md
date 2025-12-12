Der derzeitige technische Ablauf funktioniert so, dass neue Services (Auftragspositionen aus dem IC) nur dann auf eine bestehende Tour gelegt werden, wenn diese Tour sich im Status IN_PLANNING befindet.

Der relevante Codeausschnitt lautet:

select … from /PLCE/R_PDTOUR
where TOURSTATUS = IN_PLANNING


Das bedeutet:

Nur Touren im Status IN_PLANNING werden als „existierende Tour“ betrachtet.

Touren in Statuses wie SENT, DISPATCHED, ACTIVE, STARTED, IN_EXECUTION,… werden ignoriert.

Wenn eine Tour nicht IN_PLANNING ist, dann so tut das System, als gäbe es keine bestehende Tour.

Folge: Die Logik legt eine komplett neue Tour (neue Tour-ID) an und weist den neuen Service dieser neuen Tour zu.

Genau das beobachten wir im Planning Cockpit.
