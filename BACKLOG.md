# Initiative

Dať športovcovi prehľad o tom, koľko ho šport stojí. Zapísať výdavok mu trvá do 15 sekúnd a mesačný súčet podľa kategórií vidí na jednej obrazovke do 2 klepnutí.

Každá user story má akceptačné kritériá (AK) a funkčné testy (T…), ktoré ich overujú. Story je hotová, keď sú splnené všetky jej AK a prešli všetky jej testy. Testovacie prostredie, nefunkčné testy a unit testy sú v časti [Testovanie](#testovanie) na konci súboru.

---

## Epic 1: Zápis výdavku

**User story 1.1:** Ako športovec chcem pridať výdavok so sumou, dátumom a kategóriou, aby som mala všetky výdavky na šport na jednom mieste.
- Tasks: tabuľka `expenses` v SQLite, formulár na pridanie výdavku, validácia sumy
- Akceptačné kritériá:
  - AK1: Formulár má polia suma, dátum, kategória, poznámka a tlačidlo „Uložiť".
  - AK2: Suma sa dá zadať s desatinnou čiarkou aj bodkou (12,50 aj 12.50).
  - AK3: Výdavok sa uloží, len keď je suma väčšia ako 0, má najviac dve desatinné miesta a je vybraná kategória.
  - AK4: Pri neplatnej alebo prázdnej sume alebo chýbajúcej kategórii sa výdavok neuloží a pri poli sa zobrazí chybová hláška.
  - AK5: Po uložení sa formulár zatvorí a nový výdavok je v zozname.
  - AK6: Uložený výdavok zostane v aplikácii aj po jej zatvorení a opätovnom otvorení.
- Testy (unit test: U1):

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T1.1-1 | AK1, AK3, AK5 | Čisté dáta | Výdavky → „Pridať" → suma `25,90`, kategória Vybavenie → „Uložiť" | Formulár sa zatvorí, v zozname je výdavok 25,90 €, Vybavenie, 24.11.2026 |
| T1.1-2 | AK2 | – | Pridať výdavok so sumou `12.50` a kategóriou Doprava | Výdavok sa uloží a v zozname je 12,50 € |
| T1.1-3 | AK3, AK4 | – | Zadať sumu `0` a kategóriu → „Uložiť". Zopakovať so sumou `-5` | Výdavok sa neuloží, zobrazí sa hláška „Suma musí byť väčšia ako 0" |
| T1.1-4 | AK3, AK4 | – | Zadať sumu `10,555` a kategóriu → „Uložiť" | Výdavok sa neuloží, zobrazí sa hláška „Suma môže mať najviac dve desatinné miesta" |
| T1.1-5 | AK4 | – | Nechať sumu prázdnu, vybrať kategóriu → „Uložiť" | Výdavok sa neuloží, zobrazí sa hláška „Zadaj sumu" |
| T1.1-6 | AK4 | – | Zadať sumu `20`, kategóriu nevybrať → „Uložiť" | Výdavok sa neuloží, zobrazí sa hláška „Vyber kategóriu" |
| T1.1-7 | AK6 | Existuje výdavok z T1.1-1 | Reštart aplikácie | Výdavok 25,90 € je stále v zozname |

**User story 1.2:** Ako športovec chcem, aby bol dátum predvolene dnešný, aby ma zápis nezdržiaval.
- Tasks: predvyplnený dnešný dátum, výber iného dátumu
- Akceptačné kritériá:
  - AK1: Pri otvorení formulára nového výdavku je v poli dátum dnešný dátum.
  - AK2: Po klepnutí na dátum sa otvorí výber dátumu a zvolený dátum sa zobrazí v poli.
  - AK3: Výdavok sa uloží so zvoleným dátumom, aj keď je v minulosti alebo v budúcnosti.
  - AK4: Dátum sa na obrazovke zobrazuje vo formáte DD.MM.RRRR.
- Testy (unit test: U4):

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T1.2-1 | AK1, AK4 | – | Výdavky → „Pridať" | V poli dátum je 24.11.2026 |
| T1.2-2 | AK2, AK3 | – | „Pridať" → klepnúť na dátum → zvoliť 3.11.2026 → suma `15`, kategória → „Uložiť" | Výdavok je v zozname s dátumom 03.11.2026 |
| T1.2-3 | AK3 | – | Pridať výdavok s dátumom 10.12.2026 (budúci) | Výdavok sa uloží s dátumom 10.12.2026 |

**User story 1.3:** Ako športovec chcem k výdavku pridať poznámku, aby som si neskôr pamätala, na čo som peniaze minula.
- Tasks: pole pre poznámku vo formulári
- Akceptačné kritériá:
  - AK1: Poznámka je nepovinná, výdavok sa uloží aj bez nej.
  - AK2: Poznámka má najviac 200 znakov, pole viac znakov nedovolí zadať.
  - AK3: Poznámka sa zobrazí pri výdavku v zozname a celá vo formulári pri úprave.
- Testy:

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T1.3-1 | AK3 | – | Pridať výdavok s poznámkou `Nové tretry` → uložiť → otvoriť ho zo zoznamu | Poznámka je viditeľná v zozname aj vo formulári |
| T1.3-2 | AK1 | – | Pridať výdavok bez poznámky | Výdavok sa uloží, v zozname nie je žiadna poznámka |
| T1.3-3 | AK2 | – | Do poznámky vložiť text dlhý 250 znakov | Pole obsahuje len prvých 200 znakov |

## Epic 2: Kategórie výdavkov

**User story 2.1:** Ako športovec chcem vybrať kategóriu zo základnej sady (vybavenie, štartovné, tréner, doprava...), aby som výdavky rýchlo roztriedila.
- Tasks: tabuľka `categories`, naplnenie základných kategórií pri prvom spustení, výber kategórie vo formulári
- Akceptačné kritériá:
  - AK1: Po prvom spustení je vo výbere kategórie presne 9 základných kategórií zo `SPEC.md`.
  - AK2: Pri ďalšom spustení sa základné kategórie nepridajú znova (nevzniknú duplikáty).
  - AK3: Výdavok sa dá uložiť s ktoroukoľvek základnou kategóriou a v zozname sa zobrazí jej názov.
  - AK4: Základné kategórie sa nedajú zmazať ani premenovať.
- Testy:

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T2.1-1 | AK1 | Čisté dáta | Spustiť aplikáciu → „Pridať" → otvoriť výber kategórie | Vo výbere je presne 9 kategórií: Vybavenie, Štartovné a súťaže, Tréner a tréningy, Členské a poplatky, Doprava, Ubytovanie, Strava a doplnky, Zdravie a regenerácia, Iné |
| T2.1-2 | AK2 | Po T2.1-1 | Reštart aplikácie dvakrát → otvoriť výber kategórie | Stále 9 kategórií, žiadna sa neopakuje |
| T2.1-3 | AK3 | – | Pridať výdavok s kategóriou Zdravie a regenerácia | V zozname je pri výdavku názov „Zdravie a regenerácia" |
| T2.1-4 | AK4 | – | Vo výbere kategórie podržať prst na základnej kategórii | Neponúkne sa zmazanie ani premenovanie |

**User story 2.2:** Ako športovec chcem pridať vlastnú kategóriu, aby som pokryla výdavky, ktoré základná sada nemá.
- Tasks: obrazovka alebo dialóg na novú kategóriu, uloženie do databázy
- Akceptačné kritériá:
  - AK1: Vo výbere kategórie je možnosť „+ Nová kategória", ktorá otvorí dialóg s poľom na názov.
  - AK2: Po uložení sa nová kategória pridá do výberu a vo formulári sa hneď nastaví ako vybraná.
  - AK3: Prázdny názov (aj len z medzier) sa neuloží a zobrazí sa hláška.
  - AK4: Názov, ktorý už existuje (aj s inými veľkými a malými písmenami), sa neuloží a zobrazí sa hláška „Kategória už existuje".
  - AK5: Medzery na začiatku a na konci názvu sa pri uložení odstránia.
  - AK6: Vlastná kategória je vo výbere aj po reštarte aplikácie.
- Testy (unit test: U6):

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T2.2-1 | AK1, AK2 | – | „Pridať" → výber kategórie → „+ Nová kategória" → `Servis bicykla` → „Uložiť" | Dialóg sa zatvorí, vo formulári je vybraná kategória Servis bicykla a je aj vo výbere |
| T2.2-2 | AK3 | – | V dialógu nechať názov prázdny → „Uložiť". Zopakovať s názvom z troch medzier | Kategória sa neuloží, zobrazí sa hláška „Zadaj názov kategórie" |
| T2.2-3 | AK4 | – | V dialógu zadať `vybavenie` → „Uložiť" | Kategória sa neuloží, zobrazí sa hláška „Kategória už existuje" |
| T2.2-4 | AK5 | – | V dialógu zadať `  Masáže  ` → „Uložiť" | Vo výbere je kategória „Masáže" bez medzier |
| T2.2-5 | AK6 | Po T2.2-1 | Reštart aplikácie → otvoriť výber kategórie | Kategória Servis bicykla je vo výbere |

## Epic 3: Zoznam a úprava výdavkov

**User story 3.1:** Ako športovec chcem vidieť zoznam výdavkov od najnovšieho, aby som mala prehľad, čo som naposledy zapísala.
- Tasks: obrazovka so zoznamom, zoradenie podľa dátumu
- Akceptačné kritériá:
  - AK1: Výdavky sú zoradené podľa dátumu od najnovšieho. Pri rovnakom dátume je vyššie ten, ktorý bol zapísaný neskôr.
  - AK2: Pri každom výdavku je suma v eurách (napríklad 12,50 €), dátum, názov kategórie a poznámka, ak ju má.
  - AK3: Keď nie sú žiadne výdavky, zobrazí sa text „Zatiaľ nemáš žiadne výdavky" a tlačidlo „Pridať".
  - AK4: Po pridaní, úprave alebo zmazaní výdavku sa zoznam aktualizuje bez reštartu aplikácie.
- Testy (unit test: U2):

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T3.1-1 | AK1 | Čisté dáta | Pridať výdavky s dátumami 1.11., 15.11. a 5.11.2026 (v tomto poradí) | Poradie v zozname: 15.11., 5.11., 1.11. |
| T3.1-2 | AK1 | – | Pridať výdavok A a hneď po ňom výdavok B, oba s dnešným dátumom | B je v zozname nad A |
| T3.1-3 | AK3 | Čisté dáta | Otvoriť záložku Výdavky | Zobrazí sa text „Zatiaľ nemáš žiadne výdavky" a tlačidlo „Pridať" |
| T3.1-4 | AK2, AK4 | – | Pridať výdavok 7,5 €, Doprava, poznámka `Vlak na pretek` | Bez reštartu je v zozname: 7,50 €, dátum, Doprava, Vlak na pretek |

**User story 3.2:** Ako športovec chcem upraviť výdavok, aby som mohla opraviť preklep v sume alebo kategórii.
- Tasks: otvorenie výdavku zo zoznamu, formulár predvyplnený existujúcimi údajmi, uloženie zmien
- Akceptačné kritériá:
  - AK1: Klepnutím na výdavok v zozname sa otvorí formulár predvyplnený jeho sumou, dátumom, kategóriou a poznámkou.
  - AK2: Po uložení sa v zozname zobrazia nové údaje a počet výdavkov sa nezmení (nevznikne nový záznam).
  - AK3: Pri úprave platia rovnaké pravidlá validácie ako pri novom výdavku.
  - AK4: Po odchode z formulára bez uloženia zostane výdavok nezmenený.
  - AK5: Zmena sa prejaví aj v Prehľade (súčet a graf).
- Testy:

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T3.2-1 | AK1 | Existuje výdavok 25,90 €, Vybavenie, poznámka `Tretry` | Klepnúť na výdavok v zozname | Formulár má sumu 25,90, jeho dátum, kategóriu Vybavenie a poznámku Tretry |
| T3.2-2 | AK2 | Ako T3.2-1 | Zmeniť sumu na `29,90` → „Uložiť" | V zozname je 29,90 €, počet výdavkov je rovnaký ako pred úpravou |
| T3.2-3 | AK5 | Ako T3.2-1 | Zmeniť kategóriu na Doprava → „Uložiť" → otvoriť Prehľad | V legende je suma pri Doprave, Vybavenie túto sumu už nemá |
| T3.2-4 | AK3 | Ako T3.2-1 | Zmeniť sumu na `0` → „Uložiť" | Zobrazí sa hláška, po návrate do zoznamu má výdavok pôvodnú sumu |
| T3.2-5 | AK4 | Ako T3.2-1 | Zmeniť sumu na `99` → stlačiť späť bez uloženia | V zozname má výdavok pôvodnú sumu |

**User story 3.3:** Ako športovec chcem zmazať výdavok, aby som sa zbavila omylom zapísaných záznamov.
- Tasks: tlačidlo na zmazanie, potvrdzovací dialóg
- Akceptačné kritériá:
  - AK1: Tlačidlo „Zmazať" je len vo formulári existujúceho výdavku, nie pri novom výdavku.
  - AK2: Po klepnutí na „Zmazať" sa zobrazí potvrdzovací dialóg s možnosťami „Zrušiť" a „Zmazať".
  - AK3: Po potvrdení sa výdavok odstráni z databázy, zo zoznamu aj z Prehľadu.
  - AK4: Po „Zrušiť" zostane výdavok nezmenený.
- Testy:

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T3.3-1 | AK1 | – | Výdavky → „Pridať" | Vo formulári nového výdavku nie je tlačidlo „Zmazať" |
| T3.3-2 | AK2, AK4 | Existuje výdavok | Otvoriť výdavok → „Zmazať" → „Zrušiť" | Zobrazí sa dialóg, po „Zrušiť" je výdavok stále v zozname |
| T3.3-3 | AK3 | Existuje výdavok 20 € v novembri, súčet novembra je X | Otvoriť výdavok → „Zmazať" → „Zmazať" | Výdavok nie je v zozname, súčet novembra v Prehľade je X − 20 € |

## Epic 4: Prehľad výdavkov

**User story 4.1:** Ako športovec chcem vidieť súčet výdavkov za mesiac, aby som vedela, koľko ma šport stojí.
- Tasks: SQL dotaz na súčet za mesiac, obrazovka prehľadu, prepínanie mesiacov
- Akceptačné kritériá:
  - AK1: Prehľad sa otvorí jedným klepnutím na záložku a predvolene ukazuje aktuálny mesiac (napríklad „november 2026").
  - AK2: Súčet zahŕňa len výdavky s dátumom vo vybranom mesiaci (od 1. do posledného dňa vrátane).
  - AK3: Súčet sa zobrazuje v eurách s dvoma desatinnými miestami. Mesiac bez výdavkov má súčet 0,00 €.
  - AK4: Šípkami sa dá prepnúť na predchádzajúci a nasledujúci mesiac, aj cez prelom roka. Po prepnutí sa súčet načíta znova.
- Testy (unit testy: U3, U4):

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T4.1-1 | AK1 | – | Klepnúť na záložku Prehľad | Zobrazí sa „november 2026" |
| T4.1-2 | AK2, AK3 | Čisté dáta, výdavky v novembri 10,00 €, 25,50 €, 4,50 € a v októbri 100,00 € | Otvoriť Prehľad | Súčet za november je 40,00 € |
| T4.1-3 | AK2 | Čisté dáta, výdavky po 10 € s dátumami 31.10., 1.11., 30.11. a 1.12.2026 | Otvoriť Prehľad za november | Súčet je 20,00 € (len 1.11. a 30.11.) |
| T4.1-4 | AK4 | Dáta z T4.1-2 | Šípka doľava → šípka doprava | Október: 100,00 €, späť november: 40,00 € |
| T4.1-5 | AK3 | – | Prepnúť na mesiac bez výdavkov (napríklad máj 2026) | Súčet je 0,00 € |
| T4.1-6 | AK4 | – | Prepnúť na december 2026 → šípka doprava → šípka doľava | Január 2027, potom december 2026 |

**User story 4.2:** Ako športovec chcem vidieť kruhový graf rozdelenia výdavkov podľa kategórií, aby som na prvý pohľad videla, na čo idú moje peniaze najviac.
- Tasks: SQL dotaz so zoskupením podľa kategórie, kruhový graf (pie chart), legenda so sumami a percentami, prázdny stav pri nulových výdavkoch.
- Akceptačné kritériá:
  - AK1: Graf aj legenda obsahujú len kategórie, ktoré majú vo vybranom mesiaci aspoň jeden výdavok.
  - AK2: Legenda je zoradená od najväčšej sumy. Pri každej kategórii je farba, názov, suma v eurách a percento zaokrúhlené na celé číslo.
  - AK3: Súčet súm v legende sa rovná mesačnému súčtu.
  - AK4: Kategória má rovnakú farbu vo všetkých mesiacoch.
  - AK5: Keď v mesiaci nie sú výdavky, namiesto grafu sa zobrazí text „V tomto mesiaci nemáš žiadne výdavky".
  - AK6: Po prepnutí mesiaca sa graf aj legenda zobrazia pre nový mesiac.
- Testy (unit test: U5):

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| T4.2-1 | AK1, AK2, AK3 | Čisté dáta, v novembri Vybavenie 60 €, Doprava 30 €, Strava a doplnky 10 € | Otvoriť Prehľad | Graf má 3 časti. Legenda v poradí: Vybavenie 60,00 € 60 %, Doprava 30,00 € 30 %, Strava a doplnky 10,00 € 10 %. Súčet je 100,00 € |
| T4.2-2 | AK2 | Čisté dáta, v novembri len jeden výdavok 15 € Vybavenie | Otvoriť Prehľad | Graf je celý kruh, v legende Vybavenie 15,00 € 100 % |
| T4.2-3 | AK5 | – | Prepnúť na mesiac bez výdavkov | Graf sa nezobrazí, je tam text „V tomto mesiaci nemáš žiadne výdavky" |
| T4.2-4 | AK4, AK6 | Výdavky v kategórii Vybavenie v októbri aj novembri | Pozrieť farbu Vybavenia v novembri → prepnúť na október | Graf a legenda sú za október, Vybavenie má rovnakú farbu |
| T4.2-5 | AK2, AK3 | Čisté dáta, v novembri 3 výdavky po 10 € v troch rôznych kategóriách | Otvoriť Prehľad | Každá kategória má 33 % (spolu 99 % kvôli zaokrúhleniu), súčet súm je 30,00 € |
| T4.2-6 | AK1 | Dáta z T4.2-1 | Pozrieť legendu | Kategórie bez výdavkov v novembri (napríklad Ubytovanie) v legende nie sú |

---

## Testovanie

### Druhy testov

- **Funkčné testy (T…)**: sú pri každej user story vyššie. Robia sa ručne v telefóne cez Expo Go a overujú, či funkcia funguje tak, ako ju vidí športovec.
- **Nefunkčné testy (N…)**: overujú obmedzenia zo `SPEC.md` a merateľné ciele z initiative.
- **Unit testy (U…)**: automatické testy v knižnici Jest (`jest-expo`). Overujú pomocné funkcie zo `src/utils/` (opísané v `DESIGN.md`), ktoré nepotrebujú databázu ani obrazovku. Spúšťajú sa príkazom `npx jest`.

### Testovacie prostredie

- Telefón s Androidom (a iOS, ak je k dispozícii) s aplikáciou Expo Go.
- Testy predpokladajú dnešný dátum 24.11.2026. Pri inom dni treba dátumy v testoch posunúť.
- **Čisté dáta:** v nastaveniach telefónu vymazať dáta aplikácie Expo Go (alebo ju preinštalovať). Testy s predpokladom „čisté dáta" začínajú takto.
- **Reštart:** aplikáciu úplne zatvoriť (aj zo zoznamu spustených aplikácií) a znova otvoriť.

### Nefunkčné testy

| ID | Overuje | Predpoklad | Kroky | Očakávaný výsledok |
|---|---|---|---|---|
| N1 | SPEC: funguje bez internetu | Appka je načítaná v Expo Go, existujú výdavky | Zapnúť letový režim → pridať výdavok → otvoriť Prehľad | Výdavok sa uloží, zoznam aj Prehľad fungujú bez chyby |
| N2 | Initiative: zápis do 15 sekúnd | Appka je otvorená na záložke Výdavky | Stopkami zmerať čas od klepnutia na „Pridať" po uložení výdavku s dnešným dátumom (suma + kategória). Zopakovať 3-krát | Každý zápis trvá najviac 15 sekúnd |
| N3 | Initiative: prehľad do 2 klepnutí | Appka je otvorená na ľubovoľnej obrazovke | Spočítať klepnutia, kým sa zobrazí mesačný súčet a graf aktuálneho mesiaca | Najviac 2 klepnutia |
| N4 | Zachovanie dát po aktualizácii | Existujú výdavky a vlastná kategória zo staršej verzie kódu | Nahrať novú verziu kódu (git pull, `npx expo start`) a otvoriť appku | Všetky výdavky aj vlastné kategórie ostali zachované |
| N5 | SPEC: slovenčina a EUR | – | Prejsť všetky obrazovky, dialógy a chybové hlášky | Všetky texty sú po slovensky, sumy sú v tvare „12,50 €", dátumy v tvare DD.MM.RRRR |
| N6 | SPEC: Android aj iOS | Telefón s Androidom a s iOS (ak je k dispozícii) | Spustiť testy T1.1-1, T3.1-1 a T4.2-1 na oboch telefónoch | Výsledky sú rovnaké na oboch platformách |

### Unit testy (Jest)

| ID | Funkcia (`src/utils/`) | Vstup | Očakávaný výstup | Pre story |
|---|---|---|---|---|
| U1 | `parseAmount` | `"12,50"`, `"12.5"`, `"7"` | 1250, 1250, 700 (centy) | 1.1 |
| U1 | `parseAmount` | `""`, `"0"`, `"-3"`, `"10,555"`, `"abc"` | chyba validácie | 1.1 |
| U2 | `formatAmount` | 1250, 5, 0 | „12,50 €", „0,05 €", „0,00 €" | 3.1, 4.1 |
| U3 | `getMonthRange` | `"2026-11"`, `"2026-02"`, `"2028-02"` | 2026-11-01 až 2026-11-30, 2026-02-01 až 2026-02-28, 2028-02-01 až 2028-02-29 | 4.1 |
| U4 | `shiftMonth` | (`"2026-12"`, +1), (`"2027-01"`, −1) | `"2027-01"`, `"2026-12"` | 4.1 |
| U4 | `formatDate`, `todayString` | `"2026-11-03"`; dnešok | „03.11.2026"; dnešný dátum vo formáte RRRR-MM-DD | 1.2 |
| U5 | `calcPercentages` | [6000, 3000, 1000]; [1000, 1000, 1000]; [] | [60, 30, 10]; [33, 33, 33]; [] | 4.2 |
| U6 | `validateCategoryName` | `"   "`; `"Vybavenie"` pri existujúcej „vybavenie"; `"  Masáže  "` | chyba „prázdny"; chyba „duplicita"; platné „Masáže" | 2.2 |

### Záznam z testovania

Po každom testovaní sa vyplní tabuľka. Ak test neprejde, do poznámky sa zapíše, čo sa stalo, a chyba sa opraví pred uzavretím story.

| Dátum | Verzia (commit) | Zariadenie | ID testu | Výsledok (OK / chyba) | Poznámka |
|---|---|---|---|---|---|
| | | | | | |
