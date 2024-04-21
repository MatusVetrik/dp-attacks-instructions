# Sensitive data exposure 

### Prostredie: Dp-Test-Enviroment - https://github.com/MatusVetrik/dp-test-enviroment

- Všeobecne nie je správne nechávať na tvrdo napísané citlivé údaje priamo v kóde, v čistom javascripte už vôbec. Vo vývojárskej konzoli je naformátovaný zdrojový kód kde si vieme všetko dohľadať.
- Klávesou F12 si vieme otvoriť vývojársku konzolu. Preklikneme sa na tab "Sources" a môžeme vidieť všetky zdrojové kódy, ktoré boli pri načítavaní strnaky načítané.
- Postupne môžeme prechádzať skrípt po skripte a hľadať citlivé údaje. Jedným z nich môže byť napríklad API key, ktorý je v podstate prístupový kľúč ku danej službe. Nás v tomto prípade
  bude zaujímať prístupový kľúč ku Firebase API, vďaka ktorému budeme môcť získať akékoľvek údaje uložené vo Firebase úcte vývojára aplikácie alebo vlastníka aplikácie.
- Kliknime na ľubovoľný script a kombináciou kláves Ctrl + F dáme vyhľadávať kľúčové slovo v súbore. Skúsme vyhľadať "apiKey" a postupne prechádzať jednotlivé súbory kým nenájdeme zhodu.

      const firebaseConfig = {
              apiKey: "AIzaSyCx_Vvo5GYYtbjFgSm0GtQv7TWb6mrAZWg",
              authDomain: "diplomovka-test-app.firebaseapp.com",
              projectId: "diplomovka-test-app",
              storageBucket: "diplomovka-test-app.appspot.com",
              messagingSenderId: "548553603020",
              appId: "1:548553603020:web:6628aba4fcd24cbf19cf69",
            };
- Zhoda bola nájdená v súbore auth.js a nachádza sa tu celá konfigurácia pre použitie služby.
- Teraz si môžeme vytvoriť vlastný skrípt a zavolať ľubovoľné metódy z API a sprístupniť rôzne dáta o zaregistrovaných používateľoch.