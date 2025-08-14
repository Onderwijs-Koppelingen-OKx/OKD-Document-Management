## Flow x Opvragen document
Bij het opvragen van een document zijn er 3 opties: 
1. de component vraagt om de binary data en toont deze aan de eindgebruiker 
2. de component krijggt een url die heel kort geldig is en kan de browser van de gebruiker hier naar toe forwarden. het document komt dan van de djuma srever
2. de component vraagt om de link in djuma, die alleen werkt als de gebruiker ook bij jdjuma kan inloggen

## Optie 1
### Endpoint

- **`GET .../okd/v1/documents/{documentId}`**
  - **Description**: Fetches the specified document's  binary content from the DMS, identified by its `documentId`.
  - **Parameters**: 
    - `documentId` (required): A unique identifier (UUID) for the document to be retrieved.
  - **Response**:
    - **Success 200 (OK)**: Returns the complete document binary data.

### Sequence Diagram

```mermaid
sequenceDiagram
    participant Component
    participant DMS
    Component->>DMS: GET .../okd/v1/documents/{documentId}
    activate DMS
    DMS-->>Component: 200 OK, Binary Data
    deactivate DMS
```

## Optie 2
### Endpoint

- **`GET .../okd/v1/documents/{documentId}`**
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
    participant Component
    participant DMS
    browser ->> Component: show document
    activate browser
    Component->>DMS: GET .../okd/v1/documents/{documentId}
    activate DMS
    DMS->>Component: 200 OK
    deactivate DMS
    Component ->> browser: documentdownloadonetimeUrl van content
    deactivate browser
    browser->>DMS: GET /{documentdownloadonetimeUrl}
    activate DMS
    DMS->>browser: Document inhoud binary
    deactivate DMS
```

```json
GET .../okd/v1/ooapi/documents/{documentId}
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
- Openen van document eigenschappen scherm is niet mogelijk, kan alleen via een URL naar de dossier. Echter, dat kan uitsluitend vanuit dossier API, oftewel component zou kennis van zaken moeten hebben. Nut- noodzaak van deze behoefte heroverwegen i.r.t. V1


### Authenticatie:
scope die ook gebruikt is voor inzien is zelfde als voor toevoegen. (**okd:alldocuments** of de specifiekere varianten)
Als de scope een specifieker scope is dan gelden de document restricties. okd:bpvdocument mag alleen bpv documenten inzien.
verder mogen de componenten alleen de door hun aangeleverde documenten inzien.





