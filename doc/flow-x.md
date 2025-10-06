## Flow x Opvragen document

## Optie 1
Wanneer een client een interne document viewer heeft, kan deze rechtstreeks de binaire data van het document opvragen.

### Endpoint

- **`GET .../okd/v1/documents/{documentId}/download`** (request application/octet-stream)
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
    Component->>DMS: GET .../okd/v1/documents/{documentId}/download
    activate DMS
    DMS-->>Component: 200 OK, Binary Data
    deactivate DMS
```

## Optie 2
De client toont de binaire data van het document in een apart tabblad in de browser.

### Endpoint

- **`GET .../okd/v1/documents/{documentId}`** (request application/json)
  - **Description**: Fetches the specified document's url or content url from the DMS, identified by its `documentId`.
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
GET .../okd/v1/ooapi/documents/{documentId}
Host: api.yourdomain.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json
```
returns (check response from getting document details in ZGW):
```
{
    "documentId": "5ffc6127-debe-48ce-90ae-75ea80756475",
    "title": "Test title",
    "documentTempDownloadUrl": "http://aaa.djuma.nl/download/5ffc6127-debe-48ce-90ae-75ea80756475&tempaccess=#556633ga6dr53g3",
    "creatiedatum": "2025-09-03",
    "formaat": "application/pdf",
    "documentname": "test.pdf",
    "documentsize": 243857
}
```




### Authenticatie:
scope die ook gebruikt is voor inzien is dezelfde als voor toevoegen. (**okd:alldocuments** of de specifiekere varianten)
Als de scope een specifieker scope is dan gelden de document restricties. okd:bpvdocument mag alleen BPV documenten inzien.
verder mogen de componenten alleen de door hun aangeleverde documenten inzien.


### Work in progress: Response Get document details in ZGW
```
{
    "url": "https://x/DevId/api/v1/enkelvoudiginformatieobjecten/cfd9e4dc-f88b-4520-b36a-71e6d2ba88ab",
    "identificatie": "1446",
    "bronorganisatie": "802897000",
    "creatiedatum": "2025-09-03",
    "titel": "test.pdf",
    "vertrouwelijkheidaanduiding": "zaakvertrouwelijk",
    "auteur": "onbekend",
    "status": "definitief",
    "formaat": "application/pdf",
    "taal": "nld",
    "beginRegistratie": "2025-09-03T11:46:04Z",
    "bestandsnaam": "test.pdf",
    "inhoud": "https://x/DevId/api/v1/enkelvoudiginformatieobjecten/cfd9e4dc-f88b-4520-b36a-71e6d2ba88ab/download?versie=1",
    "bestandsomvang": 243857,
    "link": null,
    "beschrijving": "",
    "ontvangstdatum": null,
    "verzenddatum": null,
    "indicatieGebruiksrecht": null,
    "verschijningsvorm": "",
    "ondertekening": null,
    "integriteit": {
        "algoritme": "sha_256",
        "waarde": "38160477FA5AB580DF77D11FDE7A76DEDA04F56526CBE1CCD9BDFD247F011795",
        "datum": "2025-09-03T00:00:00"
    },
    "informatieobjecttype": "https://x/devid/api/v1/informatieobjecttypen/26754186-a0b7-4e5d-8535-e329501b2a97",
    "locked": false,
    "bestandsdelen": [],
    "lock": null
}
```
