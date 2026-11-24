## Aký problém riešim
- Športovci nemajú jednoduchý nástroj na zapisovanie výdavkov spojených so športom (vybavenie, štartovné, doprava, tréner) a nevedia, koľko ich šport v skutočnosti stojí.

## Kto sú moji používatelia
- Amatérski a výkonnostní športovci, ktorí si sami platia vybavenie, tréningy, súťaže a cestovanie.

## Ako chcem problém vyriešiť (MVP)
- Zápis výdavku
  Zadám sumu, dátum, kategóriu a krátku poznámku. Dátum je predvolene dnešný, aby zápis trval pár sekúnd.
- Kategórie výdavkov
  Základná sada: vybavenie, štartovné a súťaže, tréner a tréningy, členské a poplatky, doprava, ubytovanie, strava a doplnky, zdravie a regenerácia, iné.
  Môžem pridať vlastnú kategóriu.
- Zoznam výdavkov
  Prehľadný zoznam od najnovšieho. Výdavok môžem otvoriť, upraviť alebo zmazať.
- Prehľad
  Súčet za vybraný mesiac a rozdelenie podľa kategórií (kruhový graf rozdelenia výdavkov podľa kategórií).

## Čo v MVP nebude (non-goals)
- Fotenie bločkov a automatické čítanie (OCR). Dôvod: Je to technicky náročné a nie je potrebné na vyriešenie hlavného problému. Sumu viem zadať ručne.
- Účty, prihlasovanie a synchronizácia medzi zariadeniami. Dôvod: Aplikáciu používam na jednom telefóne. Pribudol by server a prihlasovanie.
- Rozpočet a upozornenia na prekročenie limitu. Dôvod: Najprv potrebujem vedieť, kam peniaze idú. Rozpočty môžu pribudnúť neskôr.
- Viac mien a prepočet kurzov. Dôvod: Prvá verzia počíta len v eurách.
- Export do Excelu / PDF a záloha dát. Dôvod: Dáta sú zatiaľ len v jednom telefóne. Export príde, keď bude appku používať viac ľudí.
- Zdieľanie s trénerom alebo rodičmi. Dôvod: Vyžadovalo by to účty a server.
- Zverejnenie aplikácie na internete. Dôvod: Aplikácia je zatiaľ len školský projekt a beží len u mňa.
- Opakované výdavky. Dôvod: Mesačné členské viem zapísať aj ručne raz za mesiac.
- Mazanie a premenovanie kategórií. Dôvod: Netreba riešiť, čo sa stane s výdavkami zmazanej kategórie.
- Prehľad za sezónu alebo rok a rozdelenie podľa športu. Dôvod: Na hlavný problém stačí mesačný prehľad.

## Dokumenty projektu
- `SPEC.md`: špecifikácia a rozhodnutia
- `BACKLOG.md`: user stories s akceptačnými kritériami a testami
- `DESIGN.md`: dátový model, API, obrazovky, štruktúra projektu

## Spustenie

```bash
npm install
npx expo start
```

V telefóne otvoriť aplikáciu Expo Go a naskenovať QR kód z terminálu (telefón a počítač musia byť na rovnakej Wi-Fi).

Unit testy: `npx jest`

Pozor: dáta sú uložené iba v Expo Go v telefóne. Po vymazaní dát alebo odinštalovaní Expo Go sa výdavky stratia.

## Aké technológie potrebujem
- Platforma: mobilná aplikácia (Android a iOS z jedného kódu)
- Rozhranie: React Native + TypeScript (obrazovky aplikácie)
- Zostavenie a spustenie: Expo (aplikáciu testujem priamo v telefóne cez Expo Go)
- Lokálna databáza: SQLite cez expo-sqlite (výdavky a kategórie uložené v telefóne)
- Graf prehľadu: knižnica react-native-gifted-charts (kruhový graf)
- Testy: Jest (jest-expo) na unit testy pomocných funkcií
- Nástroje: VS Code, Node.js, Git + GitHub (písanie kódu a verzie projektu)
