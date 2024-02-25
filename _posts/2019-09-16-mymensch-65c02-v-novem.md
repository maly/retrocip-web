---
id: 1164
title: 'MyMensch &#8211; 65C02 v novém'
date: 2019-09-16T16:46:47+01:00
author: Martin Maly
excerpt: Nová vývojová deska s 65C02 přímo od výrobce WDC. Tentokrát s bohatou výbavou v jednom FPGA.
layout: post
guid: https://retrocip.cz/?p=1164
permalink: /mymensch-65c02-v-novem/
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2019/09/mymensch1024-1024x198.jpg
categories:
  - Emulace
  - Hardware
  - IoT
tags:
  - "6502"
  - 65C02
  - 65C816
---
 

Víte, už jsem si myslel, že jsem jeden z mála bláznů, co přepisují staré procesory do FPGA. Tak: nejsem! Dělají to i jiní, lepší&#8230;

Třeba The Western Design Center, neboli WDC, společnost Billa Mensche, spoluautora MOS6502. Ta dodneška vyrábí nejen procesory [W65C02](https://retrocip.cz/jak-funguji-nedokumentovane-instrukce-6502/), ale i šestnáctibitovou verzi [W65C816](https://retrocip.cz/jednodeskovy-pocitac-s-procesorem-65816/), a oba dokonce jako mikrokontrolér se zabudovanými periferiemi a monitorem v ROMce (W65C134 a W65C265).

Ke všem procesorům mají i vývojové desky, [můžete se mrknout](https://www.tindie.com/stores/wdc/): [W65C02SXB](https://www.tindie.com/products/wdc/w65c02sxb/), [W65C816SXB](https://www.tindie.com/products/wdc/w65c816sxb/), [W65C134SXB](https://www.tindie.com/products/wdc/w65c134sxb/) a [W65C265SXB](https://www.tindie.com/products/wdc/w65c265sxb/). Pokud vás zaráží, že &#8222;mikrokontrolérové&#8220; verze jsou levnější než ty s původními procesory, je to proto, že na desce s &#8222;holým&#8220; procesorem jsou i periferní obvody, které u mikrokontroléru nejsou potřeba, protože jsou integrované.

Mimochodem, za necelých 20 USD nabízejí [MENSCH Microcomputer](https://www.tindie.com/products/wdc/menschtm-microcomputer/), což je vlastně W65C816, 16bitový následovník slavné 65C02, zabalený spolu s několika periferiemi do podoby jednočipu (W65C265). Na desce toho o moc víc není, ale máte většinu pinů vyvedenou, a je to taková dobře použitelná varianta, když chcete nasadit 65C816 a zároveň ušetřit za periferie.

Ale zpátky k FPGA: Ve WDC samozřejmě už delší dobu nabízejí svoje procesory jako IP (Intellectual Property) pro syntézu obvodů v logických polích. Teď se rozhodli nabízet vlastní &#8222;nový&#8220; procesor, opět jako IP, ale tentokrát připravili i vývojovou desku [W65Cx65MMC](https://westerndesigncenter.com/wdc/documentation/w65cx65mmc.pdf), familiárně zvanou MyMensch.

Deska obsahuje kromě USB převodníku, spousty pinů, tlačítek RESET a NMI, osmi LEDek a JTAG rozhraní hlavně prostor pro FPGA řady MAX10 v provedení BGA.

Mají připraveno několik variant, ta první ([Rev-A](https://www.tindie.com/products/wdc/mymensch-rev-a/)), která je dostupná na trhu a kterou po mém dotazu [dali do prodeje i na Tindie, kde není omezení pro zasílání do ČR](https://www.tindie.com/products/wdc/mymensch-rev-a/), obsahuje FPGA MAX10M08SC, ve kterém je &#8222;nahraný&#8220; obvod [W65C02i1M08SC](https://www.westerndesigncenter.com/wdc/fpga/W65C02i1M08SC.pdf).

Že jste o něm nikdy neslyšeli? Nedivím se, je to úplná novinka. Jak název napovídá, je tam W65C02 a je to v MAX10M08SC. Ale kromě samotného jádra 65C02 zaintegrovali i:

  * 2x W65C22 VIA (paralelní porty)
  * 2x W65C51 ACIA (sériové porty)
  * Rozhraní I2C
  * Rozhraní SPI
  * JTAG
  * HW násobičku a děličku
  * Paralelní rozhraní pro maticovou klávesnici 4&#215;4 s debounce logikou
  * Rozhraní pro uživatelskou FLASH paměť UFM
  * Unikátní 64bitové ID
  * 30 kB RAM pro uživatelské programy (s mechanismem ochrany paměti proti přepsání)
  * 2 kB Boot loader
  * 12 kB RAM pro data
  * Hardwarový breakpoint
  * &#8230; a zbytek jsou vstupně-výstupní piny GPIO

Celé to běží na 14.7456 MHz, ACIA mají 1.8432 MHz (tedy maximální komunikační rychlost je 115200 Bd)

[![](https://retrocip.cz/wp-content/uploads/sites/6/2019/09/w65c02i1-1024x734.png)](https://retrocip.cz/wp-content/uploads/sites/6/2019/09/w65c02i1.png)<figcaption>Blokové schéma, autor: dokumentace WDC</figcaption> 

Pokud si chcete hrát s 65C02, i když ne s tou úplně originální, a chcete k tomu použít hotový systém s mnoha a mnoha možnostmi, dokonce i s modernější 3.3V logikou, zkuste třeba právě [MyMensch](https://www.tindie.com/products/wdc/mymensch-rev-a/). 

Mimochodem, WDC na téhle technologii provozuje i nějaké [gadgety pro Internet věcí](https://wdc65xx.com/Demo)&#8230;

(Předpokládám, že můžete FPGA sami naprogramovat a použít tak desku jako běžný FPGA devkit, ale obávám se, že původní obsah už nezískáte&#8230;)