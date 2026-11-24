# Špecifikácia: aplikácia na evidenciu výdavkov športovca

## Problém

Športovci nemajú jednoduchý nástroj na zapisovanie výdavkov spojených so športom (vybavenie, štartovné, doprava, tréner), a preto nevedia, koľko ich šport v skutočnosti stojí.

## Používatelia a roly

- **Športovec** (jediná rola): amatérsky alebo výkonnostný športovec, ktorý si sám platí vybavenie, tréningy, súťaže a cestovanie. Aplikáciu používa sám na svojom telefóne, bez prihlasovania.
- Admin, tréner ani iné roly v prvej verzii nie sú.

## Funkcie

Číslovanie zodpovedá user stories v súbore `BACKLOG.md`. Akceptačné kritériá a testy ku každej funkcii sú v `BACKLOG.md`.

1. Zápis výdavku
   - 1.1 Športovec môže pridať výdavok so sumou, dátumom a kategóriou.
   - 1.2 Dátum je pri novom výdavku predvolene dnešný a dá sa zmeniť.
   - 1.3 Športovec môže k výdavku pridať poznámku.
2. Kategórie výdavkov
   - 2.1 Športovec vyberie kategóriu zo základnej sady (vybavenie, štartovné a súťaže, tréner a tréningy, členské a poplatky, doprava, ubytovanie, strava a doplnky, zdravie a regenerácia, iné).
   - 2.2 Športovec môže pridať vlastnú kategóriu.
3. Zoznam a úprava výdavkov
   - 3.1 Športovec vidí zoznam výdavkov od najnovšieho.
   - 3.2 Športovec môže výdavok otvoriť a upraviť.
   - 3.3 Športovec môže výdavok zmazať po potvrdení.
4. Prehľad výdavkov
   - 4.1 Športovec vidí súčet výdavkov za vybraný mesiac a môže prepínať medzi mesiacmi.
   - 4.2 Športovec vidí rozdelenie výdavkov podľa kategórií v kruhovom grafe s legendou, kde je pri každej kategórii suma a percento. Keď v mesiaci nie sú výdavky, namiesto grafu vidí text o prázdnom mesiaci.

## Obmedzenia

- Technológie: React Native + TypeScript, Expo, lokálna databáza SQLite (expo-sqlite).
- Jazyk rozhrania: slovenčina.
- Mena: EUR (jediná mena).
- Platforma: mobilná aplikácia (Android a iOS z jedného kódu), v prvej verzii testovaná cez Expo Go.
- Dáta sú uložené len v telefóne, aplikácia funguje bez internetu.
- Aplikácia nepotrebuje špeciálny hardvér (kameru, GPS ani senzory).

## Rozhodnutia

Otázky, ktoré vznikli pri písaní špecifikácie, sú pre prvú verziu rozhodnuté takto. `DESIGN.md` z týchto rozhodnutí vychádza.

| Otázka | Rozhodnutie pre prvú verziu |
|---|---|
| Dajú sa zapisovať opakované výdavky (napríklad mesačné členské)? | Nie. Každý výdavok sa zapíše zvlášť. Opakované výdavky môžu pribudnúť neskôr. |
| Dá sa zmazať kategória (základná aj vlastná)? | Nie. Kategórie sa v prvej verzii nedajú zmazať ani premenovať. Preto nikdy nevznikne výdavok bez kategórie. |
| Čo sa stane s výdavkami pri zmazaní kategórie? | V prvej verzii nenastane, pretože kategórie sa nemažú. |
| Je prehľad len za jeden mesiac, alebo aj za sezónu či rok? | Len za jeden mesiac s prepínaním medzi mesiacmi. |
| Dajú sa výdavky rozdeliť podľa športu? | Nie. Aplikácia eviduje výdavky bez rozlíšenia športu. |
| Čo sa zobrazí v grafe, keď sú v mesiaci nulové výdavky? | Namiesto grafu sa zobrazí text „V tomto mesiaci nemáš žiadne výdavky" a súčet 0,00 €. |
| Dá sa zadať dátum v budúcnosti? | Áno (napríklad vopred zaplatené štartovné). |

## Mimo rozsahu

V prvej verzii úmyselne nie je:

- fotenie bločkov a automatické čítanie (OCR),
- účty, prihlasovanie a synchronizácia medzi zariadeniami,
- rozpočet a upozornenia na prekročenie limitu,
- viac mien a prepočet kurzov,
- export do Excelu alebo PDF a záloha dát,
- zdieľanie s trénerom alebo rodičmi,
- zverejnenie v Google Play a App Store,
- opakované výdavky,
- mazanie a premenovanie kategórií,
- prehľad za sezónu alebo rok,
- rozdelenie výdavkov podľa športu.
