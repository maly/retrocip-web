---
id: 1119
title: Malá nápověda ke včerejšímu dotazníku
date: 2018-11-14T10:35:41+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1119
permalink: /mala-napoveda-ke-vcerejsimu-dotazniku/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "7043814478"
image: https://retrocip.cz/wp-content/uploads/sites/6/2014/03/T2eC16VHJHFFmOoS6iBRzsj7cfw60_35-300x198.jpg
categories:
  - Hardware
tags:
  - "6309"
  - "6502"
  - "6809"
  - OMEN
  - OMEN Bravo
  - OMEN Kilo
---
Já vím, že je těžké [vybírat si](https://retrocip.cz/jste-unaveni-z-gigahertzu-a-terabytu/), když nemáte kompletní informace, tak se jistě nebudete zlobit, když vám ukážu takové malé srovnání procesorů 65C02 a 6809.

<!--more-->

Oba procesory mají stejného ideového předchůdce, a tím je procesor Motorola 6800. Ten vychází z architektury počítače PDP-11 a zavedl do světa mikroprocesorů několik konceptů.

Zaprvé: Procesor samotný má na čipu jen minimum registrů, v porovnání s procesory Intel a odvozenými. Konkrétně dva osmibitové akumulátory A a B, do značné míry ortogonální, šestnáctibitový indexový registr, ukazatel zásobníku, příznaky a programový čítač.

Zadruhé: Procesor pracuje intenzivně s pamětí. Logicky &#8211; nemá totiž dostatek registrů. Tam, kde procesory Intel vesele provádějí operace mezi akumulátorem a některým z registrů, tam procesory Motorola pracují s obsahem paměti. Jejich časování je tomu uzpůsobené, takže přistupují k paměti s každým taktem. Pro adresaci paměti existuje několik různých módů &#8211; přímá adresa, nepřímá, indexovaná&#8230;

Zatřetí: Procesor definuje takzvanou &#8222;nultou stránku&#8220; (zero page), tedy oblast paměti 0000h &#8211; 00FFh. Pro přístup do ní může programátor použít zkrácenou adresu (pouze jednobajtovou). Takové instrukce jsou navíc rychlejší.

Začtvrté: Procesor využívá indexování a nepřímou adresaci. U indexování se pracuje s obsahem indexového registru a nějakou hodnotou, konstantní nebo uloženou v registru. U nepřímého adresování paměti se nejprve spočítá zadaná adresa, a až z ní se z paměti načte výsledná adresa operandu (takzvaná _efektivní_ adresa).

## 6502

Procesor 6502 je levnější a intenzivně zjednodušená varianta 6800. Místo dvou akumulátorů používá pouze jeden, indexové registry má sice dva, ale osmibitové, ukazatel zásobníku je také osmibitový (a zásobník má pevně určený prostor na adresách 0100h &#8211; 01FFh). Zjednodušená je i instrukční sada, například místo dvou instrukcí pro sčítání, bez přenosu a s přenosem, je pouze jediná (sčítání s přenosem), a programátor musí nastavit příznak přenosu na nulu, pokud chce přenos ignorovat.

Adresních módů je o něco víc, ale nejsou plně ortogonální, tj. neplatí, že pro každou instrukci můžete použít všechny módy.

Procesor 65C02 je vylepšená varianta původního 6502, která kromě technologie CMOS přinesla i některé nové instrukce, zvýšila ortogonalitu a přidala adresní módy.

Výhodou 6502 je jeho rychlost &#8211; u většiny instrukcí platí, že trvají dva, tři, čtyři takty, většinou podle toho, jak často se musí přistupovat do paměti. Proto je výkon tohoto procesoru na 1 MHz plně srovnatelný s Intelskými procesory na vyšších frekvencích.

## 6809

Tento procesor je naopak o generaci dál než 6502. Už jsem tu o něm několikrát psal, třeba [zde](https://retrocip.cz/posledni-krasavec-osmibitove-ery/), tak jen pro připomenutí: Je pokračovatelem řady 6800 a vyvinula ho opět Motorola, paralelně s vývojem většího 68000.

Nabízí opět dva osmibitové akumulátory, které mohou pracovat jako jeden šestnáctibitový. K indexovému registru X přibyl druhý registr Y, oba šestnáctibitové. K ukazateli zásobníku S přibyl druhý ukazatel zásobníku U. 

Nultá stránka, &#8222;zero page&#8220;, se u procesoru 6809 nazývá &#8222;přímá stránka&#8220; (Direct Page), protože může být umístěna kdekoli v paměti (vždy na adresách xx00h až xxFFh). Horní část adresy se bere ze speciálního osmibitového registru DP.

6809 má opět standardní sadu adresních módů: přímý operand, krátká adresa do Direct Page, dlouhá adresa, a nepřímé varianty obojího. K tomu přidává indexové módy, které jsou neskutečně bohaté. Posuďte sami:

Jako indexový registr může sloužit registr X, Y, S nebo U. Někdy i PC, tedy programový čítač. Konstanty, které se k obsahu těchto registrů přičítají, mohou být velmi krátké (5 bitů), krátké (8 bitů) nebo dlouhé (16 bitů). Popřípadě nemusí být vůbec konstantní &#8211; může se pracovat s akumulátorem A, B, případně s oběma najednou a cílová adresa je pak určena třeba šestnáctibitovým součtem hodnoty v registru X a v registrech A a B.

Další možnost je, že si necháte obsah indexového registru po operaci zvýšit o 1 nebo 2, popřípadě před operací snížit o 1 nebo 2 (postinkrement / predekrement).

A tohle všechno může být ještě nepřímé, tj. výsledná adresa není adresa operandu, ale místa, z níž se tak skutečná adresa teprve přečte.

Instrukční sada je vysoce ortogonální: pokud má nějaká instrukce operand, je téměř jisté, že ho budete moct adresovat jakýmkoli způsobem z výše popsaných.

Některé instrukce, jako ukládání do zásobníku, byly vylepšeny tak, aby odrážely programátorskou praxi, takže například umožňují jedinou instrukcí uložit téměř kompletní sadu registrů&#8230; Fajn je také interní násobička, která zjednodušuje a zrychluje operace násobení.

O rychlosti platí to, co v předchozím případě: instrukce trvají v průměru dva až pět taktů &#8211; u náročnějších adresních módů je to samozřejmě o jeden, dva takty delší. Ale třeba 2MHz 6809 v OMEN Kilo je výkonem, troufám si říct, na tom lépe než 4MHz Z80.

Mimochodem &#8211; procesor 6809 můžete nahradit jeho rychlejší variantou 6809C (až 3 MHz), nebo naprosto famózně vylepšenou verzí HD63C09 &#8211; psal jsem o ní [zde](https://retrocip.cz/6309-vse-je-neoficialni/). 6309 sice navenek funguje úplně stejně jako 6809, ale má způsob, jak se přepnout do full native módu, v němž jsou instrukce o něco rychlejší. Navíc přidává sadu vylepšených instrukcí, včetně blokových přenosů, další dva akumulátory, dvaatřicetibitovou aritmetiku&#8230;

Tak co, už víte, co chcete? 🙂