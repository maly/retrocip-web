---
id: 1022
title: 'První konstrukce s první otázkou&#8230;'
date: 2018-03-29T01:12:55+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1022
permalink: /prvni-konstrukce-s-prvni-otazkou/
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2018/03/MOS_6502AD_4585_top-790x198.jpg
categories:
  - Hardware
---
Při psaní pokračování Hrade, voltů&#8230; jsem se dostal do bodu, kdy je načase udělat první konstrukci. A tak jsem si řekl, že bych návod zveřejnil jako takovou ochutnávku a teaser, ať víte, nač se těšit.

<!--more-->

První konstrukce bude úplně minimální počítač: procesor, paměť a obvod pro práci s periferiemi.

Nejdřív jsem si říkal, že je to přeci úplně jasné, půjde tam Z80, 8255 pro paralelní porty, 8251 pro komunikaci s terminálem, 32k ROM a 32k RAM. To máte pět čipů, k tomu glue logika, takže takových šest, sedm čipů celkem.

Jenže pak jem si říkal: Z80 tu znají všichni, co takhle nějakou exotiku? No, exotiku, jak se to vezme: 6502 měli Ataristi i Commodoristi, sice tu nebyla tak rozšířená jak Intel, ale co už&#8230;

Anebo když začínám _ab ovo_, tak co takhle zkusit 8080? No, to radši ne, to bych lidi vyděsil. Co třeba 8085? Jeden jediný podpůrný obvod&#8230; Výhoda by byla, že by si majitelé PMD připadali &#8222;jako doma&#8220;.

Nebo snad něco od Motoroly? 6800 už asi není moc perspektivní, ale 6809, to je sázka na jistotu! Nejlepší osmibitový procesor (zcela vážně) té doby&#8230;

Tak jsem nechal hlasovat. Na FB to jde bohužel jen se dvěma variantami, tam vítězí Z80 nad 6502. [Na Twitteru se vede lítý boj](https://twitter.com/adent/status/979029512603230208) mezi Z80 a 8085 a zatím vítězí Intel, ale je to velmi těsné. Motorola je někde v půlce pole a 6502 kulhá až úplně vzadu.

Tak jo, přátelé, já vám to prozradím: Konstrukce se Z80 je docela fádní. Plánuju použít Z80 do jedné moc hezké konstrukce, kde s minimem součástek vytvoříte maximum efektu, včetně běhu CP/M, ale o tom potom.

Líbila by se mi konstrukce s 6502 nebo 8085. To jsou procesory o půl generaci starší a jednodušší než zbylé dva. 6502 by bylo fajn místním čtenářům připomenout, protože je to trošku jiný svět než Intel, a platí, že kolik architektur procesorů znáš, tolikrát jsi systémovým programátorem! Ta 8085 by zase byla naprostá česká klasika, a navíc by to byla tuším už druhá konstrukce s tímhle procesorem po BOB85&#8230;