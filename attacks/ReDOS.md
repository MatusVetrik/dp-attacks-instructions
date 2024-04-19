# Regex denial of service (ReDOS)

- Pri tomto utoku si ukazeme DOS (Denial of service) utok pouzitim zranitelnej funkcionality regex vyrokov
- V testovacom prostredi mame zobrazeny input pre email. Vlozeny email bude zvalidovany a regex vyrok vyhodnoti, ci ide o validny alebo nespravne zostaveny email
- Pri nahlade zdrojoveho kodu mozeme vidiet, ze regex retazec je velmi dlhy a komplxeny, z coho vyplyva, ze jeho vypocet moze v istych pripadoch trvat dlhsie
- Na zaklade tohto poznatku skusime pretazit vypocet a tym spomalit alebo celkovo vysadit beh aplikacie
- Do emailoveho vstupu mozeme postupne zapisovat dlhsie a dlhsie hodnoty, cim spozorujeme, ze vypocty priebehaju vyrazne dlhsie kazdym pokusom
- Finalne vlozime do inputu retazec dlzky najmenej 30 znakov
- Ako mozeme vidiet aplikacia nereaguje, refresh stranky nefunguje. Aplikovali sme uspesny reDOS utok.