---
id: 422
title: CCCPektrum
date: 2014-10-31T15:00:21+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=422
permalink: /cccpektrum/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3176446975"
categories:
  - Hardware
tags:
  - klon
  - Sinclair
  - Spectrum
  - ZX
---
Báťuška car izdal balšoj manifest, što mužikum dalžno pastrajit zet iks spektrum.

A tak soudruzi sedli, vymýšleli, a stvořili nakonec rovný kýbl nejrůznějších klonů, napodobenin, vylepšenin i prachsprostých oprásknutých verzí.

<!--more-->

Já teď dostal takovou poměrně unikátní &#8211; ještě nikde jsem ji neviděl. Zvenčí to vypadá takhle:

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/IMG_20141031_134524-650x365.jpg) 

Trošku jako Timex, tomu nahrává ten nápis &#8222;Personal Color Computer&#8220; a mřížka nahoře. Nápis Sinclair je naprosto pofidérní, navíc se stínováním&#8230; Ale bílá tlačítka tomu sluší, a nejsou to &#8222;gumáky&#8220;.

Zezadu je trojice DIN, tlačítko RESET a napájecí kabel. Jeden konektor je pro magnetofon, druhý pro joystick, třetí pro monitor.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/IMG_20141031_134600-650x365.jpg) 

Joystick mám taky, je na něm napsáno &#8222;Made in Ukraine&#8220;. Hmmm, že by už &#8222;postsovětský&#8220; výrobek?

Trošku mě zarazilo, že není systémový konektor. Inu, není&#8230; A když jsem to vzal do ruky, tak to rachtalo, což normálně Spectra nedělají, takže jsem to otevřel, a uvnitř&#8230;

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/IMG_20141031_134641-650x365.jpg) 

Jo, to trafo vlevo se utrhlo od krabice a rachtalo. Po pravdě řečeno jsem na to koukal chvíli s otevřenou pusou. Konstruktéři se s tím ale ani za mák necrcali. Tady trafo, to přilepíme. Pojistku neřešíme. Tady kondík a stabilizátor, přilepíme vedle, propojíme cukršpagátem, no a vlastní Spectrum sestává z několika sovětských obvodů&#8230;

Uprostřed trůní ULA, respektive její ruská kopie KA1515CHM1216 (pozor, kdybyste to googlili, tak je to [KA1515XM1216](https://www.google.cz/search?q=KA1515XM1216)). Vlevo paměti KR565RU5G (64kx1 DRAM), dole vpravo ROMka KR1013RE1-020 (obsahuje ROM ZX Spectra). Celé to pohání procesor [KR1858VM1](https://www.cpu-world.com/CPUs/Z80/USSR-KR1858VM1.html), tedy ruská Z80. No a nahoře nějaká logika: K555IR16 (74LS295, 4bit posuvný registr), K555LP5 (74LS86, 4xXOR), K561LN1 (MC14502, 6 strobovaných invertorů) a KR1533LN1 (74ALS07). _Ekvivalenty beru z [tohoto PDF](https://pdf.datasheetarchive.com/indexerfiles/Datasheets-XX1/DSAXX0011295.pdf)._

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/IMG_20141031_134658-650x365.jpg) 

No _parachod bolšoj_!

<del>A moje otázka zní: Nevíte někdo něco bližšího o tom, kdo to vyrobil, co to je za klon? Já to na webu nenašel&#8230;</del>

[Díky VELESOFTovi už vím](https://retrocip.cz/cccpektrum/#comment-1664455500), že se jedná o klon Sirius. Nějaké [info zde](https://sblive.narod.ru/ZX-Spectrum/Sirius/Sirius.htm).