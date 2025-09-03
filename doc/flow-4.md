# OKD - Flow 4 - diplomering Document overdragen naar DMS
Aanbieden van diplomering gerelateerde documenten naar het DMS. Deze documenten worden opgeslagen in het DMS als onderdeel van examendossier en gelinkt aan de student inschrijving.

## NOG TE DOEN:
ZIE ook details van flow 1
Deze flows lijkt erg op flow 1, met het verschil dat de metainformatie in deze flow 

* Een inschrijving van student

Dit word nog uitgewerkt zodra we de details van flow 1 100% uitgewerkt hebben om consistent te blijven

## Verwerking in CMS
Het DMS kan zelf bepalen hoe de documenten opgelagen en verwerkt worden: logisch onder de student inschrijving dossier

## Remarks
- Berichten van maximaal 1 GB ondersteunen. Als we in de toekomst meer dan 1 GB willen ondersteunen, dan moet de metadata en het bestand apart gestuurd worden.

## Authenticatie:
Scope voor toevoegen van examen gerelateerde documenten: **okd:alldocuments** en **okd:diplomerendocuments**.
Als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.
