---
id: 281
title: Texas Instruments TI-99/4A
date: 2014-05-08T12:09:58+01:00
author: Martin Maly
layout: page
guid: http://retrocip.uelectronics.info/?page_id=281
dsq_thread_id:
  - "2669688329"
---
Ve světě osobních počítačů tak trošku UFO.

Tento počítač, vydaný v roce 1981, obsahoval šestnáctibitový mikroprocesor TMS9900 (stejně jako jeho předchůdce TI-99/4 z roku 1979), což je de facto (jako mnoho mikroprocesorů té doby) monolitická verze existujícího minipočítače, v tomto případě [TI-990](http://en.wikipedia.org/wiki/TI-990). Jeho architektura je zajímavá tím, že je podobná filosofii procesorů 680x, 6502 apod. Samotný procesor obsahuje tři šestnáctibitové registry (PC, Status register a Workspace register) a v roli šestnácti pracovních registrů vystupuje paměť na adrese, dané Workspace registrem. To umožňuje velmi rychle přepínat kontext &#8211; vlastně jen prostou změnou obsahu workspace registru. Druhá zajímavost: procesor měl 16 datových pinů (D0 &#8211; D15) a patnáct adresních (A0 &#8211; A14). Adresace probíhala po celých slovech (dvou bajtech), takže celkový rozsah adres byl šestnáctibitový (0-65535), jako u osmibitových registrů. Procesor měl neobvyklé pouzdro DIL64 s rozestupy 0.9, což umožnilo tvůrcům vyvést všechny datové a adresové linky samostatně, bez multiplexování (jako bylo nutné u 8086). Zájemce o další vlastnosti odkážu na [TMS9900 Data Manual](http://bitsavers.trailing-edge.com/pdf/ti/TMS9900/TMS9900_DataManual.pdf).

TI-99/4A používal systém kartridží s pamětí a SW (hry, programy, Extended BASIC apod.). Kromě toho měl i rozšiřující port, kam bylo možno připojit nejrůznější periferie, od sériového modemu a řadiče FDD přes video výstup až po řečový syntezátor. Kromě portů pro kartridž a periferie obsahuje TI-99/4A konektor pro joystick, konektor pro monitor a konektor pro kazetový magnetofon.

Verze s &#8222;A&#8220; na konci byla inovovaná &#8211; obsahovala nový videořadič TMS-9918 (ano, ten, co byl pak i v [MSX](http://retrocip.uelectronics.info/msx/ "MSX")).

Počítač měl i zabudovaný BASIC, který představoval čisté peklo. Ne že by jeho syntaxe byla nějak výrazně špatná, to ne, ale jeho interpret byl napsaný &#8211; no, posuďte sami: Program v BASICu se neukládal do RAM (které měl tento stroj pouhých 256 byte &#8211; ne, to &#8222;kilo&#8220; mi opravdu nevypadlo!), ale do video RAM. Ta není namapovaná v adresovém prostoru, protože si ji obhospodařuje video řadič, přistupuje se k ní tedy jako k periferii, poeticky řečeno &#8222;jak když PMD sahá do ROM modulu&#8220;. Samotný interpret není napsaný ve strojovém kódu, ale v něčem, co se nazývá GPL, což je &#8222;strojový kód rozšířený o možnost transparentně přistupovat k různým typům paměti&#8220;. Instrukce GPL byly samozřejmě interpretovány GPL runtimem, a teprve ten byl napsaný ve strojovém kódu. Takže když si to shrneme: procesor, nad ním GPL runtime, v něm napsaný BASIC interpreter, a ten si tahá data z video RAM přes řadič videa. Představujete si výsledek? 🙂

TI-99/4A byl v USA poměrně úspěšný, ale pak, zjednodušeně řečeno, prohrál cenovou válku s Commodorem. Přesto vytvořil poměrně silný &#8222;kult&#8220; a zanechal mohutnou fanouškovskou základnu, která je aktivní dodnes.

## Parametry

  * procesor: TI TMS9900 na 3 MHz (16bit)
  * videoprocesor: TMS9918 (PAL verze TMS9929), 16 kB video RAM
  * video: různé módy, max. rozlišení 256 x 192; hardwarové sprity
  * zvukový procesor: TMS9919, později zvaný SN94624 a ještě později, při prodeji jiným odběratelům, doplněný děličkou hodin a přeznačený na SN76489
  * zvuk: 3 kanály, 5 oktáv; 1 šumový
  * paměť: 256 byte scratchpad RAM, 8 kB ROM; 18 kB GROM (Graphic ROM &#8211; ROM připojená jako periferie)

<div id='gallery-26' class='gallery galleryid-281 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_201716.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption275'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_201716-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_202019.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption277'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_202019-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_202045.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption276'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_202045-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_201741.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption274'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_201741-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_201827.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption280'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_201827-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_201923.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption279'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_201923-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_202006.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption278'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140507_202006-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style='clear: both' />
</div>