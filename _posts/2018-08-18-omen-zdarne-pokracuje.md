---
id: 1087
title: OMEN zdárně pokračuje
date: 2018-08-18T13:41:43+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1087
permalink: /omen-zdarne-pokracuje/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "6860877914"
categories:
  - Hardware
tags:
  - OMEN
  - OMEN Alpha
---
Konstrukce do druhé knihy úspěšně pokračují. Sledujte:

OMEN Alpha má už čtvrtou revizi (tentokrát mírně vylepšená systémová sběrnice, vyhozená LEDka a vyvedené vývody SID, SOD).

[Na Tindie jsem Alphu vyprodal](https://www.tindie.com/products/parallaxis/omen-alpha/) během dvou týdnů, teď čekám na nové součástky a novou várku desek. Máte-li zájem o desky, sady nebo hotové kity, [napište se na waitlist](https://www.tindie.com/products/parallaxis/omen-alpha/).

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180812_181046_HDR.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1088" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180812_181046_HDR-650x503.jpg" alt="" width="650" height="503" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180812_181046_HDR-650x503.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180812_181046_HDR-768x594.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180812_181046_HDR-1024x792.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /></a>

Nahodil jsem jednoduchý [manuál k Alfě](https://halckemy.s3.amazonaws.com/uploads/attachments/550894/manual_QRztOJ71KG.pdf), kde jsem si zavzpomínal na _vizuální estetiku_ cyklostylovaných manuálů k PMD a podobným, s nimiž jsem kdysi trávil volné chvíle.

OMEN Bravo, tedy jednodeskáč s 65C02, po prvotním fiasku (ano, jsem debil a navrhnul jsem oscilátor blbě!) běžel na první zapojení.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180809_201844_HDR.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1089" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180809_201844_HDR-650x518.jpg" alt="" width="650" height="518" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180809_201844_HDR-650x518.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180809_201844_HDR-768x612.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180809_201844_HDR-1024x816.jpg 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2018/08/20180809_201844_HDR.jpg 1080w" sizes="(max-width: 650px) 100vw, 650px" /></a>

OMEN Kilo, jednodeskáč s 68B09 aka _nejlepším osmibitem své doby_, dopadl stejným fiaskem. Ale tentokrát za to nemůžu já, ale moje skladové zásoby tohoto procesoru. Jsou mrtvé. Všechny. Takže čekám na nové&#8230;

Všechny tyhle OMENy mají stejné rozložení signálů na systémové sběrnici. Tedy, ona není úplně systémová, protože není kompletní adresová sběrnice, ale řekněme _aplikační_. Takže další přídavné moduly, co vznikají (displej, I2C/SPI rozhraní, zvukový generátor, SD karta, paměťové moduly, &#8230;), by měly být kompatibilní se všemi základními deskami. No a aby se připojovalo snáze, připravil jsem i malý backplane PCB, kam si je můžete popřipojovat.

Jeden z majitelů Alphy mě upozornil na problém: v PCB nejsou otvory na šroubky. Přiznám se, že mě to ani nenapadlo, protože mám v hlavě pořád vizi toho, že se budou tyhle desky montovat do krabiček, vytištěných na 3D tiskárně, a desky se uchytí jen takovými pacičkami, ale asi je pravda, že by se otvor či dva hodily&#8230;

Ve volných chvílích jsem napsal překladač pro BASIC. Vložíte BASIC, vypadne vám assemblerový zdroják. Zatím tedy jen pro 8080/8085. Na základní hraní, helejte, dobrý. Sice nemá moc optimalizací a je jen celočíselný, ale na druhou stranu má náznak procedur, lokálních proměnných, struktur datových i programových&#8230;

No a aktuální The Big Thing je online překladač céčka pro 6502, 8080 a Z80. Ne, nepíšu to celé sám a znovu, jen (a slovo &#8222;jen&#8220; je v hodně velkých uvozovkách) jsem přeložil existující cc65 a z88dk pomocí Emscripten do JavaScriptu. Tři dny trvalo, než to začalo fungovat, bestie! Ale když se to rozběhlo, funguje to moc hezky.

A mimochodem, víte, proč cpu vývojové věci do JavaScriptu? Není to proto, že můžou běžet v prohlížeči, ale hlavně proto, že budou fungovat na Linuxu. Macu i na Windows. _Cože? Že můžu napsat totéž v C a pak na každé platformě přeložit pomocí make? No, věřte mi, že za posledních dvacet let jsem potkal spoustu takového software, a pravda byla taková, že pokud někdo neudělal binárky pro jednotlivé platformy, tak pravděpodobnost toho, že to půjde bezproblémově přeložit a spustit, byla někde okolo 1:7&#8230;_