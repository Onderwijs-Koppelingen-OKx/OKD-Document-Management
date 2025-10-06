# OKD - Flow 2 - Examen document overdragen naar DMS
Aanbieden van examenresultaat en examenmoment gerelateerde documenten naar het DMS. Deze documenten worden opgeslagen in DMS als onderdeel van examendossier en gelinkt aan de student inschrijving.

Er zijn twee hoofddocumenttypes:
* Examenzittingverslag document
* Examenresultaat document

Deze flows lijken erg op flow 1, met het verschil dat de metainformatie in deze flow een van de volgende is:
* een examenzitting. (componentOffering in OOAPI termen)
* een examenbeoordeling/resultaat: inschrijving van student (programOfferingAssociation)

__Open vraag: sturen we een examenbeeoordeling bij de inschrijving van de student of bij de examenzitting? Hoe weet het DMS voor welke student het is? Of op de associatie van de student op de exameninschrijving?__
 
We sturen het request naar het endpoint waar de metadata hoort. Bv association/{id} of de verbintenis/inschrijving van de student te geven. Omdat we hier niet in de inschrijving flow zitten zullen we hier zo min mogelijk velden geven, net de verplichte en een consumer met meta informatie van het document.

### Sequence diagram 
```mermaid
sequenceDiagram
    Participant Inschrijven
    Participant DMS

    Inschrijven->>+DMS: PUT .../okd/v1/associations/{associationId} (beperkte meta informatie & inhoud)
    DMS->>-Inschrijven: nieuwe DMS referentie (UUID)

```
#### Endpoints voor deze flow bij DMS
- `PUT .../okd/v1/associations/{associationId}`
- `PUT .../okd/v1/offering/{componentOfferingId}`

voorbeeld:
```
PUT .../okd/v1/associations/123e4567-e89b-12d3-a456-426614174000
Host: api.yourdomain.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 2847
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="metadata"
Content-Type: application/json

{
    "associationId: "123e4567-e89b-12d3-a456-426614174000",
    "associationType": "programOfferingAssociation",
    "role": "student",
    "state": "associated",           
    "consumers": [
        {
            "consumerKey": "nl-okd",
            "studentNumber": "1234567",
            "documentType": "examinering",
            "documentSubtype" : "examenbeoordeling"
            "documentId: "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
            "documentName": "beoordeling-100245.pdf",
            "retentionPeriodSuggestion": "3Y"
            "examDateTime": "2021-12-15T11:05:04Z",
            "examComponentOffering": "c9c3875b-0c6b-435a-a8a4-211bf66620b5",
            "examCode": "MEDIAK3-W1"
            "examName": "Examen Mediabeheer K3-W1"
        }
    ]
    "person": "5ab399b8-c499-4da8-af6d-b55e66251f31"
}
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="inschrijving-100245.pdf"
Content-Type: application/binary

.........
...
%%EOF
------WebKitFormBoundary7MA4YWxkTrZu0gW--

```

Response:
```
{
    dmsDocumentId: "4e12169d-84b9-4d21-a987-f373bbbe4e6e"
}
```

Als het een examenzitting is:
- `PUT .../okd/v1/offering/{componentOfferingId}`

voorbeeld:
```
PUT .../okd/v1/offering/c9c3875b-0c6b-435a-a8a4-211bf66620b5
Host: api.yourdomain.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 2847
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="metadata"
Content-Type: application/json

{
    "offeringId": "c9c3875b-0c6b-435a-a8a4-211bf66620b5",
    "offeringType": "component",
    "name": [
        {
            "language": "nl-NL"
            value: "Examen Mediabeheer K3-W1"
        }
    ],
    "consumers": [
        {
            "consumerKey": "nl-okd",
            "documentType": "examinering",
            "documentSubtype" : "zittingsverslag"
            "documentId: "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
            "documentName": "verslag-100245.pdf",
            "retentionPeriodSuggestion": "6M"
        }
    ]

}
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="inschrijving-100245.pdf"
Content-Type: application/binary

.........
...
%%EOF
------WebKitFormBoundary7MA4YWxkTrZu0gW--

```

Response:
```
{
    "dmsDocumentId": "4e12169d-84b9-4d21-a987-f373bbbe4e6e"
}
```

## Verwerking in het DMS
Het DMS kan zelf bepalen hoe de documenten opgeslagen en verwerkt worden: of in een apart examendossier of alles onder het student inschrijvingdossier.

### OKD consumer
Het ooapi uitbreidingsmechanisme van consumers wordt gebruikt voor extra informatie:
* "consumerKey": dit moet hardcoded "nl-okd" zijn ter identificatie van de consumer,
* "documentType": grofmazig documenttype "examinering",
* "documentSubtype": subtype. Dit is door de school/component te definiëren
* "documentId: id van het document zoals de component het kent
* "documentName": naam van het toe te voegen document
* "retentionPeriodSuggestion": suggestie van bewaartermijn zoals eventueel gedefiniëerd door component. Is suggestie, DMS mag negeren vb: "3Y", "6M", "1321D"

speciaal voor examendocumenten:
* "examDate": datum wanneer examen afgenomen is, 
* "examComponentOffering": component offering id van examen. 
* "examName": examennaam bv: "Examen Mediabeheer K3-W1"

## Remarks
- Berichten van maximaal 1 GB ondersteunen. Als we in de toekomst meer dan 1 GB willen ondersteunen, dan moet de metadata en het bestand apart gestuurd worden.

## Authenticatie:
scope voor toevoegen van examen gerelateerde documenten: **okd:alldocuments** en **okd:examdocuments**.
 als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.
