# Insecure derialization

### Prostredie: Dp-Test-Enviroment - https://github.com/MatusVetrik/dp-test-enviroment

- Útok spočíva v tom, že upravíme ukladaný bearer token, ktorý obsahuje údaje používateľa a jemu prislúchajúce roly.
- Po registrácii a prihlásení sa do systému si po otvorení položky "application" vo vývojárskej konzole  môžeme všimnúť, že sa do lokálneho úložiska ukladá token. Štandartom pri takomto type tokenu je, že je kódovaný schémou base64.
- Token si skopírujeme a skúsime ho dekódovať v softvéry na enkódovanie a dekódovanie base64 reťazcov (https://www.base64encode.org/).
- Po dekódovaní tokenu máme zreťazený json s údajmi o používateľovi, teda o našom profile.
- Pridáme do poli “roles” ďaľšiu rolu "admin", ktorá reprezentuje administrátorské právomoci.
- Reťazec znova zakódujme base64 schémou a vložíme ho naspäť do lokálneho úložiska aplikácie.
- Po uložení tokenu a kliknutím na email v navigácii sa nám otvorí náš profil. V profilemôžeme vidieť naše roly, vrátane "admin" roly. Takýmto spôsobom sme si privlastnili administrátorské oprávnenia.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/insecure-deserialization.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/insecure-deserialization.mp4)