# OKD - Flow 3 - BPV Document overdragen naar DMS
Aanbieden van bpv gerelateerde documenten naar het DMS. Deze documenten worden opgeslagen in DMS als onderdeel van het students inschrijving dossier.



## NOG TE DOEN:
 ZIE ook details van flow 1
Deze flows lijkt erg op flow 1, met het verschil dat de metainformatie in deze flow 

* bpv periode
* een inschrijving van student

Dit word nog uitgewerkt zodra we de details van flow 1 100% uitgewerkt hebben om consistent te blijven

## verwerking in CMS
Het DMS kan zelf bepalen hoe de documenten opgelagen en verwerkt worden: of een apart bpv dossier of alles onder de student inschrijving dossier

## Authenticatie:
scope voor toevoegen van examen gerelateerde documenten: **okd:alldocuments** en **okd:bpvdocuments**.
 als een van deze 2 aanwezig is in het authenticatie token kan de actie uitgevoerd worden.
