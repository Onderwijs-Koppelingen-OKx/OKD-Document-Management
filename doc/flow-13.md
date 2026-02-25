# OKD - Flow 13 Aanmelden ondersteunende documenten bij de bpv in DMS

```
Note: Deze flow zijn geen onderdeel van de 1.0 versie van de OKD. Ze zijn als voorstel toegevoegd aan de spec, maar zijn nog in ontwikkeling. de definiteve versie kan verschillen. Deze versie is niet opgebouwd met OOAPI taal. In de definitieve versie zal dat wel zo zijn.
```

Als er documenten met betrekking op de BPV van een student in het DMS gecreerd of opgeslagen worden die niet afkomstig zijn van de MORA bpv module dan kan het DMS deze documenten aanmelden bij de module zodat deze getoond en opgevraagd kunnen worden. De inhoud van het document word niet uitgewisseld. 

Het DMS bepaalt het documentID en stuurt deze, samen met genoeg meta informatie over student en inschrijving zodat de bpv module het document goed kan registreren en tonen in de applicatie

Note: In het response zit wel het documentId van de component, die anders kan zijn dan die van het DMS. Bij het opvragen en verdere communicatie os het DMSid de identificatie


### Sequence diagram 
```mermaid
sequenceDiagram
    Participant BPV
    Participant DMS

    DMS->>+BPV: POST .../okd/v1/documents/_registerdmsdocument (meta informatie)
    BPV->>-DMS: nieuwe referentie (UUID)

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
    "dmsDocumentId": "d47379a6-1a66-474a-94e9-9c8090014397",
    
    "documentName": "BedrijfsVerzekeringBewijs-PimVerboon.pdf",
    "format": "application/pdf",
    "documentsize": 55857
    "description": "Bewijs van verzekering"
    "receivedDate": "2023-09-01",
    "registrationDate": "2023-09-01",

    "documentType": "bpv",
    "documentSubtype": "bedrijfsinformatie",

    "personId": "5ab399b8-c499-4da8-af6d-b55e66251f31",
    "studentNumber": "1234567"
    
    "associationId": "123e4567-e89b-12d3-a456-426614174000"
    "sequenceCode": "1.2"
}


```

Response:
```
{
    "documentId": "def8ddd7-b6fd-45a0-a75d-03c16254a688"
}
```

### Remarks
- als identificatie van de student heeft "personId" de voorkeur. Indien deze niet beschikbaar is kan "studentnumber"
- als identificatie van de juiste inschrijving heeft "associationId" de voorkeur. Indien deze niet beschikbaar is kan "sequenceCode" gebruikt worden
- als het document niet aan de inschrijving gekoppelt hoeft te zijn (algemeen persoonlijk document, inschrijving overstijgend), dan is het weglaten van associationId en sequenceCode de indicatie dat het document aan de persoon toegevoegd word ipv inschrijving
- De inhoud van de documenten wordt niet aangeboden, alleen de registratie dat het document bestaat en aan het dossier van de student inschrijving toegevoegd kan worden

## Authenticatie
Scope voor toevoegen van BPV gerelateerde documenten: **okd:alldocuments** en **okd:bpvdocuments**.
Als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.

