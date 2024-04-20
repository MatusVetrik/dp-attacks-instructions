# Sensitive data exposure

- Ďalším zvyčajným a bežným útokom býva sensitive dáta leakage. Aplikácie sú často stavané tak, že nie sú nastavené dostatočne oprávnenia nato čo je užívateľ dostupný vidieť.
  Konkrétne si predstavíme takúto chybnú konštrukciu pri routing.
- Tento krát si znova predstavíme zraniteľnosť na systéme juice shop (inštalácia link). Niektoré systémy majú chybne zapracovanie routingu, ktoré umožní prehľadávať súbory
  v podradených priečinkoch. K takejto zraniteľnosti nadchádza v prípade, že sa chybne použije file based routingu.
- Samozrejme proces zisťovania, ktorá linka je a ktorá nie je zraniteľná môže byť často zdĺhavý. Každopádne pri správnej detirminácii a analýzy kódu môžeme zistiť,
  ktoré linky sú dostupné a takto nájsť práve tu jednu. V konkrétnom prípade pre juice shop je to linke /ftp.
- Pri otvorení linky /ftp môžeme vidieť, že môžeme voľne prehľadávať súbory, ktoré sa tu nachádzajú. Sú viditeľné rôzne logy a súbory, ku ktorým by sme nemali mať prístup.