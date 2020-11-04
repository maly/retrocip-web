---
id: 119
title: Pár novinek u ASM80
date: 2014-01-24T13:34:52+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=119
permalink: /par-novinek-u-asm80/
dsq_thread_id:
  - "2168235673"
xyz_lnap:
  - "1"
categories:
  - ASM80.com
  - Emulace
tags:
  - ASM80
---
Ne, neusnul jsem, ani jsem se nevrhl na skutečný hardware&#8230; Celý ASM80 pořád vylepšuju. Co vás čeká s novou aktualizací?

<!--more-->

Něco jsem naznačoval v [rozhovoru na Zdrojáku](http://www.zdrojak.cz/clanky/martin-maly-osmibity-nemizi/). Mezitím jsem ale získal některé archivní kousky, takže na vývoj nebyl tak úplně čas, ale přesto pár novinek mám.

Včera jsem například dodělal disassembler pro 6502. Hledal jsem všude možně použitelnou tabulku instrukcí podle operačních kódů, ale buď byly neúplné, nebo byly ve formátu, který znemožňuje nějaké rozumné strojové použití a musel bych to celé opisovat. Nakonec jsem napsal jednoduchý skript, který vzal tabulku z mého assembleru a udělal ji inverzní. Vedlejším produktem je [tabulka instrukcí 6502](http://strojak.cz/instrukce-6502-v-jedne-tabulce/), včetně těch nedokumentovaných (dám ji ke stažení jako JSON nebo CSV).

Upravil jsem pár bugů v emulátorech, třeba při zapnutí debuggeru se &#8222;odemkne&#8220; klávesnice.

Napsal jsem emulátor sériového komunikačního obvodu 6850, který použil [Grant Searle ve svých konstrukcích](http://searle.hostei.com/grant/). Mám i připravený emulátor znakového terminálu, tak jsem to rovnou propojil &#8211; a fungovalo to. Grant na svých stránkách upozorňuje, že si nepřeje šíření bez souhlasu, takže jsem ho požádal o svolení a udělil mi ho. Emulátor je tedy připraven&#8230;

A když už jsem byl v těch žádostech, tak jsem si uvědomil, že Grant používá jako základní SW pro své konstrukce nějaký prehistorický MS BASIC z let 1977/1978. Pamětliv legendárního [dopisu Billa Gatese](http://upload.wikimedia.org/wikipedia/commons/1/14/Bill_Gates_Letter_to_Hobbyists.jpg) jsem požádal tedy o svolení k šíření i přímo autora tohoto BASICu, jsem zvědav, co Gates odpoví&#8230;

Emulátor JPR je už v provozu, ten jsem tu [už propagoval](http://retrocip.uelectronics.info/hrajeme-si-s-emulatorem-jpr-1/ "Hrajeme si s emulátorem JPR-1").

Další nápad, který chci realizovat, je optimalizující překladač. Minimálně pro 6502 se bude velmi hodit, tam se mi totiž stále nelíbí nutnost explicitně určit, že chci pracovat se zero page.

K tomu dělám některé úpravy, které spíš popíšu na [Webscriptu](http://webscript.cz) &#8211; například přepisuju moduly tak, aby fungovaly i jako AMD a CommonJS a zkouším jednotlivé části upravit do podoby standalone aplikací.

Zatím největší problém je dokumentace. Vím, že je potřeba, a že se jejímu sepsání nevyhnu, spíš mě děsí, že ji budu muset překládat i do angličtiny. Což mě přivádí k otázce: nenašel by se někdo, kdo by mi s tím pomohl? Díky.

Na novinky se těšte už tento víkend, kdy budu nahazovat novou verzi na web.