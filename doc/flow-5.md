# OKD - Flow 5 melden van uitschrijven bij DMS
Notificeren dat een student zijn studie/verbintenis heeft beeindigt en de bewaartermijn van zijn documenten in mag gaan.

Vanuit component Uitschrijven naar het DMS



### Sequence diagram 
```mermaid
sequenceDiagram
    Participant Uitschrijven
    Participant DMS

    Uitschrijven->>+DMS: PATCH .../okd/v1_0/associations/{associationId}

    DMS->>-Uitschrijven: nieuwe DMS referentie (UUID)

```
#### endpoints voor deze flow bij DMS
- `PATCH .../okd/v1_0/associations/{associationId}`

voorbeeld :
```
PUT .../okd/v1_0/associations/123e4567-e89b-12d3-a456-426614174000
Host: api.yourdomain.com
Content-Type: application/json
boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 2847
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json


{
    "association": {
        "associationId: "123e4567-e89b-12d3-a456-426614174000",
        "associationType": "programOfferingAssociation",
        "role": "student",
        "state": "cancelled",
        "primaryCode": {
            "codeType": "opleidingsblad",
            "code": "1.1"
        },
        "otherCodes": [
            {
                "codeType": "opleidingscode",
                "code": "23089"
            }
        ]
        "person": "111-2222-33-4444-222",
 
        "offering": "5ffc6127-debe-48ce-90ae-75ea80756475",
    }
}
```

### Class diagram of meta information
```mermaid
	classDiagram

    class programOfferingAssociation {
        offering: offeringId or programOffering object
        "role": "student",
        "state": "cancelled",
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

    class `nl-okd-assciation` {
        consumerKey : string = "nl-okd"
        "inschrijvingStartDate": "2021-09-01", 
        "inschrijvingExpectedEndDate": "2025-07-31",
        "inschrijvingFinalEndDate": "2025-06-03"
    }

    programOfferingAssociation -- ProgramOffering
    programOfferingAssociation -- Person

    programOfferingAssociation o-- `nl-okd-assciation`

```

## Authenticatie:
scope die ook gebruikt is **okd:inuitschrijven**

dit geeft een fijnmazige authorizatie mogenlijk waarbij applicaties wel examen of bpv documenten mogen toevoegen, maar niet uitschrijven
