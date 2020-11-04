---
id: 153
title: Úplně alternativní Spectrum
date: 2014-04-06T14:25:43+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=153
permalink: /uplne-alternativni-spectrum/
dsq_thread_id:
  - "2590785574"
image: http://retrocip.cz/wp-content/uploads/sites/6/2014/04/ZXSpectrum48k-1140x198.jpg
categories:
  - Hardware
tags:
  - Sinclair
  - Spectrum
  - Z80
  - ZX
---
Čímž tedy rozhodně nemyslím, že je alternativní jako třeba &#8222;alternativní medicína&#8220;, ale prostě že by bylo jiné. A počítám s tím, že mi budou skalní fandové lát a klnout, že znesvěcuju ducha, ale [to jsme si už vyřídili](http://retrocip.uelectronics.info/retro-duch/ "Retro duch"), teď se, prosím, soustředíme na to podstatné.

<a href="http://retrocip.uelectronics.info/uplne-alternativni-spectrum/zxspectrum128/" rel="attachment wp-att-155"><img loading="lazy" class="aligncenter size-full wp-image-155" alt="" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/04/zx+spectrum+128.png" width="564" height="422" /></a>

<!--more-->Když Jirka Lamač (LEC) a pánové Troller s Císařem (Sinsoft) rozšiřovali paměť ZX Spectra proto, aby na něm mohli pouštět CP/M, bylo to znesvěcení retro ducha modernou, ale protože to probíhalo na konci 80. let, tak to vlastně ve skutečnosti byl pokrok. I když upřímně: CP/M v té době byl už docela letitý systém&#8230;

Ale zpět k alternativě: Chtěl jsem tím říct, že úpravy Specter nemusí být jen, řekněme, pietní. Takových je naprostá většina: vylepšená ROMka, přidaný nějaký nový hardware, který se, pokud možno, tváří jako starý hardware a je totálně transparentní&#8230; Hlavní je kompatibilita s originálními Spectry.

Na druhé straně CP/M úprava kompatibilitu na přepnutí přepínače zahazuje a implementuje systém, který je se Spectrem už z principu nekompatibilní. Spectrum má totiž, jak si pamatujete, na začátku paměti (&#8222;dole&#8220;) ROM, nad ní videoRAM, a nad ní RAM až do konce prostoru. CP/M naopak vyžaduje, aby &#8222;dole&#8220; byla RAM a ROM s BIOSem naopak kdesi &#8222;nahoře&#8220;. Plus navíc do toho všeho padá videoRAM&#8230; Ale jo, na konci 80. let to vypadalo jako dobrý nápad, lepší než platit statisíce nekonvertibilních Kčs za systém s CP/M.

Zase jsem od alternativy odbočil. Ale nebojte, už jsem zpátky: Nechal jsem svou fantazii tuhle plynout nad otázkou: **šel by multitaskový OS udělat i na Z80?**

Tak určitě&#8230; Samozřejmě existovaly. Ale bylo by fajn si ho napsat, že? Co k tomu bude potřeba? Zaprvé &#8211; MMU. Je třeba, aby programy fungovaly kdekoli v paměti na (logicky) jednotné adrese bez ohledu na fyzické umístění. Viz .COM programy, které vždy běžely od adresy $0100, a v MS DOSu to fungovalo díky segmentovým registrům x86 (které jsou jinak peklo) naprosto transparentně. Na druhou stranu, kdyby ten systém dokázal namapovat různé banky paměti na určitou adresu, tak by to spoustu věcí usnadnilo.

Zadruhé &#8211; supervizor režim. Jinak nebude možné ochránit paměť, izolovat procesy, přidělovat zdroje&#8230; No a nebo důsledně dodržovat princip &#8222;play nice&#8220;, tedy spoléhat na to, že programátor bude dodržovat pravidla, bude třeba přistupovat k displeji a klávesnici výhradně přes služby OS, bude poslouchat dispečera, nezakáže přerušení, &#8230;

No a zatřetí by šlo udělat virtuální procesor, takovou &#8222;Javu 80&#8220; (neplést s Jawou 50).

O několik myšlenkových pochodů později mi to secvaklo: Vždyť já nemusím dělat experimentální hardware a bastlit vlastní počítač se Z80 ([hello, Grant](http://searle.hostei.com/grant/cpm/index.html)), já přeci můžu použít Spectrum, rozšířit mu paměť pro stránkování od $8000, využít všechno, co zařídí hardware a ULA (tj. klávesnici, displej a další) a tenhle systém napálit do ROMky, která bude od $0000, jak to Sir Clive mínil.

A o jednu úvahu dál jsem viděl ZX Spectrum 128, které má to přepínání paměti už jaksi samo od sebe, sice od $C000, ale nešť. K tomu by stačila malá krabička s ROMkou a s nějakým udělátorem, co by dokázal komunikovat s CF nebo SD.

Na chvíli ve mně hrklo: Ale to v té ROMce nebude originální BASIC a přijdu o kompatibilitu a to rozšíření nebude ovládané nějakými ohackovanými příkazy! No jo, to nebude. No a?

Nebudou pro to programy. Jasně, takže je můžu napsat sám! Budu jediný majitel. Bude to jen technická onanie, _kdybys radši dělal něco užitečného a pomohl spektrácké komunitě_&#8230; Nojo. Nepomůžu, jakáž pakáž&#8230;

Jen bych vás, znalejší, poprosil: **Nevíte o nějaké takové úpravě**, která by využívala HW Spectra a přitom zahodila ROM a BASIC a implementovala vlastní systém? Všechny ROMky, co jsem našel, jsou především &#8222;vylepšené a opravené originální ROM&#8220;, tu s tokenizérem, támhle s novými funkcemi, ale taková divočina, jakou jsem si vysnil, tu jsem nikde neviděl.

Aktualizace 7.4.: Díky Logoutovi za [odkaz na Matrix OS](http://clanky.1-2-8.net/2009/09/z-archivu-mb-maniax-matrix-os.html)!

PS: Omlouvám se, zapomněl jsem, že některé _memy_ nejsou všeobecně známé. Ta citace &#8222;Kdybys radši dělal něco užitečného&#8230;&#8220; mě pronásleduje od dob Bloguje vlastně u každého projektu: &#8222;_Kdyby pan Malý svou energii raději věnoval do XXX a pomohl tím komunitě!_&#8220; Nojo, nepomohl jsem, ale bylo Bloguje a byly další věci. Takže pro jednoznačnost: to nebylo nic proti Spectristům&#8230; Ostatně viz [zde](http://www.misantrop.info/retrotyden/#comment-1139363855) a [zde](http://www.misantrop.info/mam-rad-proroctvi).