# Redirect cross site scripting (XSS)

- Útok funguje na báze chybného používania linkov a redirectov na iné podstránky aplikácií.
  Útočník využije túto zraniteľnosť na premiestnenie používateľa na neznáme stránku podľa výberu útočníka. Útok spočíva zvyčajne nevinným kliknutím na link, ktorý sa reprezentuje
  ako link na aplikácie, čo je v podstate pravda ale v url má parameter, ktorý obsahuje link na škodlivú stránku.
- Pri prekliku z hlavnej stránky útokov sa nám do parametru url nastavil atribút "redirect", ktorý obsahuje hodnotu, ktorá reprezentuje absolútnu cestu kam
  máme byť následne presmerovaný. Túto hodnotu využíva tlačidlo "Go home" a po kliku na tlačidlo nás presmeruje na domovskú obrazovku aplikácie.
- Takáto funkcionalita otvára možnosti útokov týkajúce sa presmerovania používateľa na nevhodnú stránku. Stačí name do parametra vložiť link na akúkoľvek stránku
  a tento celý link vrátane domény a hostu poslať obetí s tým, že očakávame, že obeť klikne na link. Obeť klikne na link a pri kliku na tlačidlo ju presmeruje na nami zvolenu stránku.
- Url môže vyzerať nasledovné:<pre><a href="http://localhost:63342/dp-test-enviroment/src/attacksPages/redirectXss.html?redirect=https://hackertyper.net/">
  http://localhost:63342/dp-test-enviroment/src/attacksPages/redirectXss.html?redirect=https://hackertyper.net/</a></pre>
- Pri kliku na url nás presmeruje na stránku na ktorej sa nachádzame ale už s nastaveným parametrom na škodlivú stránku.
- Po kliku na tlačidlo sme boli presmerovaný na "škodlivú" stránku https://hackertyper.net.