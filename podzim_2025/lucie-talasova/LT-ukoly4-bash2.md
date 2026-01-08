# Úkoly – Bash II

## 1. Grepování yamlů

Procvič si příkazy jako `ls`, `wc`, `grep` a jak je spojit dohromady.

Stáhni a rozbal si tyto archivy s informacemi o komunitních akcích: kurzech/srazech PyLadies a srazech Pyvo.

    $ wget -O pyladies-cz.zip https://github.com/PyLadiesCZ/pyladies.cz/archive/master.zip
    $ unzip pyladies-cz.zip
    $ wget -O pyvo-data.zip https://github.com/pyvec/pyvo-data/archive/master.zip
    $ unzip pyvo-data.zip

Data si prohlédni a zjisti, co se v nich skrývá za informace.
Zvlášť doporučuju prohlédnout soubor `pyvo-data-master/series/brno-pyvo/events/2018-10-25-casove.yaml`
který je použit na stránce [pyvo.cz/brno-pyvo/2018-10/](https://pyvo.cz/brno-pyvo/2018-10/),
a soubor `pyladies.cz-master/teams/brno.yml`
který je použit na stránce [pyladies.cz/brno/#team](https://pyladies.cz/brno/#team).

Použij základní shellové příkazy (ne Python) na zodpovězení otázek níže.
Hledáš jen orientační hodnoty; nemusí to být na 100% přesné.

> [note]
> YAML soubory by se správně měly číst knihovnou na YAML, aby byla zachována struktura. Ty je ale ber jako "čistý text", kde hledané informace jsou na řádcích ve tvaru `klíč: hodnota` (případně s nějakýma mezerama a/nebo pomlčkama navíc).
> Proto odpovědi nemusí být na 100% přesné.
> 
> "Zakomentované" informace (začínající `#`) můžeš pro jednoduchost počítat
> taky. (I když jich je po COVIDu často víc než těch nezakomentovaných.)


1. Kolik bylo Pyv v Brně?
   * *Pro každý sraz existuje soubor.*
---
🟢 **Odpověď:**
   
    cd pyvo-data-master/series/brno-pyvo/events
    find . -name '*.yaml' | wc -l

    163
---
2. Kolik bylo Pyv celkem?
---
🟢 **Odpověď:**

    cd pyvo-data-master/series
    find . -name '20*.yaml' | wc -l

    599 
---
3. Z kolika přednášek na Pyvech jsou videa?  *(Předpokládej že každá přednáška může mít max. 1 video)*
   * *Videa jsou označena `video:`*
---
🟢 **Odpověď:**

    cd pyvo-data-master/series
    grep -e '-video' */events/20*.yaml | wc -l

    438
---
4. Kolik bylo kurzů/srazů PyLadies?
   * *Srazy jsou v adresáři `meetups/` a každý má jméno, `name:`*
---
🟢 **Odpověď:**

    cd pyladies.cz-master/meetups
    grep -e '- name' *.yml | wc -l

    162
---
5. (Bonusová výzva) Z kolika Pyv jsou videa?
---
🟢 **Odpověď:**

    cd pyvo-data-master/series
    grep -l "video" */events/20*.yaml | wc -l
    
    148
---
### Nápověda

Šablonami jako `adresar/*/podadresar/*` můžeš vybrat soubory z více adresářů.

Příkaz `grep` má zajímavé přepínače `-r`, `-l`/`-L`, `-h`/`-H` a `-e`.

## 2. Uniq

Příkaz `uniq` odstraní *po sobě jdoucí* duplikované řádky:

```text
$ echo '
> jedna
> dva
> dva
> tři
> tři
> tři
> jedna
> ' | uniq

jedna
dva
tři
jedna

```

Často se používá `sort | uniq`, aby se stejné řádky z celého souboru dostaly k sobě.

Použij `uniq` k zodpovězení těchto otázek:

6. Vypiš všechna místa konání Pyv (stačí mít v rámci každého řádku identifikátor jako `artbar`).
---
🟢 **Odpověď:**
    
    cd pyvo-data-master/series
    grep -h -o 'venue:[[:space:]]*.*' */events/20*.yaml | sort | uniq | grep -o '[^[:space:]]*$'

    acko
    alvi
    andini
    artbar
    avu
    baroko
    beer-factory
    beskydsky-pivovarek
    botic-restaurant
    cafe-falk
    cafe-lajka
    coolarna
    crosscafe
    cyrils-pub
    depo
    edunesto
    etaz
    frankies
    hlavni-nadrazi
    hnizdo-snu
    holesovicka-sachta
    hub-20
    h55
    impact-hub
    ires-sc
    jama
    jiraskovy-sady
    kavarna-liberal
    kaverna
    kolocava
    konvikt
    kravi-hora
    local-lokal-industry-pub
    lumir
    luzanky
    moving-station
    na-hradbach
    na-venecku
    palouk
    park-bozetechova
    picolo-piratske-centrum-olomouc
    pivnice-doga
    pivon
    pivovarske-domy
    raven-pub-bolevec
    raven-pub-city
    reset-point
    restaurace-a-penzion-u-salzmannu
    selepka
    sklipek-kiwi-com
    snyt
    sport-club
    the-pub-dejvice
    tyrsuv-sad
    u-drevaka
    u-dreveneho-orla
    u-kachnicky
    u-morice
    u-prejezdu
    u-ptaka
    u-travise
    vila-stvanice
    vinograf
    v-lochu
    vr-levsky
    zivo-u-palecka
    2to2

---
7. Přidej informaci o tom, kolikrát na kterém místě Pyvo bylo. 

Příkaz `uniq` má zajímavý přepínač `-c`.

---
🟢 **Odpověď:**
    
    cd pyvo-data-master/series
    grep -h -o 'venue:[[:space:]]*.*' */events/20*.yaml | grep -o '[^[:space:]]*$' | sort | uniq -c
    
         1 acko
      2 alvi
      2 andini
     53 artbar
      1 avu
      2 baroko
     13 beer-factory
      1 beskydsky-pivovarek
      1 botic-restaurant
      2 cafe-falk
      3 cafe-lajka
     29 coolarna
      1 crosscafe
      3 cyrils-pub
      4 depo
      1 edunesto
      1 etaz
      2 frankies
      1 hlavni-nadrazi
      2 hnizdo-snu
      5 holesovicka-sachta
      4 hub-20
      2 h55
      1 impact-hub
      1 ires-sc
      1 jama
      1 jiraskovy-sady
      2 kavarna-liberal
      9 kaverna
     13 kolocava
      1 konvikt
      1 kravi-hora
      1 local-lokal-industry-pub
      2 lumir
     12 luzanky
      1 moving-station
      2 na-hradbach
    134 na-venecku
     16 palouk
      1 park-bozetechova
     16 picolo-piratske-centrum-olomouc
      5 pivnice-doga
      1 pivon
     22 pivovarske-domy
     17 raven-pub-bolevec
      1 raven-pub-city
      1 reset-point
      5 restaurace-a-penzion-u-salzmannu
      1 selepka
     10 sklipek-kiwi-com
      1 snyt
      4 sport-club
      1 the-pub-dejvice
      1 tyrsuv-sad
     28 u-drevaka
      4 u-dreveneho-orla
      1 u-kachnicky
      6 u-morice
      1 u-prejezdu
     27 u-ptaka
      1 u-travise
      3 vila-stvanice
      1 vinograf
      1 v-lochu
     62 vr-levsky
      1 zivo-u-palecka
      1 2to2

    ---
### Bonusová výzva

Existuje zajímavý příkaz `cut`, který má zajímavé přepínače `-d` a `-f`.

8. Jaké jsou 3 nejčastější křestní jména organizátorů/koučů/atd. PyLadies?
---
🟢 **Odpověď:**

    cd pyladies.cz-master/teams
    cat *.yml | grep -- "- name:" |cut -d' ' -f3 | sort | uniq -c | sort -n -r | head -n3

    8 Tomáš
    7 Petr
    6 Veronika
---
## 3. Zástupné znaky (zkus z hlavy)

Sam má následující soubory:

```
.
├── 2015-10-23-calibration.txt
├── 2015-10-23-dataset1.txt
├── 2015-10-23-dataset2.txt
├── 2015-10-23-dataset_overview.txt
├── 2015-10-26-calibration.txt
├── 2015-10-26-dataset1.txt
├── 2015-10-26-dataset2.txt
├── 2015-10-26-dataset_overview.txt
├── 2015-11-23-calibration.txt
├── 2015-11-23-dataset1.txt
├── 2015-11-23-dataset2.txt
├── 2015-11-23-dataset_overview.txt
├── backup
│   ├── calibration
│   └── datasets
└── send_to_bob
    ├── all_datasets_created_on_a_23rd
    └── all_november_files
```

Doplň následující příkazy...


---
🟢 **Odpověď:**

    ```console
    $ cp *dataset* backup/datasets
    $ cp *calibration* backup/calibration  
    $ cp 2015-11-* send_to_bob/all_november_files/
    $ cp 2015-*-23* send_to_bob/all_datasets_created_on_a_23rd/
    ```
---
... aby výsledek vypadal takhle:

```
.
├── 2015-10-23-calibration.txt
├── 2015-10-23-dataset1.txt
├── 2015-10-23-dataset2.txt
├── 2015-10-23-dataset_overview.txt
├── 2015-10-26-calibration.txt
├── 2015-10-26-dataset1.txt
├── 2015-10-26-dataset2.txt
├── 2015-10-26-dataset_overview.txt
├── 2015-11-23-calibration.txt
├── 2015-11-23-dataset1.txt
├── 2015-11-23-dataset2.txt
├── 2015-11-23-dataset_overview.txt
├── backup
│   ├── calibration
│   │   ├── 2015-10-23-calibration.txt
│   │   ├── 2015-10-26-calibration.txt
│   │   └── 2015-11-23-calibration.txt
│   └── datasets
│       ├── 2015-10-23-dataset1.txt
│       ├── 2015-10-23-dataset2.txt
│       ├── 2015-10-23-dataset_overview.txt
│       ├── 2015-10-26-dataset1.txt
│       ├── 2015-10-26-dataset2.txt
│       ├── 2015-10-26-dataset_overview.txt
│       ├── 2015-11-23-dataset1.txt
│       ├── 2015-11-23-dataset2.txt
│       └── 2015-11-23-dataset_overview.txt
└── send_to_bob
    ├── all_datasets_created_on_a_23rd
    │   ├── 2015-10-23-dataset1.txt
    │   ├── 2015-10-23-dataset2.txt
    │   ├── 2015-10-23-dataset_overview.txt
    │   ├── 2015-11-23-dataset1.txt
    │   ├── 2015-11-23-dataset2.txt
    │   └── 2015-11-23-dataset_overview.txt
    └── all_november_files
        ├── 2015-11-23-calibration.txt
        ├── 2015-11-23-dataset1.txt
        ├── 2015-11-23-dataset2.txt
        └── 2015-11-23-dataset_overview.txt
```