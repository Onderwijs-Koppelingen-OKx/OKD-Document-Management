## Flow x Opvragen document
Bij het opvragen van een document zijn er 3 opties: 
1. de consumer app vraagt om de binary data en toont deze aan de eindgebruiker 
2. de consumer app krijggt een url die heel kort geldig is en kan de browser van de gebruiker hier naar toe forwarden. het document komt dan van de djuma srever
2. de consumer app vraagt om de link in djuma, die alleen werkt als de gebruiker ook bij jdjuma kan inloggen

## Optie 1
### Endpoint

- **`GET .../okd/v1_0/documents/{documentId}`**
  - **Description**: Fetches the specified document's  binary content from the DMS, identified by its `documentId`.
  - **Parameters**: 
    - `documentId` (required): A unique identifier (UUID) for the document to be retrieved.
  - **Response**:
    - **Success 200 (OK)**: Returns the complete document binary data.

### Sequence Diagram

```mermaid
sequenceDiagram
    participant Consumer app
    participant DMS
    Consumer app->>DMS: GET .../okd/v1_0/documents/{documentId}
    activate DMS
    DMS-->>Consumer app: 200 OK, Binary Data
    deactivate DMS
```

## Optie 2
### Endpoint

- **`GET .../okd/v1_0/documents/{documentId}`**
  - **Description**: Fetches the specified document's  url or content url from the DMS, identified by its `documentId`.
  - **Parameters**: 
    - `documentId` (required): A unique identifier (UUID) for the document to be retrieved.
  - **Accept type**: application/json
  - **Response**:
    - **Success 200 (OK)**: Returns the url and content url.

### Sequence Diagram

```mermaid
sequenceDiagram
    participant browser
    participant Consumer app
    participant DMS
    browser ->> Consumer app: show document
    activate browser
    Consumer app->>DMS: GET .../okd/v1_0/documents/{documentId}
    activate DMS
    DMS->>Consumer app: 200 OK
    deactivate DMS
    Consumer app ->> browser: documentdownloadonetimeUrl van content
    deactivate browser
    browser->>DMS: GET /{documentdownloadonetimeUrl}
    activate DMS
    DMS->>browser: Document inhoud binary
    deactivate DMS
```

```json
GET .../okd/v1_0/ooapi/documents/{documentId}
Host: api.yourdomain.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json
```
returns:
```
{
    "documentDetailUrl": "http://aaa.djuma.nl/details/5ffc6127-debe-48ce-90ae-75ea80756475",
    "documentdownloadonetimeUrl": "http://aaa.djuma.nl/download/5ffc6127-debe-48ce-90ae-75ea80756475&onetimeaccess=#556633ga6dr53g3"
}
```
## Optie 3
Dit is een variatie op 2, alleen word niet de binary data getoond, maar de detials pagina van het CMS



## Bespreekpunten
- Openen van document eigenschappen scherm is niet mogelijk, kan alleen via een URL naar de zaak. Echter, dat kan uitsluitend vanuit zaak API, oftewel consumer app zou kennis van zaken moeten hebben. Nut- noodzaak van deze behoefte heroverwegen i.r.t. V1

## Authenticatie:
scope die ook gebruikt is voor toevoegen (nader invullen)








