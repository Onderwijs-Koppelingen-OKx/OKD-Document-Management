# OKD - Flow 3 - BPV Document overdragen naar DMS
Aanbieden van BPV gerelateerde documenten naar het DMS. Deze documenten worden opgeslagen in DMS als onderdeel van het student inschrijvingsdossier.


### Sequence diagram
```mermaid
sequenceDiagram
    Participant BPV
    Participant DMS

    BPV->>+DMS: POST .../okd/v1/associations/{associationId} (meta informatie & inhoud)
    DMS->>-BPV: nieuwe DMS referentie (UUID)

```
#### endpoints voor deze flow bij DMS
- `POST .../okd/v1/associations/{associationId}`

voorbeeld:
```
POST .../okd/v1/associations/123e4567-e89b-12d3-a456-426614174000
Host: api.yourdomain.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 2847
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="metadata"
Content-Type: application/json

{
    "associationId": "123e4567-e89b-12d3-a456-426614174000",
    "associationType": "programOfferingAssociation",
    "consumers": [
        {
            "consumerKey": "nl-okd",
            "studentNumber": "1234567",
            "documentType": "bpv",
            "documentSubtype": "bpv-overeenkomst",
            "documentId": "65f64c44-e3c4-4579-8e05-a729d4b89d06",
            "documentName": "praktijkovereenkomst.pdf",
            "retentionPeriodSuggestion": "3Y",
            "enrollmentStartDate": "2021-09-01",
            "enrollmentExpectedEndDate": "2025-07-31",
            "enrollmentFinalEndDate": null,
            "sequenceCode": "1.2",
            "receivedDate": "2023-09-01",
            "registrationDate": "2023-09-01"
        }
    ],
    "person": "5ab399b8-c499-4da8-af6d-b55e66251f31",
    "offering": "5ffc6127-debe-48ce-90ae-75ea80756475"
}
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="praktijkovereenkomst.pdf"
Content-Type: application/pdf

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
------WebKitFormBoundary7MA4YWxkTrZu0gW--

```

Response:
```
{
    "dmsDocumentId": "e8eab8a4-2b2d-4366-8d96-d9f5ba66508b"
}
```

### OKD consumer
Het OOAPI uitbreidingsmechanisme van consumers wordt gebruikt voor extra informatie:
* "consumerKey": dit moet hardcoded "nl-okd" zijn ter identificatie van de consumer
* "documentType": grofmazig documenttype "bpv"
* "documentSubtype": subtype. dit is door de school/component te definiëren
* "documentId": id van het document zoals de component het kent
* "documentName": naam van het toe te voegen document
* "retentionPeriodSuggestion": suggestie van bewaartermijn zoals eventueel gedefinieerd door component. Is suggestie, DMS mag negeren vb: "3Y", "6M", "1321D"
* "receivedDate": datum waarop het document ontvangen is door de component
* "registrationDate": datum waarop het document geregistreerd is door de component

Specifiek voor de student info: (kan het dms vast een student aanmaken als id onbekend is.)
* "studentNumber": "1234567",

## Verwerking in DMS
Het DMS kan zelf bepalen hoe de documenten opgeslagen en verwerkt worden: of in een apart BPV-dossier of alles onder het student inschrijvingsdossier

## Remarks
- Berichten van maximaal 1 GB ondersteunen. Als we in de toekomst meer dan 1 GB willen ondersteunen, dan moet de metadata en het bestand apart gestuurd worden.

## Authenticatie:
Scope voor toevoegen van BPV gerelateerde documenten: **okd:alldocuments** en **okd:bpvdocuments**.
Als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.
