# OKD - Flow 4 - diplomering Document overdragen naar DMS
Aanbieden van diplomering gerelateerde documenten naar het DMS. Deze documenten worden opgeslagen in DMS als onderdeel van examendossier en gelinkt aan de students inschrijving.



## NOG TE DOEN:
 ZIE ook details van flow 1
Deze flows lijkt erg op flow 1, met het verschil dat de metainformatie in deze flow 

* een inschrijving van student

Dit word nog uitgewerkt zodra we de details van flow 1 100% uitgewerkt hebben om consistent te blijven

## verwerking in CMS
Het DMS kan zelf bepalen hoe de documenten opgelagen en verwerkt worden: logisch onder de student inschrijving dossier

## Authenticatie:
scope voor toevoegen van examen gerelateerde documenten: **okd:alldocuments** en **okd:diplomerendocuments**.
 als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.
