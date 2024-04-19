# Sensitive data exposure

- Dalsim zvycajnym a beznym utokom byva sensitive data leakage. Aplikacie su casto stavane tak, ze nie su nastavene dostatocne opravnenia nato co je uzivatel dostupny vidiet.
            Konkretne si predstavime takuto chybnu konstrukciu pri routing.
- Tento krat si znova predstavime zranitelnost na systeme juice shop (instalacia link). Niektore systemy maju chybne zapracovanie routingu, ktore umozni prehladavat subory
           v podradenych priecinkoch. K takejto zramitelnosti nadchadza v pripade, ze sa chybne pouzije file based routingu
- Smaozrejme proces zistovania, ktora linka je a ktora nie je zranitelna moze byt casto zdlhavy. Kazdopadne pri spravnej detirminacii a analyzy kodu mozeme zistit,
           ktore linky su dostupne a takto najst prave tu jednu. V konkretnom pripade pre juice shop je to linke /ftp
- Pri otovreni linky /ftp mozeme vidiet, ze mozeme volne prehladavat subory, ktore sa tu nachadzaju. Su viditelne rozne logy a subory, ku ktorym by sme nemali mat pristup