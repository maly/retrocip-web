---
id: 1121
title: 'Bylo to těsné, ale&#8230;'
date: 2018-11-19T18:17:39+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1121
permalink: /bylo-to-tesne-ale/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "7059118674"
image: http://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180812_181046_HDR-1140x198.jpg
categories:
  - Hardware
tags:
  - "6809"
  - OMEN
  - OMEN Kilo
---
[Vede Motorola.](https://docs.google.com/forms/d/1IFKfSpwKfpAIE4K-SXmpTEES22eBNapgnh1Q6rE2U_I/viewanalytics)

<!--more Jak je to možné?-->

Nebojte, o 6502 nepřijdete, je v knize a čeká na vás.

Já mám několik sad pro OMEN Kilo [připravených](https://www.tindie.com/products/parallaxis/omen-kilo-kit/), ale asi se nedostane na všechny zájemce. Proto tu s předstihem publikuju soupisku, abyste ještě stihli nakoupit tak, aby to do zimy přišlo:

  * Procesor [MC68B09P](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5338390149&customid=&icep_uq=MC68B09P&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg), popřípadě [HD63C09P](http:// http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5338390149&customid=&icep_uq=HD63C09P&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg). _POZOR! Nikoli verzi &#8222;EP&#8220;, ta má jinak zapojený oscilátor a nebude fungovat. Taky pozor na případné falešné / nefunkční obvody, stalo se mi to u tohoto procesoru několikrát. Já zkusil několik různých dodavatelů a doporučím vyhnout se těm, co mají podezřele nízké ceny (jako třeba 80 centů) nebo co mají generický obrázek &#8222;nějakého čipu&#8220;._
  * ACIA [68B50](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5338390149&customid=&icep_uq=68B50P&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg)
  * [74HC00](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5338390149&customid=&icep_uq=74hc00n&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg)
  * [74HC138](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5338390149&customid=&icep_uq=74hc138n&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg)
  * RAM [62256](http:// http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5338390149&customid=&icep_uq=62256&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg) (nebo analogický obvod 32k x 8)
  * EEPROM [28C64](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5338390149&customid=&icep_uq=AT28C64&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg) (8k x 8)
  * Krystal 7.3728 MHz
  * Kondenzátory 22pF a 100 nF
  * Rezistory 330R, 1K, 10K
  * Tlačítko
  * Pinové lišty
  * Patice pro IO
  * USB &#8211; TTL převodník pro sériovou komunikaci

S procesorem je mrzení, pokud chcete koupit kusové množství. Já beru balení po deseti, ale i tak se ukáže, že třeba tři z nich jsou vadné. Proto je radši hned po doručení prověřím na nepájivém kontaktním poli jednoduchým zapojením s krystalem, zda na výstupech E a Q naměřím to, co tam má být, tj. hodinový signál. Stačí voltmetr a hledat napětí okolo 2.5 voltu. Jakmile tam je &#8222;skoro 0&#8220; nebo &#8222;skoro 5&#8220;, tak oscilátor nešlape a obvod směřuje do elektroodpadu.<figure class="wp-block-image">

<img loading="lazy" width="876" height="810" src="http://retrocip.cz/wp-content/uploads/sites/6/2018/11/6809tester_bb.png" alt="" class="wp-image-1123" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/11/6809tester_bb.png 876w, https://retrocip.cz/wp-content/uploads/sites/6/2018/11/6809tester_bb-650x601.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/11/6809tester_bb-768x710.png 768w" sizes="(max-width: 876px) 100vw, 876px" /> </figure> 

Druhá test, který dělám (ale vy zatím nemůžete&#8230;) je, že je strčím do ověřeného Kila. Když se počítač ohlásí, je vše OK a procesor mám za funkční.

Když je to pak celé sestavené, funguje to nějak takto:<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio">

<div class="wp-block-embed__wrapper">
</div></figure>