# Transaktionen

### Der Transaktions QR-Code

Der Text eines typischen Transaktions QR-Codes sieht folgendermaßen aus:

`http://www.paymey.com/qr/100000;10%2C00;EUR;1234;1405355094;2`

Wert | Beispiel | Beschreibung
--------- | ------------ | ------------
url | http://www.paymey.com/qr/ | Fallback URL
amount | 100000 | Zu zahlender Betrag in Cent x 100
display_amount | 10%2C00 | Zu zahlender Betrag in Cent (formatiert, URL Encoded)
currency | EUR | ISO-3 Code der Währung
tan | 1234 | Die TAN der Transaktion
timestamp | 1405355094 | Zeitstempel der Transaktion
receiver_id | 2 | PAYMEY Account ID des Empfängers

### Das Transaktionsobjekt

> BEISPIEL OBJEKT

```json
{
    "id": "123",
    "object": "transaction",
    "created": "1405087308",
    "status": "request",
    "payer_id": "0",
    "payer_user_id": "0",            
    "payer_name": "",
    "receiver_id": "1",
    "receiver_user_id": "1",
    "receiver_name": "Jane Doe",
    "currency": "EUR",
    "amount": 1000,    
    "description": "Ihre Bestellung bei MusterShop",
    "metadata": {
        "order_id": "1234"        
    }
}

```

Wert | Beschreibung
--------- | ------------
id | **string**
object |  **string**, Wert ist "transaction"
created | **timestamp**
status | **string**<br>Status der Transaktion. Mögliche Werte: `request`, `pending`, `approved`, `error`, `failed`
payer_id | **string**<br> PAYMEY Account ID des Zahlenden
payer_user_id | **string** <br> PAYMEY User ID des Zahlenden
payer_name | **string** <br> Name des Zahlenden
receiver_id | **string** <br> PAYMEY Account ID des Empfängers
receiver_user_id | **string** <br> AYMEY User ID des Empfängers
receiver_name | **string** <br> Name des Empfängers
currency | **string** <br> ISO-3 Code der Währung der Transaktion.
amount | **positive integer** <br> Betrag in Cent.
description | **string** <br> 
metadata | **metadata hash** <br> Ein Array aus key/value Paaren, die an die Transaktion angehängt werden. Damit kann z.B. die Bestellnummer gespeichert werden.


## Neue Transaktion anlegen

> BEISPIEL ANFRAGE

```shell
$ curl https://api.paymey.com/v2/transactions \  
  -u keyIdent:password \
  -d amount=1000 \  
  -d currency=EUR \
  -d "description=Ihre Bestellung bei MusterShop" \
  -d "metadata[order_id]=1234" \
  -d paymey_account_id=1 \  
  -d timestamp=1405087445 \  
  -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> BEISPIEL RÜCKGABE

```json
{
    "id": "123",
    "object": "transaction",
    "created": "1405087308",
    "status": "request",
    "payer_id": "0",
    "payer_user_id": "0",            
    "payer_name": "",
    "receiver_id": "1",
    "receiver_user_id": "1",
    "receiver_name": "Jane Doe",
    "currency": "EUR",
    "amount": 1000,    
    "description": "Ihre Bestellung bei MusterShop",
    "metadata": {
        "order_id": "1234"        
    }
}

```

Um eine Transaktion durchzuführe müssen sie zuerst eine Transaktion erzeugen. Dies wird im Status `request` angelegt.

### HTTP Anfrage

`POST https://api.paymey.com.com/v2/transactions`

### Argumente

Parameter | Beschreibung
--------- | ------------
receiver_id | **required** <br> PAYMEY Account ID des Empfängers.
currency |  **required** <br> ISO-3 Code der Währung in der die Transaktion durchgeführt werden soll.
amount | **required** <br> Betrag in Cent
description | **optional** <br> String der die Transaktion beschreibt oder zusätzliche Daten enthält. Wird in der Weboberfläche angezeigt.
metadata | **optional** <br> Ein Array aus key/value Paaren, die an die Transaktion angehängt werden. Damit kann z.B. die Bestellnummer gespeichert werden.

### Rückgabewert

Gibt ein Transaktionsobjekt zurück, wenn die Transaktion angelegt wurde. Gibt einen Fehler zurück, wenn es zu einem Problem kam.


## Anfordern einer Transaktion

> BEISPIEL ANFRAGE

```shell
$ curl -G https://api.paymey.com/v2/transactions/123 \  
  -u keyIdent:password \
  -d timestamp=1405087445 \  
  -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> BEISPIEL RÜCKGABE

```json
{
    "id": "123",
    "object": "transaction",
    "created": "1405087308",
    "status": "request",
    "payer_id": "0",
    "payer_user_id": "0",            
    "payer_name": "",
    "receiver_id": "1",
    "receiver_user_id": "1",
    "receiver_name": "Jane Doe",
    "currency": "EUR",
    "amount": 1000,    
    "description": "Ihre Bestellung bei MusterShop",
    "metadata": {
        "order_id": "1234"        
    }
}

```

Retrieves the details of a charge that has previously been created. Supply the unique charge ID that was returned from your previous request, and Stripe will return the corresponding charge information. The same information is returned when creating or refunding the charge.

### HTTP Anfrage

`GET https://api.paymey.com.com/v2/transactions/<ID>`

### Argumente

Parameter | Beschreibung
--------- | ------------
id | **required** <br> ID der Transaktion

### Rückgabewert

Aktualisiert das spezifizierte Transaktionsobjekt mit den übergebenen Parametern. Alle nicht übergebenen Parameter bleiben unverändert.

Bei dieser Anfrage können nur `description` und  `metadata` verändert werden.

## Aktualisieren einer Transaktion

> BEISPIEL ANFRAGE

```shell
$ curl https://api.paymey.com/v2/transactions/123 \  
    -X PUT \
    -u keyIdent:password \
    -d "description=Neue Beschreibung"
    -d "metadata[order_id]=1234" \
    -d timestamp=1405087445 \  
    -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> BEISPIEL RÜCKGABE

```json
{
    "id": "123",
    "object": "transaction",
    "created": "1405087308",
    "status": "request",
    "payer_id": "0",
    "payer_user_id": "0",            
    "payer_name": "",
    "receiver_id": "1",
    "receiver_user_id": "1",
    "receiver_name": "Jane Doe",
    "currency": "EUR",
    "amount": 1000,    
    "description": "Ihre Bestellung bei MusterShop",
    "metadata": {
        "order_id": "1234"        
    }
}

```

Retrieves the details of a charge that has previously been created. Supply the unique charge ID that was returned from your previous request, and Stripe will return the corresponding charge information. The same information is returned when creating or refunding the charge.

### HTTP Anfrage

`PUT https://api.paymey.com.com/v2/transactions/<ID>`

### Argumente

Parameter | Beschreibung
--------- | ------------
id | **required** <br> ID der Transaktion
description | **optional** <br> String der die Transaktion beschreibt oder zusätzliche Daten enthält. Wird in der Weboberfläche angezeigt.
metadata | **optional** <br> Ein Array aus key/value Paaren, die an die Transaktion angehängt werden. Damit kann z.B. die Bestellnummer gespeichert werden.

### Rückgabewert

Gibt ein Transaktionsobjekt zurück, wenn eine gültige ID übergeben wurde. Gibt einen Fehler zurück, wenn es zu einem Problem kam.

## Bestätigen einer Transaktion

> BEISPIEL ANFRAGE

```shell
$ curl https://api.paymey.com/v2/transactions/123/confirm \  
    -X PUT \
    -u keyIdent:password \
    -d timestamp=1405087445 \  
    -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> BEISPIEL RÜCKGABE

```json
{
    "id": "123",
    "object": "transaction",
    "created": "1405087308",
    "status": "pending",
    "payer_id": "0",
    "payer_user_id": "0",            
    "payer_name": "",
    "receiver_id": "1",
    "receiver_user_id": "1",
    "receiver_name": "Jane Doe",
    "currency": "EUR",
    "amount": 1000,    
    "description": "Ihre Bestellung bei MusterShop",
    "metadata": {
        "order_id": "1234"        
    }
}

```

Retrieves the details of a charge that has previously been created. Supply the unique charge ID that was returned from your previous request, and Stripe will return the corresponding charge information. The same information is returned when creating or refunding the charge.

### HTTP Anfrage

`PUT https://api.paymey.com.com/v2/transactions/<ID>/confirm`

### Argumente

Parameter | Beschreibung
--------- | ------------
id | **required** <br> ID der Transaktion

### Rückgabewert

Gibt ein Transaktionsobjekt zurück, wenn eine gültige ID übergeben wurde. Gibt einen Fehler zurück, wenn es zu einem Problem kam.

## Liste alle Transaktionen

> BEISPIEL ANFRAGE

```shell
$ curl -G https://api.paymey.com/v2/transactions \  
  -u keyIdent:password \  
  -d paymey_account_id=2 \  
  -d timestamp=1405087445 \  
  -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> BEISPIEL RÜCKGABE

```json
{
    "object": "list",
    "url": "/v2/transactions",
    "data": [
        {
            "id": "464",
            "object": "transaction",
            "created": "1405087308",
            "status": "pending",
            "payer_id": "1",
            "payer_user_id": "1",            
            "payer_name": "John Doe",
            "receiver_id": "2",
            "receiver_user_id": "2",
            "receiver_name": "Jane Doe",
            "currency": "EUR",
            "amount": 100,
            "fee": 0,
            "net": 100,
            "description": "",
            "metadata": {
                "key1": "value1",
                "key2": "value2",
                "key3": "value3"
            }
        }
    ]
}
```

Gibt eine Liste aller Transaktionen zurück, an der `paymey_account_id` als Sender oder Empfänger beteiligt war. Die Transaktionen sind sortiert, beginnend mit der neuesten.


### HTTP Anfrage

`GET https://api.paymey.com/v2/transactions?paymey_account_id=1`

### Argumente

Parameter | Beschreibung
--------- | ------------
paymey_account_id | **required** <br> Die ID des gewünschten PAYMEY Accounts  
limit | **optional** - Default ist 10 <br> Die Anzahl der Objekte die maximal zurück gegeben wird. Das Limit darf zwischen 1 und 100 liegen.  
starting_after | **optional**<br>Durch `starting_after` wird die Objekt ID definiert, nach der die zurückgegebenen Elemente in der Liste beginnen. Wenn Sie beispielsweise eine Liste mit 10 Objekten anfordern, und diese endet mit `10`, dann können Sie durch Verwendung von `starting_after=10` die folgende Seite der Liste anfordern.  
ending_before | **optional**<br>Durch `ending_before` wird die Objekt ID definiert, vor der die zurückgegebenen Elemente in der Liste beginnen. Wenn Sie beispielsweise eine Liste mit 10 Objekten anfordern, und diese beginnt mit `10`, dann können Sie durch Verwendung von `ending_before=10` die vorhergehende Seite der Liste anfordern.

### Rückgabewert

Ein Objekt mit einem `data` Wert, der ein Array aller Transaktionen enthält, an der `paymey_account_id` als Sender oder Empfänger beteiligt war, beginnend mit der Transaktion nach der Transaktion in `starting_after`. Jedes Element in diesem Array ist ein eigenes Transaktionsobjekt. Wenn keine weiteren Transaktionen existieren, wird ein leeres Array zurückgegeben.



