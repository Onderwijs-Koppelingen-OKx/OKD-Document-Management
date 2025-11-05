## Flow x Opvragen document

## Optie 1
Wanneer een client een interne document viewer heeft, kan deze rechtstreeks de binaire data van het document opvragen.

### Endpoint

- ** `GET .../okd/v1/documents/{documentId}` ** 
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
De client toont de binaire data van het document in een apart tabblad in de browser.

### Endpoint

- **`GET .../okd/v1/documents/{documentId}/metadata **
  - **Description**: Fetches the specified document's url or content url from the DMS, identified by its `documentId`.
  - **Parameters**: 
    - `documentId` (required): A unique identifier (UUID) for the document to be retrieved.
  - **Accept type**: application/json
  - **Response**:
    - **Success 200 (OK)**: Returns the meta data with downloadurl.

### Sequence Diagram

```mermaid
sequenceDiagram
    participant browser
    participant Component
    participant DMS
    browser ->> Component: show document
    activate browser
    Component->>DMS: GET .../okd/v1/documents/{documentId}/metadata
    activate DMS
    DMS->>Component: 200 OK with document metadata
    deactivate DMS
    Component ->> browser: documentTempDownloadUrl van content
    deactivate browser
    browser->>DMS: GET /{documentTempDownloadUrl}
    activate DMS
    DMS->>browser: Document inhoud binary
    deactivate DMS
```

```json
GET .../okd/v1/ooapi/documents/{documentId}/metadata
Host: api.yourdomain.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json
```
returns :
```
{
    "documentId": "5ffc6127-debe-48ce-90ae-75ea80756475",
    "title": "Test title",
    "documentTempDownloadUrl": "http://aaa.djuma.nl/download/5ffc6127-debe-48ce-90ae-75ea80756475&tempaccess=#556633ga6dr53g3",
    "creationdate": "2025-09-03",
    "format": "application/pdf",
    "documentname": "test.pdf",
    "documentsize": 243857
}
```




### Authenticatie:
scope die ook gebruikt is voor inzien is dezelfde als voor toevoegen. (**okd:alldocuments** of de specifiekere varianten)
Als de scope een specifieker scope is dan gelden de document restricties. okd:bpvdocument mag alleen BPV documenten inzien.
verder mogen de componenten alleen de door hun aangeleverde documenten inzien.



