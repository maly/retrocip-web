---
id: 966
title: Arduino + emulátor 8080 + TINYBASIC
date: 2017-08-20T13:35:51+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=966
permalink: /arduino-emulator-8080-tinybasic/
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic2-820x198.jpg
categories:
  - ASM80.com
  - Emulace
  - Hardware
tags:
  - arduino
  - BASIC
---
Jak se vám to rýmuje? Mně kupodivu docela dobře&#8230;

<!--more-->

Zaprvé: Vezmete Arduino Uno.

Zadruhé: Použijete kód emulátoru 8080, co je pod licencí GPLv2 dostupný v rámci [emulátoru Altairu 8800](https://github.com/companje/Altair8800). Stačí jen samotný emulátor.

Zatřetí: Napíšete jednoduchý HAL &#8211; Hardware Abstraction Layer. V něm definujete, jak se čte z paměti, zapisuje do paměti a jak se pracuje s periferiemi. Já si nadefinoval, že v prostoru 0x0000-0x0fff bude ROM (tedy PROGMEM), v prostoru 0x1000-0x13ff bude RAM (vlastně jen pole hodnot byte).

_RAM by šla zvětšit na 1.5kB, i když překladač píše varování. Zkuste si to, a nezapomeňte změnit příslušné hodnoty ve zdrojáku BASICu._

Za čtvrté: Otestujete si, jak to běhá, jednoduchým příkladem ve strojáku do ROM.

Za páté: Přidáte obsluhu portů. Port 1 budiž sériový port, port 0xFE bude ovládat LEDku na Arduinu. Jen tak, pro radost. Stgrojákem ověříte, že to šlape.

Za šesté: Popadne vás _pejcha_ a řeknete si: Však proč nepoužít [Tiny BASIC](https://en.wikipedia.org/wiki/Tiny_BASIC)? Je to jednoduché, stačí implementovat jen čtení ze sériové linky a zápis do ní. [Zdrojáky jsou&#8230;](https://www.autometer.de/unix4fun/z80pack/ftp/altair/)

Za sedmé: Celé si to přeložíte [vlastním online assemblerem](https://www.asm80.com/). Stačilo jen upravit syntax makra a na několika místech změnit apostrofy na uvozovky (_DB &#8218;,&#8216;_ na _DB &#8222;,&#8220;_).

Za osmé: převedete výsledný HEX na sérii čísel, kterou includnete do toho arduinského _.ino_ tam, kde má být ROMka.

Za deváté: Sebevědomě to celé přeložíte, spustíte &#8211; a ono to funguje!

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-967" src="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic-650x460.jpg" alt="" width="650" height="460" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic-650x460.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic-768x544.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic.jpg 819w" sizes="(max-width: 650px) 100vw, 650px" /></a>Přiznávám, že kdyby to nefungovalo, tak by mi to asi sebralo trochu elánu, ale ono to fungovalo.

Za desáté: Zkusíte si přeložit i TinyBASIC verze 2. Nefunguje na první dobrou, protože musíte ještě změnit komunikační porty, ale pokud to uděláte stejně jako ve verzi 1, tak se to zase rozeběhne.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic2.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-968" src="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic2-650x515.jpg" alt="" width="650" height="515" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic2-650x515.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic2-768x609.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2017/08/tinybasic2.jpg 820w" sizes="(max-width: 650px) 100vw, 650px" /></a>Nakonec z toho celého uděláte [balíček a publikujete na GitHubu](https://github.com/maly/arduino8080basic).

Dejte si taky!