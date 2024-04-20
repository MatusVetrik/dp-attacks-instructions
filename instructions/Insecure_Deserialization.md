# Insecure derialization

- Zraniteľnosť vieme využiť v rámci testovacieho prostredia (inštalácia link).
- Útok spočíva v tom, že upravíme ukladaný bearer token, ktorý obsahuje okrem údajov užívateľa aj jemu prislúchajúce roly.
- Po registrácii a prihlásení sa do systému si po otvorení vývojárskej konzoly a tabu “application” môžeme všimnúť, že sa do local storageu ukladá token. Štandartom pri takomto type tokenu je, že je kódovaný kľúčom base64.
- Token si skopírujeme a skúsime ho dekódovať v softwarey na enkódovanie a dekódovanie base64 reťazcov (link)
- Po dekódovaní tokenu môžeme vidieť, že bola presne tento kľúč použitý na zakódovanie a teraz máme zreťazený json s údajmi o používateľovi, teda o nás.
- Teraz si pridajme do polia “roles” ďalšiu rolu a tou je rola admin
- Reťazec znova zakódujme base64 kľúčom a vložíme ho do local storageu aplikácie
- Po uložení tokenu a kliknutím na email v navigačnom bare sa nám otvorí nás profil kde už môžeme vidieť aj rolu admin. Takýmto spôsobom sme si privlastnili aspoň časť adminských oprávnení.