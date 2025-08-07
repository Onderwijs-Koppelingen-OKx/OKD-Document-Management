## Flow y Updaten document
Updaten van een document.
TODO: wat is een update: nieuwe versie of de inhoud veranderen zopnder dat de meta data verandert?
Mag dat altijd?

### Endpoint

### optie 1
*  endpoint .../okd/v1_0/documents/{documentid}/_lock POST
*  endpoint .../okd/v1_0/documents/{documentid} PATCH
*  endpoint .../okd/v1_0/documents/{documentid}/_unlock POST 

of
 *  endpoint .../okd/v1_0/documents/{documentid}/_lock DELETE 

### optie 2
direct de nieuwe inhoud van het document uploaden. Als het document gelocked is faalt de call
*  endpoint .../okd/v1_0/documents/{documentid} PATCH


### Sequence Diagram

```mermaid
sequenceDiagram
    participant Consumer app
    participant DMS
    Consumer app->>DMS: PATCH .../okd/v1_0/documents/{documentId} (alleen binary content)
    activate DMS
    DMS-->>Consumer app: 204 No content
    deactivate DMS
```

####  voorbeeld :
```
PUT /documents/dbd3e12a-ed8b-4488-ac34-26fd4f64f40b
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

