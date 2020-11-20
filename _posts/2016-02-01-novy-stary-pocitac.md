---
id: 739
title: Nový starý počítač
date: 2016-02-01T16:15:26+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=739
permalink: /novy-stary-pocitac/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "4542049520"
categories:
  - Hardware
tags:
  - "6309"
  - 65C816
  - "6809"
  - Spectrum
  - VGA
---
Mám tu pár námětů na přemýšlení&#8230; zase&#8230;

<!--more-->

Postavit si nový starý počítač, rozuměj z nových součástek počítač v duchu dob předchozích, je oblíbená zábava nás, retronadšenců. Přístupů je několik:

  1. Postavit repliku starého stroje s identickými součástkami a původním schématem
  2. Postavit funkční repliku starého stroje tak, že některé součástky (typicky ty s nízkou integrací nebo paměti) nahradíme novějšími ekvivalenty (místo osmi čipů paměti jen jeden, místo glue logiky CPLD&#8230;)
  3. Postavit emulovanou repliku starého stroje (ZX Spectrum ve FPGA třeba)
  4. Postavit počítač vlastní konstrukce se součástkovou základnou z roku, řekněme 1984.
  5. Počítač vlastní konstrukce se starým osmibitovým procesorem, ale s novějšími periferiemi
  6. Počítač vlastní konstrukce, který sice využívá starý procesor a &#8222;ducha&#8220; té doby, ale je třeba kompletně celý ve FPGA
  7. Počítač vlastní konstrukce s moderními součástkami a procesory, ale v duchu dob minulých (jednodeskáč s LED displejem a hexadecimální klávesničkou&#8230; a uvnitř Pentium třeba)

Plus cokoli mezi tím, že.

Já si teď hraju, po tom FPMI, s myšlenkou něčeho podobného. Některé věci jsou jasné:

  * Chci tam mít VGA výstup, protože monitorů je všude spousta a jsou levné, snáze sehnatelné než televize, jednoduše zapojitelné, a navíc [generování signálu pro VGA](https://vhdl.cz/vga-generujeme-obraz/) není problém zařídit pomocí FPGA.
  * Chci tam mít SD kartu. Asi není potřeba o ničem debatovat, jednodušší alternativa není.
  * Sériový port potěší. A když už bude, tak bude rovnou jako USB, protože prostě proto.
  * Paměť bude SRAM na 3.3V, třeba někde okolo 512kB, a k ní bude jednoduchá MMU, jako když se rozšiřovalo Spectrum

\[sc:ebay item=&#8220;AS6C4008 PCN&#8220;\] \[sc:ebay item=&#8220;EP2C5T144 board&#8220;\]

Jinde se ještě rozhoduju.

Třeba **klávesnice**. U FPMI jsem použil tenhle retrokousek: [sc:ebay item=&#8220;1pc White Keyboard 5x 25 keys&#8220;] 

ale tentokrát budu chtít QWERTY. Mám dvě možnosti. Zaprvé &#8211; použiju jakoukoli klávesnici s PS/2. Je jich na trhu jako smetí, jejich připojení je jednoduché a máme ji doma všichni.

Zadruhé: udělám si vlastní. Líbila by se mi taková, jako byla u JPR-1 ([ANK-1](https://www.oldcomputers.wz.cz/pocitace/JPR-1/ANK-1%20pripojena%20k%20SAPI.JPG), pro pamětníky) &#8211; membránová. Je jednoduchá na výrobu, docela levná a vypadá správně děsivě. Když totiž hledám nějaká tlačítka s hmatníkem a vyšším zdvihem, z jakých bych mohl klávesnici sestavit, tak jsou výsledky žalostné.

Další otázka: **Procesor**. Líbil by se mi [6809](https://retrocip.uelectronics.info/posledni-krasavec-osmibitove-ery/)/[6309](https://retrocip.uelectronics.info/6309-vse-je-neoficialni/). Oba mám, to není problém. Problém je, že ho budu připojovat k FPGA, které používá 3.3V. 6809/6309 na tomhle napětí nepoběží. Podle některých by to šlo, ale brutálně podtaktované. Taky bych mohl použít převodníky úrovní &#8211; třeba ty s TXS0108 zvládnou i požadované frekvence.

\[sc:ebay item=&#8220;TXS0108 converter&#8220;\] \[sc:ebay item=&#8220;6809 Motorola&#8220;\] [sc:ebay item=&#8220;HD63C09P Hitachi&#8220;]

Druhá varianta je použít [65C816](https://retrocip.uelectronics.info/asm80-podpora-pro-65c816/) od WDC. Je to zajímavý hybrid mezi osmibitovým (6502) a šestnáctibitovým procesorem, ale hlavně funguje i na 3.3V (podle datasheetu by to na tomto napětí mohlo zvládnout frekvenci 14 MHz). Ergo bych ho jen tak připojil na FPGA, bez konvertorů, a celý počítač se mi tak smrskne na procesor, FPGA, statickou RAM. [sc:ebay item=&#8220;CPU 65C816&#8243;] 

Třetí varianta je nacpat celé jádro procesoru do FPGA taky. V tom případě bych asi použil 6809 &#8211; pro 6309 jsem nenašel vhodné funkční jádro, bohužel.

Otázka je, jestli **přidávat FLASH nebo ne**. Na jednu stranu je to jeden čip navíc, který zabere nemálo pinů u FPGA, na druhou stranu je zase práce s tím jednodušší. Druhá možnost je použít sériovou paměť FLASH a pro boot použít malou, třeba 1kB rutinu, která bude klidně uložena v interní paměti FPGA, po nabootování se přepíše do RAM, odpojí ROM a zavede z té sériové FLASHky celý systém. Třetí možnost je využít to, že stejně bude SD karta, a bootovat z ní. To by ale pak bylo na místě řešit věci jako práce s filesystémem na SD kartě už v bootloaderu, a to se mi asi moc nechce. Pravděpodobně tedy půjdu cestou druhou. [sc:ebay item=&#8220;25Q64BVAIG 1pcs&#8220;] 

Takže: procesor mám, paměť i základní logiku taky, co dál? Periferie jsou jasné čtyři: klávesnice, VGA, SD a sériový port. Co dál je potřeba? Uvidím, kolik mi zůstane místa ve FPGA, a podle toho bych doplnil třeba druhý sériový port (k němu připojit [ESP8266](https://esp8266.cz)?) nebo SPI/I2C rozhraní, dostupné pro procesor jako periferie.

Zvuk? No, minimálně nějaký beeper se hodí. Dál bych ale asi zatím nešel, do budoucna můžu klidně zaintegrovat nějaký generátor hluku, buď jako samostatný čip (SAA1099, SN76489, SID, AY, &#8230;), nebo jako soft core do FPGA.

Displej chci jednoduchý grafický se základními barvami. Něco jako mělo Spectrum, trošku líp vyřešené atributy, ale zase nechci zaplácnout celý paměťový prostor (u 65816 by to asi nevadilo, ten dokáže adresovat až 16M) a ani rychlé by to moc nebylo. Nechci se zatím zabývat nějakým blitterem, akcelerátorem, hardwarovými sprity nebo něčím takovým.

Myš asi nebude hned nutná.

Zkrátka: představte si takové ZX Spectrum s procesorem 6809/65816, připojené k VGA a se SD místo diskety / kazety&#8230;

Zajímá mě, jak byste se vy rozhodovali v těch výše nastíněných otázkách&#8230;