# Regex denial of service (ReDOS)

### Prostredie: Dp-Test-Enviroment - https://github.com/MatusVetrik/dp-test-enviroment

- Pri útoku si ukážeme Denial of service použitím zraniteľnej funkcionality regulárnych výrazov (regex).
- V testovacom prostredí na obrazovke nástrojov máme zobrazený vstup pre email. Vložený email bude skontrolovaný a regex výrok vyhodnotí, či ide o validný alebo nesprávne zostavený email.
- Pri náhľade zdrojového kódu môžeme vidieť, že regex reťazec je veľmi dlhý a komplexný, z čoho vyplýva, že jeho výpočet môže v istých prípadoch trvať dlhšie.
- Na základe tohto poznatku skúsime preťažiť výpočet regulárneho výrazu a tým spomaliť alebo celkovo vysadiť beh aplikácie.
- Do emailového vstupu môžeme postupne zapisovať dlhšie a dlhšie hodnoty. Postupným experimentom môžeme evidovať, že výpočty prebiehajú výrazne dlhšie každým pokusom.
- Finálne vložíme do vstupu reťazec dĺžky najmenej 30 znakov.
- Ako môžeme vidieť aplikácia nereaguje a obnovenie stránky nefunguje.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/redos.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/redos.mp4)