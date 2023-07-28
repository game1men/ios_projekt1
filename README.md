# ios_projekt1

Jedna se o projekt na procvičení bahse do předmětu IOS. 

[zadani pdf](1._Uloha_IOS_2022.pdf) 
zadani:
---


# 1. Úloha IOS (2022)

# Popis úlohy

Cílem úlohy je vytvořit shellový skript pro analýzu záznamů osob s prokázanou nákazou koronavirem způsobujícímonemocnění COVID-19 na území České republiky. Skript bude filtrovat záznamy a poskytovat základní statistiky podle
zadání uživatele.

# Specifikace rozhraní skriptu

**JMÉNO**
corona — analyzátor záznamů osob s prokázanou nákazou koronavirem způsobujícím onemocnění COVID-
**POUŽITÍ**
corona [-h] [FILTERS] [COMMAND] [LOG [LOG2 [...]]
**VOLBY**
COMMANDinfected může být jeden z: — spočítá počet nakažených.
mergejen jednou). — sloučí několik souborů se záznamy do jednoho, zachovávající původní pořadí (hlavička bude ve výstupu
genderage — vypíše statistiku počtu nakažených osob dle věku (bližší popis je níže). — vypíše počet nakažených pro jednotlivá pohlaví.
dailymonthly — vypíše statistiku nakažených osob pro jednotlivé dny. — vypíše statistiku nakažených osob pro jednotlivé měsíce.
yearlycountries — vypíše statistiku nakažených osob pro jednotlivé roky. — vypíše statistiku nakažených osob pro jednotlivé země nákazy (bez ČR, tj. kódu CZ).
districtsregions — vypíše statistiku nakažených osob pro jednotlivé kraje. — vypíše statistiku nakažených osob pro jednotlivé okresy.
FILTERS-a DATETIME může být kombinace následujících (každý maximálně jednou): — after: jsou uvažovány pouze záznamy PO tomto datu (včetně tohoto data). DATETIME je formátu
YYYY-MM-DD-b DATETIME. — before: jsou uvažovány pouze záznamy PŘED tímto datem (včetně tohoto data).
-g GENDER(ženy). — jsou uvažovány pouze záznamy nakažených osob daného pohlaví. GENDER může být M (muži) nebo Z
-s [WIDTH]ale graficky v podobě histogramů. Nepovinný parametr u příkazů gender, age, daily, monthly, yearly, WIDTHcountries nastavuje šířku histogramů, tedy délku nejdelšího, districts a regions vypisuje data ne číselně,
řádku, na požadavky uvedenými níže.WIDTH. Tedy, WIDTH musí být kladné celé číslo. Pokud není parametr WIDTH uveden, řídí se šířky řádků
(Mapování kódů na jména je v souboru **nepovinný, viz níže** ) -d DISTRICT_FILEDISTRICT_FILE — pro příkaz districts vypisuje místo LAU 1 kódu okresu jeho jméno.
(Mapování kódů na jména je v souboru **nepovinný, viz níže** ) -r REGIONS_FILE — pro příkaz REGIONS_FILEregions vypisuje místo NUTS 3 kódu kraje jeho jméno.
-h — vypíše nápovědu s krátkým popisem každého příkazu a přepínače.

# Popis

```
1. Skript filtruje záznamy osob s prokázanou nákazou koronavirem způsobujícím onemocnění COVID-19. Pokud je skriptuzadán také příkaz, nad filtrovanými záznamy daný příkaz provede.
23.. Pokud skript nedostane ani filtr ani příkaz, opisuje záznamy na standardní výstup.Skript umí zpracovat i záznamy komprimované pomocí nástrojů gzip a bzip2 (v případě, že název souboru končí .gz
resp. .bz2).
```

```
4. V případě, že skript na příkazové řádce nedostane soubory se záznamy (vstupu. LOG, LOG2, ...), očekává záznamy na standardním
5. Pokud má skript vypsat seznam, každá položka je vypsána na jeden řádek a pouze jednou. Není-li uvedeno jinak, jepořadí řádků dáno abecedně. Položky se nesmí opakovat.
6. Grafy jsou vykresleny pomocí ASCII a jsou otočené doprava. Hodnota řádku je vyobrazena posloupností znaku mřížky#.
```
# Podrobné požadavky

```
1. Skript analyzuje záznamy ze zadaných souborů v daném pořadí.
2. Formát souboru se záznamy je CSV, kde oddělovačem je znak čárky řádkový, každý neprázdný řádek (prázdné řádky jsou takové, které obsahují jen bílé znaky) odpovídá záznamu o jedné,. Celý soubor je v kódování ASCII. Formát je
nákaze osoby ve tvaru (následující je hlavička CSV souboru)
id,datum,vek,pohlavi,kraj_nuts_kod,okres_lau_kod,nakaza_v_zahranici,nakaza_zeme_csu_kod,reportovano_khs
kde
iddatum je identifikátor záznamu (řetězec neobsahující bílé znaky a znak čárky je ve formátu YYYY-MM-DD, ,),
vekpohlavi je kladné celé číslo, je M (muž) nebo Z (žena),
kraj_nuts_kodokres_lau_kod je je kód krajekód okresu, kde byla nákaza zjištěna,, kde byla nákaza zjištěna,
nakaza_v_zahraniciznačí, že nebyl), značí, zda zdroj nákazy byl v zahraničí ( 1 značí, že zdroj nákazy byl v zahraničí, prázdné pole
nakaza_zeme_csu_kodreportovano_khs značí, zda byla nákaza reportována krajské hygienické stanici. je kód země vzniku nákazy (pro nákazu vzniklou v zahraničí),
Příklad souboru se třemi záznamy:
id,datum,vek,pohlavi,kraj_nuts_kod,okres_lau_kod,nakaza_v_zahranici,nakaza_zeme_csu_kod,reportovano_khs6f4125cb-fb41-4fb0-a478-07b69ba106a4,2020-03-01,21,Z,CZ010,CZ0100,1,IT,
f6b08ff5-203d-4a3e-aab0-a5d39ac9ab9e,2020-03-11,32,M,CZ080,CZ0804,,,1b03dcf40-04cd-4f7b-a13d-767fc43c3013,2020-03-14,38,M,,,,,
První záznam z 1. března 2020 značí nákazu 21leté ženy v kraji “Hlavní město Praha” (kód město Praha” (kód CZ0100). Žena byla nakažena v Itálii (kód IT) a nákaza byla reportována krajské hygienickéCZ010), v okrese “Hlavní
stanici.Druhý záznam značí vnitrostátní nákazu 32letého muže v Moravskoslezském kraji (kód CZ080), v okrese Nový Jičín
(kód Třetí záznam značí nákazu 38letého muže zjištěnou 14. března 2020, u níž nejsou k dispozici další informace.CZ0804), zjištěnou 11. března 2020.
3. Skript žádný soubor nemodifikuje. Skript nepoužívá dočasné soubory.
4. Záznamy ve vstupních souborech nemusí být uvedeny chronologicky a je-li na vstupu více souborů, jejich pořadí takénemusí být chronologické.
5. Pokud není při použití přepínače (zaokrouhleno dolů) dle příkazu následujícím způsobem:-s uvedena šířka WIDTH, pak každý výskyt symbolu # v grafu odpovídá počtu nákaz
genderage — 10 000 — 100 000
dailymonthly — 500 — 10 000
yearlycountries — 100 000 — 100
districtsregions — 10 000 — 1 000
```

```
6. Při použití přepínače řádku grafu. Řádek s největší hodnotou bude mít počet mřížek -s s uvedenou šířkou WIDTH je počet nákaz na mřížku upraven podle největšího počtu výskytů vWIDTH a ostatní řádky budou mít proporcionální počet
mřížek vzhledem k největší hodnotě. Při dělení zaokrouhlujte dolů. Tedy např. při 1234 bude řádek s touto hodnotou vypadat takto: ######. -s 6 a řádku s největší hodnotou
7. Pořadí argumentů stačí uvažovat takové, že nejřív budou všechny přepínače, pak (volitelně) příkaz a nakonec seznamvstupních souborů (lze tedy použít getopts).
8. Předpokládejte, že vstupní soubory nemůžou mít jména odpovídající některému příkazu nebo přepínači.
9. V případě uvedení přepínače následoval nějaký příkaz nebo soubor, neprovede se).-h se vždy pouze vypíše nápověda a skript skončí (tedy, pokud by za přepínačem
```
10. V případě neexistující hodnoty atributu u příkazů agregujte záznamy s neexistující hodnotou pod hodnotu gender, ageNone, daily, kterou ve výpisech uvádějte jako poslední., monthly, yearly, countries, districts, regions

11. Přepodkládejte, že je-li v záznamu uvedena hodnota pro nějaký atribut záznamu, pak je korektní (tj. není potřebavalidovat vstup) s následujícími výjimkami:

```
ve sloupci např., yesterdaydatum je očekáváno korektní datum ve formátu nebo 2020-02-30 jsou nevalidní hodnoty).YYYY-MM-DD, které odpovídá skutečnému dni (tedy,
V případě detekování záznamu porušujícího některou z podmínek uvedených výše vypište chybu na chybový výstup ave sloupci vek je očekáváno nezáporné celé číslo (tedy, např., -42, 18.5 nebo 1e10 jsou nevalidní hodnoty).
pokračujte ve zpracovávání dále (chybějící hodnota data/věku není porušením). Formát pro chybu je následující:
Invalid date: 6f4125cb-fb41-4fb0-a478-07b69ba106a4,2020-04-31,21,Z,CZ010,CZ0100,1,IT,1Invalid age: 033fb060-2a10-42ce-80c1-72c2e39b1981,2020-03-05,dvacet,Z,CZ042,CZ0421,,,
```
12. U příkazu age pracujte s následujícími intervaly a zarovnáním:
0-5 : 6-15 :

16-25 :26-35 : (^)
36-45 :46-55 : (^)
56-65 :66-75 : (^)
76-85 :86-95 : (^)
96-105:>105 : (^)
13. Implementace přepínačů -d a -r je nepovinná; korektní implementace může vynahradit jiné bodové ztráty.
14. U příkazů ve formátu genderhodnota: pocet, daily, monthly (bez mezery před dvojtečkou a s právě jednou mezerou za dvojtečkou), případně (pro, yearly, countries, districts, regions (bez přepínačů -d a -r) stačí vypisovat výstup
přepínač -s) hodnota: ###...#. U příkazu age je zarovnání specifikováno výše.
Pro nepovinné přepínače -r a -d je dvojtečka na pozici o jedna větší, než je počet symbolů nejdelší hodnoty, tj. např.
hodnota : 42delsi_hodnota: 1337
15. Příkaz countries nevypisuje počet nákaz v České republice (kód CZ).
16. Hodnoty ve sloupcích brát nakaza_v_zahranicinakaza_v_zahranici do úvahy). a reportovano_khs ignorujte (tj. například u příkazu countries není třeba
17. Záznamy nemusí nutně mít patřičný počet sloupců. V případě chybějícího sloupce postupujte stejně, jako kdyby v němchyběla hodnota (pokud záznamu chybí N polí, znamená, to, že chybí hodnota N nejpravějších sloupců, tedy, např.,
pokud záznam obsahuje jen 7 polí, pak chybí hodnoty sloupců nakaza_zeme_csu_kod a reportovano_khs).


```
18. Nekontrolujte, zda obsahy sloupců V případě implementace rozšíření -dkraj_nuts_kod a -r při použití hodnoty nedefinované v souboru s definicemi okresů/krajů, okres_lau_kod a nakaza_zeme_csu_kod odpovídají daným číselníkům.
vypisujte dané záznamy na chybový výstup v následujícím formátu:
Invalid value: 07958a56-6867-4245-b042-29c291c20359,2020-08-16,5,M,CZ099,CZ0999,,,
```
# Implementační detaily

```
1. Skript by měl mít v celém běhu nastaveno POSIXLY_CORRECT=yes.
2. Skript by měl běžet na všech běžných shellech (uveďte to pomocí direktivy interpretu na prvním řádku souboru, např. dash, ksh, bash). Pokud použijete vlastnost specifickou pro nějaký shell,#!/bin/bash nebo #!/usr/bin/env bash pro
bash. Můžete použít GNU rozšíření pro sed či awk. Jazyky Perl, Python, Ruby, atd. povoleny nejsou.
UPOZORNĚNÍ: skutečně testujete daným shellem. Doporučuji ověřit správnou funkčnost pomocí virtuálního stroje níže. některé servery, např. merlin.fit.vutbr.cz, mají symlink /bin/sh -> bash. Ověřte si proto, že skript
3. Skript musí běžet na běžně dostupných OS GNU/Linux, BSD a MacOS. Studentům je k dispozici virtuální stroj sobrazem ke stažení zde: http://www.fit.vutbr.cz/~lengal/public/trusty.ova (pro VirtualBox, login: trusty / heslo: trusty),
na kterém lze ověřit správnou funkčnost projektu.
4. Skript nesmí používat dočasné soubory. Povoleny jsou však dočasné soubory nepřímo tvořené jinými příkazy (např.příkazem sed -i).
```
# Odevzdání projektu

Odevzdávejte pouze skript corona (nebalte ho do žádného archivu). Odevzdejte do IS, termín Projekt 1.

# Rady

```
Dobrá dekompozice problému na podproblémy Vám může značně ulehčit práci a předejít chybám.Naučte se dobře používat funkce v shellu (uvědomte si, že spousta funkcionality, např. pro výpisy statistik, histogram,
atd., je obdobná).
```
# Návratová hodnota

```
Skript vrací úspěch v případě úspěšné operace. Interní chyba skriptu nebo chybné argumenty budou doprovázenychybovým hlášením na stderr a neúspěšným návratovým kódem.
```
# Příklady použití

Ukázky záznamů o nakažených jsou dostupné na oficiálních stránkách MZČR: aktualne.mzcr.cz/api/v2/covid-19/osoby.csv (pozor, má cca 250 MiB). Na této stráncehttps://onemocneni- jsou dispozici další datové sady
včetně popisů jejich schémat.Vzorové záznamy, na kterých jsou ukázány příklady použití níže, jsou k dispozici na této stránce. Jsou to konkrétně
následující:Kopie souboru osoby.csv z 21. února 2022 zde (cca 250 MiB).
Zkrácená verze verze Podmnožina záznamů za leden 2022 rozdělených dle jednotlivých dnů je k dispozici osoby-short.csv zde (cca 150 KiB). zde.
Komprimované verze souborů short.csv.bz2). osoby.csv a osoby-short.csv (osoby.csv.gz, osoby.csv.bz2, osoby-short.csv.gz a osoby-
Soubor osoby2.csv s ukázkami těžších záznamů, které je potřeba umět korektně zpracovat, je zde.
Příklady:


## $ cat osoby.csv | head -n 5 | ./coronaid,datum,vek,pohlavi,kraj_nuts_kod,okres_lau_kod,nakaza_v_zahranici,nakaza_zeme_csu_kod,reportovano_khs 


```
$ ./corona countries osoby.csv99: 1
```
AD: 1AE: 444 (^)
AF: 13...
ZA: 36ZM: 2
ZW: 1
(kód země 99 na prvním řádku je chyba v datové sadě; neřešte ji)
$ ./corona -g M osoby.csv | head -n 6id,datum,vek,pohlavi,kraj_nuts_kod,okres_lau_kod,nakaza_v_zahranici,nakaza_zeme_csu_kod,reportovano_khs (^)
5841443b-7df4-4af9-acab-75ca47010ec3,2020-03-01,43,M,CZ042,CZ0421,1,IT,15cdb7ece-97a2-4336-9715-59dc70a48a2c,2020-03-01,67,M,CZ010,CZ0100,1,IT, (^)
496a049f-656e-4274-a51f-72aa92d01f33,2020-03-05,49,M,CZ042,CZ0421,1,IT,1815a2219-2735-46ae-8b14-658459481b2f,2020-03-06,47,M,CZ010,CZ0100,1,IT, (^)
9f78dd0d-2e71-4d37-89a2-665b44b2a607,2020-03-06,44,M,CZ010,CZ0100,1,IT,
$ cat /dev/null | ./coronaid,datum,vek,pohlavi,kraj_nuts_kod,okres_lau_kod,nakaza_v_zahranici,nakaza_zeme_csu_kod,reportovano_khs
$ ./corona -s daily osoby.csv2020-03-01:
2020-03-02:2020-03-03: (^)
2020-03-04:...
2022-02-19: ##################################################################################2022-02-20: ##########################################
$ ./corona -s monthly osoby.csv2020-03:
2020-04:2020-05: (^)
...2022-01: ######################################################## (^)
2022-02: ##############################################
$ ./corona -s 20 yearly osoby.csv2020: ########
2021: ####################2022: ###########
$ cat osoby.csv.gz | ./corona | head -n 5id,datum,vek,pohlavi,kraj_nuts_kod,okres_lau_kod,nakaza_v_zahranici,nakaza_zeme_csu_kod,reportovano_khs (^)
6f4125cb-fb41-4fb0-a478-07b69ba106a4,2020-03-01,21,Z,CZ010,CZ0100,1,IT,15841443b-7df4-4af9-acab-75ca47010ec3,2020-03-01,43,M,CZ042,CZ0421,1,IT, (^)
5cdb7ece-97a2-4336-9715-59dc70a48a2c,2020-03-01,67,M,CZ010,CZ0100,1,IT,1d345e0e2-9056-4d3f-b790-485b12831180,2020-03-03,21,Z,CZ010,CZ0100,,,
$ cat osoby.csv.bz2 | ./corona | head -n 5id,datum,vek,pohlavi,kraj_nuts_kod,okres_lau_kod,nakaza_v_zahranici,nakaza_zeme_csu_kod,reportovano_khs (^)
6f4125cb-fb41-4fb0-a478-07b69ba106a4,2020-03-01,21,Z,CZ010,CZ0100,1,IT,15841443b-7df4-4af9-acab-75ca47010ec3,2020-03-01,43,M,CZ042,CZ0421,1,IT, (^)
5cdb7ece-97a2-4336-9715-59dc70a48a2c,2020-03-01,67,M,CZ010,CZ0100,1,IT,1d345e0e2-9056-4d3f-b790-485b12831180,2020-03-03,21,Z,CZ010,CZ0100,,,


## CZ020: 482138...


- 6f4125cb-fb41-4fb0-a478-07b69ba106a4,2020-03-01,21,Z,CZ010,CZ0100,1,IT,15841443b-7df4-4af9-acab-75ca47010ec3,2020-03-01,43,M,CZ042,CZ0421,1,IT,
- $ ./corona infected osoby.csv 5cdb7ece-97a2-4336-9715-59dc70a48a2c,2020-03-01,67,M,CZ010,CZ0100,1,IT,1d345e0e2-9056-4d3f-b790-485b12831180,2020-03-03,21,Z,CZ010,CZ0100,,,
- $ ./corona infected infected-jan22/infected-22-01-*.csv
- 741d72a4-2b6e-4703-872d-928748ca0ade,2022-01-01,3,Z,CZ020,CZ0203,,,1f39754b8-5e7f-44fd-8b65-e4e7e3b89521,2022-01-01,52,Z,CZ052,CZ0522,,, $ ./corona merge infected-jan22/infected-22-01-*.csvid,datum,vek,pohlavi,kraj_nuts_kod,okres_lau_kod,nakaza_v_zahranici,nakaza_zeme_csu_kod,reportovano_khs
- ...1a27f58f-8950-40c5-89fa-3795f4a906f4,2022-01-31,19,Z,CZ063,CZ0635,,,
- 9aebc069-89d5-4ba0-96c5-aefa1f2c6746,2022-01-31,19,M,CZ064,CZ0642,,,
- $ cat osoby.csv | ./corona genderM:
- Z:
- $ curl -s 'https://pajda.fit.vutbr.cz/ios/ios-22-1-inputs/-/raw/main/data/osoby.csv' | ./corona -a 2021-07-19infected
- $ cat osoby.csv | ./corona daily2020-03-01:
- 2020-03-02: 02020-03-03:
- 2022-02-19: 82182022-02-20: 2020-03-04: 1...
- $ cat osoby.csv | ./corona monthly2020-03:
- 2020-04: 43852020-05:
- ...2022-01:
- 2022-02:
- $ cat osoby.csv | ./corona yearly2020:
- 2021: 17508482022:
- $ ./corona districts osoby.csvCZ0100:
- CZ0201: 34423CZ0202:
- CZ0203: 54368CZ0204:
- ...CZ0806:
- None:
- $ ./corona regions osoby.csvCZ010:
- CZ080: 387509None:
- $ ./corona age osoby.csv0-5 :
- 6-15 : 51186816-25 :
- 26-35 : 51167236-45 :
- 46-55 : 57006456-65 :
- 66-75 : 22548576-85 :
- 86-95 : 3940596-105:
- >105 : 302None :
- $ ./corona infected osoby2.csv
- Invalid date: 0dc57759-d153-45c2-8d14-fb92fc028060,2020-15-03,62,Z,CZ010,CZ0100,,,1Invalid age: 5b0a9692-a72a-4f34-a014-83ae08a79f20,2020-03-10,3.1415,Z,CZ071,CZ0712,1,IT,
- $ ./corona daily osoby2.csv2020-03-01:
- 2020-03-03: 22020-03-04:
- 2020-03-05: 3Invalid date: 0dc57759-d153-45c2-8d14-fb92fc028060,2020-15-03,62,Z,CZ010,CZ0100,,,
- Invalid age: 5b0a9692-a72a-4f34-a014-83ae08a79f20,2020-03-10,3.1415,Z,CZ071,CZ0712,1,IT,
- $ ./corona -d okresy.csv districts osoby.csvBenesov : Rozšíření
- Beroun : 33545Blansko :
- Zdar nad Sazavou: 37928None : Brno-mesto : 123692...
- $ ./corona -r kraje.csv regions osoby.csvHlavni mesto Praha :
- Jihocesky kraj : 206288Jihomoravsky kraj :
- Karlovarsky kraj : 77709Kraj Vysocina :
- Kralovehradecky kraj: 190181Liberecky kraj :
- Moravskoslezsky kraj: 387509Olomoucky kraj :
- Pardubicky kraj : 183208Plzensky kraj :
- Stredocesky kraj : 482138Ustecky kraj :
- Zlinsky kraj : 198910None :


