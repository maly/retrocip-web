---
id: 523
title: National Semiconductor COPS
date: 2015-02-12T11:01:49+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=523
permalink: /national-semiconductor-cops/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3508811178"
categories:
  - Hardware
tags:
  - COP
  - COPS
  - CPU
  - National
---
National Semiconductor nevyráběl jen známý SC/MP, ale i poměrně populární řadu levných procesorů, která se udržela na trhu přes třicet let.

<!--more-->

## COP2404 &#8211; dvoujádro z roku 1982

V roce 1977 představil National Semiconductor sérii čtyřbitových procesorů COPS. Vznikla z čipů pro kalkulátory a po krátkou dobu byla známa jako Calculator Oriented Processors (COP), což bylo brzy změněno na Control Oriented Processor System (COPS). Tyto čtyřbitrové mikrokontroléry, jak jejich název napovídá, byly určeny pro řízení různých strojů. Byly použity v nejrůznějších zařízeních, od herních konzolí po myčky. Na začátku 80. let je National začal vyrábě i v CMOS verzi, a v roce 1988 rozšířil řadu na 8 bitů (řada COP8).

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/02/COP2404N-300x85.jpg) <figcaption id="figcaption\_attachment\_527" class="wp-caption-text">Dvoujádrový procesor National Semiconductor COP2404N &#8211; Foto (C) CPU Shack</figcaption> 

V roce 1981 byl oznámen procesor COP2404 (a také 2440), s dostupností od roku 1982. COP2404 byl vrcholem celé řady COPS a byl výsledkem prosté úvahy: V praxi se ukázalo, že některé úkoly se lépe řeší pomocí dvojice mikrořadičů, tak proč tedy nedat rovnou dva do jednoho pouzdra? COP2404 je tedy dvoujádrový procesor, v němž jsou dvě kompletní jádra COPS404 na jednom čipu, které se dělí o společnou RAM a společné porty (verze 2440 měla i zabudovanou ROM). Paměť byla sdílená a obě jádra mohla pracovat nezávisle na sobě, popřípadě si předávat data či řízení. O to se hardware nijak nestaral, ale návrh s tím počítal, takže programátorovi nebránil dělat ani docela komplikovanou správu úloh.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/02/COP2404_B-650x505.jpg) <figcaption id="figcaption\_attachment\_524" class="wp-caption-text">Blokové schéma COP2404 &#8211; převzato z CPUSHack</figcaption> 

&nbsp;

2404 byl zapouzdřen v 48pinovém PDIP pouzdru a byl navržen jako vývojové zařízení pro použití s externí pamětí (nejčastěji EPROM). Produkční typy byly 2440 (40 pinů), 2441 (28 pinů) a 2442 (24 pinů), všechny s 2 kB ROM. Všechny typy obsahovaly 160 x 4 bity RAM a měly instrukční cyklus 4 mikrosekundy (s hodinovým kmitočtem 4 MHz; každá instrukce zabrala 16 taktů). Byly vyráběny třímikronovým procesem NMOS (původně, časem došlo ke změnám).

Jak se technologie vyvíjela, bylo stále snazší obsloužit úkoly v reálném čase jedním rychlým řadičem s dobrým systémem přerušení, ale aspoň na nějaký čas tu byl dvoujádrový mikrořadič. National prodával čipy z řady COPS až do roku 2011, kdy jej koupil Texas Instruments. V současnosti se stále některé čipy z této řady prodávají, ačkoli už nejsou aktivně nabízeny.

## Původní COP

Řada COP400 byla označována jako COPS II. Co tedy bylo tím původním COPem?

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/02/MM5782N-300x141.jpg) <figcaption id="figcaption\_attachment\_525" class="wp-caption-text">National Semiconductor MM5782N – 400KHz 1976 Foto (C) CPU Shack</figcaption> 

Opravdu existoval COPS I, známější jako MM5781/2 a jeho odvozeniny MM5799, MM57140 a MM57152. Tyto jednočipy přišly v roce 1976 a byly vyráběny technologií PMOS. Byly navrženy jako levné a snadno použitelné. Návrh MM5781/2 vlastně pochází původně z MM5734, což byl jednočipový akumulátor pro kalkulátory (=registr, v němž se provádějí operace). Rozdíly nejsou tak velké, jak by si člověk myslel. Multifunkční kalkulačka s pamětí vyžadovala ALU, registry, akumulátor a obvody k dekódování instrukcí, a k tomu trochu RAM a dobře rozšiřitelné porty (na které se připojoval displej a klávesnice). National zde spatřoval příležitost ukousnout si trochu z trhu low-end zařízení. V té době už měli IMP-16 pro high-end, SC/MP pro středně náročné aplikace, a zároveň byli druhým zdrojem pro obvody MCS-4 a MCS-80 od Intelu. Scházelo jim ale něco, co by mohlo konkurovat WD1872 nebo řadě TMS1000 od TI, stejně jako vzestupu japonských čtyřbitových obvodů od NEC, Toshiby či Sanya.

5781/2 byly dva obvody, které dohromady daly funkční mikropočítač. 5781 obsahoval ROM (2048 x 8 bitů), programový čítač a nějakou řídicí logiku. V obvodu 5782 byla ALU, akumulátor, dekodér instrukcí a 160 x 4 bity RAM. Obvod mohl vykonat 33 různých instrukcí s taktem 70 až 400 kHz, řízeným vnějším oscilátorem.

National později zkombinoval 5781/2 do jednoho obvodu MM5799. Ten obsahoval veškerou logiku 5781/2, ale měl menší RAM (96 x 4 bity) a ROM (1500 x 8 bitů). Takt hodin zůstal stejný, ale instrukční sada byla rozšířena na 41 instrukcí. Vznikly i dvě další verze s většími možnostmi I/O. MM57140 obsahoval zabudované budiče pro LED, MM57152 byl stejný, jen obsahoval budiče pro fluorescenční displeje. Verze 140 a 152 měly 36 instrukcí, 55 x 4 bity RAM, 630 x 8 bitů ROM a maximální frekvenci sníženou na 280 kHz.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/02/NationalSemiMM57109FAN-N-300x165.jpg) <figcaption id="figcaption\_attachment\_526" class="wp-caption-text">National Semiconductor MM57109 Foto (C) CPU Shack</figcaption> 

Ironií osudu se pak samotný COP stal základem pro další kalkulačkové obvody. 57109 byl 5799 upravený tak, aby podporoval 70 různých operací s čísly, čímž se dostal na pomezí mezi jednočip a programovatelnou kalkulačku. Neměl žádnou paměť ROM, ale obsahoval 5 x 32 bitů RAM. ROM, která byla normálně použitelná pro zákazníka, v tomto případě obsahovala algoritmy pro zmíněných 70 funkcí. MM57103 a 104 byly podobné, ale navržené čistě jako vědecká kalkulačka, bez programovatelných funkcí. Obvod 5784 byla jednodušší obdoba, poskytující 5 funkcí a devítimístný akumulátor.

Řada COP400 byla představena jen o rok později, s podobnou architekturou a stejnými 1560 x 4 bity RAM, i když instrukční sada byla odlišná. Díky technologii NMOS bylo jednodušší napájení, což napomohlo jejímu rozšíření, a původní PMOS COPy upadly v zapomenutí. Dodneška je ale můžete najít v některých výrobcích ze 70. let &#8211; například v hodně starých zaprášených jukeboxech.

_(Přeloženo z originálních článků [National Semiconductor COP2404 &#8211; Dual Core Processor from 1982](https://www.cpushack.com/2014/08/25/national-semiconductor-cop2404-dual-core-processor-from-1982/) a __<a title="Permanent Link: National Semiconductor: The COP before the COPS" href="https://www.cpushack.com/2014/09/27/national-semiconductor-the-cop-before-the-cops/" rel="bookmark">National Semiconductor: The COP before the COPS</a> se svolením vydavatele. Všechny obrázky pocházejí z originálních článků na CPU Shack.)_