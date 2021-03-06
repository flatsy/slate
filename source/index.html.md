---
title: Flatsy API
language_tabs:
  - shell: Shell
  - http: HTTP
  - javascript: JavaScript
  - javascript--nodejs: Node.JS
  - ruby: Ruby
  - python: Python
  - java: Java
  - go: Go
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---
<h1 id="Introduction">Introduction</h1>

L'API Business Flatsy permet de piloter l'essemble des opérations :

  - Pilotage des biens (disponibilités, informations, mise en pause, fin des visites)
  - Pilotage des visites (consultation du planning des visites, modification des infos visiteur, demande de visite, annulation)
  - Webhooks pour être notifié des évènements liés à votre compte

Base URLs:

* <a href="https://www.flatsy.fr/">https://www.flatsy.fr/</a>

Swagger-UI :

* <a href="https://www.flatsy.fr/assets/lib/swagger-ui/index.html?url=https://www.flatsy.fr/swagger-public.json#/">Swagger UI (permet de tester les requêtes API)</a>


# Authentification

L'authentification se fait via un token JWT. Toutes les méthodes de l'API nécessitent une authentification. L'obtention du token se fait via un appel à l' [endpoint de login](#opIduserCredsLogin) avec votre email/password Flatsy.
Le token expire au bout de 30 jours. Il est nécessaire de le renouveler avant l'expiration ou de mettre en place une stratégie de réauthentification si vous recevez des réponses HTTP 401.

Ce token doit être inclus dans le header **X-Auth-Token** afin de vous authentifier pour chacun des appels API.

* API Key (api_key)
    - Parameter Name: **X-Auth-Token**, in: header.


# Environnement de test

Un environnement de test pour l'API est disponible via l'adresse **test.flatsy.fr**

La création d'un compte test est possible à l'URL : <a href="https://test.flatsy.fr/pro/signup" target="\_blank">https://test.flatsy.fr/pro/signup</a>

Si vous souhaitez utiliser une adresse de webhook, vous pourrez alors immédiatement saisir un webhook de test à l'adresse <a href="https://test.flatsy.fr/user/edit" target="\_blank">https://test.flatsy.fr/user/edit</a> une fois connecté.

*NB : en production, l'activation du webhook se fait par demande expresse à l'équipe Flatsy.*

Cet environnement vous permet de tester l'API, et est particulièrement utile pour tester les webhooks.

Le fonctionnement est identique à la production tel que mentionné dans cette documentation.

Toute le cycle de vie des biens et des visites est en revanche géré de façon automatique. A la création d'un bien, un agent de visite spécifique est immédiatement créé et affecté au bien. Cet agent est disponible tout le temps pour des visites.

Pour piloter les visites et générer des évènements automatiquement envoyés sur le webhook, il s'agit d'entrer des numéros de téléphone spéciaux pour le visiteur lors de la création de la visite. Cela permet d'envoyer les évènements désirés sur le webhook :

|Numéro de téléphone visiteur|Comportement|
|---|---|
|0600000000|La visite est immédiatement confirmée par l'agent de visite.|
|0600000001|La visite est immédiatement confirmée puis réalisée par l'agent de visite, un compte rendu est ensuite rédigé.|
|0600000002|La visite est immédiatement décalée au créneau disponible suivant par l'agent de visite|
|0600000010|La visite est annulée par l'agent de visite|
|0600000011|La visite est annulée par le visiteur|




<h1 id="Flatsy-API-businessuser">User</h1>

## me

<a id="opIdself"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /www.flatsy.fr/api/v1/business/me \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /www.flatsy.fr/api/v1/business/me HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/me',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/me',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get 'https://wwww.flatsy.fr/api/v1/business/me',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('https://wwww.flatsy.fr/api/v1/business/me', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/me");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/www.flatsy.fr/api/v1/business/me", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/me`

*Permet d'obtenir des informations sur votre compte client, peut également être utilisée pour contrôler la validité du token d'authentification*

> Example responses

> 200 Response

<h3 id="self-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Client](#schemaclient)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## login

<a id="opIduserCredsLogin"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /www.flatsy.fr/api/v1/business/login \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /www.flatsy.fr/api/v1/business/login HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/login',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "identifier": "string",
  "password": "string"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/login',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post 'https://wwww.flatsy.fr/api/v1/business/login',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('https://wwww.flatsy.fr/api/v1/business/login', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/login");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "/www.flatsy.fr/api/v1/business/login", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/login`

*Permet d'obtenir votre token d'authentification à l'API Flatsy. Le token expire au bout de 30 jours. Il est nécessaire de le renouveler avant l'expiration ou de mettre en place une stratégie de réauthentification si vous recevez des réponses HTTP 401.*

> Body parameter

```json
{
  "identifier": "string",
  "password": "string"
}
```

<h3 id="usercredslogin-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Credentials](#schemacredentials)|true|credentials|

> Example responses

> 200 Response

<h3 id="usercredslogin-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[LoginResponse](#schemaloginresponse)|


<h1 id="Flatsy-API-business">Property</h1>

## properties

<a id="properties"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /www.flatsy.fr/api/v1/business/properties \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /www.flatsy.fr/api/v1/business/properties HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/properties',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/properties',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get 'https://wwww.flatsy.fr/api/v1/business/properties',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('https://wwww.flatsy.fr/api/v1/business/properties', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/properties");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/www.flatsy.fr/api/v1/business/properties", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/properties`

*Restitue toutes les propriétés connues de Flatsy pour votre compte client*

> Example responses

> 200 Response

<h3 id="ownerproperties-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|Inline|

<h3 id="ownerproperties-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[Property](#schemaproperty)]|false|none|none|
|» id|string(uuid)|true|none|none|
|» userId|string(uuid)|true|none|none|
|» data|[PropertyData](#schemapropertydata)|true|none|none|
|»» rentSell|string|false|none|Property rent type|
|»» criterias|[Criterias](#schemacriterias)|false|none|none|
|»»» sharingAllowed|boolean|false|none|none|
|»»» prequalification|[Prequalification](#schemaprequalification)|false|none|none|
|»»»» _type|string|true|none|none|
|»» price|number(double)|false|none|Property rent type|
|»» propertyStatus|string|false|none|Property status|
|»» propertyStatusUpdatedAt|string(date-time)|false|none|none|
|»» extUrl|string|false|none|none|
|»» address|[Address](#schemaaddress)|false|none|none|
|»»» address|string|true|none|none|
|»»» zip|string|true|none|none|
|»»» city|string|true|none|none|
|»»» countryCode|string|true|none|none|
|»»» latitude|number|true|none|none|
|»»» longitude|number|true|none|none|
|»»» doorInfo|string|false|none|none|
|»» doorRef|string|false|none|none|
|»» memo|string|false|none|none|
|»» picHandles|[string]|false|none|none|
|»» picUrls|[string]|false|none|none|
|»» fields|[PropertyFields](#schemapropertyfields)|false|none|none|
|»»» surface|number|false|none|none|
|»»» additionalSurfaces|[[Surface](#schemasurface)]|false|none|additionalSurfaces|
|»»»» name|string|true|none|none|
|»»»» surface|number|true|none|none|
|»»» numRooms|integer(int32)|false|none|numRooms|
|»»» numBedrooms|integer(int32)|false|none|numBedrooms|
|»»» orientation|string|false|none|none|
|»»» text|string|false|none|none|
|»»» deposit|number|false|none|none|
|»»» monthlyCharges|number|false|none|none|
|»»» agencyFee|number|false|none|none|
|»»» availableFrom|string|false|none|none|
|»»» floor|integer(int32)|false|none|floor number|
|»»» heating|string|false|none|none|
|»»» cave|boolean|false|none|cave|
|»»» keeper|boolean|false|none|keeper|
|»»» parking|boolean|false|none|parking|
|»»» furnished|boolean|false|none|furnished|
|»»» leaseDurationYears|integer(int32)|false|none|leaseDurationYears|
|»»» works|string|false|none|none|
|»»» dpe|string|false|none|none|
|»»» constructionYear|integer(int32)|false|none|constructionYear|
|»» visitAvailabilities|[[Interval](#schemainterval)]|false|none|availabilities|
|»»» start|string(date-time)|true|none|none|
|»»» end|string(date-time)|true|none|none|
|»»» isWeeklyRecurring|boolean|false|none|none|
|»» externalRef|string|false|none|none|
|»» additionalRefs|[string]|false|none|none|
|»» pickedVisitorId|string|false|none|none|
|»» tenantPhone|string|false|none|none|
|»» extraFields|object|false|none|extraFields|
|»» metadata|object|false|none|metadata|
|» createdAt|string(date-time)|false|none|none|
|» updatedAt|string(date-time)|false|none|none|
|» ownerContact|[Contact](#schemacontact)|true|none|none|
|»» name|string|true|none|none|
|»» phone|string|false|none|none|
|»» email|string|false|none|none|
|»» pictureUrl|string|false|none|none|
|»» displayAddress|string|false|none|none|
|»» phone2|string|false|none|none|
|»» businessHours|string|false|none|none|
|» lastViewingAt|string(date-time)|false|none|none|
|» inCharge|string|false|none|none|
|» viewingsPlanned|number|false|none|none|
|» viewingsDone|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|rentSell|property.basics.rentsell.rent|
|rentSell|property.basics.rentsell.sell|
|propertyStatus|property.basics.status.available|
|propertyStatus|property.basics.status.unavailable|
|propertyStatus|property.basics.status.standby|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## saveProperty

<a id="opIdsaveProperty"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://www.flatsy.fr/api/v1/business/properties \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST https://www.flatsy.fr/api/v1/business/properties HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://www.flatsy.fr/api/v1/business/properties',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "inCharge": "string",
  "rentSell": "property.basics.rentsell.rent",
  "criterias": {
    "sharingAllowed": true,
    "prequalification": {
      "_type": "string"
    }
  },
  "price": 0,
  "extUrl": "string",
  "address": {
    "address": "string",
    "zip": "string",
    "city": "string",
    "countryCode": "string",
    "latitude": 0,
    "longitude": 0,
    "doorInfo": "string"
  },
  "doorRef": "string",
  "picUrls": [
    "string"
  ],
  "fields": {
    "surface": 0,
    "additionalSurfaces": [
      {
        "name": "string",
        "surface": 0
      }
    ],
    "numRooms": 0,
    "numBedrooms": 0,
    "orientation": "string",
    "text": "string",
    "deposit": 0,
    "monthlyCharges": 0,
    "agencyFee": 0,
    "availableFrom": "string",
    "floor": 0,
    "heating": "string",
    "cave": true,
    "keeper": true,
    "parking": true,
    "furnished": true,
    "leaseDurationYears": 0,
    "works": "string",
    "dpe": "string",
    "constructionYear": 0
  },
  "externalRef": "string",
  "additionalRefs": [
    "string"
  ],
  "tenantPhone": "string",
  "extraFields": {}
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://www.flatsy.fr/api/v1/business/properties',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post 'https://www.flatsy.fr/api/v1/business/properties',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('https://www.flatsy.fr/api/v1/business/properties', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("https://www.flatsy.fr/api/v1/business/properties");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://www.flatsy.fr/api/v1/business/properties", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/properties`

*Permet de créer ou de mettre à jour un bien. A noter que les disponibilités de visite du bien seront par défaut à toute la semaine de 7h à 21h. Elles peuvent être modifiées via l'endpoint dédié.*

*Les critères de préqualification doivent être absents ou renseignés conformément à [Préqualification](#schemaprequalification)*

*Il est possible de passer des metadonnées sous forme de clés/valeurs à ce bien via l'attribut metadata, qui pourront être retrouvées par la suite*

> Body parameter

```json
{
  "inCharge": "string",
  "rentSell": "property.basics.rentsell.rent",
  "criterias": {
    "sharingAllowed": true,
    "prequalification": {
      "_type": "string"
    }
  },
  "price": 0,
  "extUrl": "string",
  "address": {
    "address": "string",
    "zip": "string",
    "city": "string",
    "countryCode": "string",
    "latitude": 0,
    "longitude": 0,
    "doorInfo": "string"
  },
  "doorRef": "string",
  "picUrls": [
    "string"
  ],
  "fields": {
    "surface": 0,
    "additionalSurfaces": [
      {
        "name": "string",
        "surface": 0
      }
    ],
    "numRooms": 0,
    "numBedrooms": 0,
    "orientation": "string",
    "text": "string",
    "deposit": 0,
    "monthlyCharges": 0,
    "agencyFee": 0,
    "availableFrom": "string",
    "floor": 0,
    "heating": "string",
    "cave": true,
    "keeper": true,
    "parking": true,
    "furnished": true,
    "leaseDurationYears": 0,
    "works": "string",
    "dpe": "string",
    "constructionYear": 0
  },
  "externalRef": "string",
  "additionalRefs": [
    "string"
  ],
  "tenantPhone": "string",
  "extraFields": {},
  "metadata": {}
}
```

<h3 id="saveProperty-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[PropertySaveRequest](#schemapropertysaverequest)|true|property save request|

> Example responses

> 200 Response

<h3 id="saveProperty-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Property](#schemaproperty)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
api_key
</aside>

## agentByPropertyRef

<a id="opIdagentByPropertyRef"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /test.flatsy.fr/api/v1/business/property/agent \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /test.flatsy.fr/api/v1/business/property/agent HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: '/test.flatsy.fr/api/v1/business/property/agent',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('/test.flatsy.fr/api/v1/business/property/agent',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get '/test.flatsy.fr/api/v1/business/property/agent',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('/test.flatsy.fr/api/v1/business/property/agent', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/test.flatsy.fr/api/v1/business/property/agent");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/test.flatsy.fr/api/v1/business/property/agent", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/property/agent`

*Renvoie les informations sur l'agent en charge de ce bien*

<h3 id="agentbypropertyref-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|ref|query|string|false|none|

> Example responses

> 200 Response

<h3 id="agentbypropertyref-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[AgentInfo](#schemaagentinfo)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
api_key
</aside>

## sendmail

<a id="opIdsendViewingMail"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /www.flatsy.fr/api/v1/business/sendmail/{ref} \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /www.flatsy.fr/api/v1/business/sendmail/{ref} HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/sendmail/{ref}',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "email": "string",
  "firstName": "string",
  "lastName": "string",
  "phone": "string",
  "fields": {}
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/sendmail/{ref}',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post 'https://wwww.flatsy.fr/api/v1/business/sendmail/{ref}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('https://wwww.flatsy.fr/api/v1/business/sendmail/{ref}', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/sendmail/{ref}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "/www.flatsy.fr/api/v1/business/sendmail/{ref}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/sendmail/{ref}`

*Permet d'envoyer un email d'invitation à prendre RDV à un visiteur pour un bien.*

> Body parameter

```json
{
  "email": "string",
  "firstName": "string",
  "lastName": "string",
  "phone": "string",
  "fields": {}
}
```

<h3 id="sendviewingmail-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|ref|path|string|true|property reference|
|body|body|[Visitor](#schemavisitor)|true|visitor info|

> Example responses

> 200 Response

<h3 id="sendviewingmail-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|string|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>






## property

<a id="opIdownerProperty"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /www.flatsy.fr/api/v1/business/property \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /www.flatsy.fr/api/v1/business/property HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/property',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/property',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get 'https://wwww.flatsy.fr/api/v1/business/property',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('https://wwww.flatsy.fr/api/v1/business/property', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/property");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/www.flatsy.fr/api/v1/business/property", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/property`

*Permet d'obtenir un bien en saisissant sa référence. Les informations renvoyés contiennent notamment le statut du bien (en ligne, terminé, en pause), ainsi que les contraintes de disponibilités pour ce bien (à ne pas confondre avec les disponibilités de visite, qui sont obtenues via l'endpoint [slots](#opIdslots))*

<h3 id="ownerpropertybyref-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|ref|query|string|false|none|

> Example responses

> 200 Response

<h3 id="ownerpropertybyref-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Property](#schemaproperty)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## propertyAvailability

<a id="opIdupdatePropertyAvailability"></a>

> Code samples

```shell
# You can also use wget
curl -X PUT /www.flatsy.fr/api/v1/business/property/availability?ref=18%2F1280%2F02%2F0227 \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
PUT /www.flatsy.fr/api/v1/business/property/availability?ref=18%2F1280%2F02%2F0227 HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/property/availability',
  method: 'put',
  data: '?ref=18%2F1280%2F02%2F0227',
  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "availability": [
    {
      "start": "2018-07-09T09:05:00Z",
      "end": "2018-07-09T09:05:00Z",
      "isWeeklyRecurring": true
    }
  ]
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/property/availability?ref=18%2F1280%2F02%2F0227',
{
  method: 'PUT',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.put 'https://wwww.flatsy.fr/api/v1/business/property/availability',
  params: {
  'ref' => 'string'
}, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.put('https://wwww.flatsy.fr/api/v1/business/property/availability', params={
  'ref': '18/1280/02/0227'
}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/property/availability?ref=18%2F1280%2F02%2F0227");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("PUT", "/www.flatsy.fr/api/v1/business/property/availability", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`PUT /api/v1/business/property/availability`

*Met à jour les disponibilités d'une propriété*

L'objet interval représente un intervalle de temps de disponibilité pour un agent ou pour un bien.
Le booleen isWeeklyRecurring indique si ce créneau se reporte d'une semaine sur l'autre à partir de sa date de début.


> Body parameter

```json
{
  "availability": [
    {
      "start": "2018-07-09T09:05:00Z",
      "end": "2018-07-09T09:05:00Z",
      "isWeeklyRecurring": true
    }
  ]
}
```

<h3 id="updatepropertyavailability-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|ref|query|string|true|property reference|
|body|body|[AvailabilityUpdateRequest](#schemaavailabilityupdaterequest)|true|availability update request|

> Example responses

> 200 Response

<h3 id="updatepropertyavailability-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Property](#schemaproperty)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## updatePropertyStatus

<a id="opIdupdatePropertyStatus"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /www.flatsy.fr/api/v1/business/property/{ref}/status \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /www.flatsy.fr/api/v1/business/property/{ref}/status HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/property/{ref}/status',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "propertyStatus": "available"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/property/{ref}/status',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post 'https://wwww.flatsy.fr/api/v1/business/property/{ref}/status',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('https://wwww.flatsy.fr/api/v1/business/property/{ref}/status', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/property/{ref}/status");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "/www.flatsy.fr/api/v1/business/property/{ref}/status", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/property/{ref}/status`

*Permet de rétablir les visites pour un bien en passant le champ "propertyStatus" à  "available".
Permet de mettre un bien en pause (empêcher temporairement la réservation de visites) en passant le champ "propertyStatus" à "standby".*

> Body parameter

```json
{
  "propertyStatus": "available"
}
```

<h3 id="updatepropertystatus-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|ref|path|string|true|property reference|
|body|body|[PropertyStatusChangeRequest](#schemapropertystatuschangerequest)|true|status change request|

> Example responses

> 200 Response

<h3 id="updatepropertystatus-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Property](#schemaproperty)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## terminateProperty

<a id="opIdterminateProperty"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /www.flatsy.fr/api/v1/business/property/{ref}/terminate \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /www.flatsy.fr/api/v1/business/property/{ref}/terminate HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/property/{ref}/terminate',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "pickedVisitorEmail": "string"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/property/{ref}/terminate',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post 'https://wwww.flatsy.fr/api/v1/business/property/{ref}/terminate',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('https://wwww.flatsy.fr/api/v1/business/property/{ref}/terminate', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/property/{ref}/terminate");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "/www.flatsy.fr/api/v1/business/property/{ref}/terminate", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/property/{ref}/terminate`

*Permet de mettre fin aux visites d'un bien. Saisir un email visiteur permet de prévenir uniquement les autres visiteurs des visites passées et futures que le bien a trouvé preneur.*

> Body parameter

```json
{
  "pickedVisitorEmail": "string"
}
```

<h3 id="terminateproperty-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|ref|path|string|true|property reference|
|body|body|[PropertyTerminateRequest](#schemapropertyterminaterequest)|true|terminate viewings request|

> Example responses

> 200 Response

<h3 id="terminateproperty-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Property](#schemaproperty)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## coverage

<a id="opIdcoverage"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /test.flatsy.fr/api/v1/business/coverage \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /test.flatsy.fr/api/v1/business/coverage HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: '/test.flatsy.fr/api/v1/business/coverage',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('/test.flatsy.fr/api/v1/business/coverage',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get '/test.flatsy.fr/api/v1/business/coverage',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('/test.flatsy.fr/api/v1/business/coverage', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/test.flatsy.fr/api/v1/business/coverage");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/test.flatsy.fr/api/v1/business/coverage", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/coverage`

*Permet d'obtenir une liste de codes postaux où Flatsy est présent. Il est possible de confier des visites à Flatsy sur ls codes postaux envoyés par cette méthode. Pour tout autre secteur, veuillez vous adresser à l'équipe Flatsy (team@flatsy.fr).*

> Example responses

> 200 Response

<h3 id="coverage-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|Inline|

<h3 id="coverage-responseschema">Response Schema</h3>

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

<h1 id="Flatsy-API-businessmessage">Messages</h1>

La messagerie fonctionne bien par bien. Chaque bien donne lieu à une conversation entre l'utilisateur qui confie un bien et les agents de visite qui interviennent sur le bien.

<a id="opIdbusinessMessages"></a>

## messages

> Code samples

```shell
# You can also use wget
curl -X GET /test.flatsy.fr/api/v1/business/messages \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /test.flatsy.fr/api/v1/business/messages HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: '/test.flatsy.fr/api/v1/business/messages',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('/test.flatsy.fr/api/v1/business/messages',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get '/test.flatsy.fr/api/v1/business/messages',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('/test.flatsy.fr/api/v1/business/messages', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/test.flatsy.fr/api/v1/business/messages");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/test.flatsy.fr/api/v1/business/messages", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/messages`

*Renvoie tous les messages à propos de tous les biens confiés à Flatsy*

> Example responses

> 200 Response

<h3 id="businessmessages-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|Inline|

<h3 id="businessmessages-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[Conversation](#schemaconversation)]|false|none|none|
|» id|string(uuid)|true|none|none|
|» propertySummary|[PropertySummary](#schemapropertysummary)|true|none|none|
|»» id|string(uuid)|false|none|none|
|»» displayAddress|string|true|none|none|
|»» pictureUrl|string|false|none|none|
|»» ownerPictureUrl|string|false|none|none|
|»» latitude|number(double)|true|none|none|
|»» longitude|number(double)|true|none|none|
|»» externalRef|string|false|none|none|
|»» doorNumber|string|false|none|none|
|»» rentSell|string|false|none|Property rent or sell|
|» messages|[[Message](#schemamessage)]|true|none|none|
|»» id|string(uuid)|true|none|none|
|»» senderId|string(uuid)|false|none|none|
|»» recipientId|string(uuid)|false|none|none|
|»» text|string|true|none|none|
|»» propertyId|string(uuid)|false|none|none|
|»» propertyRef|string|false|none|none|
|»» createdAt|string(date-time)|false|none|none|
|»» readAt|string(date-time)|false|none|none|
|»» senderContact|[Contact](#schemacontact)|true|none|none|
|»»» name|string|true|none|none|
|»»» phone|string|false|none|none|
|»»» email|string|false|none|none|
|»»» pictureUrl|string|false|none|none|
|»»» displayAddress|string|false|none|none|
|»»» phone2|string|false|none|none|
|»»» businessHours|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|rentSell|property.basics.rentsell.rent|
|rentSell|property.basics.rentsell.sell|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
api_key
</aside>


## conversation

<a id="opIdconversation"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /test.flatsy.fr/api/v1/business/messages/conversation \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /test.flatsy.fr/api/v1/business/messages/conversation HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: '/test.flatsy.fr/api/v1/business/messages/conversation',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('/test.flatsy.fr/api/v1/business/messages/conversation',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get '/test.flatsy.fr/api/v1/business/messages/conversation',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('/test.flatsy.fr/api/v1/business/messages/conversation', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/test.flatsy.fr/api/v1/business/messages/conversation");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/test.flatsy.fr/api/v1/business/messages/conversation", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/messages/conversation`

*Renvoie la conversation à propos d'un bien avec l'agent de visite. Il est nécessaire de préciser soit un id de bien soit une référence client.*


<h3 id="conversation-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|propertyId|query|string(uuid)|false|with property id|
|propertyRef|query|string|false|with property ref|

> Example responses

> 200 Response

<h3 id="conversation-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Conversation](#schemaconversation)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
api_key
</aside>

## save

<a id="opIdsave"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /test.flatsy.fr/api/v1/business/messages/save \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /test.flatsy.fr/api/v1/business/messages/save HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: '/test.flatsy.fr/api/v1/business/messages/save',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "text": "string",
  "propertyId": "string",
  "propertyRef": "string"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('/test.flatsy.fr/api/v1/business/messages/save',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post '/test.flatsy.fr/api/v1/business/messages/save',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('/test.flatsy.fr/api/v1/business/messages/save', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/test.flatsy.fr/api/v1/business/messages/save");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "/test.flatsy.fr/api/v1/business/messages/save", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/messages/save`

*Permet d'envoyer un message*

> Body parameter

```json
{
  "text": "string",
  "propertyId": "string",
  "propertyRef": "string"
}
```

<h3 id="save-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[MessagePost](#schemamessagepost)|true|message save|

> Example responses

> 200 Response

<h3 id="save-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Message](#schemamessage)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
api_key
</aside>



<h1 id="Flatsy-API-businessvisit">Visit</h1>

Avant de lire cette section, il est préférable de se familiariser avec le [schema de la visite](#tocSvisit)





## visit

<a id="opIdvisit"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /www.flatsy.fr/api/v1/business/visit/{id} \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /www.flatsy.fr/api/v1/business/visit/{id} HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/visit/{id}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/visit/{id}',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get 'https://wwww.flatsy.fr/api/v1/business/visit/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('https://wwww.flatsy.fr/api/v1/business/visit/{id}', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/visit/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/www.flatsy.fr/api/v1/business/visit/{id}", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/visit/{id}`

*Obtient une visite par son id*

<h3 id="visit-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string(uuid)|true|none|

> Example responses

> 200 Response

<h3 id="visit-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Visit](#schemavisit)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## updateVisitor

<a id="opIdupdateVisitor"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /www.flatsy.fr/api/v1/business/update/visitor \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /www.flatsy.fr/api/v1/business/update/visitor HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/update/visitor',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "email": "string",
  "firstName": "string",
  "lastName": "string",
  "phone": "string",
  "fields": {}
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/update/visitor',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post 'https://wwww.flatsy.fr/api/v1/business/update/visitor',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('https://wwww.flatsy.fr/api/v1/business/update/visitor', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/update/visitor");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "/www.flatsy.fr/api/v1/business/update/visitor", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/update/visitor`

*Met à jour les informations concernant un visiteur. Le champ fields peut contenir un nombre arbitraire de clé-valeurs et peut donc être utilisé pour stocker des méta-données*

> Body parameter

```json
{
  "email": "string",
  "firstName": "string",
  "lastName": "string",
  "phone": "string",
  "fields": {}
}
```

<h3 id="updatevisitor-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|visitId|query|string(uuid)|false|none|
|visitorId|query|string|false|none|
|body|body|[Visitor](#schemavisitor)|true|update visitor|

> Example responses

> 200 Response

<h3 id="updatevisitor-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Visitor](#schemavisitor)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## visits

<a id="opIdvisits"></a>

> Code samples

```shell
# You can also use wget
curl -X GET /www.flatsy.fr/api/v1/business/visits \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET /www.flatsy.fr/api/v1/business/visits HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/visits',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/visits',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get 'https://wwww.flatsy.fr/api/v1/business/visits',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('https://wwww.flatsy.fr/api/v1/business/visits', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/visits");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "/www.flatsy.fr/api/v1/business/visits", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/visits`

*Permet de rechercher dans les visites. En particulier, permet d'obtenir toutes les visites pour un bien en renseignant le paramètre ref.*

<h3 id="visits-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|startTime|query|string|false|startTime|
|endTime|query|string|false|endTime|
|direction|query|string|false|direction|
|page|query|integer(int32)|false|page|
|by|query|integer(int32)|false|by|
|ref|query|string|false|ref|

> Example responses

> 200 Response

<h3 id="visits-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|Inline|

<h3 id="visits-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[Visit](#schemavisit)]|false|none|none|
|» id|string(uuid)|true|none|none|
|» date|string(date-time)|false|none|none|
|» createdAt|string(date-time)|false|none|none|
|» flatguideId|string(uuid)|false|none|none|
|» code|string|false|none|none|
|» propertyVisits|[[PropertyVisit](#schemapropertyvisit)]|true|none|none|
|»» propertyId|string(uuid)|true|none|none|
|»» visitId|string(uuid)|true|none|none|
|»» readAt|string(date-time)|false|none|none|
|»» note|string|false|none|none|
|»» comment|string|false|none|none|
|»» positivePoints|string|false|none|none|
|»» negativePoints|string|false|none|none|
|»» questions|string|false|none|none|
|»» visitorInterest|integer(int32)|false|none|visitorInterest|
|» visitorVisits|[[VisitorVisit](#schemavisitorvisit)]|true|none|none|
|»» visitId|string(uuid)|true|none|none|
|»» askedPropertyId|string(uuid)|true|none|none|
|»» status|string|false|none|Visit status|
|»» initialDate|string(date-time)|false|none|none|
|»» rating|integer(int32)|false|none|rating|
|»» comment|string|false|none|none|
|»» visitor|[Visitor](#schemavisitor)|true|none|none|
|»»» email|string|true|none|none|
|»»» firstName|string|true|none|none|
|»»» lastName|string|true|none|none|
|»»» phone|string|true|none|none|
|»»» fields|object|false|none|custom fields|
|»» fileUrl|string|false|none|none|
|»» fileHandles|[string]|false|none|none|
|»» statusUpdatedAt|string(date-time)|false|none|none|
|» propertySummary|[PropertySummary](#schemapropertysummary)|true|none|none|
|»» id|string(uuid)|false|none|none|
|»» displayAddress|string|true|none|none|
|»» pictureUrl|string|false|none|none|
|»» ownerPictureUrl|string|false|none|none|
|»» latitude|number(double)|true|none|none|
|»» longitude|number(double)|true|none|none|
|»» externalRef|string|false|none|none|
|»» doorNumber|string|false|none|none|
|»» rentSell|string|false|none|Property rent or sell|
|» durationMinutes|integer(int32)|true|none|none|
|» metadata|object|false|none|metadata|

#### Enumerated Values

|Property|Value|
|---|---|
|status|visit.status.proposed|
|status|visit.status.confirmed|
|status|visit.status.cancelled.visitor|
|status|visit.status.cancelled.owner|
|status|visit.status.cancelled.flatguide|
|status|visit.status.cancelled.rescheduled|
|status|visit.status.cancelled.terminated|
|status|visit.status.done|
|rentSell|property.basics.rentsell.rent|
|rentSell|property.basics.rentsell.sell|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## visitCancel

<a id="opIdvisitorCancel"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /www.flatsy.fr/api/v1/business/visit/cancel \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /www.flatsy.fr/api/v1/business/visit/cancel HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/visit/cancel',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "visitStatus": "visit.status.proposed",
  "visitorId": "string",
  "visitId": "string"
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/visit/cancel',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post 'https://wwww.flatsy.fr/api/v1/business/visit/cancel',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('https://wwww.flatsy.fr/api/v1/business/visit/cancel', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/visit/cancel");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "/www.flatsy.fr/api/v1/business/visit/cancel", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/visit/cancel`

*Permet d'annuler une visite. A noter qu'une visite peut - suivant la configuration - concerner plusieurs visiteurs. Il est donc nécessaire de préciser de quel visiteur il s'agit. Les deux status possibles sont :*

- *visit.status.cancelled.visitor quand le visiteur souhaite annuler*
- *visit.status.cancelled.owner quand le propriétaire souhaite annuler.*

Le champ *visitorId* correspond à l'email du visiteur.



> Body parameter

```json
{
  "visitStatus": "visit.status.proposed",
  "visitorId": "string",
  "visitId": "string"
}
```

<h3 id="visitorcancel-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[VisitorCancelRequest](#schemavisitorcancelrequest)|true|visitor update status|

> Example responses

> 200 Response

<h3 id="visitorcancel-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Visitor](#schemavisitor)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>



## slots

<a id="opIdslots"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://www.flatsy.fr/api/v1/business/visit/slots \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
GET https://www.flatsy.fr/api/v1/business/visit/slots HTTP/1.1

Accept: */*

```

```javascript
var headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://www.flatsy.fr/api/v1/business/visit/slots',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');

const headers = {
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://www.flatsy.fr/api/v1/business/visit/slots',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.get 'https://www.flatsy.fr/api/v1/business/visit/slots',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.get('https://www.flatsy.fr/api/v1/business/visit/slots', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("https://www.flatsy.fr/api/v1/business/visit/slots");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://www.flatsy.fr/api/v1/business/visit/slots", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /api/v1/business/visit/slots`

*Permet d'obtenir les disponibilités de visites pour un bien. Le champ isPriority de la réponse précise si ce créneau doit être affiché de façon prioritaire. __Nous recommandons d'afficher les créneaux en deux temps à vos utilisateurs. D'abord les créneaux prioritaires, puis, si la personne n'est pas disponible, le reste des créneaux. Ainsi, on optimise la répartition des créneaux et on évite au maximum les risques d'annulation.__ A noter qu'il est nécessaire de fournir soit l'id flatsy du bien, soit votre référence unique pour ce bien*

<h3 id="slots-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|propertyId|query|string(uuid)|false|property id|
|reference|query|string|false|reference|
|isReschedule|query|boolean|false|isReschedule|

> Example responses

> 200 Response

<h3 id="slots-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|Inline|

<h3 id="slots-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[ViewingSlot](#schemaviewingslot)]|false|none|none|
|» date|string(date-time)|false|none|none|
|» available|boolean|true|none|none|
|» isPriority|boolean|true|none|none|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
api_key
</aside>

## visitCreate

<a id="opIdvisitCreate"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /www.flatsy.fr/api/v1/business/visit/create \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /www.flatsy.fr/api/v1/business/visit/create HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/visit/create',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```javascript--nodejs
const request = require('node-fetch');
const inputBody = '{
  "propertyId": "string",
  "date": "2018-07-09T09:05:00Z",
  "visitor": {
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "phone": "string",
    "fields": {}
  }
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

fetch('https://wwww.flatsy.fr/api/v1/business/visit/create',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => '*/*',
  'X-Auth-Token' => 'API_KEY'
}

result = RestClient.post 'https://wwww.flatsy.fr/api/v1/business/visit/create',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*',
  'X-Auth-Token': 'API_KEY'
}

r = requests.post('https://wwww.flatsy.fr/api/v1/business/visit/create', params={

}, headers = headers)

print r.json()

```

```java
URL obj = new URL("/www.flatsy.fr/api/v1/business/visit/create");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"*/*"},
        "X-Auth-Token": []string{"API_KEY"},

    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "/www.flatsy.fr/api/v1/business/visit/create", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`POST /api/v1/business/visit/create`

*Crée une nouvelle demande de visite. Attention les dates sont au format UTC*

*Il est possible de passer des metadonnées sous forme de clés/valeurs à cette visite via l'attribut metadata, qui pourront être retrouvées par la suite lorsqu'un objet visite est renvoyé par l'API.*

> Body parameter

```json
{
  "propertyId": "string",
  "date": "2018-07-09T09:05:00Z",
  "visitor": {
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "phone": "string",
    "fields": {},
    "metadata": {}
  }
}
```

<h3 id="visitcreate-parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[VisitCreateRequest](#schemavisitcreaterequest)|true|visit creation data|

> Example responses

> 200 Response

<h3 id="visitcreate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Visit](#schemavisit)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

## visitUpdate

<a id="opIdvisitUpdate"></a>

> Code samples

```shell
# You can also use wget
curl -X POST /www.flatsy.fr/api/v1/business/visit/reschedule \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'X-Auth-Token: API_KEY'

```

```http
POST /www.flatsy.fr/api/v1/business/visit/reschedule HTTP/1.1

Content-Type: application/json
Accept: */*

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'*/*',
  'X-Auth-Token':'API_KEY'

};

$.ajax({
  url: 'https://wwww.flatsy.fr/api/v1/business/visit/reschedule',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

`POST /api/v1/business/visit/reschedule`

*Permet de modifier la date d'une visite. La visite précdénete est annulée, et une nouvelle est créée*

> Body parameter

```json
{
  "visitorId": "string",
  "visitId": "string",
  "date": "2018-09-20T14:19:52.607Z"
}
```

<h3 id="visitupdate-parameters">Parameters</h3>

Parameter | In | Type | Required | Description
--- | --- | --- | --- |---
visitUpdateData | body | <a href="#visitreschedulerequest">VisitRescheduleRequest</a> | true | visit update data

> Example responses

> 200 Response

<h3 id="visitupdate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful operation|[Visit](#schemavisit)|

<aside class="warning">
Cette action nécessite d'être authentifié. Consultez la section Authentification pour plus d'informations.:
api_key
</aside>

# Webhook

<h2 id="tocWebhookintro">Introduction</h2>

<a id="webhookintro"></a>

Vous avez la possibilité de définir une adresse de webhook (via la page profil de votre compte et sur demande à team@flatsy.fr).
Le Webhook vous permet de recevoir des évènements WebhookEvent, qui indiquent en temps réel les changements qui ont lieu sur vos visites, ainsi que les nouveaux messages reçus via la messagerie.
Les évènements sont envoyés avec une requête POST à l'adresse définie, et au format JSON. Nous attendons une réponse HTTP 200 OK de la part de votre serveur.

Le champ eventType permet de discerner les différents types d'évènements.

Afin de pouvoir tester les webhooks en cas d'émission de nouveau message, en mode test (host test.flatsy.fr), tout message contenant un "?" recevra une réponse automatique dans la foulée.


## Fin d'une visite et compte rendu

Pour chaque visite nous envoyons un code unique au visiteur. L'agent saisit ce code sur son app mobile flatsy, ce qui nous permet de tracer la fin de la visite. C’est à ce moment que la visite passe en “done”.

Un évènement est envoyé quand la visite passe de l'état confirmé (confirmed) à terminé (done). Lorsque la visite passe à l'état terminé, il est possible que l'agent de visite ait commencé à en rédiger le compte rendu.

__Attention__ : en moyenne un délai de 2h est nécessaire après la visite pour que l'agent rédige son compte rendu définitif. Il est donc indispensable de mettre en place une stratégie pour éviter d'afficher à vos utilisateurs finaux des comptes rendus temporaires. Vous pouvez par exemple considérer que le compte rendu est un brouillon dans un délai de quelques heures après la visite.

Chaque mise à jour de compte rendu donne lieu à un évènement WebhookEvent où seul le champ Visit est renseigné. Il est donc indispensable de mettre à jour les comptes rendus en accord avec ces évènements.

Notez qu'il est toujours possible d'accéder aux comptes rendus dans leur dernier état connu en appelant l'endpoint [visit](#opIdvisit) ou [visits](#opIdvisits)

## Liste exhaustive des évènements déclenchant l'envoi d'un WebhookEvent

|Evenement|Nom technique|Champs envoyés|
|---|---|---|
| Nouvelle visite non confirmée | visit.new.unconfirmed |visit,visitor,status |
| Nouvelle visite directement confirmée (saisie par agent ou par flatsy) |visit.new.confirmed| visit, visitor, status |
| Visite passe de demandée à confirmée (proposed  -> confirmed) |visit.confirmed| visit, status |
| Visite terminée |visit.done| visit, visitor, status | visit, status (done), oldstatus (confirmed) |
| Visite change de date |visit.date.changed| visit, visitor, previousDate, previousId |
| Visite annulée par agent (pour tous les visiteurs du créneau)|visit.cancelled.agent| visit, status (cancelled.flatguide), oldStatus |
| Visite annulée par client (pour tous les visiteurs du créneau)|visit.cancelled.owner| visit, status (cancelled.owner), oldStatus |
| Visite annulée pour un seul visiteur |visit.cancelled.visitor| visit, visitor, status, oldStatus |
| Visite annulée car bien loué/vendu |visit.cancelled.endofvisits| visit, visitor, status (cancelled.terminated), oldStatus |
| Compte rendu modifié |visit.report.updated| visit |
| Nouveau message |message.new| visit |

### Cas des annulations visiteurs

Attention, quelle que soit la cause de l'annulation pour un visiteur en particulier, l'évènement envoyé est visit.cancelled.visitor. Le statut précis de la visite est à trouver dans le champ status.

### Cas des changements de date

Un changement de date de visite donne lieu à 3 évènements :

* Annulation de la visite originelle (cancelled.rescheduled)
* Modification de la visite (champs previousDate et previousId remplis), permettant de faire le lien entre les deux visites
* Evenement de création de la nouvelle visite




<h2 id="tocSWebhookEvent">WebhookEvent</h2>

<a id="schemawebookevent"></a>

```json
{
  "visit": "string",
  "visitor": "string",
  "status": "string",
  "oldStatus": "string",
  "previousDate": 0,
  "previousId": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|eventType|string|true|none| Le type d'évènement|
|visit|[Visit](#schemavisit)|false|none| la visite dans son état __après__ le changement.|
|visitor|[Visitor](#schemavisitor)|false|none|le visiteur concerné par l'évènement si besoin est de le préciser.|
|status|string|false|none|le nouveau status de la visite si celui-ci a changé.|
|oldStatus|string|false|none|l'ancien status de la visite si celui-ci a changé.|
|previousDate|string(date-time)|false|none|la date de l'ancienne visite si cette visite correspond à un changement de date|
|previousId|string(uuid)|false|none|l'id de l'ancienne visite si cette visite correspond à un changement de date|
|message|[Message](#schemamessage)|false|none|le message reçu s'il s'agit d'un évènement de message|

#### Status des visites possibles

|Property|Value|
|---|---|
|status|visit.status.proposed|
|status|visit.status.confirmed|
|status|visit.status.cancelled.visitor|
|status|visit.status.cancelled.owner|
|status|visit.status.cancelled.flatguide|
|status|visit.status.cancelled.rescheduled|
|status|visit.status.cancelled.terminated|
|status|visit.status.done|

#### Types d'évènements

|eventType|Description|
|---|---|
|visit.new.unconfirmed|Nouvelle visite non confirmée|
|visit.new.confirmed|Visite créée et confirmée|
|visit.confirmed|Visite confirmée|
|visit.done|Visite terminée|
|visit.date.changed|Changement de date de visite|
|visit.cancelled.agent|Annulation par l'agent|
|visit.cancelled.owner|Annulation volontaire|
|visit.cancelled.endofvisits|Annuation pour fin des visites|
|visit.report.updated|Compte rendu modifié|
|message.new|Nouveau message|

# Schemas

<h2 id="tocSaddress">Address</h2>

<a id="schemaaddress"></a>

```json
{
  "address": "string",
  "zip": "string",
  "city": "string",
  "countryCode": "string",
  "latitude": 0,
  "longitude": 0,
  "doorInfo": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|address|string|true|none|none|
|zip|string|true|none|none|
|city|string|true|none|none|
|countryCode|string|true|none|none|
|latitude|number|true|none|none|
|longitude|number|true|none|none|
|doorInfo|string|false|none|none|

<h2 id="tocSclient">Client</h2>

<a id="schemaclient"></a>

```json
{
  "id": "string",
  "firstName": "string",
  "lastName": "string",
  "fullName": "string",
  "shortName": "string",
  "email": "string",
  "phone": "string",
  "address": {
    "address": "string",
    "zip": "string",
    "city": "string",
    "countryCode": "string",
    "latitude": 0,
    "longitude": 0,
    "doorInfo": "string"
  },
  "photoURL": "string",
  "phone2": "string",
  "businessHours": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string(uuid)|true|none|none|
|firstName|string|false|none|none|
|lastName|string|false|none|none|
|fullName|string|false|none|none|
|shortName|string|false|none|none|
|email|string|true|none|none|
|phone|string|false|none|none|
|address|[Address](#schemaaddress)|false|none|none|
|photoURL|string|false|none|none|
|phone2|string|false|none|none|
|businessHours|string|false|none|none|

<h2 id="tocSloginresponse">LoginResponse</h2>

<a id="schemaloginresponse"></a>

```json
{
  "token": "string",
  "userId": "string",
  "errors": [
    "string"
  ],
  "expiration": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|token|string|false|none|none|
|userId|string(uuid)|false|none|none|
|errors|[string]|true|none|none|
|expiration|string|false|none|none|

<h2 id="tocScredentials">Credentials</h2>

<a id="schemacredentials"></a>

```json
{
  "identifier": "string",
  "password": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|identifier|string|true|none|none|
|password|string|true|none|none|

<h2 id="tocScontact">Contact</h2>

<a id="schemacontact"></a>

```json
{
  "name": "string",
  "phone": "string",
  "email": "string",
  "pictureUrl": "string",
  "displayAddress": "string",
  "phone2": "string",
  "businessHours": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|phone|string|false|none|none|
|email|string|false|none|none|
|pictureUrl|string|false|none|none|
|displayAddress|string|false|none|none|
|phone2|string|false|none|none|
|businessHours|string|false|none|none|

<h2 id="tocScriterias">Criterias</h2>

<a id="schemacriterias"></a>

```json
{
  "sharingAllowed": true,
  "prequalification": {
    "_type": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|sharingAllowed|boolean|false|none|none|
|prequalification|[Prequalification](#schemaprequalification)|false|none|none|


<h2 id="tocSdefaultsellprequalification">DefaultSellPrequalification</h2>

<a id="schemadefaultsellprequalification"></a>

```json
{
  "_type": "string",
  "name": "string",
  "explain": "string",
  "warning": "string"
}

```

### Properties

*allOf - discriminator: Prequalification._type*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[Prequalification](#schemaprequalification)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» name|string|true|none|none|
|» explain|string|false|none|none|
|» warning|string|false|none|none|

<h2 id="tocSgliprequalification">GLIPrequalification</h2>

Le ratio représente le rapport minimal salaire/loyer toléré par la GLI. Les garants ne sont acceptés que pour les étudiants, conformément à la loi.

<a id="schemagliprequalification"></a>

```json
{
  "_type": "string",
  "name": "string",
  "ratio": 0,
  "explain": "string",
  "warning": "string"
}

```

### Properties

*allOf - discriminator: Prequalification._type*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[Prequalification](#schemaprequalification)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» name|string|true|none|none|
|» ratio|number(double)|true|none|none|
|» explain|string|false|none|none|
|» warning|string|false|none|none|

<h2 id="tocSinterval">Interval</h2>

L'objet interval représente un intervalle de temps de disponibilité pour un agent ou pour un bien.
Le booleen isWeeklyRecurring indique si ce créneau se reporte d'une semaine sur l'autre à partir de sa date de début.

<a id="schemainterval"></a>

```json
{
  "start": "2018-11-29T15:35:03Z",
  "end": "2018-11-29T15:35:03Z",
  "isWeeklyRecurring": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|start|string(date-time)|true|none|none|
|end|string(date-time)|true|none|none|
|isWeeklyRecurring|boolean|false|none|none|

<h2 id="tocSlogementsocial">LogementSocial</h2>

La zone correspond au zonage des logements sociaux (A, Abis, B1, B2, C)
Le ratio correpond au rapport minimum salaire/loyer

Si actionLogement est setté à true, le logement n'accepte pas les fonctionnaires.


<a id="schemalogementsocial"></a>

```json
{
  "_type": "string",
  "name": "string",
  "explain": "string",
  "actionLogement": true,
  "resource": "string",
  "zone": "string",
  "ratio": 0,
  "warning": "string"
}

```

### Properties

*allOf - discriminator: Prequalification._type*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[Prequalification](#schemaprequalification)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» name|string|true|none|none|
|» explain|string|false|none|none|
|» actionLogement|boolean|true|none|none|
|» resource|string|true|none|none|
|» zone|string|true|none|none|
|» ratio|number(double)|true|none|none|
|» warning|string|false|none|none|

<h2 id="tocSnichefiscale">NicheFiscale</h2>

La zone correspond au zonage des logements sociaux (A, Abis, B1, B2, C)
Le ratio correpond au rapport minimum salaire/loyer
La loi est par exemple "pinel" pour un logement Loi Pinel.

<a id="schemanichefiscale"></a>

```json
{
  "name": "string",
  "explain": "string",
  "loi": "string",
  "zone": "string",
  "ratio": 0,
  "warning": "string"
}

```

### Properties

*allOf - discriminator: Prequalification._type*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[Prequalification](#schemaprequalification)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» name|string|true|none|none|
|» explain|string|false|none|none|
|» loi|string|true|none|none|
|» zone|string|true|none|none|
|» ratio|number(double)|true|none|none|
|» warning|string|false|none|none|

<h2 id="tocSprequalification">Prequalification</h2>

La préqualification des visiteurs permet de filtrer ceux-ci lorsqu'ils ne remplissent pas les critères de revenu fixés spécifiquement pour un bien.
Flatsy support différents types de préqualification : 

|Nom|Lien|Commentaire|
|---|---|---|---|
|Collecte sans vérification|[DefaultRentPrequalification](#schemadefaultrentprequalification)|Par défaut pour les locations.|
|Collecte minimale sans vérification|[DefaultSellPrequalification](#schemadefaultsellprequalification)|Par défaut pour les ventes.|
|Garantie Loyers Impayés|[GLIPrequalification](#schemagliprequalification)|
|Solvabilité Garant ou Locataire|[TenantOrGuarantorSolvability](#schematenantorguarantorsolvability)|
|Solvabilité Garant et Locataire|[TenantAndGuarantorSolvability](#schematenantandguarantorsolvability)|
|Logements sociaux|[LogementSocial](#schemalogementsocial)|
|Niches fiscales (ex: Pinel...)|[NicheFiscale](#schemanichefiscale)|

<a id="schemaprequalification"></a>

```json
{
  "_type": "string"
}

```

### Properties

*None*

<h2 id="tocSproperty">Property</h2>

Attention : la préqualification doit être soit absente, soit conforme aux précisions de [Préqualification](#schemaprequalification)

<a id="schemaproperty"></a>

```json
{
  "id": "string",
  "userId": "string",
  "data": {
    "rentSell": "property.basics.rentsell.rent",
    "criterias": {
      "sharingAllowed": true,
      "prequalification": {
        "_type": "string"
      }
    },
    "price": 0,
    "propertyStatus": "property.basics.status.available",
    "propertyStatusUpdatedAt": "2018-09-10T16:44:29Z",
    "extUrl": "string",
    "address": {
      "address": "string",
      "zip": "string",
      "city": "string",
      "countryCode": "string",
      "latitude": 0,
      "longitude": 0,
      "doorInfo": "string"
    },
    "doorRef": "string",
    "memo": "string",
    "picHandles": [
      "string"
    ],
    "picUrls": [
      "string"
    ],
    "fields": {
      "surface": 0,
      "additionalSurfaces": [
        {
          "name": "string",
          "surface": 0
        }
      ],
      "numRooms": 0,
      "numBedrooms": 0,
      "orientation": "string",
      "text": "string",
      "deposit": 0,
      "monthlyCharges": 0,
      "agencyFee": 0,
      "availableFrom": "string",
      "floor": 0,
      "heating": "string",
      "cave": true,
      "keeper": true,
      "parking": true,
      "furnished": true,
      "leaseDurationYears": 0,
      "works": "string",
      "dpe": "string",
      "constructionYear": 0
    },
    "visitAvailabilities": [
      {
        "start": "2018-09-10T16:44:29Z",
        "end": "2018-09-10T16:44:29Z",
        "isWeeklyRecurring": true
      }
    ],
    "externalRef": "string",
    "additionalRefs": [
      "string"
    ],
    "pickedVisitorId": "string",
    "tenantPhone": "string",
    "extraFields": {}
  },
  "createdAt": "2018-09-10T16:44:29Z",
  "updatedAt": "2018-09-10T16:44:29Z",
  "ownerContact": {
    "name": "string",
    "phone": "string",
    "email": "string",
    "pictureUrl": "string",
    "displayAddress": "string",
    "phone2": "string",
    "businessHours": "string"
  },
  "lastViewingAt": "2018-12-26T11:08:52Z",
  "firstViewingAt": "2018-12-26T11:08:52Z",
  "initialStaffedAt": "2018-12-26T11:08:52Z",
  "inCharge": "string",
  "viewingsPlanned": 0,
  "viewingsDone": 0,
  "weight": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string(uuid)|true|none|none|
|userId|string(uuid)|true|none|none|
|data|[PropertyData](#schemapropertydata)|true|none|none|
|createdAt|string(date-time)|false|none|none|
|updatedAt|string(date-time)|false|none|none|
|ownerContact|[Contact](#schemacontact)|true|none|none|
|lastViewingAt|string(date-time)|false|none|none|
|firstViewingAt|string(date-time)|false|none|none|
|initialStaffedAt|string(date-time)|false|none|none|
|inCharge|string|false|none|none|
|viewingsPlanned|number|false|none|none|
|viewingsDone|number|false|none|none|
|weight|number|false|none|none|

<h2 id="tocSpropertydata">PropertyData</h2>

<a id="schemapropertydata"></a>

```json
{
  "rentSell": "property.basics.rentsell.rent",
  "criterias": {
    "sharingAllowed": true,
    "prequalification": {
      "_type": "string"
    }
  },
  "price": 0,
  "propertyStatus": "property.basics.status.available",
  "propertyStatusUpdatedAt": "2018-09-10T16:44:29Z",
  "extUrl": "string",
  "address": {
    "address": "string",
    "zip": "string",
    "city": "string",
    "countryCode": "string",
    "latitude": 0,
    "longitude": 0,
    "doorInfo": "string"
  },
  "doorRef": "string",
  "memo": "string",
  "picHandles": [
    "string"
  ],
  "picUrls": [
    "string"
  ],
  "fields": {
    "surface": 0,
    "additionalSurfaces": [
      {
        "name": "string",
        "surface": 0
      }
    ],
    "numRooms": 0,
    "numBedrooms": 0,
    "orientation": "string",
    "text": "string",
    "deposit": 0,
    "monthlyCharges": 0,
    "agencyFee": 0,
    "availableFrom": "string",
    "floor": 0,
    "heating": "string",
    "cave": true,
    "keeper": true,
    "parking": true,
    "furnished": true,
    "leaseDurationYears": 0,
    "works": "string",
    "dpe": "string",
    "constructionYear": 0
  },
  "visitAvailabilities": [
    {
      "start": "2018-12-26T11:08:52Z",
      "end": "2018-12-26T11:08:52Z",
      "isWeeklyRecurring": true
    }
  ],
  "externalRef": "string",
  "additionalRefs": [
    "string"
  ],
  "pickedVisitorId": "string",
  "tenantPhone": "string",
  "extraFields": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|rentSell|string|false|none|Property rent type|
|criterias|[Criterias](#schemacriterias)|false|none|none|
|price|number(double)|false|none|Property rent type|
|propertyStatus|string|false|none|Property status|
|propertyStatusUpdatedAt|string(date-time)|false|none|none|
|extUrl|string|false|none|none|
|address|[Address](#schemaaddress)|false|none|none|
|doorRef|string|false|none|none|
|memo|string|false|none|none|
|picHandles|[string]|false|none|none|
|picUrls|[string]|false|none|none|
|fields|[PropertyFields](#schemapropertyfields)|false|none|none|
|visitAvailabilities|[[Interval](#schemainterval)]|false|none|availabilities|
|externalRef|string|false|none|none|
|additionalRefs|[string]|false|none|none|
|pickedVisitorId|string|false|none|none|
|tenantPhone|string|false|none|none|
|extraFields|object|false|none|extraFields|

#### Enumerated Values

|Property|Value|
|---|---|
|rentSell|property.basics.rentsell.rent|
|rentSell|property.basics.rentsell.sell|
|propertyStatus|property.basics.status.available|
|propertyStatus|property.basics.status.unavailable|
|propertyStatus|property.basics.status.standby|

<h2 id="tocSpropertyfields">PropertyFields</h2>

<a id="schemapropertyfields"></a>

```json
{
  "surface": 0,
  "additionalSurfaces": [
    {
      "name": "string",
      "surface": 0
    }
  ],
  "numRooms": 0,
  "numBedrooms": 0,
  "orientation": "string",
  "text": "string",
  "deposit": 0,
  "monthlyCharges": 0,
  "agencyFee": 0,
  "availableFrom": "string",
  "floor": 0,
  "heating": "string",
  "cave": true,
  "keeper": true,
  "parking": true,
  "furnished": true,
  "leaseDurationYears": 0,
  "works": "string",
  "dpe": "string",
  "constructionYear": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|surface|number|false|none|none|
|additionalSurfaces|[[Surface](#schemasurface)]|false|none|additionalSurfaces|
|numRooms|integer(int32)|false|none|numRooms|
|numBedrooms|integer(int32)|false|none|numBedrooms|
|orientation|string|false|none|none|
|text|string|false|none|none|
|deposit|number|false|none|none|
|monthlyCharges|number|false|none|none|
|agencyFee|number|false|none|none|
|availableFrom|string|false|none|none|
|floor|integer(int32)|false|none|floor number|
|heating|string|false|none|none|
|cave|boolean|false|none|cave|
|keeper|boolean|false|none|keeper|
|parking|boolean|false|none|parking|
|furnished|boolean|false|none|furnished|
|leaseDurationYears|integer(int32)|false|none|leaseDurationYears|
|works|string|false|none|none|
|dpe|string|false|none|none|
|constructionYear|integer(int32)|false|none|constructionYear|

<h2 id="tocSsurface">Surface</h2>

<a id="schemasurface"></a>

```json
{
  "name": "string",
  "surface": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|surface|number|true|none|none|

<h2 id="tocSagentinfo">AgentInfo</h2>

<a id="schemaagentinfo"></a>

```json
{
  "name": "string",
  "phone": "string",
  "presentation": "string",
  "rating": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|phone|string|false|none|none|
|presentation|string|true|none|none|
|rating|number(double)|true|none|none|

<h2 id="tocSvisitor">Visitor</h2>

<a id="schemavisitor"></a>

```json
{
  "email": "string",
  "firstName": "string",
  "lastName": "string",
  "phone": "string",
  "fields": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|email|string|true|none|none|
|firstName|string|true|none|none|
|lastName|string|true|none|none|
|phone|string|true|none|none|
|fields|object|false|none|custom fields|


<h2 id="tocStenantandguarantorsolvability">TenantAndGuarantorSolvability</h2>

Préqualification qui accepte le locataire s'il est solvable et si son garant est solvable. 
Le ratio correspond au rapport minimum salaire/loyer toléré.

<a id="schematenantandguarantorsolvability"></a>

```json
{
  "_type": "string",
  "name": "string",
  "ratio": 0,
  "explain": "string",
  "warning": "string"
}

```

### Properties

*allOf - discriminator: Prequalification._type*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[Prequalification](#schemaprequalification)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» name|string|true|none|none|
|» ratio|number(double)|true|none|none|
|» explain|string|false|none|none|
|» warning|string|false|none|none|

<h2 id="tocStenantorguarantorsolvability">TenantOrGuarantorSolvability</h2>

Préqualification qui accepte le locataire s'il est solvable ou si son garant est solvable. 
Le ratio correspond au rapport minimum salaire/loyer toléré.

<a id="schematenantorguarantorsolvability"></a>

```json
{
  "_type": "string",
  "name": "string",
  "ratio": 0,
  "explain": "string",
  "warning": "string"
}

```

### Properties

*allOf - discriminator: Prequalification._type*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[Prequalification](#schemaprequalification)|false|none|none|

*and*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» name|string|true|none|none|
|» ratio|number(double)|true|none|none|
|» explain|string|false|none|none|
|» warning|string|false|none|none|

<h2 id="tocSpropertysaverequest">PropertySaveRequest</h2>

<a id="schemapropertysaverequest"></a>

```json
{
  "inCharge": "string",
  "rentSell": "property.basics.rentsell.rent",
  "criterias": {
    "sharingAllowed": true,
    "prequalification": {
      "_type": "string"
    }
  },
  "price": 0,
  "extUrl": "string",
  "address": {
    "address": "string",
    "zip": "string",
    "city": "string",
    "countryCode": "string",
    "latitude": 0,
    "longitude": 0,
    "doorInfo": "string"
  },
  "doorRef": "string",
  "picUrls": [
    "string"
  ],
  "fields": {
    "surface": 0,
    "additionalSurfaces": [
      {
        "name": "string",
        "surface": 0
      }
    ],
    "numRooms": 0,
    "numBedrooms": 0,
    "orientation": "string",
    "text": "string",
    "deposit": 0,
    "monthlyCharges": 0,
    "agencyFee": 0,
    "availableFrom": "string",
    "floor": 0,
    "heating": "string",
    "cave": true,
    "keeper": true,
    "parking": true,
    "furnished": true,
    "leaseDurationYears": 0,
    "works": "string",
    "dpe": "string",
    "constructionYear": 0
  },
  "externalRef": "string",
  "additionalRefs": [
    "string"
  ],
  "tenantPhone": "string",
  "extraFields": {},
  "metadata": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|inCharge|string|false|none|none|
|rentSell|string|false|none|Property rent type|
|criterias|[Criterias](#schemacriterias)|false|none|none|
|price|number(double)|false|none|Price|
|extUrl|string|false|none|none|
|address|[Address](#schemaaddress)|false|none|none|
|doorRef|string|false|none|none|
|picUrls|[string]|false|none|none|
|fields|[PropertyFields](#schemapropertyfields)|false|none|none|
|externalRef|string|true|none|none|
|additionalRefs|[string]|false|none|none|
|tenantPhone|string|false|none|none|
|extraFields|object|false|none|extraFields|
|metadata|object|false|none|metadata|

#### Enumerated Values

|Property|Value|
|---|---|
|rentSell|property.basics.rentsell.rent|
|rentSell|property.basics.rentsell.sell|

<h2 id="tocSavailabilityupdaterequest">AvailabilityUpdateRequest</h2>



<a id="schemaavailabilityupdaterequest"></a>

```json
{
  "availability": [
    {
      "start": "2018-09-10T16:44:29Z",
      "end": "2018-09-10T16:44:29Z",
      "isWeeklyRecurring": true
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|availability|[[Interval](#schemainterval)]|false|none|availabilities|

<h2 id="tocSpropertystatuschangerequest">PropertyStatusChangeRequest</h2>

<a id="schemapropertystatuschangerequest"></a>

```json
{
  "propertyStatus": "available"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|propertyStatus|string|false|none|Property status|

#### Enumerated Values

|Property|Value|
|---|---|
|propertyStatus|available|
|propertyStatus|standby|

<h2 id="tocSpropertyterminaterequest">PropertyTerminateRequest</h2>

<a id="schemapropertyterminaterequest"></a>

```json
{
  "pickedVisitorEmail": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|pickedVisitorEmail|string|false|none|none|

<h2 id="tocSconversation">Conversation</h2>

<a id="schemaconversation"></a>

```json
{
  "id": "string",
  "propertySummary": {
    "id": "string",
    "displayAddress": "string",
    "pictureUrl": "string",
    "ownerPictureUrl": "string",
    "latitude": 0,
    "longitude": 0,
    "externalRef": "string",
    "doorNumber": "string",
    "rentSell": "property.basics.rentsell.rent"
  },
  "messages": [
    {
      "id": "string",
      "senderId": "string",
      "recipientId": "string",
      "text": "string",
      "propertyId": "string",
      "propertyRef": "string",
      "createdAt": "2018-09-10T16:44:29Z",
      "readAt": "2018-09-10T16:44:29Z",
      "senderContact": {
        "name": "string",
        "phone": "string",
        "email": "string",
        "pictureUrl": "string",
        "displayAddress": "string",
        "phone2": "string",
        "businessHours": "string"
      }
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string(uuid)|true|none|none|
|propertySummary|[PropertySummary](#schemapropertysummary)|true|none|none|
|messages|[[Message](#schemamessage)]|true|none|none|

<h2 id="tocSmessage">Message</h2>

<a id="schemamessage"></a>

```json
{
  "id": "string",
  "senderId": "string",
  "recipientId": "string",
  "text": "string",
  "propertyId": "string",
  "propertyRef": "string",
  "createdAt": "2018-09-10T16:44:29Z",
  "readAt": "2018-09-10T16:44:29Z",
  "senderContact": {
    "name": "string",
    "phone": "string",
    "email": "string",
    "pictureUrl": "string",
    "displayAddress": "string",
    "phone2": "string",
    "businessHours": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string(uuid)|true|none|none|
|senderId|string(uuid)|false|none|none|
|recipientId|string(uuid)|false|none|none|
|text|string|true|none|none|
|propertyId|string(uuid)|false|none|none|
|propertyRef|string|false|none|none|
|createdAt|string(date-time)|false|none|none|
|readAt|string(date-time)|false|none|none|
|senderContact|[Contact](#schemacontact)|true|none|none|

<h2 id="tocSpropertysummary">PropertySummary</h2>

<a id="schemapropertysummary"></a>

```json
{
  "id": "string",
  "displayAddress": "string",
  "pictureUrl": "string",
  "ownerPictureUrl": "string",
  "latitude": 0,
  "longitude": 0,
  "externalRef": "string",
  "doorNumber": "string",
  "rentSell": "property.basics.rentsell.rent"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string(uuid)|false|none|none|
|displayAddress|string|true|none|none|
|pictureUrl|string|false|none|none|
|ownerPictureUrl|string|false|none|none|
|latitude|number(double)|true|none|none|
|longitude|number(double)|true|none|none|
|externalRef|string|false|none|none|
|doorNumber|string|false|none|none|
|rentSell|string|false|none|Property rent or sell|

#### Enumerated Values

|Property|Value|
|---|---|
|rentSell|property.basics.rentsell.rent|
|rentSell|property.basics.rentsell.sell|

<h2 id="tocSpropertyvisit">PropertyVisit</h2>

<a id="schemapropertyvisit"></a>

```json
{
  "propertyId": "string",
  "visitId": "string",
  "readAt": "2018-09-10T16:44:29Z",
  "note": "string",
  "comment": "string",
  "positivePoints": "string",
  "negativePoints": "string",
  "questions": "string",
  "visitorInterest": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|propertyId|string(uuid)|true|none|none|
|visitId|string(uuid)|true|none|none|
|readAt|string(date-time)|false|none|none|
|note|string|false|none|none|
|comment|string|false|none|none|
|positivePoints|string|false|none|none|
|negativePoints|string|false|none|none|
|questions|string|false|none|none|
|visitorInterest|integer(int32)|false|none|visitorInterest|

<h2 id="tocSvisit">Visit</h2>

L'objet visite contient, en plus des informations basiques (date, date de création, durée etc.. ) deux aspects principaux :

  - VisitorVisits contient les demandes et informations concernant les visiteurs, qui peuvent être plusieurs sur une visite suivant le paramétrage
  - PropertyVisits contient les comptes rendus de chacun des biens visités durant la visite (les biens peuvent être également plusieurs suivant le paramétrage)

Ce modèle implique que certaines méthodes de l'API demandent de préciser le visiteur concerné par la méthode.


<a id="schemavisit"></a>

```json
{
  "id": "string",
  "date": "2018-09-10T16:44:29Z",
  "createdAt": "2018-09-10T16:44:29Z",
  "flatguideId": "string",
  "code": "string",
  "propertyVisits": [
    {
      "propertyId": "string",
      "visitId": "string",
      "readAt": "2018-09-10T16:44:29Z",
      "note": "string",
      "comment": "string",
      "positivePoints": "string",
      "negativePoints": "string",
      "questions": "string",
      "visitorInterest": 0
    }
  ],
  "visitorVisits": [
    {
      "visitId": "string",
      "askedPropertyId": "string",
      "status": "visit.status.proposed",
      "initialDate": "2018-09-10T16:44:29Z",
      "rating": 0,
      "comment": "string",
      "visitor": {
        "email": "string",
        "firstName": "string",
        "lastName": "string",
        "phone": "string",
        "fields": {}
      },
      "fileUrl": "string",
      "fileHandles": [
        "string"
      ],
      "statusUpdatedAt": "2018-09-10T16:44:29Z"
    }
  ],
  "propertySummary": {
    "id": "string",
    "displayAddress": "string",
    "pictureUrl": "string",
    "ownerPictureUrl": "string",
    "latitude": 0,
    "longitude": 0,
    "externalRef": "string",
    "doorNumber": "string",
    "rentSell": "property.basics.rentsell.rent"
  },
  "durationMinutes": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string(uuid)|true|none|none|
|date|string(date-time)|false|none|none|
|createdAt|string(date-time)|false|none|none|
|flatguideId|string(uuid)|false|none|none|
|code|string|false|none|none|
|propertyVisits|[[PropertyVisit](#schemapropertyvisit)]|true|none|none|
|visitorVisits|[[VisitorVisit](#schemavisitorvisit)]|true|none|none|
|propertySummary|[PropertySummary](#schemapropertysummary)|true|none|none|
|durationMinutes|integer(int32)|true|none|none|

<h2 id="tocSvisitorvisit">VisitorVisit</h2>

<a id="schemavisitorvisit"></a>

```json
{
  "visitId": "string",
  "askedPropertyId": "string",
  "status": "visit.status.proposed",
  "initialDate": "2018-09-10T16:44:29Z",
  "rating": 0,
  "comment": "string",
  "visitor": {
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "phone": "string",
    "fields": {}
  },
  "fileUrl": "string",
  "fileHandles": [
    "string"
  ],
  "statusUpdatedAt": "2018-09-10T16:44:29Z"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|visitId|string(uuid)|true|none|none|
|askedPropertyId|string(uuid)|true|none|none|
|status|string|false|none|Visit status|
|initialDate|string(date-time)|false|none|none|
|rating|integer(int32)|false|none|rating|
|comment|string|false|none|none|
|visitor|[Visitor](#schemavisitor)|true|none|none|
|fileUrl|string|false|none|none|
|fileHandles|[string]|false|none|none|
|statusUpdatedAt|string(date-time)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|visit.status.proposed|
|status|visit.status.confirmed|
|status|visit.status.cancelled.visitor|
|status|visit.status.cancelled.owner|
|status|visit.status.cancelled.flatguide|
|status|visit.status.cancelled.rescheduled|
|status|visit.status.cancelled.terminated|
|status|visit.status.done|

<h2 id="tocSvisitorcancelrequest">VisitorCancelRequest</h2>

<a id="schemavisitorcancelrequest"></a>

```json
{
  "visitStatus": "visit.status.proposed",
  "visitorId": "string",
  "visitId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|visitStatus|string|false|none|Visit status|
|visitorId|string|true|none|Email du visiteur (champ email de [Visitor](#schemavisitor))|
|visitId|string(uuid)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|visitStatus|visit.status.proposed|
|visitStatus|visit.status.confirmed|
|visitStatus|visit.status.cancelled.visitor|
|visitStatus|visit.status.cancelled.owner|
|visitStatus|visit.status.cancelled.flatguide|
|visitStatus|visit.status.cancelled.rescheduled|
|visitStatus|visit.status.cancelled.terminated|
|visitStatus|visit.status.done|

<h2 id="tocSstatuschangerequest">StatusChangeRequest</h2>

<a id="schemastatuschangerequest"></a>

```json
{
  "visitStatus": "visit.status.proposed",
  "visitorId": "string",
  "visitId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|visitStatus|string|false|none|Visit status|
|visitorId|string|false|none|none|
|visitId|string(uuid)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|visitStatus|visit.status.proposed|
|visitStatus|visit.status.confirmed|
|visitStatus|visit.status.cancelled.visitor|
|visitStatus|visit.status.cancelled.owner|
|visitStatus|visit.status.cancelled.flatguide|
|visitStatus|visit.status.cancelled.rescheduled|
|visitStatus|visit.status.cancelled.terminated|
|visitStatus|visit.status.done|

<h2 id="tocSviewingslot">ViewingSlot</h2>

<a id="schemaviewingslot"></a>

```json
{
  "date": "2018-09-10T16:44:29Z",
  "available": true,
  "isPriority": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|date|string(date-time)|false|none|none|
|available|boolean|true|none|none|
|isPriority|boolean|true|none|none|

<h2 id="tocSvisitcreaterequest">VisitCreateRequest</h2>

<a id="schemavisitcreaterequest"></a>

```json
{
  "propertyId": "string",
  "ref": "string",
  "date": "2018-09-10T16:44:29Z",
  "visitor": {
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "phone": "string",
    "fields": {}
  },
  "clientId": "string",
  "initialDate": "2018-09-10T16:44:29Z"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|propertyId|string(uuid)|false|none|none|
|ref|string|false|none|none|
|date|string(date-time)|false|none|none|
|visitor|[Visitor](#schemavisitor)|true|none|none|
|clientId|string(uuid)|false|none|none|
|initialDate|string(date-time)|false|none|none|
|metadata|object|false|none|none|


<h2 id="tocSmessagepost">MessagePost</h2>


Un des deux champ propertyId ou propertyRef est requis


<a id="schemamessagepost"></a>

```json
{
  "text": "string",
  "propertyId": "string",
  "propertyRef": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|text|string|true|none|none|
|propertyId|string(uuid)|true (ou properyRef)|none|none|
|propertyRef|string|true (ou propertyId)|none|none|


<h2 id="tocSvisitreschedulerequest">VisitRescheduleRequest</h2>

<a id="schemavisitreschedulerequest"></a>

```json
{
  "visitorId": "string",
  "visitId": "string",
  "date": "2018-09-20T14:19:52.607Z"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|visitorId|string|false|none|none|
|visitId|string|false|none|none|
|date|string(date-time)|false|none|none|
