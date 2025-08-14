## Flow y Updaten document
Updaten van de inhoud van een document, zonder dat de meta data verandert.

#### bespreken:
 Is deze flow relevant? of is een nieuwe revisie altijd een nieuw document.

### Endpoint

### optie 1 (interactief aanpassen)
Het document kan eerst gelocked worden, dan word er bewerkt, als dat klaar is word het document overschreven in het DMS en daarna unlocked.
*  endpoint .../okd/v1/documents/{documentid}/_lock POST
*  endpoint .../okd/v1/documents/{documentid} PATCH
 *  endpoint .../okd/v1/documents/{documentid}/_lock DELETE 

```mermaid
sequenceDiagram
    participant Component
    participant DMS
    Component->>DMS: POST .../okd/v1/documents/{documentId}/lock
    activate DMS
    DMS-->>Component: 204 No content
    deactivate DMS
    activate Component
    Component --> Component: update document
    Component->>DMS: PATCH .../okd/v1/documents/{documentId}
    deactivate Component

    activate DMS
    DMS-->>Component: 204 No content
    deactivate DMS
     Component->>DMS: DELETE .../okd/v1/documents/{documentId}/lock
    activate DMS
    DMS-->>Component: 204 No content
    deactivate DMS

```
### optie 2
direct de nieuwe inhoud van het document uploaden. Als het document gelocked is faalt de call
*  endpoint .../okd/v1/documents/{documentid} PATCH


### Sequence Diagram

```mermaid
sequenceDiagram
    participant Component
    participant DMS
    Component->>DMS: PATCH .../okd/v1/documents/{documentId} (alleen binary content)
    activate DMS
    DMS-->>Component: 204 No content
    deactivate DMS
```

####  voorbeeld :
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

### Authenticatie:
scope die ook gebruikt is voor updaten is zelfde als voor toevoegen. (**okd:alldocuments** of de specifiekere varianten)
