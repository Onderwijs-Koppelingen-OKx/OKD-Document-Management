## OKD - Flow y Updaten document
Updaten van de inhoud van een document, zonder dat de meta data verandert.

## X.1 Updaten binaire data van een document
Direct de nieuwe inhoud van het document uploaden. Als het document gelocked is faalt de call. Er wordt alleen binaire data geupdate en geen metadata van het document.

*  endpoint .../okd/v1/documents/{documentid} PATCH

### Sequence Diagram of Update binaire data document

```mermaid
sequenceDiagram
    participant Component
    participant DMS
    Component->>DMS: PATCH .../okd/v1/documents/{documentId} (alleen binary content)
    activate DMS
    DMS-->>Component: 204 No content
    deactivate DMS
```

### Class diagram of Update binaire data document
```mermaid
classDiagram
  class Document {
    documentId : string
    documentType : string
    documentName : string
  }
  
```

####  Example of request Update binaire data document :
```
PATCH .../okd/v1/documents/dbd3e12a-ed8b-4488-ac34-26fd4f64f40b
Host: api.yourdomain.com
Content-Type: application/pdf
Content-Length: 12847
Content-Disposition: form-data; name="file"; filename="inschrijving-100245.pdf"
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json

%PDF-1.4
1 0 obj
<<
/Type /Catalog
/Pages 2 0 R
>>
endobj
2 0 obj
<<
/Type /Pages
/Kids [3 0 R]
/Count 1
>>
endobj
...
[Binary PDF content continues]
...
%%EOF
```

Remarks
- Er wordt alleen de binaire data geupdate en niet metadata van het document.
- Het DMS is verantwoordelijk voor het locken van een document. Wanneer het document gelockt is, dan krijgt de client een foutmelding.
- Documenten mogen niet groter zijn dan 1 GB.

### Authenticatie:
Scope die ook gebruikt is voor updaten is zelfde als voor toevoegen. (**okd:alldocuments** of de specifiekere varianten)
