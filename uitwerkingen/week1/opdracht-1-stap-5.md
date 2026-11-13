# Opdracht

Bekijk de bestanden in de repo. Noteer minstens drie dingen die ontbreken om de app automatisch te testen, te bouwen,
uit te rollen of in de gaten te houden. Geef per punt in één zin aan waarom het nodig is. Dit lijstje gebruik je in
opdracht 3.

# Uitwerking

| Tekortkoming                                                                                                                   | Waarom is dit nodig?                                                                                     |
|--------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| De pipeline definitie ontbreekt volledig.                                                                                      | Nodig om o.a. de build, test en release stappen te orkestreren en maakt de automatisering mogelijk.      |
| Release infrastructure                                                                                                         | Nodig om de applicatie op een server uit te rollen.                                                      |
| Een build script                                                                                                               | Nodig om de broncode van de applicatie te bouwen tot uitvoerbare code die op de infrastructuur draait.   |
| De health check status (healthy / degraded) reflecteert niet echt de belasting van de server maar of de database bereikbaar is | Een echte status is nodig om te bepalen wanneer opgeschaald moet worden.                                 |
| Logging wordt niet weggeschreven                                                                                               | Nodig om het systeem in de gaten te kunnen houden en op afwijkingen of bijzonderheden te kunnen reageren. |