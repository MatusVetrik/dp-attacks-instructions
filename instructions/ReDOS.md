# Regex denial of service (ReDOS)

- Pri tomto útoku si ukážeme DOS útok použitím zraniteľnej funkcionality regex výrokov
- V testovacom prostredí máme zobrazený input pre email. Vložený email bude zvalidovaný a regex výrok vyhodnotí, či ide o validný alebo nesprávne zostavený email
- Pri náhľade zdrojového kódu môžeme vidieť, že regex reťazec je veľmi dlhý a komplexný, z čoho vyplýva, že jeho výpočet môže v istých prípadoch trvať dlhšie
- Na základe tohto poznatku skúsime preťažiť výpočet a tým spomaliť alebo celkovo vysadiť beh aplikácie
- Do emailového vstupu môžeme postupne zapisovať dlhšie a dlhšie hodnoty, čím spozorujeme, že výpočty prebiehajú výrazne dlhšie každým pokusom
- Finálne vložíme do inputu reťazec dĺžky najmenej 30 znakov
- Ako môžeme vidieť aplikácia nereaguje, refresh stránky nefunguje. Aplikovali sme úspešný reDOS útok.