---
id: 447
title: FORTH na kometě
date: 2014-11-15T10:56:20+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/445-autosave-v1/
permalink: /445-autosave-v1/
---
Dívat se do vesmíru, to je jako hledět přímo do minulosti.

Ono to platí doslova &#8211; světlo hvězd, které vidíme dnes, je desítky, stovky, tisíce světelných let staré, světlo těch nejvzdálenějších k nám letělo miliardy let, takže nevidíme to, jak ty hvězdy vypadají &#8222;právě teď&#8220;, ale jak vypadaly tehdy. (Vzhledem k absolutní rychlosti světla je celá koncepce &#8222;současnosti&#8220; o něco zamotanější, ale do toho nechci zabředávat, já to mám jen jako přirovnání na úvod.)

Totéž se dá říct o kosmické technologii, kterou vypouštíme do vesmíru: když se na ni díváme, vidíme stav techniky před zhruba čtvrtstoletím. Podívejme se na Rosettu a její kometární misi &#8211; jistě jste zachytili.

Rosetta byla vypuštěna v roce 2004, v roce 2011 proletěla okolo Jupitera, program ji přepnul do &#8222;sleep mode&#8220; kvůli šetření energií a letos v lednu se zase probudila a ohlásila z blízkosti komety. Centrální procesor Rosetty je [Dynex MAS31750](http://en.wikichip.org/wiki/Dynex_MAS31750) na 25 MHz &#8211; šestnáctibitový procesor se zvýšenou odolností proti radiaci, který implementuje standard [MIL-STD-1750A](http://en.wikipedia.org/wiki/MIL-STD-1750A). Byl použit v několika různých sondách.

Procesor není žádný nováček, ale má to jednoduché vysvětlení: Rosetta byla vypuštěna před deseti lety, v roce 2004. Deset let ji sestavovali, tedy od roku 1994. Návrh pochází zhruba z roku 1990, 1991&#8230; No a v té době už museli vybrat vyzkoušený a ověřený procesor, který bude schopen něco takového ustát. Představte si sami sebe tehdy &#8211; dali byste tam x86? Ne, je tam procesor standardu 1750A, který v té době splňoval výše uvedené požadavky. Jeho architektura je navíc otevřená, takže ji může implementovat libovolný výrobce a máte tedy k dispozici dostatečný počet &#8222;druhých zdrojů&#8220;.

Pro zajímavost &#8211; v současnosti se používají čipy jako je [RAD750](http://en.wikipedia.org/wiki/RAD750) a [RAD6000](http://en.wikipedia.org/wiki/IBM_RAD6000), založené na architektuře PowerPC. Nezapomínejme na to, že návrh takové kosmické sondy zabere mnoho let, takže logicky neobsahují při startu ty nejnovější technologie, nehledě na to, že musí fungovat mnoho dalších let v podmínkách vysoké radiace, ve vakuu a v teplotách blízkých absolutní nule, což diskvalifikuje právě ty nejnovější procesory, které nejsou dostatečně vyzkoušené. Zjednodušeně řečeno: &#8222;Na Word dobrý, ale sondu za miliony dolarů tomu nesvěřím!&#8220; ESA v současnosti navrhuje nový procesor pro kosmické sondy, založený na architektuře SPARC-V8 a pojmenovaný [LEON](http://en.wikipedia.org/wiki/LEON).

Procesory x86 se ale přesto v některých kosmických zařízeních používají. Především tam, kde je hodně energie a kde se dají v případě potřeby vyměnit. Tedy například na ISS nebo v Hubbleově teleskopu.

A teď k té kometě. Rosetta nesla modul Philae, který se před pár dny od mateřské sondy oddělil a přistál na kometě. Nebylo to úplně ideální, ale to nechme stranou a stesky nad tím, jak se &#8222;všechno pokazilo&#8220;, nechť si [píšou novináři v Technetu](http://technet.idnes.cz/philae-na-komete-potrebuje-energii-d59-/tec_vesmir.aspx?c=A141114_113028_tec_vesmir_vse). Philae má samozřejmě taky vlastní procesor, lépe řečeno dva. Jsou to šestnáctibitové zásobníkové procesory typu [Harris RTX2010](http://en.wikipedia.org/wiki/RTX2010), taktované na 8 MHz ([datasheet RTX2010 zde](http://www.intersil.com/content/dam/Intersil/documents/hs-r/hs-rtx2010rh.pdf)).

Architektura RTX2000 je z roku 1988 a je to vylepšený procesor Novix N4000, což je dvouzásobníkový procesor, určený pro běh jazyka FORTH, navržený autorem Forthu Chuckem Moorem (a nebyl jediný, viz článek [From FORTH to Stack Processors and Beyond](http://www.cpushack.com/2013/02/21/charles-moore-forth-stack-processors/)). Jeho architektura je rovněž otevřená, a kdybyste si chtěli takový procesor implementovat ve svém FPGA, tak [můžete](http://www.mpeforth.com/rtx.htm).

Procesory jsou v sondě dva ne proto, aby zvýšily výkon, ale jsou zapojené ve dvou redundantních systémech, z nichž jeden funguje jako primární a druhý jako kontrolní, připravený v případě nějakých problémů převzít řízení. Samozřejmě, že u takové techniky nelze rozsvítit červenou LED a čekat, až to někdo zrestartuje, ale všechny chyby musí být ošetřeny tak, aby se sonda dokázala za všech podmínek znovu probrat a uvést do rozumného stavu.

Redundance znamená dvojnásobnou zátěž pro napájení, a i proto byl vybrán takový procesor, který má velmi nízký odběr a přitom dostatečný výkon. V modulu jsou navíc i různé vědecké přístroje, které ([podle CPUshack](http://www.cpushack.com/2014/11/12/here-comes-philae-powered-by-an-rtx2010/)) rovněž používají RTX2010 (takže jich je na palubě celkem 10). Kromě nich modul obsahuje i ADSP-21020, pár jednočipů 80C3x a několik FPGA.