# Auth

Um vor der Durchführung des Pairing den Log-in zu überprüfen und eine korrekte Signierung des Aufrufs zu ermöglichen, kommen Standard-API-Zugangsdaten zum Einsatz. Diese sind nur für den Aufruf von `/auth/login` gültig.

Parameter | Wert
--------- | ------------
defaultKeyIdent | MjgwNmU2YjMzMTU0ZmE0YWUxMDEzMTM1
defaultKeySecret | MDY2YzM3MWI1ZTU5ZjZjYzJhMmFjYTNjODc0NmZhNTFjOTlm
defaultPassword | ZjZjYzJhMmFjYTNjODc0NmZhNTFjOTlmMmM0MDg3MTQ5MTUx

## Log-in überprüfen

> BEISPIELANFRAGE

```shell
$ curl -G https://api.paymey.com/v2/auth \  
  -u keyIdent:password \  
  -d "email=john@paymey.com" \  
  -d password=password \  
  -d timestamp=1405087445 \  
  -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> BEISPIELRÜCKGABE

```json
{
    "user_id": 1,
    "account_id": 1    
}
```

Zur initialen Ermittlung der `user_id` und der `account_id` müssen die betreffende E-Mail-Adresse und das Passwort übermittelt werden.

### http-Anfrage

`POST https://api.paymey.com.com/v2/auth`

### Argumente

Parameter | Beschreibung
--------- | ------------
email | **required** <br> E-Mail-Adresse des Nutzeraccounts.
password | **required** <br> Passwort des Nutzeraccounts.

### Rückgabewert

Wenn die Zugangsdaten korrekt sind, liefert diese Anfrage die `user_id` und die `account_id` zurück.


## Pairing abschließen

> BEISPIELANFRAGE

```shell
$ curl -G https://api.paymey.com/v2/auth/123 \  
    -X PUT \
    -u keyIdent:password \  
    -d "email=john@paymey.com" \  
    -d timestamp=1405087445 \  
    -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> BEISPIELRÜCKGABE

```json
{    
    "account_id": 1,    
    "email": "john@paymey.com"
}
```

Bestätigt das Pairing des Gerätes und aktiviert das zugehörige Authentifizierungs-Tripel.

### http-Anfrage

`PUT https://api.paymey.com/v2/auth/<ID>`

### Argumente

Parameter | Beschreibung
--------- | -----------
id | **required** <br> Die ID des gewünschten Accounts.  
email | **required** <br> E-Mail-Adresse des Nutzeraccounts.

### Rückgabewerte

Bei Erfolg liefert der Aufruf die `account_id` und die `email` zurück.
