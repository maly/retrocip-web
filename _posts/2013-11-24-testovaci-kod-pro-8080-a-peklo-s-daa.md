---
id: 29
title: Testovací kód pro 8080 a peklo s DAA
date: 2013-11-24T20:43:16+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=29
permalink: /testovaci-kod-pro-8080-a-peklo-s-daa/
dsq_thread_id:
  - "1995730616"
categories:
  - ASM80.com
tags:
  - "8080"
---
Jak vyzkoušet emulátor procesoru? Nejlépe programem, který používá všechny instrukce. A co když vám zatvrzele padá?

<!--more-->

Ti, co sledují můj Twitter, to už vědí: V pátek v noci jsem napsal emulátor pro BOB-85, v sobotu pak PMD. S PMD bylo trošku problémů, a po odpoledni stráveném s debuggerem a výpisy ROMky i BASICu padlo moje podezření na emulační jádro procesoru 8080. Pravděpodobně tam nějaká instrukce nefunguje jak má.

Jak ji najít? Nejlepší je použít nějaký &#8222;instrukční pangram&#8220;:

> **Pangram** (z [řeckého](https://cs.wikipedia.org/wiki/%C5%98e%C4%8Dtina "Řečtina") _pan gramma_, „každé písmeno“) je [věta](https://cs.wikipedia.org/wiki/V%C4%9Bta_(lingvistika) "Věta (lingvistika)") či úsek textu obsahující všechna písmena [abecedy](https://cs.wikipedia.org/wiki/Abeceda "Abeceda"). Jedná se zpravidla o slovní hříčku, cílem je zpravidla vytvořit co nejkratší, popř. vtipný či jinak zajímavý text s touto vlastností.

Tedy kód, který obsahuje všechny instrukce a otestuje, jestli dělají co mají a jestli jsou příznaky nastavené tak jak by měly být apod. Pro procesor 8080 je takových testů několik, já použil takzvaný [Kelly Smith Test](https://github.com/begoon/i8080-core/blob/master/TEST.ASM) (Microcosm test). Po pár krocích bylo jasno: špatně se počítá paritní bit! Upravil jsem tedy emulační jádro, a test začal fungovat&#8230;

&#8230; a fungoval až skoro na konec, kde se testuje instrukce DAA, tedy dekadické zarovnání po aritmetické operaci. Tam sveřepě padal.

Podíval jsem se, jak je instrukce DAA napsána, nevykazovala chybu. Podíval jsem se, jak je definováno její chování:

> Instrukce DAA (decimal adjust accumulator) dekadicky koriguje součet kladných čísel v kódu BCD a při tom jediná využívá indikátor AC. Korekce probíhá podle tohoto algoritmu: je-li číslo v dolní tetrádě střadače A větší než 9 nebo AC=1, přičte se ke střadači 6; obsahuje-li pak horní tetráda střadače číslo větší než 9 nebo je-li CY=1, přičte se ke střadači ještě 60H.

Tak. Takhle by to _mělo_ fungovat, a takhle je to taky implementované. Což je v pořádku, ale test na tom havaroval. On totiž téhle instrukci zlomyslně předhazoval hodnoty, které by normálně dostávat neměla, a testoval, že se zachová jako opravdová 8080.

Například &#8211; v A je hodnota 55H a AC je nastavený. Podle výše uvedeného algoritmu by měl být výsledek 5BH. Logicky. Ale stejně tak je to (logicky) nesmysl &#8211; výsledkem DAA přeci nemůže být 5BH!

Chvíli jsem pátral a přemýšlel. Oněch 55H + AC nastavený na vstupu je nesmysl &#8211; jaká dvě čísla by se musela sčítat, aby došlo k přetečení u nejnižší tetrády (čtveřice bitů), a zároveň byl výsledek 5? Inu, musely by se sčítat čísla mimo rozsah 0-9, protože 9+9 dá dohromady 12H, víc ani ťuk. Takže 55 a AC na vstupu je chyba, hurá, příčina odhalena, matematik si může jít odpočinout, ale programátorovi to není nic platné, protože mu test neběží.

A tak jsem se dal do googlení, a nejpodrobnější informaci, co jsem sehnal, je ta, že DAA je &#8222;správně&#8220; implementovaná třeba v Z80, ale v 8080 se chová &#8222;trochu&#8220; jinak pro nestandardní vstupní data. Mnoho autorů &#8222;emulátoru 8080&#8220; s tímhle skončilo a často jsem četl prohlášení: &#8222;DAA prostě nefunguje úplně stejně, ale on to stejně nikdo příčetný nepoužívá&#8220;. Hm, to sice je řešení, ale test stejně neběží. Co teď?

Vypadá to (ale fakt jen vypadá!), že chování DAA by se mohlo řídit o něco složitějším algoritmem. Nějak takhle:

> Je-li číslo dolního nibble >9 nebo příznak AC nastaven, přičítá se 6. **Je-li příznak AC nastaven a zároveň je dolní nibble v rozsahu [3..9], jedná se o chybná vstupní data a šestka se nepřičítá.**

Mno, bylo by fajn to ještě pořádně ověřit na skutečném, křemíkovém procesoru, ale bohužel, moje PMI ještě není provozuschopné. Každopádně když jsem to takto napsal, tak celý test prošel bez problémů.

Jo a taky se &#8222;zázračně&#8220; rozběhalo PMD.

¶

Odkazy:

  * [Další test 8080/8085](https://www.idb.me.uk/sunhillow/8080.html)
  * [Diskuse o DAA (IanB)](https://www.motherboardpoint.com/8080-daa-opcode-t163192.html)
  * [Autor VHDL emulátoru 8080 o DAA](https://opencores.org/project,light8080,demos)