---
id: 438
title: Minutka
date: 2014-11-08T15:44:28+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/434-autosave-v1/
permalink: /434-autosave-v1/
---
Sice jsem chtěl úplně něco jiného, ale prostě to tak nějak vyšlo, že jsem nakonec udělal takovou blbůstku&#8230;

<!--more-->

Před časem se mi zmínil známý, že si koupil [digitální hodiny s jednočipem](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5337564113&customid=&icep_uq=C51+4+Bits+Digital+LED+Electronic+Clock&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=175745&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg) a jestli by to prý nešlo přeprogramovat tak, aby z toho byla spíš kuchyňská minutka.

Po chvíli hledání jsem identifikoval kit YSZ-4. Je tam čtyřmístný LED displej a 89C2051 od Atmelu, což je procesor s jádrem x51 v malém pouzdru. U toho dvě tlačítka a bzučák.

Chtěl jsem nejdřív zjistit, co tam vlastně je a jak to funguje. Podařilo se mi přečíst FLASH, ale disassembling k ničemu nebyl. Chtěl jsem použít aspoň rutiny pro displej, ale pak se ukázalo mnohem snazší to napsat na zelené louce.

Kdybyste náhodou chtěli taky, tak schéma je tady:

<img loading="lazy" class="aligncenter size-medium wp-image-435" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/11/pafnuc-schema-650x441.png" alt="YSZ-4" width="650" height="441" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/11/pafnuc-schema-650x441.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/11/pafnuc-schema.png 654w" sizes="(max-width: 650px) 100vw, 650px" /> 

Displej má [datasheet zde](http://www.kz68.com/detail/2891/20130916/2Y11642681.html), to nejdůležitější, co vás asi bude zajímat, je, že dvojtečka je zapojená jako DP u pozice 2 (druhá zleva).

S x51 jsem nedělal, no, dobrých sedm let. Takže jsem se musel zorientovat. Použil jsem nakonec [překladač SDCC](http://sdcc.sourceforge.net/) (ne, fakt se mi nechtělo oprašovat assembler) a nad ním IDE s názvem [MCU 8051 IDE](http://www.moravia-microsystems.com/mcu-8051-ide/) (autor Moravia Microsystems, tedy ČR &#8211; příjemně potěšilo). IDE má i simulátor a emulaci jednoduchého hardware &#8211; a je mezi nimi i &#8222;multiplex display&#8220;, tedy to, co mají ty hodiny. Na rozdíl od hodin jsou v emulaci segmenty spínané jedničkou (přes tranzistor na zem), takže jsem si to musel ošetřit&#8230; Ale jinak obrovské zrychlení práce (díky tomu IDE jsem to celé měl za dvě hodiny hotové).

<img loading="lazy" class="aligncenter size-medium wp-image-436" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/11/hodiny-650x365.jpg" alt="hodiny" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/11/hodiny-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/11/hodiny.jpg 800w" sizes="(max-width: 650px) 100vw, 650px" /> 

Po úvodním HELLO WORLD to už šlo rychle. Věděl jsem, že multiplex funguje, věděl jsem, jak jsou zapojené segmenty, tak zbývalo už jen nastavit správně časovač. Použil jsem [online kalkulačku](https://www.easycalculation.com/engineering/electrical/uc-time-delay.php) pro zjištění konstant (16bitový časovač, 50 ms), no a zbytek bylo už jen programování pro ZŠ.

Ovládání jsem vyřešil pomocí těch dvou tlačítek. Jedno jsem nazval SET, tím si nastavím čas v minutách. Při podržení přičítá rychleji. Jakmile ho pustím, začne se po malé pauze odpočítávat k nule. Malá pauza je nezbytná, když člověk &#8222;domačkává&#8220; poslední minuty, aby se mu hodiny nespouštěly mezi jednotlivými stisknutími. _(Teď mě napadá, že to šlo udělat elegantněji, než jsem to nakonec vyřešil, jenom úpravou počítadla přerušení, ale už to měnit nebudu.) Maximáln_



Časovač pak počítá k nule, a když dopočítá, spustí se řev. Drobný detail &#8211; to zařízení není reproduktor, se kterým by bylo potřeba kmitat, jak jsem si myslel, je to opravdu bzučák, takže při sepnutí píská, při vypnutí mlčí. No a když je spuštěný alarm, tak ho vypnete stisknutím druhého tlačítka, tlačítka RESET. Tímtéž tlačítkem taky vypnete běžící odpočítávání.

  
Takhle prosté to je.

A kdybyste někdo chtěli, tak [zdroják je zde ke stažení](https://github.com/maly/51clock).