---
id: 818
title: O objektu, struktuře a gotu
date: 2016-06-22T23:46:04+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=818
permalink: /o-objektu-strukture-a-gotu/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "4931548188"
image: http://retrocip.cz/wp-content/uploads/sites/6/2014/06/120px-NAND_ANSI_Labelled.svg_.png
categories:
  - Jen tak
  - Software
tags:
  - Assembler
  - Jednočipy
  - Programování
  - Software
---
A taky o globální proměnné.

Programátoři se od pradávna dělili na Opravdové Programátory a Pojídače koláčů. Tenhle sloupek Eda Posta je tak notoricky známý, že ho ani nebudu připomínat, a místo toho rovnou řeknu, co mám na srdci&#8230;

Totiž já už to psal, ale klidně to připomenu: Učte se programovat mikrokontroléry, naučte se assembler aspoň jednoho procesoru, a uvidíte, jak vám věci začnou dávat smysl.

Uvědomil jsem si to, když mi jeden velmi zkušený programátor ukazoval svůj kód pro Arduino a divil se, že se mu to nevejde do paměti. Koukám do kódu, a ten byl &#8211; přátelé! &#8211; ten byl nádherný! Srdce každého učitele programování by nad tím zaplesalo. Nahoře měl konstruktor a destruktor, pod tím ty struktury, správně přetypované malloc()&#8230;

Polknul jsem.

&#8222;Co? Mám chybu v tom pointerovém typecastu? To snad ne&#8230;&#8220;

Povídám: &#8222;Co to tady tím mallocem tvoříš?&#8220;

Prý že nějaké úložiště dat&#8230; V hlavní smyčce se volají dvě funkce, první nějak sebere data, vytvoří tuhle strukturu, předá ji druhé, ta je zpracuje a strukturu zase zruší. Napsal bych objekt, ale nemělo to metody. Tady by asi ten učitel povytáhnul obočí.

Funkce, která data zpracovávala, měla taky nějaké lokální proměnné. Ještě že nepoužil rekurzi, pak by bylo neštěstí hotové.

Dal jsem mu stručnou nalejvárnu. Naštěstí šlo o chápavého a velmi liberálního programátora. Kdyby byl rigidní, tak mě ještě dneska škrtí a křičí, že globální proměnná neexistuje a GOTO je ďábel!

Pojďme si to trošku nahodit na koleje. Pokud jste programátor bankovních systémů v Javě, pokud děláte weby nebo pokud programujete, cojávím, matematické algoritmy v &#8230; čemkoli, zkrátka pokud programujete cokoli s výjimkou low level systémů, tak si opravdu vystačíte s objektovým / objektově orientovaným přístupem, ti drsnější z vás se naučí funkcionální, a neberte to teď nijak zle: vystačíte si s tím a budete v tom klidně dobří. Ale nepouštějte se, probůh, do low level, protože tam jsou lvi a temnota a zmatení a jistoty přestávají platit.

Trošku to připomíná situaci člověka, který je zvyklý na newtonovskou mechaniku, a najednou je postaven před teorii relativity. Všechno je jinak a sčítání rychlostí není sčítání a tak. Jenže platí, a to si zapište červenou tužkou hned vedle tabulky ASCII kódů, že v normálním životě si většina lidí vystačí s newtonovskou mechanikou, a kdo by při výpočtu doby dojezdu z Prahy do Brna autem po D1 montoval relativistické vzorce, dopočítal by se téhož, ale působil by značně nepatřičně. Tam stačí obyčejné &#8222;vzdálenost lomeno rychlost&#8220;. _Teda &#8211; na D1 je to potřeba ještě vynásobit imaginární jednotkou _i_, protože D1 se řídí vlastními zákony a žádný vzorec nikdy nezohlední počet bílých Transitů, Felicií jedoucích 110 v levém pruhu, vymletý pravý pruh od Humpolce po Jihlavu ani Slováků v audinách tak temných, že mají zatemněné i přední sklo, ale &#8211; rozumíme si, ne?_

Zkrátka úplně stejně, jako jsou relativistické vzorce vhodné jen pro některé oblasti, tak jsou low level postupy taky vhodné převážně pro low level programování. Ale zase je dobré je znát, abyste si nepřipadali před cyklotronem a Arduinem jako mistři světa, protože umíte newtonovskou fyziku a Quicksort.

\[sc:ebay item=&#8220;arduino nano usb&#8220;\]\[sc:ebay item=&#8220;arduino uno USB&#8220;\]\[sc:ebay item=&#8220;arduino 2560 USB&#8220;\]\[sc:ebay item=&#8220;arduino due SAM3X8E&#8220;\]

Pár základních věcí tedy zde:

Zaprvé &#8211; globální proměnné existují a jsou užitečné. Pokud se pracuje s jednou instancí něčeho (tedy jako že singleton), tak to uložte do &#8222;globální proměnné&#8220;. V praxi to dostane v RAMce, které v jednočipu není nikdy dost, tolik bajtů, kolik to skutečně zabírá, bude to na konstantní adrese, a nazdar bazar, bude to tam furt, přistupuje se k tomu jednoduše a hotovo dvacet.

Zadruhé &#8211; žádná halda. Víte vy, jak se pracuje s haldou? Jasně že víte, ale otázka zní: jak to ten procesor vlastně dělá, že tu haldu má, a že vám z ní přiděluje paměť, o kterou si řeknete? No, on si totiž vezme celou volnou paměť, co má k dispozici (a to se liší podle architektury), a udělá si v ní bloky. V zásadě si na začátku zapíše na začátek celé haldy pár servisních bajtů, typicky: toto je prázdný blok, má délku tolik a tolik. Pak uděláte malloc(), knihovna koukne do paměti, najde značku pro &#8222;prázdný blok&#8220;, z ní odpočítá požadovaný počet bajtů, a na začátek zapíše: Alokovaný blok, tolik a tolik bajtů, a za konec zapíše: Prázdný blok, tolik a tolik bajtů. Chvilku je přidělujte a rušte, nejlépe s různými a nesoudělnými délkami, a za chvilku v RAMce nebudete mít k hnutí. Prostě ne, dynamická alokace paměti je v jednočipu prostě hloupý nápad a pokud můžete, tak se jí vyhněte.

Zadvouapůlté &#8211; ukazatel. Když už máte nějakou dynamickou paměť přidělenou, tak k ní přistupujete přes ukazatel a přes index. Což tedy v praxi znamená, že buď má procesor nějaké indexové registry, a do nich si uloží hodnotu ukazatele a pak přistupuje k paměti prostřednictvím nepřímé adresy, nebo je nemá, a pak si musí pokaždé adresu dopočítat. Naštěstí je dneska mají, taky překladače umí hodně dobře optimalizovat, ale raději se na to nespoléhejte, protože i překladač může narazit na omezení, buď svoje, nebo procesoru.

Zatřetí &#8211; žádné lokální proměnné. Taková lokální proměnná, to je místo na zásobníku. Zásobník, helejte, to není nějaká abstraktní struktura ze skript &#8222;Algoritmy I &#8211; datové struktury&#8220;, ale docela zásadní součást procesoru na nízké úrovni. Když voláte podprogram (což je ve vyšších jazycích plusmínus ekvivalent &#8222;volání funkce&#8220;), tak se na zásobník, což je struktura v paměti, uloží adresa, ze které se skáče, aby se po returnu mohl procesor vrátit tam, kde byl naposledy. Na zásobník se taky ukládají věci, které si potřebujete na chvíli někam schovat. Zkrátka zásobník je velmi důležitá věc, bez ní by vám program ve strojáku fungoval naprosto nestrukturovaně. U &#8222;velkých&#8220; procesorů to nebývá úplně takový problém, tam bývá zásobník hodně velký a s chybou &#8222;stack overflow&#8220;, která nastává ve chvíli, kdy toho na zásobník uložíte moc, se setkáte jen zřídka. Nebo ještě hůř &#8211; nesetkáte se s ní až do chvíle, než vám někdo pomocí téhle chyby nezruší systém a nenabourá se do něj. U malých procesorů to už problém je, tam není dost RAMky. Některé dokonce mají omezenou velikost (třeba 6502 měl jen 256 bajtů na zásobníku) a některé jednočipy mají hardwarově zásobník zcela mimo adresní prostor a pevně omezený například na 16 hodnot.

No, a proč o zásobníku mluvím? Protože zrovna lokální proměnné se dělají tak, že po vstupu do podprogramu (=funkce, procedury, jakkoli) se na zásobníku alokuje množství paměti, potřebné pro všechny proměnné, a pak se k nim přistupuje (zase) nepřímo. Moderní překladače to řeší tak, že se snaží alokovat pro tyto situace některé z registrů, ale zase: nejde to vždy a všude, nespoléhejte na to.

Mimochodem &#8211; když předáváte funkci parametry, víte, co se stane? Záleží na překladači, ale obecně platí, že zase uloží všechny na zásobník a zavolají podprogram. Pak je opět zahodí. Znovu: překladače optimalizují a snaží se nacpat parametry do registrů, ale když jich je moc, tak i registry dojdou.

Začtvrté &#8211; rekurze. Není to dobrý nápad, důvod vizte výš.

Za páté &#8211; goto. Nebo GO TO. Zkrátka přímý skok na nějaké místo programu. Od dob strukturovaného programování je to zlo, protože to rozbíjí strukturu. Ve skutečnosti za všemi těmi for a loop a while není nic jiného než prachsprosté goto, jen zakapotované tak, že cílovou adresu skoku řeší překladač a ne chybující programátor. I známé _break_ a _continue_ jsou jen syntaktický cukřík místo goto. V assembleru se mu ale říká většinou jump (JMP, JP, JUMP) nebo branch. Je to velmi silný nástroj, takže chápu, proč ve vyšších jazycích moc nebývá, ale tam dole, tam, kde to smrdí křemíkem, se bez něj neobejdete. A stačí jedna chyba, a program vám najednou skáče, kam nemá, kde ho nikdo nečekal, a dějou se věci veliké.

Holt jednočipy, osmibity a assembler nejsou pro měkkýše&#8230; Jsou to mocné nástroje, ale nebezpečné a v rukou nezkušeného můžou napáchat obrovské škody. Ale rozhodně pomůže, když se je naučíte &#8211; i kdyby hlavním důvodem měla být ta úleva, že programujete v něčem s podstatně vyšší mírou abstrakce.