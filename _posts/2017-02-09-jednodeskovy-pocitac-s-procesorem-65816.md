---
id: 946
title: Jednodeskový počítač s procesorem 65816
date: 2017-02-09T09:52:40+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=946
permalink: /jednodeskovy-pocitac-s-procesorem-65816/
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2017/02/20170208_172413-600x198.jpg
categories:
  - Hardware
tags:
  - "6502"
  - 65C816
---
To, že se 6502 stále ještě neodebrala do věčných lovišť asi víte. A asi víte, že existuje něco jako procesor 65816, což byl procesor, který ve [WDC](https://westerndesigncenter.com/wdc/) vyvinuli jako 16bitovou variantu 6502. Dokonce se používal v nějakých turbo kartách pro Commodora nebo v Applech. A jestli tohleto všechno víte, tak vězte i to, že se oba kousky stále vyrábějí, a dokonce jsou i ve variantě &#8222;mikrokontrolér&#8220;.

WDC vyrábí a dodává i devkity &#8211; asi nejrozumnější jsou ty řady SXB (Xxcelr8r). Buď si vyberete verzi s holým procesorem, okolo kterého je několik obvodů (VIA, PIA, ACIA), k tomu paměť a USB konvertor, a zaplatíte 200 USD, nebo si vyberete verzi s mikrokontrolérem, kde jsou porty a časovače integrované s jádrem, takže na desce je už jen paměť a USB převodník, a ušetříte docela slušný raneček peněz.

Já zvolil variantu W65C265SXB: tenhle mikrokontrolér má jádro 65C816, které samozřejmě funguje i v režimu 65C02, a k němu jsou připojené 4xUART, 8x časovač a RTC. Přivítal bych nějaké SPI/I2C, ale není. Nevadí. Na desce je 32kb SRAM, mapovaná v rozsahu 00:0000 až 00:7FFF, a je tam i patice na 128k FLASH. 65C816 má 24bitovou adresu, dokáže tedy přímo adresovat až 16MB paměti. Hezké je, že všechny vývody jsou vyvedené na pinové lišty.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/02/20170208_172413.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-947" src="https://retrocip.cz/wp-content/uploads/sites/6/2017/02/20170208_172413.jpg" alt="" width="600" height="648" /></a>

Programování je jednoduché &#8211; přes USB. Uvnitř mikrokontroléru je &#8222;Mensch Monitor&#8220;, tedy klasický monitor, jak si jej pamatujeme. Využívá jednu ze sériových linek, připojenou k FTDI převodníku, takže vy potřebujete jen sériový terminál, a nastavit ho na 9600 Bd. Můžete si pak prohlížet paměť, měnit ji, spouštět programy, nahrávat soubory ve formátu S28 a tak&#8230;

WDC k tomu dodává softwarové nástroje. Je trochu trága se k nim dostat, protože web zůstal duchem v dobách největší slávy procesoru 6502. Přímé odkazy na stažení nástrojů vedou na 404, popřípadě se neobjeví, co by se objevit mělo, ale nakonec jsem po nějakých děsných experimentech dosáhl svého a stáhnul jsem dev tools. Ty jsou duchem v téže době. Nejmodernější je asi EasySXB, což by měl být takový jako že sériový terminál, který ale umožňuje snazší ovládání &#8211; třeba výpis registrů máte přehledně v panelu. Bohužel už při připojení to po vás chce zadat port, kde je SXB připojené, a nenabídne ani seznam COM portů, které jsou aktivní. Takže si to musíte někde najít a opsat.

Nakonec jsem použil TeraTerm, a ten šlapal bez problémů. Zkusil jsem si udělat jednoduchý program na blikání LEDkou, přeložil jsem ho svým ASM80, a ejhle- fungoval! Ta radost&#8230;

Jinak je to moc hezká destička a udělala mi radost. Až bude čas, udělám k ní nějaké příjemnější programovadlo, třeba jako Chrome plugin.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/02/20170208_172422.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-949" src="https://retrocip.cz/wp-content/uploads/sites/6/2017/02/20170208_172422-366x650.jpg" alt="" width="366" height="650" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2017/02/20170208_172422-366x650.jpg 366w, https://retrocip.cz/wp-content/uploads/sites/6/2017/02/20170208_172422-576x1024.jpg 576w, https://retrocip.cz/wp-content/uploads/sites/6/2017/02/20170208_172422.jpg 600w" sizes="(max-width: 366px) 100vw, 366px" /></a>