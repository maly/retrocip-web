---
id: 385
title: Jednodeskáč, za který mě milovníci jednodeskáčů proklejí
date: 2014-08-22T16:01:34+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=385
permalink: /jednodeskac-za-ktery-me-milovnici-jednodeskacu-prokleji/
dsq_thread_id:
  - "2949859121"
image: http://retrocip.cz/wp-content/uploads/sites/6/2014/08/12710-01-600x198.jpg
categories:
  - Hardware
tags:
  - "6502"
  - Atmel
  - AVR
  - Z80
---
až do šestého kolene a třetího lokte. 🙂

<!--more-->

Začalo to bublinovým displejem.

<img loading="lazy" class="aligncenter size-thumbnail wp-image-386" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/08/12710-01-150x150.jpg" alt="" width="150" height="150" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/08/12710-01-150x150.jpg 150w, https://retrocip.cz/wp-content/uploads/sites/6/2014/08/12710-01.jpg 600w" sizes="(max-width: 150px) 100vw, 150px" /> 

Je to normální LED sedmisegmentovka, se čtyřma znakama, ale plastový kryt dělá takovou jako že čočku, takže ty pidiznaky (3 milimetry, jestli jsem se dobře koukal) jsou trošku zvětšené. Něco jako má PMI-80.

Což mi tedy vnuklo myšlenku jich pár vzít, když byly tak levný, a udělat si něco s _retrofílinkem_. Jako jo, emulátor jest pěkná věc, ale ten jednodeskáč&#8230; Si ho člověk vezme klidně do postele a může si do zblbnutí zadávat hexakódy.

Když tedy zadávat, tak to musí mít nějakou klávesnici. Matici tlačítek, hmmm, musí to mít 16 tlačítek pro hexadecimální znaky, nějaký ten reset, pár funkčních tlačítek, šlo by to s 4&#215;5, ale 5&#215;5 bude lepší. A tak jsem koukal po eBay na klávesnici s maticí 5&#215;5. Nakonec jsem objevil doslova za lacino [klávesnici, která jako by vypadla z oka počítačům TNS](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=2&pub=5575085282&toolid=10001&campid=5337554641&customid=&icep_item=130541363513&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg). Takové velké hmatníky, které se dají rozebrat a strčit do toho papírek s popiskem, celé to je na kusu plechu&#8230;

<img loading="lazy" class="aligncenter size-medium wp-image-387" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/08/1deska-650x365.jpg" alt="1deska" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/08/1deska-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/08/1deska.jpg 800w" sizes="(max-width: 650px) 100vw, 650px" /> 

Takže tím mám dvě hlavní komponenty z krku.

Co potřebuje jednodeskáč dál? Procesor, paměť RAM, paměť ROM, a pokud možno i nějaký ten interface. A tady jsem, přátelé, zpychnul.

Už jsem psal, že nemám [toho pravýho retroducha](http://retrocip.uelectronics.info/retro-duch/ "Retro duch"). Mohl bych vzít Z80 nebo 6502, přišmudlat k němu nějakou bižuterii a mít jednodeskáč jak víno. Anebo postavit repliku &#8211; nabízí se KIM-1, nebo třeba to [PMIZ-80](http://css-electronics.8u.cz/8bity/pmiz80a/pmiz80a_comp.html). Jenže mně tu v šuplíku leží několik ATMega1284.

No hele, má to čtyři paralelní porty, spoustu RAMky, ještě víc ROMky, to mi neříkejte, že by s tím něco nešlo&#8230; A vono šlo! Je teda trošku problém, protože AVR nespustí kód v RAMce, takže budu muset vymýšlet nějaké hacky pro zapisování do FLASH paměti, ale půjde to! Klávesnici a displej zapojím hezky postaru, jako v PMI, klávesnici do matice 3&#215;8 (ano, to je 24 tlačítek, tlačítko numero 25 bude RESET), no a vlastně budu mít postaráno i o sériové rozhraní, tím pádem tedy o připojení k nějakému PC. Cha!

A pak mě osvítilo ještě víc: Vždyť já můžu tím AVRkem velmi dobře emulovat jak Z80, tak i tu 6502! Do Flashky se mi vejde, cojávím, jak emulátor toho procesoru, tak i ROMka pro jednodeskáč, když na to přijde, tak i 8kB BASIC, má to 16 kB RAM, co chtít víc? Jednodeskový chameleon, kam si člověk nahraje podle nálady procesor, jaký chce, a pak si může hrát&#8230; Nebo tam můžou být oba, a já si po spuštění vyberu, který se má použít. Ba co dím, můžou tam být všechny tři!

Já vím, já vím&#8230; Před mým PMI to radši nebudu říkat nahlas&#8230;

<img loading="lazy" class="aligncenter size-full wp-image-390" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/08/klav.jpg" alt="klav" width="600" height="338" />