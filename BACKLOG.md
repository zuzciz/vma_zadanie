# Initiative

Dať športovcovi prehľad o tom, koľko ho šport stojí. Zapísať výdavok mu trvá do 15 sekúnd a mesačný súčet podľa kategórií vidí na jednej obrazovke do 2 klepnutí.

---

## Epic 1: Zápis výdavku

**User story 1.1:** Ako športovec chcem pridať výdavok so sumou, dátumom a kategóriou, aby som mala všetky výdavky na šport na jednom mieste.
- Tasks: tabuľka `expenses` v SQLite, formulár na pridanie výdavku, validácia sumy

**User story 1.2:** Ako športovec chcem, aby bol dátum predvolene dnešný, aby ma zápis nezdržiaval.
- Tasks: predvyplnený dnešný dátum, výber iného dátumu

**User story 1.3:** Ako športovec chcem k výdavku pridať poznámku, aby som si neskôr pamätala, na čo som peniaze minula.
- Tasks: pole pre poznámku vo formulári

## Epic 2: Kategórie výdavkov

**User story 2.1:** Ako športovec chcem vybrať kategóriu zo základnej sady (vybavenie, štartovné, tréner, doprava...), aby som výdavky rýchlo roztriedila.
- Tasks: tabuľka `categories`, naplnenie základných kategórií pri prvom spustení, výber kategórie vo formulári

**User story 2.2:** Ako športovec chcem pridať vlastnú kategóriu, aby som pokryla výdavky, ktoré základná sada nemá.
- Tasks: obrazovka alebo dialóg na novú kategóriu, uloženie do databázy

## Epic 3: Zoznam a úprava výdavkov

**User story 3.1:** Ako športovec chcem vidieť zoznam výdavkov od najnovšieho, aby som mala prehľad, čo som naposledy zapísala.
- Tasks: obrazovka so zoznamom, zoradenie podľa dátumu

**User story 3.2:** Ako športovec chcem upraviť výdavok, aby som mohla opraviť preklep v sume alebo kategórii.
- Tasks: otvorenie výdavku z detailu, formulár predvyplnený existujúcimi údajmi, uloženie zmien

**User story 3.3:** Ako športovec chcem zmazať výdavok, aby som sa zbavila omylom zapísaných záznamov.
- Tasks: tlačidlo na zmazanie, potvrdzovací dialóg

## Epic 4: Prehľad výdavkov

**User story 4.1:** Ako športovec chcem vidieť súčet výdavkov za mesiac, aby som vedela, koľko ma šport stojí.
- Tasks: SQL dotaz na súčet za mesiac, obrazovka prehľadu, prepínanie mesiacov

**User story 4.2:** Ako športovec chcem vidieť kruhový graf rozdelenia výdavkov podľa kategórií, aby som na prvý pohľad videla, na čo idú moje peniaze najviac.
- Tasks: SQL dotaz so zoskupením podľa kategórie, kruhový graf (pie chart), legenda so sumami a percentami, prázdny stav pri nulových výdavkoch.
