# Cross site request forgery (CSRF)

### Prostredie: DVWA - https://github.com/digininja/DVWA  

- Cross Site Request Forgery (CSRF) spočíva v posielaní požiadavok na server z iného zdroja ako je naša aplikácia. To znamená,
  že sa odošle rovnaká požiadavka ako by sme odoslali z aplikácie ale z akéhokoľvek miesta na internete. V dnešnej dobe má väčšina prehliadačov defaultne nastavený atribút v hlavičke
  požiadavky, ktorý značí, že request môže byť odoslaný len v rámci jednej domény alebo aj “sameorigin”.

- Útok si predstavíme na systéme Damn Vulnerable Web App (DVWA), kde môžeme predviesť ideálny scenár na demonštráciu CSRF a to zmenou hesla prihláseného používateľa.

- Po prihlásení (admin:password) sa presunieme na položku “DVWA security” v menu aplikácie a nastavíme security level na “low”.

- Teraz sa môžeme presunúť na položku “csrf” v menu. Tu sa nachádza formulár na zmenu hesla.

- Zmeníme heslo a po potvrdení si všimnime atribúty v url. Pri odoslaní formuláru sa odošle po6iadavka, ktor8 obsahuje nové heslo a kontrolu nového hesla.

- Vytvoríme si skript, ktorý po6iadavku odošle z iného zdroju ako je aplikácia. Vytvoríme HTML súbor, ktorý vyzerá nasledovne:
   ```
    <html>
      <body>
         <h1>CSRF</h1>
         <img src="http://localhost:4280/vulnerabilities/csrf/?password_new=password1&password_conf=password1&Change=Change#"/>
      </body>
   </html>```
  
- V zdroji obrázku sa nachádza odkaz na adresu, z ktorej sa posiela požiadavka na zmenu hesla. Heslo môžeme ľubovoľne zmeniť, ale musí byť rovnaké aj v parametri "password_conf".

- Súbor si uložíme na zariadenie a otvoríme v prehliadači.

- POžiadavka sa automaticky odoslala a keď otvoríme položku "network" vo vývojárskej konzole tak uvidíme, že požiadavka prebehka úspešne. Kliknutím na požiadavku vo vývojarskej konzole sa 
  sa presunieme na obrazovku zmenu hesla v DVWA.

- Teraz skúsime odhlásenie a opätovné prihlásenie pod novými prihlasovacími údajmi aby sme si overili, že útok naozaj prebehol úspešne.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/csrf.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/csrf.mp4)