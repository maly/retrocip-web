---
id: 432
title: CCCPektrum
date: 2014-11-01T12:17:00+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/422-autosave-v1/
permalink: /422-autosave-v1/
---
Báťuška car izdal balšoj manifest, što mužikum dalžno pastrajit zet iks spektrum.

A tak soudruzi sedli, vymýšleli, a stvořili nakonec rovný kýbl nejrůznějších klonů, napodobenin, vylepšenin i prachsprostých oprásknutých verzí.

Já teď dostal takovou poměrně unikátní &#8211; ještě nikde jsem ji neviděl. Zvenčí to vypadá takhle:

<img loading="lazy" class="aligncenter size-medium wp-image-423" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/IMG_20141031_134524-650x365.jpg" alt="IMG_20141031_134524" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/10/IMG_20141031_134524-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/10/IMG_20141031_134524-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Trošku jako Timex, tomu nahrává ten nápis &#8222;Personal Color Computer&#8220; a mřížka nahoře. Nápis Sinclair je naprosto pofidérní, navíc se stínováním&#8230; Ale bílá tlačítka tomu sluší, a nejsou to &#8222;gumáky&#8220;.

Zezadu je trojice DIN, tlačítko RESET a napájecí kabel. Jeden konektor je pro magnetofon, druhý pro joystick, třetí pro monitor.

<img loading="lazy" class="aligncenter size-medium wp-image-424" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/IMG_20141031_134600-650x365.jpg" alt="IMG_20141031_134600" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/10/IMG_20141031_134600-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/10/IMG_20141031_134600-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Joystick mám taky, je na něm napsáno &#8222;Made in Ukraine&#8220;. Hmmm, že by už &#8222;postsovětský&#8220; výrobek?

Trošku mě zarazilo, že není systémový konektor. Inu, není&#8230; A když jsem to vzal do ruky, tak to rachtalo, což normálně Spectra nedělají, takže jsem to otevřel, a uvnitř&#8230;

<img loading="lazy" class="aligncenter size-medium wp-image-428" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/IMG_20141031_134641-650x365.jpg" alt="IMG_20141031_134641" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/10/IMG_20141031_134641-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/10/IMG_20141031_134641-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Jo, to trafo vlevo se utrhlo od krabice a rachtalo. Po pravdě řečeno jsem na to koukal chvíli s otevřenou pusou. Konstruktéři se s tím ale ani za mák necrcali. Tady trafo, to přilepíme. Pojistku neřešíme. Tady kondík a stabilizátor, přilepíme vedle, propojíme cukršpagátem, no a vlastní Spectrum sestává z několika sovětských obvodů&#8230;

Uprostřed trůní ULA, respektive její ruská kopie KA1515CHM1216 (pozor, kdybyste to googlili, tak je to [KA1515XM1216](https://www.google.cz/search?q=KA1515XM1216)). Vlevo paměti KR565RU5G (64kx1 DRAM), dole vpravo ROMka KR1013RE1-020 (obsahuje ROM ZX Spectra). Celé to pohání procesor [KR1858VM1](http://www.cpu-world.com/CPUs/Z80/USSR-KR1858VM1.html), tedy ruská Z80. No a nahoře nějaká logika: K555IR16 (74LS295, 4bit posuvný registr), K555LP5 (74LS86, 4xXOR), K561LN1 (MC14502, 6 strobovaných invertorů) a KR1533LN1 (74ALS07). _Ekvivalenty beru z [tohoto PDF](http://pdf.datasheetarchive.com/indexerfiles/Datasheets-XX1/DSAXX0011295.pdf)._

<img loading="lazy" class="aligncenter size-medium wp-image-429" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/IMG_20141031_134658-650x365.jpg" alt="IMG_20141031_134658" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/10/IMG_20141031_134658-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/10/IMG_20141031_134658-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

No _parachod bolšoj_!

<del>A moje otázka zní: Nevíte někdo něco bližšího o tom, kdo to vyrobil, co to je za klon? Já to na webu nenašel&#8230;</del>

[Díky VELESOFTovi už vím](http://retrocip.cz/cccpektrum/#comment-1664455500), že se jedná o klon Sirius. Nějaké info zde.