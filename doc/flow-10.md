# OKD - Flow 10 Opvragen documentsubtypes

```
Note: Deze flow zijn geen onderdeel van de 1.0 versie van de OKD. Ze zijn als voorstel toegevoegd aan de spec, maar zijn nog in ontwikkeling. de definiteve versie kan verschillen. Deze versie is niet opgebouwd met OOAPI taal. In de definitieve versie zal dat wel zo zijn.
```

Als er documenten van een student in het DMS gecreerd of opgeslagen worden die niet afkomstig zijn van de module dan kan het DMS deze documenten aanmelden bij de module zodat deze getoond en opgevraagd kunnen worden. hierbij moet er extra meta informatie megegeven worden. bij elk documentType kan een school zelf subtypes gedefinieerd hebben. Deze moeten door het dms opgevraagd kunen worden. 

### Sequence diagram 
```mermaid
sequenceDiagram
    Participant Module
    Participant DMS

    DMS->>+Module: GET .../okd/v1/documents/documenttypes (meta informatie)
    Module->>-DMS: documenttypes with subtypes

```
#### endpoints voor deze flow bij DMS
- `GET .../okd/v1/documents/documenttypes`

voorbeeld :
```
GET .../okd/v1/documents/documenttypes
Host: api.yourdomain.com
Content-Type: application/json
Content-Length: xxxxx
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json


```

Response:
```
[
    {
        "documentType": "enrollment",
        "documentSubTypes": [
            "documentSubtype": "vrijstellingsaanvraag",
            "documentSubtype": "Aanmelding",
            "documentSubtype": "etc. etc"
        ]
    },
    {
        "documentType": "examination",
        "documentSubTypes": [
            "documentSubtype": "toetsresultaat",
            "documentSubtype": "toetsverslag",
            "documentSubtype": "presentieLijst",
            "documentSubtype": "etc. etc"
        ]
    },
    {
        "documentType": "bpv",
        "documentSubTypes": [
            "documentSubtype": "poc",
            "documentSubtype": "bedrijfs Informatie",
            "documentSubtype": "etc. etc"
        ]
    },
        {
        "documentType": "graduation",
        "documentSubTypes": [
            "documentSubtype": "diploma voorkant",
            "documentSubtype": "diploma foto",
            "documentSubtype": "etc. etc"
        ]
    }
]
```

### Remarks
- De documentSubtypes zijn door de school aanpasbaar, er kunnen geen aannames gedaan worden.


## Authenticatie:
voor het uitvoeren van dit request is geen specifieke scope nodig. een geldig token is voldoende

