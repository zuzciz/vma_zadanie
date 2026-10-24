# Špecifikácia: aplikácia na evidenciu výdavkov športovca

## Problém

Športovci nemajú jednoduchý nástroj na zapisovanie výdavkov spojených so športom (vybavenie, štartovné, doprava, tréner), a preto nevedia, koľko ich šport v skutočnosti stojí.

## Používatelia a roly

- **Športovec** (jediná rola): amatérsky alebo výkonnostný športovec, ktorý si sám platí vybavenie, tréningy, súťaže a cestovanie. Aplikáciu používa sám na svojom telefóne, bez prihlasovania.
- Admin, tréner ani iné roly v prvej verzii nie sú.

## Funkcie

Číslovanie zodpovedá user stories v súbore `BACKLOG.md`.

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
   - 4.2 Športovec vidí rozdelenie výdavkov podľa kategórií v kruhovom grafe s legendou, kde je pri každej kategórii suma a percento.

## Obmedzenia

- Technológie: React Native + TypeScript, Expo, lokálna databáza SQLite (expo-sqlite).
- Jazyk rozhrania: slovenčina.
- Mena: EUR (jediná mena).
- Platforma: mobilná aplikácia (Android a iOS z jedného kódu), v prvej verzii testovaná cez Expo Go.
- Dáta sú uložené len v telefóne, aplikácia funguje bez internetu.
- Aplikácia nepotrebuje špeciálny hardvér (kameru, GPS ani senzory).

## Mimo rozsahu

V prvej verzii úmyselne nie je:

- fotenie bločkov a automatické čítanie (OCR),
- účty, prihlasovanie a synchronizácia medzi zariadeniami,
- rozpočet a upozornenia na prekročenie limitu,
- viac mien a prepočet kurzov,
- export do Excelu alebo PDF a záloha dát,
- zdieľanie s trénerom alebo rodičmi,
- zverejnenie v Google Play a App Store.

## Otvorené otázky

- Majú sa dať zapisovať aj opakované výdavky (napríklad mesačné členské)? Alebo stačí každý výdavok zapísať zvlášť?
- Čo sa stane s výdavkami, keď športovec zmaže vlastnú kategóriu? Zmažú sa spolu s ňou, alebo sa presunú do kategórie „iné"?
- Dá sa zmazať aj základná kategória, alebo len vlastná?
- Má byť prehľad vždy len za jeden mesiac, alebo aj za celú sezónu či rok?
- Zapisuje sa len jeden šport, alebo má mať športovec možnosť výdavky rozdeliť podľa športu?
- Čo sa zobrazí v grafe, keď sú v mesiaci nulové výdavky?”
