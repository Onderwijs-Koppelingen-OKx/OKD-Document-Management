# OKD - Flow 2 - examen Document overdragen naar DMS
Aanbieden van examenresultaat en examenmoment gerelateerde documenten naar het DMS. Deze documenten worden opgeslagen in DMS als onderdeel van examendossier en gelinkt aan de student inschrijving.

Er zijn twee hoofd documenttypes:
* examenzittingverslag document
* examen resultaat document

Deze flows lijken erg op flow 1, met het verschil dat de metainformatie in deze flow een van de volgende is:
* een examen zitting. (componentOffering in OOAPI termen)
* een examen beoordeling/resultaat: inschrijving van student (programOfferingAssociation)

__Open vraag: sturen we een examen beeoordeling bij de inschrijving van de student? of bij de examenzitting. Hoe weet het DMS voor welke student het is? Of op de associatie van de student op de exameninschrijving?__
 
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

voorbeeld :
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
            "documentType": "examinering",
            "documentSubtype" : "examenbeoordeling"
            "documentId: "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
            "documentName": "beoordeling-100245.pdf",
            "bewaartermijnsuggestie": "3Y"
            "examDate": "2021-09-01", 
            "examComponentOffering": "c9c3875b-0c6b-435a-a8a4-211bf66620b5",
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

voorbeeld :
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
    "name": "Examen Mediabeheer K3-W1",
    "program": {
        "......gaan we deze vullen ?"
    },
    "organization": "38bdbeb1-12b2-48fd-84f8-653e7adfaf99",
    "consumers": [
        {
            "consumerKey": "nl-okd",
            "documentType": "examinering",
            "documentSubtype" : "zittingsverslag"
            "documentId: "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
            "documentName": "verslag-100245.pdf",
            "bewaartermijnsuggestie": "6M"

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
    dmsDocumentId: "4e12169d-84b9-4d21-a987-f373bbbe4e6e"
}
```

## Verwerking in het DMS
Het DMS kan zelf bepalen hoe de documenten opgeslagen en verwerkt worden: of een apart examen dossier of alles onder de student inschrijving dossier

## Remarks
- Berichten van maximaal 1 GB ondersteunen. Als we in de toekomst meer dan 1 GB willen ondersteunen, dan moet de metadata en het bestand apart gestuurd worden.

## Authenticatie:
scope voor toevoegen van examen gerelateerde documenten: **okd:alldocuments** en **okd:examdocuments**.
 als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.
