# Insecure direct object reference (IDOR)

- Insecure direct object reference je zranitelnost spocivajuca v tom, ked neumyselne sprostredkujeme pristup ku objektu, ktory by osoba bez dostacujucich prav nemala vidiet alebo ked je pristup k objektom zle navrhnuty
- Idealnym prikadom pre IDOR je pripad zobrazeny v systeme juiceshop (instalacia juiceshop prostredia). Pri zobrazeni vyvojarskek konzoly a tabu “network” mozeme vidiet rozne poziadavky. Vacsina z nich su obycajne dotahovanie kaskadnych stylov, ikon a skriptov ale pri vyfiltrovani XHttp requestov mozeme vidiet tie, ktore prijamo dopytuju server o data.
- Detailnou analyzou pri prehlidani odpovedi od servera mozeme najst zaujimave data ale nic co by sme vidiet nemohli. Az na jeden endpoint a tym je /api/users
- Tato api vracia podla identifikacneho cisla udaje o pouzivatelovi z databazy. Zrovna takato konstrukcia byva najcastejsim miestom kde da vyskuytuje IDOR.
- Pouzitim Burp nastroja (instalacia link) si vieme spustit interceptor, ktory odchyti kazdu poziadvku a spristupni nam detail poziadavky ako aj jej upravu.
- Po spusteni interceptora a odchyteni spominanej poziadavky si cele telo poziadavky skopirujeme a presunieme do repetera. Repeater umoznuje opakovane odosielanie poziadvky na server, cim vieme posielat poziadavky a zaroven ju aj upravovat.
- Skusime odoslat poziadvku s povodnym id a mozeme vidiet, ze sa nam vratili data s nasimi udajmi. Takymto sposobom mozeme skusat rozne id a ziskavat informacie o zaregistrovanych pouzivateloch a to napr. ich email, deluxeTokeny a rozne casove data o aktivite uzivatela. 
