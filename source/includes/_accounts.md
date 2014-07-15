# Accounts

## Liste alle PAYMEY Accounts

> BEISPIEL ANFRAGE

```shell
$ curl -G https://api.paymey.com/v2/accounts/123 \  
  -u keyIdent:password \  
  -d timestamp=1405087445 \  
  -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> BEISPIEL RÜCKGABE

```json

{
    "object": "list",
    "url": "/v2/accounts",
    "account_id": 123,
    "data": [
        {
            "id": "1",
            "object": "paymey_account",
            "created": "1382693942",
            "name": "Account name",
            "stat_name": "Accounts stat name",
            "balance": "12,34",
            "currency": "EUR",
            "status": "2"
        }
    ]
}
```

Gibt eine Liste aller zu einem Account zugehörigen PAYMEY Accounts zurück. Die Accounts sind sortiert, beginnend mit dem neuesten.


### HTTP Request

`GET https://api.paymey.com/v2/accounts/<ID>`

### Argumente

Parameter | Description
--------- | -----------
id | **required** <br> Die ID des gewünschten Accounts  
limit | **optional** - Default ist 10 <br> Die Anzahl der Objekte die maximal zurück gegeben wird. Das Limit darf zwischen 1 und 100 liegen.  
starting_after | **optional**<br>Durch `starting_after` wird die Objekt ID definiert, nach der die zurückgegebenen Elemente in der Liste beginnen. Wenn Sie beispielsweise eine Liste mit 10 Objekten anfordern, und diese endet mit `10`, dann können Sie durch Verwendung von `starting_after=10` die folgende Seite der Liste anfordern.  
ending_before | **optional**<br>Durch `ending_before` wird die Objekt ID definiert, vor der die zurückgegebenen Elemente in der Liste beginnen. Wenn Sie beispielsweise eine Liste mit 10 Objekten anfordern, und diese beginnt mit `10`, dann können Sie durch Verwendung von `ending_before=10` die vorhergehende Seite der Liste anfordern.

### Rückgabewerte

Ein Objekt mit einem `data` Wert, der ein Array aller PAYMEY Accounts enthält, die zum Account mit der ID <ID> gehören, beginnend mit dem Account nach der Account in `starting_after`. Jedes Element in diesem Array ist ein eigenes PAYMEY Account Objekt. Wenn keine weiteren PAYMEY Accounts existieren, wird ein leeres Array zurückgegeben.
