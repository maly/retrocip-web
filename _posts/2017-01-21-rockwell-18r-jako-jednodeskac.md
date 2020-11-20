---
id: 932
title: Rockwell 18R jako jednodeskáč
date: 2017-01-21T13:21:07+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=932
permalink: /rockwell-18r-jako-jednodeskac/
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image005-450x198.jpg
categories:
  - Hardware
tags:
  - "6502"
  - kalkulačka
---
Ani nevím, co mě to popadlo a jak mě to napadlo, ale jednoho dne jsem si řekl: Když je PMI-80 postavené na klávesnici a displeji ze staré kalkulačky, tak proč by nešlo vzít starou kalkulačku s LEDkovým displejem a klávesnicí, a zabudovat dovnitř PMI-80? No dobře, tak ne přímo PMI, ale třeba něco menšího, jiného, KIM-1 třeba&#8230; A začal jsem se porozhlížet po starých vrakových kalkulačkách.

Osud mi jako první přihrál Rockwell 18R. Zas taková vzácnost to není, takže mě moc netrápí, že ji předělám. Ale má krásný &#8222;bublinkový&#8220; displej, 8 pozic, a klávesnici 5&#215;4. Teď jen aby to nebylo uvnitř nějaké divoké&#8230;<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image007.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-937" src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image007-366x650.jpg" alt="" width="366" height="650" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image007-366x650.jpg 366w, https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image007.jpg 450w" sizes="(max-width: 366px) 100vw, 366px" /></a>

Stačilo odšroubovat čtyři šroubky, a ukázaly se vnitřnosti v plné kráse. Nádhera! Klávesnice zapojená jako ta nejprostší matice, dokonce i fyzicky odpovídalo umístění vývodů, a k tomu jeden plošňák, který je seskládal hezky dohromady. A nad tím vším malá destička s displejem a něčím, co vypadalo jako konektor.

Hledal jsem, kde je procesor, a při bližším zkoumání se ukázalo, že to, co vypadalo jako konektor, je plastová krytka. A pod ní byl čtvercový čip, a k němu byly tenkými drátky připájené vývody.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image001.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-936" src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image001-650x366.jpg" alt="" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image001-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image001-768x432.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image001.jpg 800w" sizes="(max-width: 650px) 100vw, 650px" /></a>

Takže &#8211; devět vývodů od klávesnice (5 řad, 4 sloupce), 16 vývodů od displeje, navíc s krásnými pájecími ploškami &#8211; těch 8 nalevo jsou jednotlivé segmenty, a po desce jsou pak porůznu rozmístěné katody pro jednotlivé pozice.

Takže stačí odpojit ten čip (bohužel, nebyla to 6502, jak jsem trošku doufal, ale nějaký specializovaný obvod, jak jinak), a připojit displej a klávesnici k vlastní konstrukci.

Asi nejjednodušší způsob bude použít nějaký malý jednočip na desce. Pokud slyšíte &#8222;Arduino Nano&#8220;, slyšíte správně &#8211; mám jich tu haldy, tak proč je nepoužít, že? Jen mám trošku problém. Počítejte se mnou:

Displej má 16 vývodů. 8 segmentů + 8 pozic. Klávesnice 9. To máme dohromady 25 vývodů. Arduino Nano 328 má&#8230; počkejte&#8230;

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/arduino-nano-atmega328-pinout.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-940" src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/arduino-nano-atmega328-pinout-650x461.png" alt="" width="650" height="461" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/arduino-nano-atmega328-pinout-650x461.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2017/01/arduino-nano-atmega328-pinout.png 707w" sizes="(max-width: 650px) 100vw, 650px" /></a>

Jestli dobře vidím, tak 14 digitálních a 8 analogových, tedy 22 celkem, a z toho A6 a A7 víceméně docela pofidérní &#8211; jako vstupy maximálně. Hm, hm, hm. Co s tím? Když si řeknu, že třeba KIM-1 používal jen šest pozic, ušetřím 2 vodiče, takže mi jich stačí zapojit 23. Což je pořád o jeden víc, než jich mám k dispozici.

Proto použiju stejný trik, jako byl v tom PMI-80 (a nejen tam). Využiju toho, že displej pravidelně občerstvuju, pozici po pozici, a připojím řádky klávesnice na pět pozic displeje. A protože to jsou katody, budou vlastně &#8222;spínané k nule&#8220;. Což se mi hodí, protože připojím sloupce klávesnice přímo na digitální vstupy a nastavím jim interní pullupy. Takže ve výsledku to máme: 8 segmentů + 8 pozic (z toho 5 řádkových vodičů) + 4 sloupcové = 20 vývodů. A jsme na svých. Trošku harakiri může nastat na TX/RX, když do toho bude zvenčí pindat USB/UART převodník, ale uvidím, jak se to v praxi vyvrbí. Přinejhorším obětuju dvě pozice.

Klávesnice má 20 tlačítek. 16 jich nechám pro hexadecimální znaky, zbývají 4. Což je pro KIM-1 málo, tak to budu muset vymyslet nějak chytřeji. Možná se inspiruju u [Heathkitu ET3400](https://www.old-computers.com/museum/computer.asp?c=785&st=1) &#8211; ten si vystačil s 16 klávesami a RESETem a klávesy měly vždy dvě funkce.

A kdyby mě snad jednodeskáč znudil, tak vždycky můžu nasadit firmware z [Calcuina](https://retrocip.cz/calcuino-1/).



<div id='gallery-11' class='gallery galleryid-932 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image005.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption939'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image005-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image006.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption938'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image006-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image007.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption937'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image007-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image001.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption936'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image001-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image002.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption935'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image002-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image003.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption934'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image003-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image004.jpg" title="Rockwell 18R" class="highslide" onclick="return hs.expand(this,{captionId:'caption933'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2017/01/image004-150x150.jpg" width="150" height="150" alt="Rockwell 18R" /></a>
    </dt>
    
    <dd class="gallery-caption" id="caption933">
      <span class="imagecaption">Rockwell 18R</span><br />
    </dd>
  </dl>
  
  <br style='clear: both' />
</div>