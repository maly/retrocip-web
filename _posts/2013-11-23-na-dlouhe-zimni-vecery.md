---
id: 8
title: Na dlouhé zimní večery
date: 2013-11-23T11:40:11+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=8
permalink: /na-dlouhe-zimni-vecery/
dsq_thread_id:
  - "1991822179"
categories:
  - Hardware
---
Ne že bych neměl o zábavu postaráno&#8230;

<!--more-->

Do sbírky úlovků přibyl nový kousek &#8211; tedy vlastně starý. Legendární _školský mikropočítač_ PMI-80. Přišel bez záruky a bez zdroje, takže mám o zábavu postaráno, čekají mě restaurátorské práce. Na první pohled je bez viditelných poškození, tak snad bude stačit jen vyčistit (a spájet vhodný zdroj). 

<div id='gallery-1' class='gallery galleryid-8 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image001.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption13'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image001-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image003.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption11'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image003-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image002.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption12'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image002-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image005.jpg" title="PMI-80" class="highslide" onclick="return hs.expand(this,{captionId:'caption9'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image005-150x150.jpg" width="150" height="150" alt="PMI-80" /></a>
    </dt>
    
    <dd class="gallery-caption" id="caption9">
      <span class="imagecaption">PMI-80</span><br />
    </dd>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image004.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption10'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2013/11/image004-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style='clear: both' />
</div>

Zároveň s tím jsem dopsal emulátor PMI-80 v JavaScriptu, včetně nahrávání na virtuální &#8222;pásky&#8220;. Zkoušel jsem zprovoznit i zvuk, ale zdálo se mi to hodně šílené &#8211; ale co já vím, třeba PMI takhle šíleně má znít?! Uvidíme.

Včera večer jsem si sednul a adaptoval emulátor PMI na emulátor legendárního BOBa-85. Po dvou hodinách mám emulátor, který zobrazuje co má, funguje jak má, obsluhuje klávesnici a i jinak vypadá funkčně. Teď jen napojit &#8222;virtuální magnetofon&#8220; na signály SID, SOD a odladit&#8230;

V každém takhle velkém projektu jsou, logicky, chyby. Martin Bórik mě upozornil na chybu v emulaci instrukce DAD SP (zapomenuté závorky), včera jsem při psaní BOBa narazil na problém s instrukcí RAL (místo a<<1 bylo a<<7)&#8230; Ale to je v pořádku, chytání chyb k vývoji patří.

Mám teď v ruce knížku &#8222;Mikroprocesorová technika&#8220; od Milana Babáka (vydalo SNTL v roce 1986). Popisuje procesor 8080 a obvody kolem něj (8212, 8214, 8251, 8253, 8255), assembler, a &#8211; což je nejcennější &#8211; asi 30 stránek je věnováno popisu jednodeskového TEMS 80-03! Jsem opět o krůček blíž emulaci i téhle raritky&#8230;