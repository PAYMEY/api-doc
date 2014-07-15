# Paginierung

> BEISPIEL AUFRUF

```shell
$ curl "https://api.paymey.com/v2/transactions"
  -H "Authorization: meowmeowmeow"
```

> BEISPIEL RÜCKGABE

```json
{
    "type": "invalid_request_error",
    "message": "Missing required param: paymey_account_id",
    "status_code": 400,
    "error_code": 120000
} 
```

Alle Entitäten die eine "list" API Methode anbieten, arbeiten nach dem selben Schema.

Im Allgemeinen werden Listen sortiert zurückgegeben, wobei mit dem neuesten Element begonnen wird.

PAYMEY verwendet hier eine Cursor-basierte Paginierung. Durch die Verwendung der Parameter `starting_after` und `ending_before` können Sie steuern welchen Teil der Liste Sie sehen möchten.

Parameter | Bedeutung
--------- | -------
limit | **optional**<br>Limitiert die Anzahl der zurückgegebenen Elemente. Das Limit kann zwischen 1 und 100 gesetzt werden.
starting_after | **optional**<br>Durch `starting_after` wird die Objekt ID definiert, nach der die zurückgegebenen Elemente in der Liste beginnen. Wenn Sie beispielsweise eine Liste mit 10 Objekten anfordern, und diese endet mit `10`, dann können Sie durch Verwendung von `starting_after=10` die folgende Seite der Liste anfordern.
ending_before | **optional**<br>Durch `ending_before` wird die Objekt ID definiert, vor der die zurückgegebenen Elemente in der Liste beginnen. Wenn Sie beispielsweise eine Liste mit 10 Objekten anfordern, und diese beginnt mit `10`, dann können Sie durch Verwendung von `ending_before=10` die vorhergehende Seite der Liste anfordern.
