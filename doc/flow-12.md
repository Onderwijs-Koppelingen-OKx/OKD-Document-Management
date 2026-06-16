# OKD - Flow 12 Aanmelden ondersteunende documenten bij de examinering in DMS

```
Note: Deze flow zijn geen onderdeel van de 1.0 versie van de OKD. Ze zijn als voorstel toegevoegd aan de spec, maar zijn nog in ontwikkeling. de definitieve versie kan verschillen. Deze versie is niet opgebouwd met OOAPI taal of concepten. In de definitieve versie zal dat wel zo zijn.
```

Als er documenten met betrekking op de examinering van een student in het DMS gecreëerd of opgeslagen worden die niet afkomstig zijn van de MORA-examinering module dan kan het DMS deze documenten aanmelden bij de module zodat deze getoond en opgevraagd kunnen worden. De inhoud van het document wordt niet uitgewisseld. 

Het DMS bepaalt het documentID en stuurt deze, samen met genoeg meta-informatie over student en inschrijving zodat de inschrijf module het document goed kan registreren en tonen in de applicatie.

#### Opmerking: Hier kan een overlap met de OKE ontstaan.
In de OKE kunnen documenten direct gelinkt worden aan het afnamemoment omdat die informatie al eerder in de OKE-flows uitgewisseld is. Het scenario in deze flow is dat er op een examen document op een manier het volledige toets code in barcode/QR formaat staat. Hiermee kan het document aan het resultaat gekoppeld worden (is niet hetzelfde als aan de toets, ivm pogingen etc). De ontvangende module zal op basis van de code zijn best doen om het document te koppelen. lukt dit niet dan wordt het een algemeen document in het examendossier van de student. 

Documenten gerelateerd aan een toetsmoment (procesverbaal, presentielijst) worden obv de toetsmoment uuid geregistreerd. Dit uuid kan in een barcode/QR formaat worden opgenomen waarna het DMS met deze informatie het document bij het juiste toetsmoment kan registreren.

Note: In het response zit wel het documentId van de component, die anders kan zijn dan die van het DMS. Bij het opvragen en verdere communicatie is het DMSDocumentId de identificatie

### Sequence diagram 
```mermaid
sequenceDiagram
    Participant Examineren
    Participant DMS

    DMS->>+Examineren: POST .../okd/v1/documents/_registerdmsdocument (meta informatie)
    Examineren->>-DMS: nieuwe referentie (UUID)

```
#### endpoints voor deze flow bij SIS
- `POST .../okd/v1/documents/_registerdmsdocument`
voorbeeld 1 (voor resultaatbijlage) :
```
POST .../okd/v1/documents/_registerdmsdocument
Host: api.yourdomain.com
Content-Type: application/json
Content-Length: xxxxx
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json


{
    "dmsDocumentId": "e575f30d-25e6-4e7e-8f9c-b081413a99f3",
    
    "documentName": "Toets-4-20260102-100245.pdf",
    "format": "application/pdf",
    "documentSize": 243857,
    "description": "Toets ingescanned",
    "receivedDate": "2026-01-05",
    "registrationDate": "2026-01-02",

    "documentType": "examination",
    "documentSubtype": "Toetsresultaat",

    "personId": "5ab399b8-c499-4da8-af6d-b55e66251f31",
    
    "examComponentOfferingId": "7742f703-08c2-426b-868a-1d548071a91e"
}

```

Response:
```
{
    "documentId": "4e12169d-84b9-4d21-a987-f373bbbe4e6e"
}
```

voorbeeld 2 (voor resultaatbijlage) :
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
    "documentSize": 243857,
    "description": "Toets ingescanned",
    "receivedDate": "2026-01-05",
    "registrationDate": "2026-01-02",

    "documentType": "examination",
    "documentSubtype": "Toetsresultaat",

    "personId": "5ab399b8-c499-4da8-af6d-b55e66251f31",
    
    "associationId": "123e4567-e89b-12d3-a456-426614174000",

    "examCode": "NE-3F-N1",
    "examCodePath": "/EIND/SE/Toets1"
}

```

Response:
```
{
    "documentId": "b5779660-bd2a-4138-9a03-23b4b2703307"
}
```

voorbeeld (voor procesverbaal) :
```
POST .../okd/v1/documents/_registerdmsdocument
Host: api.yourdomain.com
Content-Type: application/json
Content-Length: xxxxx
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json


{
    "dmsDocumentId": "7bb8ba50-70a5-4dc0-9c78-d9cf3cddc40c",
    
    "documentName": "Procesverbaal_toetsmoment_20260602.pdf",
    "format": "application/pdf",
    "documentSize": 243857,
    "description": "Procesverbaal ingescanned",
    "receivedDate": "2026-06-03",
    "registrationDate": "2026-06-04",

    "documentType": "examination",
    "documentSubtype": "Toetsmoment",
    
    "examComponentOfferingId": "7742f703-08c2-426b-868a-1d548071a91e"
}

```

Response:
```
{
    "documentId": "9e7d2f85-5e9c-464c-8294-1c770a003956"
}
```

### Remarks
- als identificatie van de student heeft "personId" de voorkeur. Indien deze niet beschikbaar is kan "studentNumber" gebruikt worden
- als identificatie van de juiste inschrijving heeft "associationId" de voorkeur. Indien deze niet beschikbaar is kan "sequenceCode" gebruikt worden
- als het document niet aan de inschrijving gekoppeld hoeft te zijn (algemeen persoonlijk document, inschrijving overstijgend), dan is het weglaten van associationId en sequenceCode de indicatie dat het document aan de persoon toegevoegd wordt ipv inschrijving
- Indien het toetsmoment bekend is waar het resultaat betrekking op heeft dan kan het document (resultaatbijlage) op de juiste plek geregistreerd worden met de examComponentOfferingId en de personId of studentNumber.Als het toetsmoment of de studuent niet gevonden kan worden wordt een foutmelding gegeven. Zie voorbeeld 1 (resultaatbijlage)
- Alternatief is om op basis van examCode, examCodePath en de receivedDate een document (resultaatbijlage) op de juiste plek te registreren. Indien dit niet lukt, wordt er een foutmelding gegeven. De poging wordt bepaald op basis van de receivedDate. Als dit niet te bepalen is wordt de laatste poging gebruikt. Zie voorbeeld 2 (resultaatbijlage)
- voor het registreren van een document bij een toetsmoment (bijv. procesverbaal of presentielijst) moet alleen de examComponentOfferingId worden aangeboden. Als het toetsmoment gevonden kan worden obv dit uuid wordt het document bij dit toetsmoment geregistreerd. Als het toetsmoment niet gevonden kan worden wordt een foutmelding gegeven
- De inhoud van de documenten wordt niet aangeboden, alleen de registratie dat het document bestaat en aan het dossier van de student inschrijving toegevoegd kan worden

## Authenticatie:
Scope voor toevoegen van examen gerelateerde documenten: **okd:alldocuments** en **okd:examdocuments**.
Als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.
