# Insecure derialization

- Insecure deserialization je …
- Tuto zranitelnost vieme vyuzit v ramci testovacieho prostredia (instalacia link).
- Utok spociva v tom, ze upravime ukladany bearer token, ktory obsahuje okrem udajov uzivatela aj jemu prisluchajuce roly.
- Po registraciii a prihlaseni sa do systemu si po otovreni vyvojarskej konzoly a tabu “application” mozeme vsimnut, ze sa do local storageu uklada token. Standartom pri takomto type tokenu je, ze je kodovany klucom base64.
- Token si skopirujeme a skusime ho dekodovat v softwarey na enkodovanie a dekodovanie base64 retazcov (link)
- Po dekodovoani tokenu mozeme vidiet, ze bola presne tento kluc pouzity na zakodovanie a teraz mame zretazeny json s udajmi o pouzivatelovi, teda o nas.
- Teraz si pridajme do polia “roles” dalsiu rolu a tou je rola admin
- Retazec znova zakodujme base64 klucom a vvlozme ho do local storageu aplikacie
- Po ulozeni tokenu a kliknutim na email v navigacnom bare sa nam otvori nas profil kde uz mozeme vidiet aj rolu admin. Takymto sposobom sme si privlastnili aspon cast adminskych opravneni.