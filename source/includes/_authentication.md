# Authentifizierung  

PAYMEY verwendet ein Triple aus KeyIdent, KeySecret und API-Passwort, wobei KeyIdent und Passwort für die HTTP-Authentifizierung verwendet werden und das KeySecret zur Signierung der einzelnen Anfragen.

## HTTP-Authentifizierung

```shell
$ curl https://api.paymey.com/v2/... \      
    -u keyIdent:password \    
    -d timestamp=1405087445 \  
    -d signature=ZGIwMGQ0NDVjNmQ5NjRhMTgx...
```

> Ersetzen Sie `keyIdent` und `password` mit Ihren Zugangsdaten.

Die [HTTP-Authentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication#Client_side) wird wie folgt konstruiert:

1. KeyIdent und Passwort werden zu einem String zusammengesetzt "KeyIdent:Passwort"
2. Dieser String wird Base64 encodiert.
3. Die Authenifiezierungsmethode wird mit einem Leerzeichen dem encodierten String vorangestellt.

Beispiel:

`Authorization: Basic WVdVeFl6ZzNaVGN4TUdWa01q...`

<aside class="notice">
    Beim Aufruf von `/auth/login` vor der Durchführung des Pairings kommen Standard-API-Zugangsdaten zum Einsatz.
</aside>

## Signatur

### Zur Signierung werden folgende Daten benötigt:

* **Endpunkt** - Der Host der HTTP-Anfrage
* **API-Aktion** - Die Aktion der API welche aufgerufen werden soll
* **Parameter** - Die erforderlichen und optionalen Parameter des Aufrufs
* **Zeitstempel** - Der Zeitpunkt zu dem der Aufruf durchgeführt wird im Unix-Timestamp-Format.
* **Signatur** - Dieser Wert muss berechnet werden und bestätigt, dass der Aufruf valide ist und nicht verändert wurde.

Beispiel:

`https://api.paymey.com/v2/transactions?paymey_account_id=1&timetamp=1404989965 &signature=ZTYyYzZkNDk0MjJmYjcyOGRlMzI1YjM5OGNkNzhiZGY1YjdjNzcxMzViZTU1ZjFiZmQyZTg0ZWVhNGI5Mzg3Zg%3D%3D`


### Formatierung der Anfrage

1. Beginnen Sie mit der Anfragemethode (GET, POST, PUT oder DELETE) gefolgt von einem Zeilenumbruch.  
`GET\n`

2. Danach folgt der http-Host:  
`https://api.paymey.com/\n`

3. Fügen Sie nun die API-Aktion hinzu:  
`/v2/transactions\n`

4. Fügen Sie die Anfrage Parameter (die Name/Value-Paare ohne vorangestelltes `?` und URL-encoded) in lexikographischer Ordnung hinzu (lexikographische Ordnung beachtet Groß- und Kleinschreibung).  Die Parameternamen werden dabei durch das Gleichzeichen `=` von ihren Werten getrennt. Die jeweiligen Paare aus Parameter und Wert werden durch das Und-Zeichen `&` verbunden.  
`paymey_account_id=1&timetamp=1404989965`

5. Der zu signierende String sieht dann folgendermaßen aus:  
`GET\n`  
`https://api.paymey.com/\n`  
`/v2/transactions\n`  
`paymey_account_id=1&timetamp=1404989965`  

### Berechnung der Signatur

Nachdem Sie den Anfrage-String erstellt haben, berechnen Sie die Signatur, indem Sie einen Hash-basierten Message Authentication Code (HMAC) erzeugen, wobei das **HMAC-SHA256** Protokoll und das **KeySecret** verwendet wird.

Die resultierende Signatur muss Base-64 encodiert und danach URL-encodiert werden.

**Pseudo-Code**

`hash = hash_hmac('sha256', requestString, keySecret);`  
`base64 = base64_encode(hash);`  
`signature = url_encode(base64);`  

Das Ergebnis wird dann zur Anfrage hinzugefügt.

## Gerätepairing

Um das Triple aus KeyIdent, KeySecret und API-Passwort auf das Smartphone zu bekommen, verwendet PAYMEY einen QR-Code der die Daten enthält.

Dieser kann in der [PAYMEY-Webapp](https://webapp.paymey.com) (bzw. der [Sandboxanwendung](http://webapp.sandbox.pmydev.com)) generiert werden. 
Loggen Sie sich dazu ein und navigieren Sie zu "Geräte > Neue Geräte hinzufügen", vergeben Sie einen Namen für das Gerät und klicken Sie auf "Speichern".

Der QR-Code enthält folgende Daten in der Dargestellten Reihenfolge und jeweils durch ein Semikolon `;` getrennt.

Pos | Parameter | Beschreibung
--------- | ------- | -----------
1 | **account_id** |  Die PAYMEY-Account-ID zu der das Zugriffstripple gehört
2 | **user_id** |  Die ID des Benutzers bei PAYMEY
3 | **email** |  Die E-Mail-Addresse des zugehörigen PAYMEY-Accounts
4 | **name** |  Vorname und Nachname des Benutzers
5 | **key_ident** |  KeyIdent
6 | **key_secret** |  KeySecret
7 | **passwort** |  Das API-Passwort


