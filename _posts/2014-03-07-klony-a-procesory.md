---
id: 128
title: Klony a procesory
date: 2014-03-07T16:11:24+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=128
permalink: /klony-a-procesory/
dsq_thread_id:
  - "2381067355"
image: https://retrocip.cz/wp-content/uploads/sites/6/2014/03/Zilog_Z80-588x198.jpg
categories:
  - Hardware
tags:
  - "6502"
  - "8080"
  - Intel
  - Motorola
  - Z80
  - Zilog
---
V šerém dávnověku mikroprocesorů byl i ten konkurenční boj takový jiný&#8230;

<!--more-->

V [kurzu assembleru](https://strojak.cz) jsem se dostal do bodu, kde začnu popisovat Z80. Nejjednodušší by bylo hezky po programátorsku odkázat na díly o procesoru 8080 a říct: &#8222;Přesně takhle, jen jiná syntaxe, a tohle je navíc!&#8220; Asi to nějak zmíním, ale instrukce přeci jen proberu trošku jinak.

Každopádně mě přemýšlení nad tím, jak popsat vznik Z80, přivedlo k zajímavé věci z historie procesorů a integrovaných obvodů, která dneska už může znít docela neobvykle.

[<img loading="lazy" class="aligncenter size-full wp-image-129" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/03/Zilog_Z80.jpg" alt="Zilog_Z80" width="588" height="324" />](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/03/Zilog_Z80.jpg)

První mikroprocesor, a to je už notoricky známé, byl Intel 4004. Intel ho vyráběl ve třech verzích &#8211; tedy navenek. Uvnitř to byl stejný čip, lišilo se jen pouzdro &#8211; plastické, keramické, nebo plasto-keramické.

Kromě Intelu ale vyráběl stejný procesor i National Semiconductor (NatSemi) pod označením INS4004. Stal se tak _druhým zdrojem_ (second source).

&#8222;Druhý zdroj&#8220; byl u integrovaných obvodů poměrně běžný koncept. Například známou TTL řadu 74xx představil výrobce Texas Instruments (obvody nesly označení SN74xx). Jenže kromě TI začaly vyrábět funkčně ekvivalentní obvody i další výrobci &#8211; namátkou Fairchild, zmíněný NatSemi, Philips, za železnou oponou to byla například Tesla (MH74xx), polská Unitra (UCY74xx), vyráběly se i v Maďarsku (Tungsram), Rumunsku i ve východním Německu (VEB Kombinat Mikroelektronik Erfurt &#8211; KME, např. německý D174 byl ekvivalent SN7474). Většinou se výrobci snažili, aby stejně označený obvod měl stejné parametry jako vzor, popřípadě &#8222;o něco lepší&#8220;, například aby byl rychlejší, měl nižší spotřebu, větší teplotní toleranci atd.

Stejný princip tehdy fungoval i u procesorů. První osmibitový mikroprocesor 8008 tak kromě Intelu vyráběl i Siemens (SAB8008), Microsystems International (MF8008) a východoněmecký KME (U808D).

Ekvivalentní procesory vznikaly buď oficiálně &#8211; licencováním, nebo trošku černějším způsobem. Nepředpokládám, že Intel prodal licenci na procesor 8080 Tesle. Podle toho, co vím, vznikaly &#8222;východní&#8220; mikroprocesory pomocí zpětného inženýrství pokoutně dovezených čipů, studia datasheetů a trošky průmyslové špionáže. Výsledky nebyly někdy stoprocentně shodné, například se vypráví (já nemohu ověřit), že východoevropské ekvivalenty měly občas vyšší spotřebu a horší dynamické parametry, takže například nešly tak snadno přetaktovat, někdy zase naopak byly v některých parametrech lepší.

Každopádně už zmíněný procesor 8080A, vyvinutý Intelem, má co do ekvivalentů velmi pestrou historii. Dodnes známý a existující výrobce AMD vytvořil vlastní klon 8080A pomocí zpětného inženýrství a pod označením 9080A jej začal nabízet v roce 1975. V roce 1976 podepsal AMD s Intelem dohodu, a stal se autorizovaným &#8222;druhým zdrojem&#8220;.

Svoje 8080A vyráběli například Japonci (Mitsubishi M5L8080A, NEC D8080, OKI MSM8080A, Toshiba TMP9080A), Rusové (580VM80 &#8211; jejich značení je legendárně zmatečné), Polsko i ČSSR (MCY7880, resp. MHB8080A), NatSemi, NTE, Siemens, Signetics i Texas Instruments. Dovedete si představit, že by dneska Core 2 Duo vyrábělo kromě Intelu dvanáct dalších výrobců a nabízeli ho pod svými značkami?

Intel pak nabídl vylepšenou variantu 8085A. Samozřejmě i tahle varianta měla spoustu &#8222;druhých zdrojů&#8220; &#8211; AMD AM8085A, NEC D8085A, Mitsubishi M5L8085A, Siemens SAB8085A, Toshiba TMP8085A&#8230; V tehdejším SSSR se vyráběl ekvivalent pod označením IM1821VM85A (jasný, ne?)

Ekvivalent vyráběl i výrobce OKI pod označením MSM80C85A &#8211; jejich verze měla výrazně nižší spotřebu než originál od Intelu. Čímž se dostáváme k jedné důležité motivaci, a tou je &#8222;vylepšit originál&#8220;.

## Zlepšovatelé

Odchod Federica Faggina od Intelu a založení Zilogu je notoricky známá historie. Faggin, který se podílel na návrhu 8080, nakonec od Intelu odešel a přidal se k Zilogu, kde navrhl &#8222;silně vylepšený procesor 8080&#8220;. Jeho cílem bylo navrhnout procesor, který by byl na úrovni strojového kódu kompatibilní s 8080. Tedy měl stejné registry a stejné instrukce se stejnými operačními kódy. To znamenalo, že existující binární programy pro 8080 nebylo nutné překompilovávat, což usnadnilo vstup jejich Z80 na trh. Navíc v Zilogu snížili počet potřebných napájecích napětí na jedno (+5V), celý procesor zrychlili, přidali obvody pro automatický refresh dynamických pamětí, přidali druhou sadu registrů, dva indexové registry a spoustu nových instrukcí, které jednak zvýšily ortogonalitu instrukční sady, jednak přinesly zcela nové operace, například blokové přesuny, bloková porovnání či bitové operace.

I procesor Z80 měl spoustu &#8222;druhých zdrojů&#8220;. Jejich značení je taky pěkná přehlídka kreativity. Ostatně Zilog dnes svou Z80 označuje Z84C00xx (XX udává rychlost) a podpůrné obvody (PIO, SIO, CTC, DMA), které se blahé paměti označovaly např. jako Z80PIO, jsou dnes Z84C10&#8230; Takže se nelekejte, když místo Z80 uvidíte Z84&#8230;

Goldstar vyráběl Z8400A, SGS používal taky Z8400, Ates Z80ACPU, Thomson Z84C00, Toshiba TMPZ84C00. Ale třeba Sharp vyráběl totéž pod označením LH0080, ROHM používal BU18400A, NEC značil D780C, Mostek MK3880, Kawasaki KL5C8400, v NDR vyráběli Z80 pod označením U880D (neplést s U808D!), v SSSR dostal klon označení T34VM1 nebo KP1858BM1.

Kromě těchto klonů vznikly odvozeniny. Například Hitachi navrhlo HD64180, což byla vylepšená a rozšířená verze Z80. Vylepšení bylo hlavně v oblasti technologické (CMOS, mikrokód), rozšíření pak představovaly především zabudované periferie a jednotka řízení paměti. Hitachi pak tuhle verzi licencovalo zpět Zilogu, který ji prodával pod označením Z64180, no a po lehkém přepracování u Zilogů se z tohoto čipu stala známá Z180.

Zmiňovaný klon od Kawasaki zase kupříkladu zrychlil provádění některých instrukcí a umožňoval běh až na 33MHz.

Toshiba integrovala některé periferie spolu s jádrem Z80 do jednoho pouzdra s 84 / 100 vývody pod označením Z84013/Z84015 (CMOS verze Z84C13/Z84C15). Dneska tyto čipy vyrábí a dodává jako &#8222;second source&#8220; také&#8230; Zilog!

Téměř shodná historie se odehrála i v &#8222;paralelní větvi mikroprocesorového vývoje&#8220;, tedy u Motoroly. První procesor 6800 měl rovněž několik &#8222;druhých zdrojů&#8220;. Historie s Fagginem se u Motoroly opakuje v podobě odchodu některých inženýrů do nově vzniklé firmy MOS Technology, kde vyvinuli procesor 6502. Ten není &#8222;rozšířenou variantou 6800&#8220;, ale spíš &#8222;sice nekompatibilní, ale levnou variantou&#8220;, díky čemuž se 6502 objevila ve spoustě garážových počítačů té doby. 6502 se vyrábí dodneška, a o jeho vylepšenou variantu 65C816, kombinující 8bitové a 16bitové jádro, se postarali vývojáři z WDC.

Motorola nabídla několik variant 6800 (např. se zakomponovanou pamětí), a nakonec vyvinula [procesor 6809](https://retrocip.uelectronics.info/posledni-krasavec-osmibitove-ery/ "Poslední krasavec osmibitové éry"), který je jednoznačně vrcholem tehdejší osmibitové éry (dokonce i Bill Gates měl prohlásit, že to je &#8222;nejlépe navržený osmibitový procesor&#8220; &#8211; a ten by to měl vědět, protože pro něj psal BASIC). Tento procesor má mnoho komplexních adresních módů (například &#8222;přečti bajt z adresy, která je v registru X, a pak zvyš hodnotu X o 1&#8220;), nabízí dva akumulátory A a B (které se dohromady tváří jako 16bitový registr D), dva šestnáctibitové indexové registry X a Y, dva 16bitové ukazatele zásobníku U a S (to by se to implementoval FORTH, že?), nabízí (podobně jako 6800 nebo 6502) možnost adresovat paměť zkrácenou adresou v nulté stránce &#8211; ale tady nejsme omezeni na nultou stránku, protože si pomocí registru DP můžeme nastavit, se kterou stránkou se pracuje. Uvnitř je i hardwarová násobička 8&#215;8 bitů&#8230; Navíc je sada silně ortogonální (takže se nestává tak často, že by nějaká kombinace operandů &#8222;nešla použít&#8220;).

Vlastní 6809 vyráběl opět Hitachi, a opět, jako v případě Z80, připravili japonští návrháři i vylepšenou variantu, která se vyrábí pod [označením 6309](https://retrocip.uelectronics.info/6309-vse-je-neoficialni/ "6309: Vše je neoficiální!"). K vlastnostem 6809 přidává další dva akumulátory E a F, které dohromady tvoří další 16bitový akumulátor W&#8230; který spolu s akumulátorem D tvoří 32bitový akumulátor Q. Přibyl i &#8222;odkládací&#8220; 16bitový registr V, přibyl &#8222;registr 0&#8220;, v němž je vždycky 0 a který zjednodušuje nulování registrů &#8211; místo zapsání přímé hodnoty stačí jen přenést hodnotu z tohoto registru, přibyly nové instrukce, blokové operace, instrukce pro dělení a dlouhé násobení, instrukce pro aritmetiku, které výsledek neukládají do akumulátoru, ale do paměti&#8230;

> Ano, kdyby bylo hlasování o nejzajímavější historický osmibit, tak sorry, Z80, jsi sice srdcovka, ale můj favorit je 6309!

## Zdroje nejsou&#8230;

Takováhle &#8222;výroba ekvivalentů&#8220; a &#8222;vylepšených ekvivalentů&#8220; kvetla poměrně dlouho. Šestnáctibitové procesory z rodiny x86 měly ze začátku &#8222;druhých zdrojů&#8220; dost, nejen _známé firmy_ AMD a Cyrix. Postupem času se od sebe originál a &#8222;druhý zdroj&#8220; oddělily, přestaly být kompatibilní vývodově, pak i parametry, a dnes už se jedná o dva odlišné světy s odlišnými chipsety i pouzdry, i když na binární úrovni do jisté míry stále kompatibilní.

U kolegů z Motoroly to platilo ještě pro verzi 68000 &#8211; tu vyráběli Hitachi, Mostek, ST, Rockwell, Signetics, Thomson nebo Toshiba. Verzi 68020 a další už jen Motorola. Klon si vyrobil Philips pod označením 68070, ale o jeho kompatibilitě víc nevím.

A tak víceméně skončila u procesorů trošičku divoká éra &#8222;druhých zdrojů&#8220;. Nejčastější situace, kdy víc výrobců nabízí podobné procesory, je dneska ve světě ARMových procesorů, kdy si několik desítek výrobců licencuje &#8222;ARM jádro&#8220; (kterých je taky několik verzí) a každý vyrábí vlastní variantu. Navzájem jsou nezaměnitelné, mají jiné parametry, ale základ je stejný a do jisté míry mohou programy pracovat i na jiném &#8222;ARMovém&#8220; procesoru.

Podobně se dodnes v portfoliu několika různých výrobců najde různě vylepšené jádro jednočipu 8052. Atmel nebo Dallas vyrábějí své vlastní varianty tohoto jednočipu, které jsou vývodově kompatibilní.

Na jednu stranu se tím zjednodušil výběr platformy a kompatibilita, na druhou stranu jsme ale ochuzeni o různá vylepšení a plody lidské tvořivosti, jako je UB8830D, což byl východoněmecký klon jednočipu Zilog Z8, který měl v ROM z výroby připravený interpret Tiny BASICu.

Taky máte dojem, že je dneska ve světě procesorů už docela nuda? 🙂

(Náhledový obrázek procesoru Z80 pochází z [Wikipedie](https://en.wikipedia.org/wiki/File:Zilog_Z80.jpg). Spousta encyklopedických informací o procesorech a jejich výrobcích je k dispozici na webu [CPU world](https://www.cpu-world.com/CPUs/CPU.html).)