# OKD - Flow 1 Opslaan ondersteunende documenten bij de inschrijving in DMS
Aanbieden van documenten rondom de inschrijving van een student.
Vanuit model Inschrijven naar het DMS: ondersteunende documenten bij de inschrijving die in het student/inschrijvings dossier horen.

## 1.1 nieuw document : Optie A, (DMS ontvangt meta data en binary)
### Sequence diagram 
```mermaid
sequenceDiagram
    Participant Inschrijven
    Participant DMS

    Inschrijven->>+DMS: POST .../okd/v1/document (meta informatie & inhoud)
    DMS->>-Inschrijven: nieuwe DMS referentie (UUID)

```
#### endpoints voor deze flow bij DMS
- `POST .../okd/v1/documents

voorbeeld :
```
POST .../okd/v1/documents
Host: api.yourdomain.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 2847
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="metadata"
Content-Type: application/json

{
    "consumers": [
        "consumerKey": "nl-okd",
        "documentType": "inschrijving",
        "documentSubtype" : "vrijstellingsaanvraag"
        "documentId: "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
        "documentName": "inschrijving-100245.pdf",
        "bewaartermijnsuggestie": "3Y",
        "association": {
            "associationId: "123e4567-e89b-12d3-a456-426614174000",
            "associationType": "programOfferingAssociation",
            "role": "student",
            "state": "associated",
            "primaryCode": {
                "codeType": "opleidingsblad",
                "code": "1.1"
            },
            "otherCodes": [
                {
                    "codeType": "opleidingscode",
                    "code": "23089"
                }
            ],            
            "consumers": [
                {
                    "consumerKey": "nl-okd",
                    "inschrijvingStartDate": "2021-09-01",
                    "inschrijvingExpectedEndDate": "2025-07-31",
                    "inschrijvingFinalEndDate": null
                }
            ],
            "person": {
                "personId": "111-2222-33-4444-222",
                "primaryCode": 
                {
                    "codeType": "studentNumber",
                    "code": "1234567"
                },
                "givenName": "Maartje",
                "surnamePrefix": "van",
                "surname": "Damme",
                "displayName": "Maartje van Damme",
                "activeEnrollment": true,
                "affiliations": 
                [
                    "student"
                ],
                "mail": "vandamme.mcw@student.roc.nl",
                "languageOfChoice":	[
                    "nl-NL"
                ],
                "otherCodes": [
                    {
                        "codeType": "eckid",
                        "code": "00000"
                    }
                ]
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
            },
 
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
    dmsdocumentid: "4e12169d-84b9-4d21-a987-f373bbbe4e6e"
}
```

## optie B, endpoint is metadata
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
    "association": {
        "associationId: "123e4567-e89b-12d3-a456-426614174000",
        "associationType": "programOfferingAssociation",
        "role": "student",
        "state": "associated",
        "primaryCode": {
            "codeType": "opleidingsblad",
            "code": "1.1"
        },
        "otherCodes": [
            {
                "codeType": "opleidingscode",
                "code": "23089"
            }
        ],            
        "consumers": [
            {
                "consumerKey": "nl-okd",
                "documentType": "inschrijving",
                "documentSubtype" : "vrijstellingsaanvraag"
                "documentId: "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
                "documentName": "inschrijving-100245.pdf",
                "bewaartermijnsuggestie": "3Y"
                "inschrijvingStartDate": "2021-09-01", 
                "inschrijvingExpectedEndDate": "2025-07-31",
                "inschrijvingFinalEndDate": null
            }
        ]
        "person": {
            "personId": "111-2222-33-4444-222",
            "primaryCode": 
            {
                "codeType": "studentNumber",
                "code": "1234567"
            },
            "givenName": "Maartje",
            "surnamePrefix": "van",
            "surname": "Damme",
            "displayName": "Maartje van Damme",
            "activeEnrollment": true,
            "affiliations": 
            [
                "student"
            ],
            "mail": "vandamme.mcw@student.roc.nl",
            "languageOfChoice":	[
                "nl-NL"
            ],
            "otherCodes": [
                {
                    "codeType": "eckid",
                    "code": "00000"
                }
            ]
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
        },
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
    dmsdocumentid: "4e12169d-84b9-4d21-a987-f373bbbe4e6e"
}
```

## Optie C (deze heeft niet onze voorkeur!)
## 1.1 nieuw document : Optie B, DMS ontvangt meta data en DMS haalt binary op) (deze heeft niet onze voorkeur!)

### Sequence diagram
```mermaid
sequenceDiagram
    Participant Inschrijven
    Participant DMS

    Inschrijven->>+DMS:  PUT .../okd/v1/documents/{documentid} (alleen meta informatie)
    DMS->>-Inschrijven: nieuwe referentie (UUID)
    DMS->>+Inschrijven:  GET .../okd/v1/documents/{documentid}
    Inschrijven->>-DMS: (binary content)



```
#### endpoints voor deze flow bij DMS
- `PUT .../okd/v1/documents/{documentId}` 
#### endpoints voor deze flow bij andere 
- `GET .../okd/v1/documents/{documentId}` 




### Class diagram of meta information
```mermaid
	classDiagram

    class programOfferingAssociation {
        offering: offeringId or programOffering object
        person: personId or Person object
        comsumers: nl-okd-assciation
    }
    class Person {
        "personId": "111-2222-33-4444-222",
        "primaryCode": 1234
        "codeType": "studentNumber- "1234567",
        "givenName": "Maartje",
        "surnamePrefix": "van",
        "surname": "Damme",
        "displayName": "Maartje van Damme",
        "activeEnrollment": true,
        "affiliations": "student",
        "mail": "vandamme.mcw\@student.roc.nl",
    }
    class ProgramOffering {
        offeringId : uuid-string
        primaryCode : IdentifierEntry
        offeringType : string = "program"
        name : LanguageTypedString[]
        description : LanguageTypedString[]
        teachingLanguage : string
        resultExpected : boolean
        consumers : nl-okd-Offering
        startDate : date-string
        endDate : date-string
        program : programId or Program object
        organization : organizationId or Organization object
    }
    class Program {
        "programId": "123e4567-e89b-12d3-a456-426614174000",
        "primaryCode":  "C12063128",
        "programType": "program",
        "name": "Netwerk- en mediabeheerder",
        "abbreviation": "N&M",
        "description":"In deze MBO-opleiding ...",
        "teachingLanguage": "nld",
        "modeOfStudy": "full-time",
        "levelOfQualification": "4"
    }

    class `nl-okd-assciation` {
        consumerKey : string = "nl-okd"
		"documentType": enum = "inschrijving",
        "documentSubtype" : string= "vrijstellingsaanvraag"
        "documentId: "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
        "documentName": "inschrijving-100245.pdf",
        "bewaartermijnsuggestie": "3Y"
        "inschrijvingStartDate": "2021-09-01", 
        "inschrijvingExpectedEndDate": "2025-07-31",
        "inschrijvingFinalEndDate": null
    }
    class Organization {
        organizationId : uuid-string
        primaryCode : IdentifierEntry
        organizationType : string
        name : string
        description : LanguageTypedString[]
        addresses : Address object[]
        link : uri-string
        logo : uri-string
        otherCodes : IdentifierEntry[]
        parent : organizationId or Organization object
        children : organizationId[] or Organization object[]
        validFrom : date-string
        validTo : date-string
    }
    programOfferingAssociation -- ProgramOffering
    programOfferingAssociation -- Person
    ProgramOffering --o Program
    programOfferingAssociation o-- `nl-okd-assciation`
    ProgramOffering --o Organization 

```



### Remarks
~~- Het DMS retourneert de documentreferentie (bijv UUID), hiermee kan document op een later moment gedownload of ingezien worden.~~
~~- Het student inschrijvings dossier wordt aangemaakt bij het sturen van het eerste document als het nog niet bestaat.~~
~~- DMS krijgt de inhoud van het document indezelfde call als meta informatie~~
- Berichten van maximaal 1 GB ondersteunen. Als we in de toekomst meer dan 1 GB willen ondersteunen, dan moet de metadata en het bestand apart gestuurd worden.


## Authenticatie:
scope voor toevoegen van inschrijving gerelateerde documenten: **okd:alldocuments** en **okd:inuitschrijven**.
 als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.

