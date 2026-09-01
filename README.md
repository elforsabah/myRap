Good catch — 012, 015, 016 and 018 are already taken by the Entsorgungsanlage logic. Everything BMS-related moves to 019 and up.

Messages to create in Z_MSG_SVR_TOUR_EXT
No	Text
019	Tour &1 (Vorlage &2) hat keine BMS-Kolonne
020	BMS-Schnittstelle nicht konfiguriert oder inaktiv — ZTOUR_BMS_CFG pflegen
021	Auftrag &1 an BMS übermittelt (BMS-Nr. &2)
022	Auftrag &1 abgelehnt: &2
023	Auftrag &1: kein Behältertyp im Entsorgungsauftrag
024	Tour konnte nicht gelesen werden — Freigabe nicht möglich
025	Tour &1: Service zur Zuordnung nicht gefunden
026	Leistungsart &1 nicht gemappt — Auftrag &2, Tour &3 nicht übertragen
027	Auftrag &1 wurde nie an BMS freigegeben — Storno nicht möglich
028	Auftrag &1 hat kein Datum — nicht übertragen
029	Auftrag &1: kein Entsorgungsauftrag gefunden — nicht übertragen
030	Auftrag &1: Kunde &2 hat keine vollständige Adresse
031	Auftrag &1: Storno-Payload konnte nicht erzeugt werden
032	Auftrag &1 im BMS storniert
033	Auftrag &1 konnte nicht storniert werden: &2
Check nothing above 018 is already in use before creating these — your screenshot stops at 020.

touranbmsfreigeben
