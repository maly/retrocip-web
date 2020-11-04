---
id: 61
title: Emulační nálož
date: 2013-12-08T18:00:46+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=61
permalink: /emulacni-naloz/
dsq_thread_id:
  - "2035685659"
categories:
  - Emulace
---
Je neděle, tak si dejme nějaké oddychovější téma. Třeba: jak to vlastně vypadá s JavaScriptovými emulátory?

<!--more-->

Asi nejpodrobnější informace má nepodepsaný blog s titulem [Emulator 101](http://www.emulator101.com.s3-website-us-east-1.amazonaws.com/). Narazil jsem na něj naprostou náhodou, a ačkoli informace, které přináší, jsem už většinou znal, tak ho mohu zájemcům doporučit &#8211; je hezky přehledný a popisný.

Ze světa Apple jsou nejzajímavější kousky, které napsal [Will Scullin](http://www.scullinsteel.com/apple//about.html). Emuluje Apple 1, Apple ][ i [Apple//jse](http://www.scullinsteel.com/apple//e).

Stejný procesor, jiný výrobce: [KIM-1](http://www.robsayers.com/jskim1/)

A ještě jednou 6502: [Acorn Electron](http://elkjs.azurewebsites.net/).

A co ve světě Z80? Tak určitě musím začít ZX Spectrem. Nejlepší JS emulátor téhle legendy je [QAOP](http://torinak.com/qaop). ZX80 i ZX81 mají svou JavaScriptovou podobu z Ruska: [ZX8x](http://nocanvas.zame-dev.org/0004/). Zajímavé na téhle emulaci je řešení displeje. Není to canvas, je to pole DIVů, které mají na pozadí GIF se znakovou sadou a pozicováním se vybírá ten pravý.

Pokrevním bratrancem ZX81 byl stroj s názvem Jupiter Ace. Trošku exot, protože po spuštění na vás vyběhl nikoli obvyklý BASIC, ale FORTH. Jak to vypadalo? Koukněte se sami na [Jupiler](http://jupiler.retrolandia.net/JAs).

Od stejného autora si můžete vyzkoušet i [Roland](http://roland.retrolandia.net/), což je emulátor strojů od Amstradu (CPC464,664, 6128).

Druhá skupina domácích počítačů používala procesor 6502 nebo odvozený. Třeba [Commodore 64](http://www.kingsquare.nl/jsc64). Pro Atari 800 jsem, kupodivu, emulátor v JavaScriptu nenašel. Ale nevadí, nebuďte zklamaní &#8211; vždycky můžete zkusit emulátor sovětského domácího počítače [Radio-86RK](http://rk86.ru/) (který neměl nic spoečného ani s radiem, ani s procesorem x86).

Svět &#8222;jako že opravdových počítačů, ale dneska tak akorát na retrohraní&#8220; zastupuje emulátor [obecného stroje s OS CP/M](http://www.tramm.li/i8080/). Pokud vás doba &#8222;cépéemky&#8220; (jak tomu říkali tehdy inženýři &#8222;od fochu&#8220;), nebo též &#8222;sípíemky&#8220; (jak tomu říkali nadšenci) minula, nebo naopak okouzlila, tak si můžete dosytosti vyhrát.

Když postoupíme dál, k šestnáctibitům, tak mi z paměti vyskočí čtyři stroje. První PC, pak Amiga, Atari ST a Macintosh. Máme to v JavaScriptu? Máme.

[Macintosh prosím zde](http://jamesfriend.com.au/pce-js/), i s OS 7.

Jestli chcete staré PC, vyberte si: [s CP/M-86](http://jsmachines.net/disks/pc/cpm/), [s Windows 1.01](http://jsmachines.net/configs/pc/machines/5160/cga/256kb/win101/), nebo třeba [XTčko s 10MB harddiskem](http://jsmachines.net/configs/pc/machines/5160/cga/256kb/demo/)? Anebo novější, [na kterém běží Linux nebo FreeBSD](http://copy.sh/v24/)?

Zmínil jsem Atari ST, to by bylo třeba zde: [EstyJS](http://estyjs.azurewebsites.net/) (neplést s Este). No a Amigu emuluje [SAE](http://www.scriptedamigaemulator.net/).

Některé z těchto strojů se objeví i v ASM80.com. A z českých, které chystám? Je tam připravená trojice SAPI1, ONDRA a IQ151 a mám sto chutí nechat vás hlasovat, protože sám nevím, do kterého se pustit dřív.