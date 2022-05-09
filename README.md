# Web app: HAMSTERWARS
### Fullstackutveckling

**Din uppgift är att bygga en fullstack lösning för kommande virala sensationen, webplatsen HAMSTERWARS.**
Webbplatsen är en spinoff på [Kittenwar](http://www.kittenwar.com), en hemsida där matcher mellan två bilder slumpas fram och besökarna röstar på den de finner gulligast. Poäng ska räknas, listor ska sammanställas.

Uppgiften är uppdelad i två delar: **backend** och **fullstack**. Del 1 handlar om att bygga ett API. Du ska i del 2 bygga en frontend till API:et med React. Båda delarna ska publiceras online på Heroku.

1. [Del 1 - backend](backend.md)
1. [Del 2 - frontend](frontend.md)


### Hur börjar jag? (fullstack)
Vi går igenom detta på en lektion. Leta upp presentationen eller inspelningen på Teams. Här är en förkortad variant:
1. Skapa ett *frontend-projekt* med `npm create vite@latest`. Välj React och TypeSript. Du ska alltså inte utgå från det här repot, utan göra ett nytt, eget.
1. `cd namnet-på-ditt-projekt/`
1. Lägg till alla filer i git med `git init`.
1. Installera alla npm-paket du behöver för backend och frontend. De ska alltså installeras i samma projekt. Du ska bara ha ett git-repo.
1. Skapa mappen `backend`. Kopiera över din backend-kod från backend-inlämningen in i den mappen.
1. Tänk på att använda *environment variables* och *.gitignore* för att undvika att hemligheter ligger i ditt repo.
1. Publicera till Heroku.


### Test-skript
Skriv `npm run test-frontend` för att starta en lokal webbserver. Öppna index.html i en webbläsare och följ instruktionerna på sidan för att testa.

Detta använder du för att kontrollera om ditt API uppfyller specifikationen.


---
