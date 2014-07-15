# Fehler

> BEISPIEL RÜCKGABE

```json
{
    "type": "invalid_request_error",
    "message": "Missing required param: paymey_account_id",
    "status_code": 400,
    "error_code": 120000
} 
```

PAYMEY verwendet HTTP Response Codes um Erfolg oder Fehler einer Anfrage zu kennzeichnen.

Im Allgemeinen weisen Codes im Bereich von 2xx auf einen Erfolg hin, der Bereich 4xx kennzeichnet einen Fehler in den angegebenen Informationen (z.B. ein fehlender Parameter, eine Transaktion existiert nicht, usw.), und Fehler im 5xx Bereich signalisieren einen einen Fehler auf den PAYMEY Servern.


Error Code | Bedeutung
---------- | -------
200 | OK -- Alles bestens.
400 | Bad Request -- Liegt meist an einem fehlenden Parameter
401 | Unauthorized -- Fehlende oder falsche Credentials, falsche Signatur oder Account noch nicht freigeschaltet
402 | Request Failed -- Parameter korrekt, aber Anfrage schlug fehl.
404 | Not Found -- Die angeforderte Ressource existiert nicht
429 | Too Many Requests -- Zu viele Zugriffe (noch nicht aktiviert)
500 | Internal Server Error -- Fehler auf der Seite von PAYMEY
503 | Service Unavailable -- Wir sind vorübergehend nicht erreichbar. Bitte versuchen Sie es erneut.


### Attribute

Attribute | Bedeutung
--------- | -------
type | z.B. `invalid_request_error`
message | Die Fehlermeldung im Klartext
status_code | Der HTTP Response Code
error_code | Der PAYMEY Fehler Code
