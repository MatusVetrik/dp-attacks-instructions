# Sensitive data exposure 

- Vseobecne nie je spravne nechavat na tvrdo napisane citlive udaje priamo v kode, v cistom javascripte uz vobec. Vo vyvojarskej konzoli je naformatovany zdrojovy kod kde si vieme vsetko dohladat.
- Klavesou F12 si vieme orvorit vyvojarsku konzolu. Preklikneme sa na tab "Sources" a mozeme vidiet vsetky zdrojove kody, ktore boli pri nacitavani strnaky nacitane.
- Postupne mozeme prechadzat skript po skripte a hladat citilive udaje. Jednym z nich moze byt napriklad API key, ktory je v podstate pristupovy kluc ku danej sluzbe. Nas v tomto pripade
        bude zaujimat pristupovy kluc ku Firebase API, vdaka ktoremu budeme moct zistak akekolvek udaje ulozene vo Firebase ucte vyvojara aplikacie alebo vlastnika aplikacie.
- Kliknime na lubovolny skript a kombinaciou klaves Ctrl + F dame vyhladavat klucove slovo v subore. Skusme vyhladat "apiKey" a postupne prechadzat jednotlive subory kym nenajdeme zhodu.
    <pre>
            const firebaseConfig = {
              apiKey: "AIzaSyCx_Vvo5GYYtbjFgSm0GtQv7TWb6mrAZWg",
              authDomain: "diplomovka-test-app.firebaseapp.com",
              projectId: "diplomovka-test-app",
              storageBucket: "diplomovka-test-app.appspot.com",
              messagingSenderId: "548553603020",
              appId: "1:548553603020:web:6628aba4fcd24cbf19cf69",
            };
        </pre>
- Zhoda bola najdena v subore auth.js a nachadza sa tu cela konfiguracia pre pouzitie sluzby.
- Teraz si mozeme vytvorit vlastny skript a zavolat lubovolne metody z API a spristupnit rozne data o zaregistrovanych pouzivateloch.