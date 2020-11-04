---
id: 761
title: '&#8222;Ještě že tam byla Z80!&#8220;'
date: 2016-03-06T15:48:37+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=761
permalink: /jeste-ze-tam-byla-z80/
xyz_lnap:
  - "1"
image: http://retrocip.cz/wp-content/uploads/sites/6/2016/03/Z806502-588x198.jpg
categories:
  - Hardware
tags:
  - "6502"
  - Atari
  - hate
  - rant
  - Spectrum
  - Z80
---
Ukaž, že máš to správný zilogovský srdíčko! 6502 jsou buzeranti, vole, ZILÓÓÓG! &#8211; MOS, MOS, MOSMOSMOS, olééé oléééé&#8230;

<!--more-->

Jsem tuhle získal lehce kuriozní kousek, totiž [Oric Atmos](http://www.old-computers.com/museum/computer.asp?c=79). Je to takový stroj ze začátku 80. let, a když se na něj díváte, připadá vám, že se díváte na Spectrum, jen trochu jiné. Posuďte sami: Uvnitř jen pár čipů &#8211; procesor, ULA, paměť (16 nebo 48 kB RAM), UHF modulátor, generátor AY-3-8912&#8230; K tomu zadrátovaný BASIC a moc hezký manuál, kde je všechno, od zapojení přes programování v BASICu až po základy assembleru 6502 a zapojení konektorů.

Hopla. Jojo, 6502. Ale jinak jako vejce vejci. Takový od pohledu milý stroj, jen s lepší klávesnicí. Jo, kdyby Plusko mělo 6502, tak vypadalo třeba nějak takhle. Což je taková myšlenka, kterou jsem napsal veřejně, a zase mě překvapilo (jo, jsem nepoučitelnej), že něco takového dokáže vyvolat silný protitlak. Na Twitteru ještě dobrý, ale v soukromých zprávách jsem se dozvěděl prapodivné věci. Kdybych to shrnul: _6502 je shit, Z80 je boží, zde mám 14 argumentů, proč tomu tak je, a jestli tomu nerozumíš, tak by sis o tom měl něco přečíst a přestat plivat na Z80!_

Jako jo, argumenty jsou vždycky dobré, ale když se dozvím, že 6502 je horší, protože pracuje jen do 2MHz, zato Z80 dá až 4MHz, tak se musím smát. Měl by sis o tom něco přečíst, lol! A Z80 má víc registrů a ty umí pracovat i v párech, takže jsi úplně mimo, Q.E.D. Mno, tak to dopadá vždycky, když se v mládí naučíš pracovat s jedním procesorem, je ti patnáct a bereš otázky &#8222;Z80, nebo 6502?&#8220; smrtelně vážně, asi jako &#8222;Sparta vs Baník&#8220;. Soupeřovi se posmíváš, ale víš o něm kulový, a tak máš hned na všechno rychlou, jednoduchou a nesprávnou odpověď: &#8222;Z80 má 7 osmibitových registrů a dva 16bitové indexregistry, 6502 má jeden akumulátor a dva indexregistry, osmibitové, takže, cha chaaa, 88 bitů versus 24 bitů, to snad musí vidět i slepej!&#8220; Mnojo, ale že 6502 má vlastně 256 registrů plus akumulátor, protože práce se ZP je hodně rychlá&#8230;? Že u 6502 zabere jedna instrukce 2-4 takty (až 6 u nepřímo indexovaných operandů) a u Z80 začínáme na čtyřech taktech, to neví! Vidí jen 4MHz a 2MHz&#8230; Ale cítí se povolaný srovnávat procesory.

Z druhé strany to není o moc lepší. Poslouchat řeči o tom, jak je Spectrum nanic, protože Atari mělo tři procesory navíc (verš sponzorován kampaní na Kickstarteru) nebo sáhodlouhé rozbory kvality her, co se odrážejí od tvrzení, že Atari mělo &#8222;lepší grafiku&#8220;, to je taky ztráta času jako fík, a když takový nadšený Atarista zabrousí na téma procesoru a tvrdí, že 6502 byl vlastně první RISC a že měl ortogonální instrukční sadu, tak nezbývá, než ho usadit dobře mířeným STACK OVERFLOW.

Zkrátka si říkám, že v roce 2016 už je načase opustit mentální obzor patnáctiletého uhrovatého nerda. Jasně, můžu být fanoušek procesoru Z80, a taky jsem, ale přesto mohu být fascinován podivností [CDP1802](http://retrocip.uelectronics.info/cosmac-rca-1802/) nebo elegancí [6809](http://retrocip.uelectronics.info/posledni-krasavec-osmibitove-ery/)/[6309](http://retrocip.uelectronics.info/6309-vse-je-neoficialni/). A když jsme u těch počítačů, tak si přiznat, že i ty, co jsme neměli, mají svoje kouzlo&#8230; (Mimochodem, moje idea ZX Spectra s procesorem 6809 je díky FPGA realizovatelná, už jsem si zkusil PoC, a až bude čas&#8230;)

A protože opakování je matka modrosti (_mother of the blueness_), tak ocituju [sám sebe](http://kcc.misantrop.info/2015/07/05/fanboy/):

> Psát „spectristickej humáč, jedině Atari!“ bylo pochopitelné, když byl rok 1988 a vám bylo patnáct. Nebo obojí dohromady.V roce 1995 už to bylo těžce demodé, v roce 2005 se takovéhle projevy braly vážně snad jen v IT amišovských komunitách. V roce 2015 už ani jako retro…
> 
> Fascinují mě lidé, kteří mají v roce 2015 zapotřebí mi sdělovat svůj názor na osmibitové procesory tímhle stylem. _Pardon, ne fascinují, to druhé slovo… Jo: Prudí! _Přátelé, v roce 2015 jsou osmibity součást historie a je ideální prostor k tomu je studovat, dívat se, jak byl který navržen a přemýšlet, proč zrovna takto. Je to úžasná kapitola vývoje techniky a skvělá příležitost se na všechno podívat s _odstupem šestapadesáti bitů_.
> 
> Anebo příležitost ukázat ortodoxnost své víry, pokud vám je stále patnáct.

Ať nám naše staré křemíky ještě dlouho slouží, přátelé!