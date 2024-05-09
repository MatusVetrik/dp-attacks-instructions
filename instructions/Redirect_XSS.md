# Redirect cross site scripting (XSS)

### Prostredie: Dp-Test-Enviroment - https://github.com/MatusVetrik/dp-test-enviroment

- Útok funguje na báze chybného používania odkazov a redirectov na iné podstránky aplikácií.
  Útočník využije zraniteľnosť na premiestnenie používateľa na neznámu stránku podľa výberu útočníka. Útok spočíva zvyčajne kliknutím na odkaz, ktorý sa reprezentuje
  ako odkaz na aplikáciu, ale v url má parameter, ktorý obsahuje adresu na škodlivú stránku.
- Pri prekliku z hlavnej stránky útokov sa nám do parametru URL nastavil atribút "redirect", ktorý obsahuje hodnotu reprezentujúcu absolútnu cestu k domovskej obrazovke. Hodnotu využíva tlačidlo "Go home" a po kliku na tlačidlo nás presmeruje na domovskú obrazovku aplikácie.
- Funkcionalita otvára možnosti útokov týkajúce sa presmerovania používateľa na nevhodnú stránku. Stačí do parametra vložiť adresu na akúkoľvek stránku
  a celú url vrátane domény a hostu poslať obeti s tým, že očakávame, že obeť klikne na odkaz. Pri kliku na odkaz a následne na tlačidlo bude presmerovaná na nami zvolenu stránku.
- Url môže vyzerať nasledovne: <pre><a href="http://localhost:63342/dp-test-enviroment/src/attacksPages/redirectXss.html?redirect=https://hackertyper.net/">
  http://localhost:63342/dp-test-enviroment/src/attacksPages/redirectXss.html?redirect=https://hackertyper.net/</a></pre>
- Pri kliku na url nás presmeruje na stránku, na ktorej sa nachádzame, ale už s nastaveným parametrom na škodlivú stránku.
- Po kliku na tlačidlo sme boli presmerovaný miesto domovskej obrazovky na stránku https://hackertyper.net.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/redirect-xss.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/redirect-xss.mp4)