---
id: 980
title: 'Jednodesková výzva: první kolo'
date: 2017-09-10T11:03:07+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=980
permalink: /jednodeskova-vyzva-prvni-kolo/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "6132744985"
image: https://retrocip.cz/wp-content/uploads/sites/6/2017/09/800px-MOS_KIM-1_IMG_4211_cropped_scale-800x198.jpg
categories:
  - Emulace
  - Hardware
tags:
  - "6502"
  - "8080"
  - jednodeskáč
  - Jednodesková výzva
  - Z80
---
Tak, moji milí, doufám, že jste nažhavení do programování, že jste si oprášili [stroják](https://strojak.cz) a že jste načerpali [ducha jednodeskových počítačů z 80. let](https://retrocip.cz/jednodeskova-vyzva-predehra/). Pojďme na to.

<!--more-->

Nejprve pár věcí k samotné výzvě: rozhodl jsem se, že bude mít několik kol, a že bude mít dvě hlavní kategorie. První kategorií je Z80/8080, druhá kategorie je 6502. V každém kole zadám úkol, hodně volně, abych nechal prostor vaší tvůrčí energii a neotřelým myšlenkám, a každý, kdo úkol splní, dostane za dané kolo bod. Pokud udělá věci &#8222;nad plán&#8220;, dostane další body. Dva z vás (z každé kategorie jeden), co projdou celou výzvou a získají nejvíc bodů, na konci ode mne dostanou přesně ten jednodeskáč, co budu v průběhu celé výzvy stavět&#8230;

Podmínky jsou prosté. Píše se v assembleru pro ten který procesor, a soutěžní příspěvky musí být zveřejněny jako assemblerovský zdroják (tedy žádný HEX) v nějaké otevřené podobě. Ideálně na GitHubu jako repo nebo jako Gist. Do komentářů sem napište odkaz, a do zdrojáku pak nějaký kontakt na sebe. Nezapomeňte uvést licenci (doporučuju BSD, MIT nebo CC) a nezapomeňte na nějaký návod k obsluze.

Zadání pro první kolo je zde:

Máte holý počítač. To znamená jen procesor, paměti a jeden sériový port. Máte 1 kB RAM, 4 kB ROM a sériový obvod 6850 (nebojte, je velmi jednoduchý). Počítač nepoužívá přerušení. Napište pro takový počítač jednoduchý obslužný program (monitor), který bude umožňovat základní činnosti, tedy měnit obsah paměti a spouštět programy. Pokud zvládnete i víc funkcí, třeba debugování, breakpoint apod., budou body navíc.

Inspirace: třeba sériové rozhraní KIM-1&#8230;

Stroje:

Z80: 4kB ROM (0000h-0fffh), 1 kB RAM (8000h-83ffh), 6850 zapojena na IO portech 00h (řízení/status) a 01h (data).

6502: 4kB ROM (f000h-ffffh), 1kB RAM (0000h-03ffh), 6850 zapojena na adresách a000h (řízení / status) a a001h (data).

Pro jednoduchost prosím inicializujte 6850 pomocí zápisu hodnoty 15h do řídicího registru. Bližší info najdete třeba [na Strojáku](https://strojak.cz/6502-ahoj-svete/). Chybové stavy 6850 můžete ignorovat.

Vše je jasné? Tedy vzhůru do toho! Držím palce!

_Příště: sedmisegmentovky a klávesnice._

PS: Můžete psát a ladit v online IDE [ASM80.com](https://www.asm80.com/) &#8211; [zde je návod, jak si nastavit virtuální stroj&#8230;](https://www.uelectronics.info/2017/09/03/generic-emulators-for-asm80-com/)