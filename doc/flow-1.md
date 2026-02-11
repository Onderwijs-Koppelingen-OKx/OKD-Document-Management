# OKD - Flow 1 Opslaan ondersteunende documenten bij de inschrijving in DMS
Aanbieden van documenten rondom de inschrijving van een student.
Vanuit model Inschrijven naar het DMS: ondersteunende documenten bij de inschrijving die in het student/inschrijvings dossier horen.

De call wordt gedaan op het endpoint waar de meta informatie normaal opgeslagen wordt, alleen is het request een multipart Post waarbij eerst de metadata in json formaat en daarna de inhoud van het document als binary (pdf) gestuurd wordt.

Het DMS bepaalt daarna het documentID en retourneert deze. Dit id is nodig om documenten in te zien, te updaten of te verwijderen. In het DMS worden documenten uit veel bronnen opgeslagen en de uniciteit van het id kan ook alleen vanuit het DMS gegarandeerd worden.

De calls vanuit de inschrijven module bevatten relatief veel metadata. Dit is in de inschrijf module beschikbaar en het is de bron om student- en inschrijvinggegevens naar het DMS te sturen. Andere modules zoals BPV en Examinering verwijzen naar deze informatie en daar is de metadata ook veel kleiner.

Note: In het request zit wel het documentId van de component, die anders kan zijn dan die van het DMS.

### Sequence diagram 
```mermaid
sequenceDiagram
    Participant Inschrijven
    Participant DMS

    Inschrijven->>+DMS: POST .../okd/v1/associations/{associationId} (meta informatie & inhoud)
    DMS->>-Inschrijven: nieuwe DMS referentie (UUID)

```
#### endpoints voor deze flow bij DMS
- `POST .../okd/v1/associations/{associationId}`

voorbeeld :
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
    "role": "student",
    "state": "associated",
    "consumers": [
        {
            "consumerKey": "nl-okd",
            "documentType": "enrollment",
            "documentSubtype": "vrijstellingsaanvraag",
            "documentId": "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
            "documentName": "inschrijving-100245.pdf",
            "retentionPeriodSuggestion": "3Y",
            "enrollmentStartDate": "2021-09-01",
            "enrollmentExpectedEndDate": "2025-07-31",
            "enrollmentFinalEndDate": null,
            "sequenceCode": "1.2",
            "receivedDate": "2023-09-01",
            "registrationDate": "2023-09-01"
        }
    ],
    "person": {
        "personId": "5ab399b8-c499-4da8-af6d-b55e66251f31",
        "primaryCode": {
            "codeType": "studentNumber",
            "code": "1234567"
        },
        "givenName": "Maartje",
        "surnamePrefix": "van",
        "surname": "Damme",
        "displayName": "Maartje van Damme",
        "initials": "MCW",
        "dateOfBirth": "2003-09-30",
        "gender": "F",
        "activeEnrollment": true,
        "affiliations": [
            "student"
        ],
        "mail": "vandamme.mcw@student.roc.nl",
        "languageOfChoice": [
            "nl-NL"
        ],
        "otherCodes": []
    },
    "offering": {
        "offeringId": "5ffc6127-debe-48ce-90ae-75ea80756475",
        "primaryCode": {
            "codeType": "identifier",
            "code": "25190BOL"
        },
        "offeringType": "program",
        "name": "Netwerk- en mediabeheerder BOL (25190)",
        "program": {
            "programId": "123e4567-e89b-12d3-a456-426614174000",
            "primaryCode": {
                "codeType": "identifier",
                "code": "C12063128"
            },
            "programType": "program",
            "name": [
                {
                    "language": "nl-NL",
                    "value": "Netwerk- en mediabeheerder"
                }
            ],
            "abbreviation": "N&M",
            "description": [
                {
                    "language": "nl-NL",
                    "value": "In deze MBO-opleiding word je opgeleid voor het officieel erkende diploma 'MBO Netwerkbeheerder, niveau 4'. Met dit diploma ben je breed opgeleid en kun je het netwerk van een organisatie beheren. Dit is hét diploma voor de professionele netwerkbeheerder op het hoogste MBO-niveau. Je legt een uitstekende basis voor een mooie carrière als netwerkbeheerder. Bovendien is dit een diploma waarmee je eventueel probleemloos kunt doorstuderen naar een HBO-opleiding"
                }
            ],
            "teachingLanguage": "nld",
            "otherCodes": [
              {
                "codeType": "opleidingsCode",
                "code": "25480BOL"
              }
            ],
            "modeOfStudy": "full-time",
            "levelOfQualification": "4"
        },
        "organization": {
            "organizationID": "38bdbeb1-12b2-48fd-84f8-653e7adfaf99",
            "primaryCode": {
                "codeType": "identifier",
                "code": "ICTE"
            },
            "organizationType": "department",
            "name": [
                {
                    "language": "nl-NL",
                    "value": "ICT-academie"
                }
            ],
            "shortname": "ICTA",
            "parent": {
                "organizationID": "650e1627-9f3d-4176-ab5a-e82eef0d219d",
                "primaryCode": {
                    "codeType": "identifier",
                    "code": "CICT"
                },
                "name": [
                    {
                        "language": "nl-NL",
                        "value": "Cluster ICT en EIS"
                    }
                ]
            }
        }
    }
}
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="inschrijving-100245.pdf"
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
    "dmsDocumentId": "4e12169d-84b9-4d21-a987-f373bbbe4e6e"
}
```


### OKD consumer
Het ooapi uitbreidingsmechanisme van consumers wordt gebruikt voor extra informatie:
* "consumerKey": dit moet hardcoded "nl-okd" zijn ter identificatie van de consumer
* "documentType": grofmazig documenttype "inschrijving"
* "documentSubtype" : subtype. Dit is door de school/component te definiëren
* "documentId: id van het document zoals de component het kent
* "documentName": naam van het toe te voegen document
* "retentionPeriodSuggestion": suggestie van bewaartermijn zoals eventueel gedefinieerd door component. Is suggestie, DMS mag negeren vb: "3Y", "6M", "1321D"

Specifiek voor inschrijven : 3 datums voor de inschrijving van deze student:
* "enrollmentStartDate": "2021-09-01", 
* "enrollmentExpectedEndDate": "2025-07-31",
* "enrollmentFinalEndDate": null
### Remarks

- Berichten van maximaal 1 GB ondersteunen. Als we in de toekomst meer dan 1 GB willen ondersteunen, dan moet de metadata en het bestand apart gestuurd worden.
- We ondersteunen alleen student als personAffliations. In de toekomst zou dit nog altijd uitgebreid kunnen worden voor bijvoorbeeld 'Employee'.

- Onderstaande eigenschappen zijn noodzakelijk om studenten uit bron op te vragen en te identificeren op moment dat studentnummer niet aanwezig is, bijvoorbeeld vanuit een stroom waarbij brief via scan toegevoegd wordt aan DMS.

"dateOfBirth": "2003-09-30",
"gender": "F",


## Authenticatie:
scope voor toevoegen van inschrijving gerelateerde documenten: **okd:alldocuments** en **okd:enrollmentderollment**.
 als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.

