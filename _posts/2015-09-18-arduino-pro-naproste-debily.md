---
id: 705
title: Arduino pro naprosté debily (?)
date: 2015-09-18T13:24:47+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=705
permalink: /arduino-pro-naproste-debily/
xyz_lnap:
  - "1"
image: http://retrocip.cz/wp-content/uploads/sites/6/2015/08/IMG_20150822_102301-1140x198.jpg
categories:
  - Hardware
tags:
  - arduino
  - Atmel
  - AVR
---
V tomto článku bych rád vytyčil pevný bod pro další diskuse, a definitivně rozčísnul otázku: _Jsou naprostí debilové ti, kdo Arduino používají, nebo ti, kdo si myslí to, co je v titulku?_

<!--more-->

Před časem jsem [citoval z článku Phila Torrona o Arduinu](http://kcc.misantrop.info/2015/05/21/jednoduche/). Phil zmiňuje odpor a pohrdání, jaké Arduino vzbuzuje u těch, co se považují za odborníky. &#8222;Žvatlavé programování pro zhulence&#8220;, v originále &#8222;baby-talk programming for pothead&#8220;, je docela roztomilý výrok. Psal jsem, že i u nás je mnohem víc diskusí, kde se podobní &#8222;odborníci&#8220; exponují, než míst, kde se s Arduinem něco rozumného dělá, což je škoda.

Nemusel jsem ani dlouho čekat. Stačilo zmínit, že se Štěpánem děláme [workshop o Arduinu](http://arduino101.cz), a dozvěděli jsme se, jaký nesmysl to je. Tentokrát to neproběhlo na obvyklých místech (Root, Lupa), ale na ABC Linuxu, což je takový Root v bledě modrém. V [diskusi](http://www.abclinuxu.cz/zpravicky/workshop-arduino-101-v-brne) se tam projevilo hned několik znalců, kteří o Arduinu vědí zjevně kulové, zato si tím jsou naprosto jisti a jsou ochotní se kdykoli zhádat.

Diskuse samozřejmě graduje přesně ve stylu každé podobné diskuse: _Arduino je úplně špatné, protože je příliš jednoduché_, během deseti příspěvků se diskutuje o tom, jestli čtyřjádro nebo dvoujádro, co má pomalý či rychlý přístup na NAS, jak tam funguje Ethernet&#8230; zkrátka zase diskuse, ze které se nedozvíte nic užitečného, pokud si zrovna nejdete poměřovat nerdození.

Některá tvrzení naprosto přesně ukazují, že dotyčný o Arduinu neví ale zhola nic, pouze si _něco myslí_. Pojďme si jich pár dát:

> protože arduino vychovává lidi k demenci (např. místo toho, aby byli lidi učeni nakreslit poctivé schéma, jsou jim předkládány [debilní čmáranice jak pro pomocnou školu](http://2.bp.blogspot.com/-yaibZo6IhvE/TzA7XA_TQBI/AAAAAAAAA2I/EU-fPE-YEDk/s1600/Pneumatic-Servo-Schematic_bb.png). A to nemluvím o opomíjení základních technik při návrhu elektronických obvodů, jakými je např. užití blokovacích kondenzátorů, atd, atd.)

Schémata jsou samozřejmě všude, s nimi není problém. &#8222;Debilní čmáranice&#8220; je featura nástroje zvaného Fritzing, který jaksi není ani součást Arduina, ani podmínka používání Arduina, ale je to jen nástroj pro vizualizaci schématů pro lidi, kteří schémata neumějí číst. Ano, i takoví jsou, a i takoví mají právo si něco postavit. Oni si postaví jednu věc, druhou, a pak se schémata naučí. Když je nepustíte k elektronice a nenecháte je na nic sáhnout, DOKUD se to všechno nenaučí, tak utečou. Je to výukový nástroj pro lidi, kteří se teprve učí! A pro ně je Arduino skvělé: mohou si s elektronikou začít hrát dřív, než je podobní _strážci kultu_ otráví teorií!

> protože arduino převálcovalo spoustu menších projektů. Stejně, jako nesnáším monopolizaci u MicroShitu, nesnáším ji i u Arduina. Pro naprostou většinu jednoduchých projektů není arduino třeba, lze si v pohodě vystačit s MCU jako je AVR nebo PIC. Dřív to bylo tak, že člověk našel na netu tu knihovnu pro určitou součástku, tu knihovnu pro jinou součástku, tu knihovnu pro nějaký protokol. Dnes to všechno pohltil jeden moloch &#8211; arduino. Vycucat potřebné části, tak aby to fungovalo s **mým** build systémem je dost na hlavu. Často je jednodušší si vše napsat vlastní. Dřív bylo nutno knihovny psát tak slušně, aby si je kdokoliv mohl do svého projektu bez problému integrovat.

Tam je toho tolik, že člověk neví, odkud začít. Hejt na Microsoft? Ale no ovšem! Hejt na monopol? Jistě! Staré zlaté časy, kdy jen opravdoví programátoři&#8230; zato dneska&#8230;? Included! Sbírka nesmyslů od A do Z. Kde začít? Třeba u toho, že knihovny jsou stejně tak dobré či špatné jako u jakéhokoli jiného open source? Že hardware má takový monopol jako jiný open source, třeba Linux? Nebo se pozastavit nad tím, v čem má výhodu &#8222;samostatné MCU&#8220;?

Ono totiž Arduino je v první řadě devkit. Je tam AVR s bootloaderem a převodník USB<>serial. Okolo AVR je nezbytná bižuterie (krystal, kondenzátory), ale nic víc. &#8222;Holé Arduino&#8220;, kterému se říká Arduino Nano, je v podstatě ryzí devkit. Není problém se k němu chovat jako k devkitu od Atmelu a programovat ho v assembleru a nahrávat software přes avrdude.

> A navíc, používat C++ pro mikrokontroler &#8211; to mohl vymyslet jen opravdový debil!

Keil, IAR, GNU&#8230; jeden debil vedle druhého. Všichni ti vývojáři z velkých firem, co si na Arduinu prototypují nějaká zapojení a testují periferie: jeden větší než druhý! Některé výroky zkrátka vypovídají mnohem víc o tom, kdo je pronáší&#8230;

> nelíbí se mi, že je bundlován hardware se softwarem a s vývojovým prostředím. Už jen proto, že to vysává potenciál, aby někdo např. pro stejný harware přišel s úplně jiným konceptem psaní SW.

No, a mně se zase nelíbí, když lidé píšou podobné nesmysly, aniž by se byť jen trošku namáhali zjistit stav věcí. Totiž: Nic není s ničím bundlováno, pro Arduino lze psát klidně v Eclipse a překládat si to assemblerem a nahrávat přes avrdude, nebo v AVR Studiu, nebo asi ve třiceti dalších různých prostředích, nástrojích a přístupech. BASIC je, Java je, máme [Céu](http://arduino.cc/forum/index.php/topic,90129.0.html), FORTH, Python, máme online IDE Codebender, máme vizuální nástroje, vycházející ze Scratche (ArduBlock, ModKit, Minibloq)&#8230; Takže efekt &#8222;vysávání potenciálu, aby někdo např. pro stejný hardware přišel s úplně jiným konceptem psaní SW&#8220; spíš nepozorujeme, než pozorujeme&#8230;



Arduino má totiž tolik uživatelů, že se pro ně vyplatí dělat alternativní koncepty a nástroje; to se Atmelu samotnému za patnáct let nepodařilo. A navíc &#8211; díky mezivrstvě v C++ (_jen debil mohl vymyslet&#8230;_) se naprostá většina těchto nástrojů dá přeportovat pro jiná MCU, na jiná _blabla_duina, ať už je pohání MSP430, PIC, ARM, &#8230;

> C++ pre arduino nie je kompletné keďže nietoré veci by sa tam implementovali dosť blbo

Mnojo. Je tak nekompletní, jak jen avr-g++ může být. Včetně např. možnosti psát inline assembler a navzájem volat rutiny.

> Okrem toho mi vadí to, že sa C++ skrýva za .ino skratku, prechrúme sa preprocesorom a ak je nebodaj chyba niekde v knižnici alebo môj kód spôsobí chybu v knižnici tak si s chybovou hláškou môžem maximálne tak

Nejen že se skrývá za .ino, ono navíc pracuje s konceptem &#8222;sketche&#8220;, což je vlastně celý adresář, který se musí jmenovat stejně jako to .ino! Hrůza. _Pardon, promiňte lacinou ironii_.

Co bych naopak podepsal, tak je zpracování chyb. Ale ne preprocesorem, ale v IDE. Někdy totiž probublá ryze programátorská hláška z hlubin GNU C++, a pak je to docela divočina. No a hrozně blbě se na Arduinu věci ladí.

> AVR je pomalý osmibit s malou pamětí

To je klasická nerdhláška. Z dob, kdy byl Zdroják pod Rootem a takovíhle odborníci mi chodili komentovat pod články, rád používám odpověď _&#8222;&#8230; a neumí to rošádu!_&#8220; AVR je pro spoustu aplikací naprosto dostatečně výkonný stroj, protože ve světě embedded zařízení a IoT a podobných zločinů není všechno binární &#8211; buď _enterprise aplikace v Javě / C++ a zpracování videa v reálném čase_, nebo _pitomost_. Naopak, pro velké množství aplikací je výkon osmibitu na 16MHz nejen dostatečný, ale i vhodný. Namátkou aplikace, o které jsem se bavil tuhle: sledování kontejnerů se zbožím. Na každém kontejneru je osmibit, GPS a vysílačka. Jakmile se kontejner zastaví, tak se zařízení probere, zjistí polohu, odvysílá ji, a zase se uspí. Díky tomu vydrží na jednu baterku třeba dva roky.

**Zkrátka:** Pokud chci proti něčemu bojovat a nebýt přitom za kašpara, tak bych si měl zjistit, co chci znectít a jak to udělám. Na Arduinu se dá najít spousta chyb, ale při vší úctě, jaké jsem směrem k _diskutujícím na českých portálech o Linuxu_ schopen, citované výtky to rozhodně nejsou.

Já sám se na Arduino dívám jako na devkit první volby, když potřebuju otestovat něco s elektronikou. Je tam jednočip s nezbytnou bižuterií, je tam převodník na USB, takže to snadno připojím, je pro to spousta rozšiřujících modulů a knihoven, snadno připojím téměř cokoli, díky tomu, že Arduin je celá řada, od miniaturních Nano s ATMega168 za dolar přes Mega2560 po nové Due a Zera s ARM Cortex-M3 (resp. M0) či Yún, kde je zabudovaný Linux, najdu bez problémů vhodný typ pro danou aplikaci&#8230; Nejsem svázaný ani s IDE, ani nemusím nic navrhovat ve Fritzingu, to je nesmysl jak vrata.

Zároveň je Arduino ideální pro začátečníky, kteří díky němu poznají, že mikroelektronika může být mnohem zajímavější a zábavnější, než diskuse podobných &#8222;odborníků&#8220; naznačují.

Nenechte se jimi otrávit, vezměte si Arduino do ruky a zkuste si to sami. Když nebudete mít nesmyslné požadavky (&#8222;ono mi to nepřehrává video ve Full HD, je to nanic!&#8220;), tak zjistíte, že to je fajn laciný devboard se spoustou doplňků.

\[sc:ebay item=&#8220;arduino nano usb&#8220;\]\[sc:ebay item=&#8220;arduino uno USB&#8220;\]\[sc:ebay item=&#8220;arduino 2560 USB&#8220;\]\[sc:ebay item=&#8220;arduino due SAM3X8E&#8220;\]