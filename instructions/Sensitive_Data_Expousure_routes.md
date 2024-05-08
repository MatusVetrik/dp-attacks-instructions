# Sensitive data exposure

### Prostredie: Juice-Shop - https://github.com/juice-shop/juice-shop#from-sources

- Predstavíme si chybnú konštrukcii pri routingu aplikácie, kde vďaka nedostatočným oprávnením na viditľnosti stránok získame citlivé súbory.
- Niektoré systémy majú chybne zapracovanie routingu, ktoré umožní prehľadávať súbory v podradených priečinkoch. K takejto zraniteľnosti dodchádza v prípade, že sa chybne použije file based routingu.
- Samozrejme zisťovanie, ktorá adresa je a ktorá nie je zraniteľná môže byť často zdĺhavý proces. Každopádne pri správnej detirminácii a analýzy kódu môžeme zistiť, ktoré adresy sú dostupné a zraniteľné.
- Pre juice shop je to adresa /ftp. Pri otvorení yavolaní adresy môžeme voľne prehľadávať súbory, ktoré sa tu nachádzajú. Sú viditeľné rôzne logy a súbory, ku ktorým by sme nemali mať prístup.

- Video návod sa nachádza v priečinku "video_instructions": /video_instructions/sensitive-data-exposure-routes.mp4 (https://github.com/MatusVetrik/dp-attacks-instructions/blob/main/video_instructions/sensitive-data-exposure-routes.mp4)
