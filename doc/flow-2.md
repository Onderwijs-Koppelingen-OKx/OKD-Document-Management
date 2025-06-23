# OKD - Flow 2 - Document overdragen naar DMS
Aanbieden van examenresultaat en examenmoment gerelateerde documenten naar het DMS. Deze documenten worden opgeslagen in DMS als onderdeel van examendossier. 

## Flow 2.1 Document bij toetsresultaat overdragen naar DMS
### Sequence diagram
```mermaid
sequenceDiagram
    Participant Examineren
    Participant DMS

    Examineren->>+DMS: Overdragen document bij toetsresultaat (meta-data & inhoud)
    Note right of Examineren: endpoint /ooapi/documents POST (v6 heeft alleen GET!)
    DMS->>-Examineren: 200 - OK! (inclusief referentie)
```

Remark:
De volgende informatie wordt aangeboden richting DMS
- inhoud van het document
- document.bronorganisatie
- document.creatiedatum
- document.titel
- document.auteur
- document.taal
- document.type
- student
- afnamemoment toets
- toetsgegevens
- opleiding/verbintenis



### Class diagram van document bij toetsresultaat overdragen naar DMS
todo

### Example of request
```json
POST /documents
```
todo

Remarks:
- todo



## Flow 2.2 Document bij toetsmoment overdragen naar DMS
Todo! Identiek aan toetsresultaat bijlagen, maar excl. student.


## Flow 2.3 Document bij toetsmoment overdragen naar DMS




  




het DMS reageerd met een de link waar het document te bekijken is in het DMS.

Het student inschrijvings dossier wordt aangemaakt bij het sturen van het eerste document als het nog niet bestaat.

DMS krijgt de inhoud van het document indezelfde call als meta informatie

## Flow 1.1 Document overdragen vanuit Inschrijving aan DMS
### Sequence diagram
```mermaid
sequenceDiagram
    Participant Inschrijven
    Participant DMS

    Inschrijven->>+DMS: document (meta informatie & inhoud)
    DMS->>-Inschrijven: nieuwe link in DMS

```

### Class diagram 


### domain model
- document id
- persoon id, naam
- verbintenis id en bladnummer
- opleiding
- document type (mimetype)
- document catagory
- voorgestelde bewaartermijn (dagen)
- 

