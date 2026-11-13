# Opdracht
In deze opdracht ga je een eerste DevOps-plan voor dotNOVI schetsen. Dit wordt de basis voor je eindopdracht (deelopdracht 1):

- Neem je lijstje uit opdracht 1 als startpunt.
- Kies per fase van de DevOps lifecycle een tool.
- Beschrijf per fase in 1 of 2 zinnen wat je daar gaat doen.
- Kies een branching-strategie en beschrijf waarom deze past bij dotNOVI.

# Uitwerking
> **Notitie**: Ik heb iets meer tools uitgekozen en iets meer omschreven. Daarnaast zijn niet per sé alle punten uit de antwoorden van opdracht 1 stap 5 hiermee opgelost, zo weet ik bijvoorbeeld niet hoe ik logs precies wegschrijf en wat ik daarvoor kan inzetten.

## Tools per DevOps lifecycle fase
|DevOps lifecycle fase|Tool(s)|Gedachten|
|-|-|-|
|Plan|GitHub projects|Project bijhouden d.m.v. een Kanban bord met taken|
|Code|GitHub, git, VS Code (voorkeur)|- Git voor version control<br/>- GitHub repository om de broncode op te hosten<br/>- GitHub repository branch policies voor governance en controle
|Build|GitHub Actions, git, Docker|**Docker voor containerisatie van de applicatie**<br/>**GitHub Actions als pipeline met de volgende stappen:**<br/>- Repository clonen<br/> - Image maken met docker build (Dockerfile met twee builds, één voor de release (o.a. zonder tests en minified) en één voor test)<br/>- Configureer een hardened docker image van Ubuntu als startpunt (Zorgt ervoor dat de build altijd op dezelfde manier op hetzelfde OS wordt uitgevoerd waardoor het altijd werkt)<br/>- Kopieer nodige bestanden afhankelijk van de build (release/test)<br/>- Zorg dat npm in de image wordt geïnstalleerd<br/>- Installeer dependencies (geen dev dependencies voor release)<br/>- Image wegschrijven als artifact met automatisch versienummer (release-build-1.0.0)/(test-build-1.0.0)
|Test|GitHub Actions, Docker, Docker Compose, jest|**GitHub Actions als pipeline met de volgende stappen:**</br>- Docker compose file voor unit testen runnen (ontbreken in huidig project)<br/>- Test build image uit artifacts opspinnen (test-build-1.0.0)<br/>- Unit testen uitvoeren (goedkoop en snel, vroeg falen) (gebruikt jest)<br/>- Docker compose file voor integratie testen runnen<br/>- Database container opspinnen met test data (indien nodig)<br/>- Integratie testen uitvoeren (gebruikt jest)</br>**Unit en integratie testen parallel uitvoeren (stap moet direct falen wanneer een van de twee faalt)**|
|Release|GitHub Actions, Docker|**GitHub Actions als pipeline met de volgende stap:**<br/>- Release image wegschrijven naar een gedeelde container registry|
|Deploy|GitHub Actions, Terraform, Kubernetes|**Terraform voor infrastructure as code**</br>**Kubernetes voor blue/green switch en/of rolling deployment zonder downtime**</br>**GitHub Actions als pipeline met de volgende stappen:**</br>- Infrastructuur met infrastructure as code deployen van Terraform<br/>- Eerst infra, daarna release-build-1.0.0 ophalen uit de gedeelde container registry en deployen via Kubernetes |
|Operate|Kubernetes|Kubernetes voor auto-scaling o.b.v. health checks|
|Monitor|Grafana|Grafana om logs en metrics inzichtelijk te maken|

## Gekozen branching-strategie en reden
**<u>GitHub Flow is de gekozen branching-strategie</u>** omdat deze eenvoudig is, maar nog steeds de mogelijkheid biedt om branch-protection op de main branch te ondersteunen. Dit waarborgt stabiliteit beter dan een trunk-based branching-strategie doordat PR's afgedwongen kunnen worder waarbij verplichte checks eerst moeten slagen voordat naar de main branch gemerged kan worden. GitFlow is te uitgebreid en overbodig voor dit project.