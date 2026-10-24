# Návrh: aplikácia na evidenciu výdavkov športovca

Tento dokument opisuje dátový model, rozhranie na prácu s dátami (API) a štruktúru projektu. Zatiaľ neobsahuje žiadny kód. Vychádza zo `SPEC.md` a `BACKLOG.md`.

## Prehľad architektúry

Aplikácia nemá server. Všetko beží v telefóne a dáta sú uložené lokálne v SQLite. Vrstvy:

1. **Obrazovky** (React Native): zobrazujú údaje a reagujú na klepnutia.
2. **Dátová vrstva** (repozitáre): jediné miesto, ktoré komunikuje s databázou. Obrazovky nikdy nepíšu SQL priamo.
3. **Databáza** (SQLite cez expo-sqlite): ukladá výdavky a kategórie.

Slovo „API" tu znamená funkcie dátovej vrstvy, ktoré volajú obrazovky. Nejde o internetové API.

## Dátový model

### Tabuľka `categories` (kategórie)

| Stĺpec | Typ | Popis |
|---|---|---|
| `id` | celé číslo, primárny kľúč | Jedinečný identifikátor |
| `name` | text, povinný, jedinečný | Názov kategórie (napríklad „Vybavenie") |
| `is_default` | 0 alebo 1 | 1 = základná kategória, 0 = vlastná kategória športovca |

### Tabuľka `expenses` (výdavky)

| Stĺpec | Typ | Popis |
|---|---|---|
| `id` | celé číslo, primárny kľúč | Jedinečný identifikátor |
| `amount_cents` | celé číslo, povinné, väčšie ako 0 | Suma v centoch (12,50 € sa uloží ako 1250) |
| `date` | text, povinný | Dátum výdavku vo formáte RRRR-MM-DD |
| `category_id` | celé číslo, povinné | Cudzí kľúč na `categories.id` |
| `note` | text, nepovinný | Poznámka k výdavku |
| `created_at` | text, povinný | Čas vytvorenia záznamu (ISO formát) |

### Vzťahy

- Jedna kategória má viac výdavkov (1 : N). Každý výdavok patrí práve do jednej kategórie.

### Rozhodnutia v modeli

- **Suma v centoch:** Ukladá sa ako celé číslo, aby sa predišlo chybám pri zaokrúhľovaní desatinných čísel. Na obrazovke sa zobrazí v eurách.
- **Dátum ako text RRRR-MM-DD:** Dá sa jednoducho zoradiť a filtrovať podľa mesiaca.
- **Základné kategórie:** Vložia sa do databázy pri prvom spustení appky: vybavenie, štartovné a súťaže, tréner a tréningy, členské a poplatky, doprava, ubytovanie, strava a doplnky, zdravie a regenerácia, iné.
- **Mazanie kategórií:** V prvej verzii sa kategórie nemažú, takže nevzniká problém s výdavkami bez kategórie (pozri otvorené otázky v `SPEC.md`).

## Rozhranie dátovej vrstvy (API)

### Kategórie

| Funkcia | Čo robí | Pre user story |
|---|---|---|
| `getCategories` | Vráti všetky kategórie (základné aj vlastné) | 2.1 |
| `addCategory(name)` | Pridá vlastnú kategóriu, odmietne prázdny alebo duplicitný názov | 2.2 |

### Výdavky

| Funkcia | Čo robí | Pre user story |
|---|---|---|
| `addExpense(amount, date, categoryId, note)` | Uloží nový výdavok | 1.1, 1.2, 1.3 |
| `getExpenses` | Vráti zoznam výdavkov od najnovšieho, aj s názvom kategórie | 3.1 |
| `getExpense(id)` | Vráti jeden výdavok na úpravu | 3.2 |
| `updateExpense(id, ...)` | Uloží zmeny existujúceho výdavku | 3.2 |
| `deleteExpense(id)` | Zmaže výdavok | 3.3 |

### Prehľad

| Funkcia | Čo robí | Pre user story |
|---|---|---|
| `getMonthTotal(month)` | Vráti súčet výdavkov za vybraný mesiac | 4.1 |
| `getTotalsByCategory(month)` | Vráti súčet výdavkov za mesiac zoskupený podľa kategórie, od najväčšieho. Tieto údaje slúžia na kruhový graf aj na legendu | 4.2 |

### Pravidlá validácie

- Suma musí byť väčšia ako 0 a zadáva sa v eurách s najviac dvoma desatinnými miestami.
- Dátum je povinný, predvolene dnešný.
- Kategória je povinná a musí existovať.
- Názov novej kategórie nesmie byť prázdny ani sa opakovať.

## Kruhový graf (Prehľad)

- **Knižnica:** react-native-gifted-charts, jej kruhový graf (pie chart).
- **Zdroj údajov:** funkcia `getTotalsByCategory(month)`. Graf nemá vlastné dáta, len zobrazuje jej výsledok pre vybraný mesiac.
- **Komponent:** `CategoryPieChart` v `src/components/`. Dostane zoznam kategórií so sumami a vykreslí graf aj legendu.
- **Legenda:** Pri každej kategórii je farba, názov, suma v eurách a percento z mesačného súčtu. Percentá sa počítajú v appke zo súm, preto po zaokrúhlení nemusia dať presne 100 %.
- **Farby:** Každá kategória má pevne priradenú farbu, aby bola rovnaká pri prepnutí mesiaca. Vlastné kategórie dostanú ďalšie farby z palety.
- **Prázdny stav:** Ak v mesiaci nie sú žiadne výdavky, namiesto grafu sa zobrazí text „V tomto mesiaci nemáš žiadne výdavky".
- **Prepínanie mesiacov:** Po zmene mesiaca sa graf aj mesačný súčet načítajú znova.

## Obrazovky a navigácia

Dolná lišta s dvoma záložkami:

| Obrazovka | Účel | User stories |
|---|---|---|
| **Výdavky** (záložka) | Zoznam výdavkov od najnovšieho, tlačidlo „Pridať" | 3.1 |
| **Prehľad** (záložka) | Mesačný súčet, prepínanie mesiacov, kruhový graf rozdelenia výdavkov podľa kategórií s legendou (suma a percento) | 4.1, 4.2 |
| **Nový / upraviť výdavok** | Formulár so sumou, dátumom, kategóriou a poznámkou; pri úprave aj tlačidlo „Zmazať" s potvrdením | 1.1 – 1.3, 3.2, 3.3 |
| **Nová kategória** | Dialóg alebo malá obrazovka s jedným poľom na názov | 2.2 |

Prechody: zo záložky Výdavky sa dá otvoriť formulár (nový alebo existujúci výdavok). Z formulára sa dá otvoriť dialóg na novú kategóriu a po uložení sa vybraná kategória nastaví na novú.

## Štruktúra projektu

```
vma_zadanie/
├── app/                  obrazovky a navigácia
│   ├── (tabs)/           záložky Výdavky a Prehľad
│   └── expense/          formulár nového a upravovaného výdavku
├── src/
│   ├── db/               pripojenie k databáze, vytvorenie tabuliek, základné kategórie
│   ├── repositories/     funkcie dátovej vrstvy (výdavky, kategórie, prehľad)
│   ├── components/       opakovane použité časti rozhrania (napríklad kruhový graf CategoryPieChart)
│   ├── utils/            práca so sumami (cent na euro) a dátumami
│   └── types/            typy údajov (Expense, Category)
├── README.md             plán projektu
├── SPEC.md               špecifikácia
├── DESIGN.md             tento návrh
└── backlog.md            initiative, epics, user stories
```

## Poradie implementácie

1. Založiť Expo projekt a vytvoriť databázu s tabuľkami a základnými kategóriami.
2. Dátová vrstva pre kategórie a výdavky.
3. Formulár nového výdavku a zoznam výdavkov (epic 1 a 3.1).
4. Úprava a mazanie výdavku (3.2, 3.3).
5. Vlastné kategórie (2.2).
6. Prehľad za mesiac a podľa kategórií (epic 4).