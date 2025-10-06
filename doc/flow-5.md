# OKD - Flow 5 Melden van uitschrijven bij DMS
Notificeren dat een student zijn studie/verbintenis heeft beëindigd en de bewaartermijn van zijn documenten in mag gaan.

Vanuit component Uitschrijven naar het DMS


### Sequence diagram 
```mermaid
sequenceDiagram
    Participant Uitschrijven
    Participant DMS

    Uitschrijven->>+DMS: PATCH .../okd/v1/associations/{associationId}

    DMS->>-Uitschrijven: 204 No-content

```
#### Endpoints voor deze flow bij DMS
- `PATCH .../okd/v1/associations/{associationId}`

voorbeeld:
```
PATCH .../okd/v1/associations/123e4567-e89b-12d3-a456-426614174000
Host: api.yourdomain.com
Content-Type: application/json
boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 2847
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json


{
    "associationId: "123e4567-e89b-12d3-a456-426614174000",
    "associationType": "programOfferingAssociation",
    "role": "student",
    "state": "finished",
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
    "person": "5ab399b8-c499-4da8-af6d-b55e66251f31",
    "offering": "5ffc6127-debe-48ce-90ae-75ea80756475"
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
     	"initials": "MCW",
		"dateOfBirth": "2003-09-30",
		"gender": "F",
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
        "enrollmentStartDate": "2021-09-01", 
        "enrollmentExpectedEndDate": "2025-07-31",
        "enrollmentFinalEndDate": "2025-06-03"
    }

    programOfferingAssociation -- ProgramOffering
    programOfferingAssociation -- Person

    programOfferingAssociation o-- `nl-okd-assciation`

```

## Authenticatie:
scope die ook gebruikt is  **okd:alldocuments** en **okd:enrollmentderollment**

Dit geeft een fijnmazige authorizatie mogelijkheid waarbij applicaties wel examen of BPV documenten mogen toevoegen, maar niet uitschrijven.
