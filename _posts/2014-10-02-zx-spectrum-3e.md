---
id: 399
title: ZX Spectrum +3e
date: 2014-10-02T08:02:55+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=399
permalink: /zx-spectrum-3e/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3074846563"
categories:
  - Hardware
tags:
  - Sinclair
  - Spectrum
  - Z80
  - ZX
---
Necelý rok jsem byl spokojeným majitelem Spectra +2A. Připojil jsem k němu divIDE a bylo dobře. Jenže člověka občas popadne pejcha, že&#8230;

<!--more-->

Popadla i mne. Furt jsem mlsně koukal na úpravu, co se jmenuje [ZX +3e](https://www.worldofspectrum.org/zxplus3e/). Je to vlastně upravená ROM původního Spectra +2A/+3, ve které jsou opravené nějaké ty chybky a která je rozšířená o [nové příkazy](https://www.worldofspectrum.org/zxplus3e/commands.html), především pro práci s diskem (včetně _kanálových_ [operací se soubory](https://www.worldofspectrum.org/zxplus3e/channels.html)).

+3e dokáže pracovat s různými [IDE interfejsy](https://www.worldofspectrum.org/zxplus3e/interface.html). Obvyklý postup je ten, že kouknete, co máte (já mám to divIDE), adekvátně tomu si [vypálíte obsah ROMek](https://www.worldofspectrum.org/zxplus3e/p3eroms.html), vyměníte a frčíte. Já jsem líný a bohatý, já si za pár dolarů objednal hotové kombo &#8211; vypálené ROM a &#8222;simple 8bit IDE&#8220;, což je vlastně IDE připojené přímo na datovou sběrnici procesoru (plus kousek logiky ze 74LS10 a tranzistoru). Ve skutečnosti je to taková destička s objímkou, která se nasadí mezi procesor a desku.

Tak tohle mi po pár dnech přišlo a já se pořád odhodlával otevřít Spectrum. Sice jsem si přečetl, že výměna je jednoduchá, ale víte jak &#8211; zrovna to vaše bude jiný _issue_, něco bude jinde, jinak&#8230; Po pravdě jsem se nejvíc děsil toho, jak budu pájet tři čipy z desky ven. Sice jsem někde zahlédl, že jsou tyhle čipy v paticích, ale moc jsem tomu nevěřil.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/image002.jpg)  
![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/image001-650x365.jpg) 

Až jsem to Spectrum vzal do ruky, odpojil divIDE, kouknu dovnitř &#8211; a fakt že jo, tři patice tam jsou! Takže jsem vzal do ruky šroubovák, celé to otevřel, s uspokojením zjistil, že klasika všech Specter, totiž děsivé kšandy, zůstala zachovaná, a lup! už šly ven ROMky i procesor. Nastrkal jsem tam ty nové kousky, zapnul &#8211; a nic! Na obrazovce byly vodorovné černo-bílé pruhy.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/image003-365x650.jpg)![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/image004-650x365.jpg)![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/image005-650x365.jpg) 

Naštěstí jsem nepodlehl panice. Zdálo se mi, že ta deska sedí v procesorové patici poněkud pofidérně, takže jsem pomačkal, zatlačil, a na druhý pokus všechno fungovalo.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/image006-650x365.jpg) 

Takříkajíc: Jupí! Teď už jen připojit vhodný IDE disk. Velmi pravděpodobně se vykašlu na reálné disky a místo toho využiju redukci na CF, která mi tu leží. Krátkým plochým kabelem ji připojím, a pokud se mi zadaří, tak ji umístím někam na okraj Spectra tak, aby šla karta vytahovat. A když ne, budu mít holt interní disk.

No a divIDE půjde k tomu krásně zachovalému Plusku, co mělo být původně na experimenty, ale zželelo se mi ho&#8230;