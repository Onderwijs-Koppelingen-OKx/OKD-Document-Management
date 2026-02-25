# OKD - Flow 11 Aanmelden ondersteunende documenten bij de examinering in DMS

```
Note: Deze flow zijn geen onderdeel van de 1.0 versie van de OKD. Ze zijn als voorstel toegevoegd aan de spec, maar zijn nog in ontwikkeling. de definiteve versie kan verschillen. Deze versie is niet opgebouwd met OOAPI taal. In de definitieve versie zal dat wel zo zijn.
```

Als er documenten met betrekking op de examinering van een student in het DMS gecreerd of opgeslage worden die niet afkomstig zijn van de MORA examinering module dan kan het DMS deze documenten aanmelden bij de module zodat deze getoond en opgevraagd kunnen worden. De inhoud van het document word niet uitgewisseld. 

Het DMS bepaalt het documentID en stuurt deze, samen met genoeg meta informatie over student en inschrijving zodat de inschrijf module het document goed kan registreren en tonen in de applicatie

Note: In het response zit wel het documentId van de component, die anders kan zijn dan die van het DMS. Bij het opvragen en verdere communicatie os het DMSid de identificatie

### Sequence diagram 
```mermaid
sequenceDiagram
    Participant Examineren
    Participant DMS

    DMS->>+Examineren: POST .../okd/v1/documents/_registerdmsdocument (meta informatie)
    Examineren->>-DMS: nieuwe referentie (UUID)

```
#### endpoints voor deze flow bij DMS
- `POST .../okd/v1/documents/_registerdmsdocument`

voorbeeld :
```
POST .../okd/v1/documents/_registerdmsdocument
Host: api.yourdomain.com
Content-Type: application/json
Content-Length: xxxxx
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json


{
    "dmsDocumentId": "dbd3e12a-ed8b-4488-ac34-26fd4f64f40b",
    
    "documentName": "Toets-4-20260102-100245.pdf",
    "format": "application/pdf",
    "documentsize": 243857
    "description": "Toets ingescanned "
    "receivedDate": "2026-01-05",
    "registrationDate": "2026-01-02",

    "documentType": "examination",
    "documentSubtype": "Toetsresultaat",

    "personId": "5ab399b8-c499-4da8-af6d-b55e66251f31",
    "studentNumber": "1234567"
    
    "associationId": "123e4567-e89b-12d3-a456-426614174000"
    "sequenceCode": "1.2"
}

```

Response:
```
{
    "documentId": "4e12169d-84b9-4d21-a987-f373bbbe4e6e"
}
```

### Remarks
- als identificatie van de student heeft "personId" de voorkeur. Indien deze niet beschikbaar is kan "studentnumber"
- als identificatie van de juiste inschrijving heeft "associationId" de voorkeur. Indien deze niet beschikbaar is kan "sequenceCode" gebruikt worden
- als het document niet aan de inschrijving gekoppelt hoeft te zijn (algemeen persoonlijk document, inschrijving overstijgend), dan is het weglaten van associationId en sequenceCode de indicatie dat het document aan de persoon toegevoegd word ipv inschrijving
- De inhoud van de documenten wordt niet aangeboden, alleen de registratie dat het document bestaat en aan het dossier van de student inschrijving toegevoegd kan worden

## Authenticatie:
scope voor toevoegen van examen gerelateerde documenten: **okd:alldocuments** en **okd:examdocuments**.
 als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.

