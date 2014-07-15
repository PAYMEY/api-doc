# Metadaten

> BEISPIEL ANFRAGE

```shell
$ curl https://api.paymey.com/v2/transactions \  
  -u keyIdent:password \
  -d amount=1000 \  
  -d currency=EUR \
  -d "description=1 Paar Sportschuhe, Bestellnr. XY-1234" \
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
    "description": "1 Paar Sportschuhe, Bestellnr. XY-1234",
    "metadata": {
        "order_id": "1234"        
    }
}

```

PAYMEY bietet bei einigen Objekten den `metadata` Parameter an.

Dieser Parameter kann dazu benutzt werden, Key-Value Paare an das Objekt zu hängen. Dadurch können Anwender-spezifische Informationen hinzugefügt werden, beispielweise die Bestellnummer zu einer Transaktion.

Einige Objekte bieten zusätzlich einen `description` Parameter. Dieser kann z.B. dazu benutzt werden um eine Klartext-Beschreibung zu einer Transaktion hinzuzufügen ("1 Paar Sportschuhe, Bestellnr. XY-1234")

<aside class="notice">
Metadata kann bis zu 10 Werte-Paare pro Objekt speichern.
</aside>
