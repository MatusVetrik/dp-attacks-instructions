# Cross site request forgery (CSRF)

- Tento krat si predatavime utol zvany cross site request forgery (CSRF), ktory spociva v tom, ze sa posielaju requesty na server z ineho zdroja ako je nasa aplikacia. To znamena,
   ze sa odosle rovnaky request ako by sme odoslali z aplikacie ale z akehokolvek miesta na internete. V dnesnej dobe ma vacsina prehliadacov defaultne nastaveny atribuy v hlavicke
   requestov, ktory znaci, ze request moze byt poslany len v ramci jednej domeny alebo aj “sameorigin”.

- Utok si predstavime na systeme Damn Vulnerable Web App (DVWA), kde mozeme predviest idealny scenar na demonstraciu csrf a to zmena hesla prihlaseneho pouzivatela.

- Instalacia DVWA je detailne zobrazene na tejto linke -> link. Idealnym riesenim je si naclonovat repozitar a spusti cez Docker-compose.

- Po prihlaseni (admin:password) sa presunieme na polozku “DVWA security” v menu aplikacie a nastavime security level na “low”.

- Teraz sa mozeme presunut na polozku “csrf” v menu. Tu sa nachadza formular na zmenu hesla.

- Zmeňe heslo a po potvrdeni si vsimnime atrubuty v url. Pri odoslani formularu sa posle request, ktory obsahuje nove heslo a kontrolu noveho hesla.

- Vytvorime si skript, ktory takyto request odosle z ineho zdroju ako je aplikacia. Vytvorme html subor, ktory vyzera nasledovne.

- V zdroji obrazku sa nachadza linka na rovnaku adresu ako bol request pre zmenu hesla. Heslo mozeme zmenit na lubovolnu hodnotu, musi byt vsal rovnaka v novom hesle aj v kontrole.

- Subor si ulozme na plochu a otvorme ho v prehliadaci.

- Request sa automaticky odoslal a ked si otvorime vyvojarsku konzolu a network tal uvidime, ze prebehol uspesne. Mozeme nam rovno kliknut v konzole a nastane redirect na csrf obrazovku DVWA.

- Teraz sa mozeme skusit odhlasit a prihlasit novymi prihlasovacimi udajmi aby sme si overili, ze utok naozaj prebehol uspesne.