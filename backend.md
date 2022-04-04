# HAMSTERWARS backend

+ Backend: REST API med Node.js, Express och Firestore
+ Frontend: web app med React

Bilder och JSON data som du behöver för uppgiften finns i detta repo.


---
## Specifikation
#### Godkänt-nivå
##### Hamster-objekt
Ett hamster-objekt ska innehålla följande egenskaper. Du får lägga till fler om du behöver, men du får inte ta bort några. Observera att wins/defeats/games ska vara noll ursprungligen.

| Egenskap | Datatyp | Värde |
|:---------|:--------|:------|
|id        |string   |Id för hamster-objektet (när man hämtar från databasen) |
|name      |string   |Hamsterns namn |
|age       |number   |Hamsterns ålder i hela år |
|favFood   |string   |Hamsterns favoritmat |
|loves     |string   |Hamsterns favoritaktivitet |
|imgName   |string   |Namn på filen som har bild på hamstern |
|wins      |number   |Antal vinster |
|defeats   |number   |Antal förluster |
|games     |number   |Antal matcher totalt |

Hamster-objekt ska lagras som *documents* i en *collection* i Firestore. Du ska bygga ett API som med hjälp av datan har följande resurser. *Id* ska vara en unik kod, som används i API-resurser när man behöver referera till ett specifikt objekt. (Du ska inte skapa id själv, det görs av Firestore när man lägger till ett objekt. Innan man lagt till ett hamster-objekt i databasen finns alltså inget id.)

##### Statiska resurser
En statisk resurs är en fil som webbservern ska kunna skicka som svar på ett request, med hjälp av `express.static` middleware. Exempel: HTML, CSS, JS, JPG.

| Statisk resurs  | Respons        |
|-----------------|----------------|
| `/:filnamn`       | Frontend-filer |
| `/img/:filnamn`   | Hamsterbild    |


##### Statuskoder
Alla API-resurser ska returnera JSON eller en HTTP statuskod:
+ 200 (ok) - Om servern lyckats med att göra det som resursen motsvarar.
+ 400 (bad request) - Om requestet är felaktigt gjort, så att servern inte kan fortsätta. Exempel: om POST /hamsters skickar med ett objekt som inte är ett hamster-objekt.
+ 404 (not found) - Om resursen eller objektet som efterfrågas inte finns. Exempel: id motsvarar inte något dokument i databasen. `GET /hamsters/felaktigt-id`
+ 500 (internal server error) - Om ett fel inträffar på servern. Använd `try + catch` för att fånga fel.


##### Endpoints
| Metod  | Resurs          | Body | Respons |
|:-------|:----------------|------|----------------------------|
| GET    | `/hamsters`     | -    | Array med alla hamsterobjekt  |
| GET    | `/hamsters/random` | -    | Ett slumpat hamsterobjekt  |
| GET    | `/hamsters/:id` | -    | Hamsterobjekt med ett specifikt id.<br>404 om inget objekt med detta id finns. |
| POST   | `/hamsters`     | Hamster-objekt (utan id) | Id för det nya objekt som skapats i databasen. Ska returneras inuti ett objekt: `{ id: "..." }`.  |
| PUT    | `/hamsters/:id` | Ett objekt med ändringar.  | Bara statuskod. |
| DELETE | `/hamsters/:id` | -    | Bara statuskod. |

*Obs! Exempel på ändringsobjekt för PUT: för att sätta antalet vinster till 10 och antalet matcher till 12 ska du använda ändringsobjektet `{ wins: 10, games: 12 }`.*



---
#### VG-nivå
##### Match-objekt
Appen ska spara resultatet av genomförda matcher i databasen, i matchobjekt.

| Egenskap | Datatyp | Värde |
|:---------|:--------|:------|
|id        |string   |Matchens id |
|winnerId  |string   |Id för vinnande hamstern |
|loserId   |string   |Id för förlorande hamstern |

##### Endpoints
| Metod  | Resurs          | Body | Respons |
|:-------|:----------------|------|----------------------------|
| GET    | `/matches`     | -    | Array med alla matchobjekt  |
| GET    | `/matches/:id` | -    | Matchobjekt med ett specifikt id. |
| POST   | `/matches`     | Match-objekt (utan id) | Ett objekt med id för det nya objekt som skapats i databasen: `{ id: "123..." }` |
| DELETE | `/matches/:id` | -    | Bara statuskod. |
| GET    | `/matchWinners/:id` | -    | Array med matchobjekt för alla matcher, som hamstern med *id* har vunnit. Statuskod 404 om id inte matchar en hamster som vunnit någon match.  |
| GET    | `/winners`      | -    | En array med hamsterobjekt för de 5 som vunnit flest matcher   |
| GET    | `/losers`       | -    | En array med hamsterobjekt för de 5 som förlorat flest matcher   |
| GET    | `/hamsters/cutest` | -    | Array med objekt för de hamstrar som vunnit flest matcher. |

Endpoint /hamsters/cutest är till för att du ska kunna visa på appens startsida vilken hamster som är sötast. Vi räknar precis som man räknar målskillnad i sportens värld: vinster minus förluster. Det kan i teorin bli oavgjort ibland. Hur det ska visas för användaren, är ett problem som vi löser i frontend-delen av projektet. Exempel:
* Snurre har vunnit tio matcher och förlorat en. `10 - 1 === 9`
* Pelle har vunnit tre matcher och förlorat noll. `3 - 0 === 3`
* Osvald har vunnit 20 matcher och förlorat 11. `20 - 11 === 9`
* Snurre och Osvald har lika stor målskillnad, fler än alla andra; därför ska bådas hamster-objekt finnas i arrayen.


#### Level ups
Resurser som är bra träning, men inte nödvändiga för högsta betyg.

1. `GET /defeated/:hamsterId`  - array med id för alla hamstrar den valda hamstern har besegrat
1. `GET /score/:challenger/:defender`  - två hamster-id som parameter. Respons ska vara ett objekt `{ challengerWins, defenderWins }` med antal vinster för respektive hamster, när de har mött varandra.
1. `GET /fewMatches`  - returnerar en array med id för de hamstrar som spelat minst antal matcher. Arrayen ska ha minst ett element.
1. `GET /manyMatches`  - returnerar en array med id för de hamstrar som spelat flest antal matcher. Arrayen ska ha minst ett element.



---
## Frågor och svar
**Q:** Kan man importera data i Firestore, så man slipper skriva in allt manuellt? <br>
**A:** Ja! Skriv skript som du kan köra direkt med Node. Skapa en mapp `scripts/` i ditt backend-projekt med filerna: resetDatabase.js, importData.js. [Den här videon](https://www.youtube.com/watch?v=Qg2_VFFcAI8) kan vara till hjälp.

**Q:** jag skulle vilja ha testdata, där användaren har klickat på några hamstrar. Finns det? <br>
**A:** nej, men det är lätt att skapa själv. Gör en kopia av `data.json`, `testData.json`, och lägg in några matcher i den manuellt. Gör ett skript `importTestData.js`.

**Q:** Hur ska man ladda upp nya bilder på hamstrar? <br>
**A:** Man ska inte ladda upp bilder. Användaren ska skriva en URL som är en länk till en bild på nätet. Man ska även kunna skriva en länk till en fil som redan finns på Express-servern - för att det ska vara lättare för dig att testa.

**Q:** Kan jag veta om jag är godkänd? <br>
**A:** Ja, genom att redovisa på en lektion eller genom att köra test-skriptet på ditt API. Se [README.md](README.md).

**Q:** Ska man använda TypeScript? <br>
**A:** TypeScript är valfritt att använda i backend-delen, men obligatoriskt i frontend.

**Q:** Allt i test-skriptet blir rött? <br>
**A:** Kontrollera att du har installerat CORS middleware i Express. Test-skriptet måste ha det för att kunna köras.

**Q:** Det fungerar i Insomnia (eller Postman), men inte i testskriptet? <br>
**A:** Du har inte följt specifikationen. Läs igenom tabellen **Endpoints** igen!

**Q:** Det fungerar på *localhost*,  men inte när jag pushat till *Heroku*? <br>
**A:** Du måste skriva koden flexibel, så att den gör olika saker beroende på om den körs på localhost eller Heroku. Använd *environment variables* för **porten** och **Firestore-nyckeln**.

**Q:** Jag får felet "App crashed" på Heroku? <br>
**A:** Titta i Herokus loggfiler. Alla fel och allt som du skriver ut med console.log kommer där.



---
## Bedömning
Observera att du måste följa specifikationen *exakt*. Du *måste* använda de namn och statuskoder som står här.

För *godkänt* krävs
+ korrekt inlämning
+ du använder teknikerna Node, Express och Firestore
+ ditt API är publicerat online
+ ditt API följer specifikationen
+ ditt API implementerar G-nivån

För *väl godkänt* krävs dessutom
+ ditt API implementerar VG-nivån


---
## Inlämning
Du ska lämna in på LearnPoint: ditt repo och länkar till GitHub och din publicerade app.
+ Fil: ladda upp repot som en **zip-fil** (ta inte med node_modules)
+ Kommentar: skriv länkarna till både **GitHub** och **publicerad app**

Exempel - så här ska du skriva länkarna:
```
GitHub: https://github.com/din-github-användare/ditt-repo
Publicerad: https://my-hamsterswars-submission.herokuapp.com
```
