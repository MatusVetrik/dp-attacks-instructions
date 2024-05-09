# Cross site request forgery (CSRF)

### Prostredie: DVWA - https://github.com/digininja/DVWA  

- Cross Site Request Forgery (CSRF) spočíva v posielaní požiadaviek na server z iného zdroja ako je naša aplikácia.  V dnešnej dobe má väčšina prehliadačov defaultne nastavený atribút v hlavičke
  požiadavky, ktorý značí, že požiadavka môže byť odoslanýálen v rámci jednej domény alebo aj “sameorigin”. V prípade, že takáto ochrana nie je zavedená, aplikácia je zraniteľná voči CSRF.

- Útok si predstavíme na systéme Damn Vulnerable Web App (DVWA), kde môžeme predviesť ideálny scenár na demonštráciu CSRF a to zmenou hesla prihláseného používateľa.

- Po prihlásení (admin:password) sa presunieme na položku “DVWA security” v menu aplikácii a nastavíme security level na “low”.

- Teraz sa môžeme presunúť na položku “csrf” v menu. Tu sa nachádza formulár na zmenu hesla.

- Zmeníme heslo a po potvrdení môžeme vidieť atribúty v url. Pri odoslaní formuláru sa odošle požiadavka, ktorá obsahuje nové heslo a kontrolu nového hesla.

- Vytvoríme kód, ktorý požiadavku odošle z iného zdroju ako je aplikácia. Vytvoríme HTML súbor, ktorý vyzerá nasledovne:
   ```
    <html>
      <body>
         <h1>CSRF</h1>
         <img src="http://localhost:4280/vulnerabilities/csrf/?password_new=password1&password_conf=password1&Change=Change#"/>
      </body>
   </html>```
  
- V zdroji obrázku sa nachádza odkaz na adresu, z ktorej sa posiela požiadavka na zmenu hesla. Heslo môžeme ľubovoľne zmeniť, ale musí byť rovnaké aj v parametri "password_conf".

- Súbor si uložíme na zariadenie a otvoríme v prehliadači.

- Požiadavka sa automaticky odoslala, keď otvoríme položku "network" vo vývojárskej konzole tak vidíme, že požiadavka prebehla úspešne. Kliknutím na požiadavku sa presunieme na obrazovku zmenu hesla v DVWA.

- Teraz skúsime odhlásenie a opätovné prihlásenie pod novými prihlasovacími údajmi aby sme si overili, že útok naozaj prebehol úspešne.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/csrf.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/csrf.mp4)