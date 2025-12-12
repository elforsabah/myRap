
Wenn im IC ein Auftrag erzeugt wird, welcher eine Routeninformation enthält, wird der neu angelegte Service nicht auf eine Tour geschoben, wenn diese bereits an der RA gesendet und aktiv ist.

Im Planning Cokpit wird eine neue Tour, mit neuer Tour ID angelegt. Das ist nicht wünschenswert.

Das bevorzugte Verhalten ist, dass, solange eine Tour noch nicht abgeschlossen ist, die neu angelegten Service direkt auf die geplante, gesendete oder aktive Tour gedroppt werden. Nur wenn der Tourkopf geschlossen (COMPLETED) ist, dann soll ein neuer Tourkopf angelegt werden.

Der neu hinzugefügte Service soll auch direkt an den RA gesendet werden (no dev tbd)
