# Cross site request forgery (CSRF)

### Prostredie: DVWA - https://github.com/digininja/DVWA  

- Tento krát si predstavíme útok zvaný cross site request forgery (CSRF), ktorý spočíva v tom, že sa posielajú requesty na server z iného zdroja ako je naša aplikácia. To znamená,
  že sa odošle rovnaký request ako by sme odoslali z aplikácie ale z akéhokoľvek miesta na internete. V dnešnej dobe má väčšina prehliadačov defaultne nastavené atribúty v hlavičke
  requestu, ktorý značí, že request môže byť odslaný len v rámci jednej domény alebo aj “sameorigin”.

- Útok si predstavíme na systéme Damn Vulnerable Web App (DVWA), kde môžeme predviesť ideálny scenár na demonštráciu CSRF a to zmena hesla prihláseného používateľa.

- Inštalácia DVWA je detailne zobrazené na tejto linke -> link. Ideálnym riešením je si naklonovať repozitár a spustiť cez Docker-compose.

- Po prihlásení (admin:password) sa presunieme na položku “DVWA security” v menu aplikácie a nastavíme security level na “low”.

- Teraz sa môžeme presunúť na položku “csrf” v menu. Tu sa nachádza formulár na zmenu hesla.

- Zmeníme heslo a po potvrdení si všimnime atribúty v url. Pri odoslaní formuláru sa odošle request, ktorý obsahuje nové heslo a kontrolu nového hesla.

- Vytvoríme si skript, ktorý takýto request odošle z iného zdroju ako je aplikácia. Vytvoríme html súbor, ktorý vyzerá nasledovne. 
   ```
    <html>
      <body>
         <h1>CSRF</h1>
         <img src="http://localhost:4280/vulnerabilities/csrf/?password_new=password1&password_conf=password1&Change=Change#"/>
      </body>
   </html>```
  
- V zdroji obrázku sa nachádza linka na rovnakú adresu ako bol request pre zmenu hesla. Heslo môžeme zmeniť na ľubovoľnú hodnotu, musí byť však rovnaká v novom hesle ako aj v kontrole hesla.

- Súbor si uložíme na zariadenie a otvoríme v prehliadači.

- Request sa automaticky odoslal a keď si otvoríme vývojársku konzolu a network tak uvidíme, že prebehol úspešne. Môžeme naň rovno kliknúť v konzole a nastane redirect na csrf obrazovku DVWA.

- Teraz si skúsime odhlásenie a opätovné prihlásenie pod novými prihlasovacími údajmi aby sme si overili, že útok naozaj prebehol úspešne.