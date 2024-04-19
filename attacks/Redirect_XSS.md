# Redirect cross site scripting (XSS)

- Predstavime si utok zvany Redirect XSS (Cross site scripting). Utok funguje na baze chybneho pouzivania linkov a redirectov na ine podstranky aplikacii.
        Utocnik vyuzije tuto zranitelnost na premiestnenie pouzivatela na nezname stranku podla vyberu utocnika. Utok spociva zvycajne nevinnym kliknutim na link, ktory sa reprezentuje
        ako link na aplikacie, co je v podstate pravda ale v url ma parameter, ktory obsahuje link na skodlivu stranku.
- Pri prkeliku z hlavnej stranky utokov sa nam do parametru url nastavil atribut "redirect", ktory obsahuje hodnotu, ktora repzrezentuje absolutnu cestu kam
        mame byt nasledne presmerovany. Tuto hodnotu vyuziva tlacidlo "Click to go home" a po kliku na tlacidlo nas presmeruje na domovsku obrazovku aplikacie.
- Takato funkcionalita otvara moznosti utokov tykajuce sa presmerovania pouzivatela na nevhodnu stranku. Staci name do parametra vlozit link na akukolvek stranku
        a tento cely link vratane domeny a hostu poslat obeti s tym, ze ocakvame ze obet klikne na link. Obet klikne na link a pri kliku na tlacidlo ju presmeruje na nami zvolenu stranku.
- Url moze vyzerat nasledovne:<pre><a href="http://localhost:63342/dp-test-enviroment/src/attacksPages/redirectXss.html?redirect=https://hackertyper.net/">
            http://localhost:63342/dp-test-enviroment/src/attacksPages/redirectXss.html?redirect=https://hackertyper.net/</a></pre> 
- Pri kliku na url nas presmeruje na stranku na ktorej sa nachadzame ale uz s nastavenym parametrom na skodlivu stranku.
- Po kliku na tlacidlo sme boli presmerovany na "skodlivu" stranku https://hackertyper.net.