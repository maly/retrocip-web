---
id: 466
title: SMART Card
date: 2014-12-28T17:01:56+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=466
permalink: /smart-card/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3367395582"
image: http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152347-1140x198.jpg
categories:
  - Hardware
tags:
  - cpld
  - Spectrum
  - ZX
---
Já vím, já vím, nazvat přídavnou kartu SMART je docela klišé. Ale co už &#8211; autor ji tak nazval, a na nás je, abychom ji použili a říkali jí třeba Klára, když na to přijde.

<!--more-->

Phil Ruston z Retrolea je můj oblíbený retropočítačník. Vlastním jeho V6Z80P, a tak když jsem zahlédl, že nabízí nový hardware a přečetl jsem si popis, neváhal jsem. [SMART Card](http://www.retroleum.co.uk/smart-card-for-zx-spectrum/), jak se to celé jmenuje, je přídavná karta k ZX Spectru a nabízí (za tu cenu!) velmi zajímavé možnosti.

Obsahuje 256kB ROM (přesněji FlashRAM), do které se vejde 16 ROMek, které karta dokáže připojit namísto standardní ROM ZX Spectra. Na desce je i 128 kB RAM, kterou si můžete namapovat po osmikilových blocích do prostoru 2000h-3FFFh. K tomu tam je slot na SD paměťovou kartu, tlačítka RESET a NMI, standardní devítipinový konektor pro joystick (namapován jako Kempston), a celé to dohromady drží glue logika v CPLD ([XC9572XL](http://www.xilinx.com/support/documentation/data_sheets/ds057.pdf)). Můžete použít i piny RX a TX na sériovou komunikaci.

Karta má především dvě oblasti využití. Jednak jako rychlé načítání a spouštění programů ze SD karty (loader &#8222;umí&#8220; formát SNA a s novým firmware i formát TAP, kde nahrazuje standardní rutiny ZX Spectra vlastním načítáním).

Druhá možnost je využít ji jako diagnostický nástroj &#8211; Phil nabízí &#8222;Diagnostic ROM&#8220;, která otestuje funkčnost pamětí a dalšího hardware (klávesnice, ULA, reproduktor, interní ROM, &#8230;) Pokud je Spectrum aspoň trochu živé (tzn. funguje mu zobrazování), můžete zjistit např. to, jaký paměťový čip je třeba vyměnit.

<img loading="lazy" class="aligncenter size-medium wp-image-471" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/12/IMG_20141228_152448-650x366.jpg" alt="IMG_20141228_152448" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152448-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152448-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Z hlediska ZX Spectra jde o interface na portech FAF3h, FAF7h a FAFBh (joystick je samozřejmě na portu 1Fh). Zapojení je následující:

FAF3h ovládá RAM. Bity 0-3 určují, která banka má být připojena do prostoru 2000h-3FFFh (pokud je to povolené). Bit 7 povoluje SRAM &#8211; pokud je nastaven, překrývá tato RAM interní či externí ROM. Bit 4 ovládá sériovou linku, bity 5 a 6 jsou určené pro SD kartu (SPI a CS).

Port FAF7h slouží ke čtení a zápisu z/na SD kartu. Poslaný bajt je interně serializován a odeslán do SD, při čtení je naopak deserializován a vystaven na tomto portu.

Port FAFBh ovládá ROM. Bity 0-3 opět udávají blok ROM, který má být připojen do spodních 16kB. Jestli bude připojena externí ROM, nebo interní, to opět ovládá bit 7. Bit 6 pak zapíná funkci &#8222;přepnutí na systémovou ROM po průchodu adresou xx72h&#8220; (pro spouštění snapshotů).

Pomocí přepínačů se vybírá startovací ROM, povoluje zápis do FLASH a zapíná testovací režim.

Já tam vidím ještě jedno možné využití &#8211; můžete si snadno vyzkoušet [svou vlastní ROMku](http://retrocip.uelectronics.info/uplne-alternativni-spectrum/ "Úplně alternativní Spectrum") na reálném zařízení.

<div id='gallery-6' class='gallery galleryid-466 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152347.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption474'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152347-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152409.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption473'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152409-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152423.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption472'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152423-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152448.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption471'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152448-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152458.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption470'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152458-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152541.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption468'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152541-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152612.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption467'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/12/IMG_20141228_152612-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style='clear: both' />
</div>