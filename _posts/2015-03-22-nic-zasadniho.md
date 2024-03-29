---
id: 533
title: 'Nic zásadního&#8230;'
date: 2015-03-22T11:28:40+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=533
permalink: /nic-zasadniho/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3616596456"
categories:
  - Emulace
  - Jen tak
  - Software
---
Tenhleten příspěvek si píšu především sám pro sebe, abych věděl a nezapomněl, co že jsem to chtěl naprogramovat&#8230;

<!--more-->

Rozepsal jsem editor spritů, co na požádání vygeneruje DB (jako že sekvenci direktiv DB, ne databázi&#8230;) pro váš assembler. Je, jak jinak, integrovaný do ASM80, a je trošičku nabuzený: má masky, umí dělat sprity vícebarevné a až do velikosti 64&#215;64, snadno se s ním dělají animované sekvence, je tam připravená podpora pro vrstvy, no a tak dál&#8230; A samozřejmě generovat v různém pořadí bajtů a s různým prokladem &#8222;bitmapa/maska&#8220;.

Pak jsem práce na chvíli přerušil a udělal [Gamesfer](https://gamesfer.com) aka Burzu herních kódů. Ta je teď hotová, takže bych se rád vrátil k tomu rozdělanému a pokračoval. Takže bod 1: Dodělat editor spritů.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/03/screenshot-localhost-2015-03-22-11-26-45-650x386.png) 

Na bod 2 mě navedl Busy: Do assembleru přidat opakování prvního průběhu, dokud nebudou pořešena všechna návěští. Úprava bude poměrně jednoduchá. _(Aktualizace 4.4. &#8211; je hotovo!)_

Na bod 3 mě navedli z Commodore Free. Totiž v [novém čísle](https://www.commodorefree.com/magazine/vol9/issue86.html) napsali o ASM80, a tak jsem si řekl, že by bylo fajn tam konečně dodělat generování .PRG včetně toho BASICového zavaděče. _(Aktualizace 4.4. &#8211; je hotovo, a mám i [videotutoriál](https://www.youtube.com/watch?v=LwnarnR5Z-4))_

Bod 4 je takový mo(nu)mentální nápad: Udělat spectristický herní box (softwarový), což by byl v podstatě jen emulátor (dodělal bych jen podporu pro gamepad v roli joysticku) s jednou funkcí navíc: Najít a stáhnout hru! Zadám název, engine ji najde v archivech, stáhne, rozbalí, uloží lokálně a spustí. Tradá. Bezúdržbový black box. Principiálně [Herní klasika](https://herni-klasika.cz/starquake/) v chromeless prohlížeči (a s tím stahováním).

Bod 5 jsem zjevně zapomněl, zatímco jsem sepisoval bod 4. Jo, už vím: CP/M včetně standalone konzolové verze, integrované do systému a la Cygwin. Stovky děkovných dopisů&#8230;

Bod 6 je &#8222;dopsat [strojak.cz](https://strojak.cz) a doplnit tam Z80, možná i 680x&#8220;

Jo a jestli pojedete na Barcamp do Plzně příští týden, tak se těším [ve 13:00 naviděnou](https://plzenskybarcamp.cz/2015/arduino-day).