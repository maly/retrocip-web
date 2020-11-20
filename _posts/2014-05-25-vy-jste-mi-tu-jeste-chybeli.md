---
id: 307
title: Vy jste mi tu ještě chyběli!
date: 2014-05-25T20:03:29+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=307
permalink: /vy-jste-mi-tu-jeste-chybeli/
dsq_thread_id:
  - "2712097627"
image: https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180711-1140x198.jpg
categories:
  - Hardware
tags:
  - "6502"
  - Acorn
  - ORIC
  - Tangerine
---
Ne, vážně: tentokrát o dvou počítačích, co byly v zemi původu oblíbené, ale u nás by se jejich majitelé dali tehdy spočítat na prstě jedné ruky&#8230;

<!--more-->

## Acorn Electron

Značku Acorn jsme znali i tady. Byl to přeci ten zlý, co Sinclairovi vyfouknul velkou zakázku na výukové počítače (ostatně vznikl o těchto dvou firmách a jejich souboji i [film](https://www.youtube.com/watch?v=sIcAyFVK0gE)). Byla to firma, co vyráběla BBC Micro, školní počítače, které se staly v Británii velmi populární. Založil ji (a ostatně &#8211; jako spoustu britských počítačových firem té doby) vývojář od Sinclaira Chris Curry, který se se Sirem Clivem nerozešel moc v dobrém.

Poté, co Acorn uspěl ve výběrovém řízení a začal dodávat BBC Micro, byl brán jako jednoznačný Sinclairův rival a čekalo se, jak v roce 1982 odpoví na oznámení barevného ZX Spectra. Curryho tým odpověděl počítačem s 32 kB RAM, 32 kB ROM, rovněž barevným a s cenou o něco málo nižší než mělo mít Spectrum (což se nakonec nepodařilo). Navíc kompatibilním s BBC Micro. Což byl pochopitelný obchodní tah &#8211; zacílit na stejnou skupinu, která by kupovala Spectrum, a nabídnout jim srovnatelný počítač, ale kompatibilní s tím, co už znali ze školy a z televizních kurzů programování.

Nejdůležitější faktor, který umožnil snížit cenu, bylo snížení počtu součástek. Použili k tomu stejný trik jako Sinclair, totiž zákaznický čip ULA od Ferranti. A podobně jako Sinclair použili drobný trik u pamětí. Zatímco Sinclair použil &#8222;vadné&#8220; 64kbit čipy, Acorn pro svých 32 kB RAM použil správné 64kbit čipy, ale jen čtyři (viz [schéma](https://www.acornatom.nl/hardware/Electron-iss4.png)), takže se k obsahu přistupovalo vlastně nadvakrát (o což se starala taky ULA). To ve výsledku vede ke známému paradoxu dvojí rychlosti počítače Electron, která záleží na tom, jestli se čte z RAM, nebo z ROM. (Plus navíc je tu i efekt zpomalování video RAM, jako u Spectra, takže při nejhorších podmínkách je reálná rychlost zhruba čtvrtinová.)

ULA se dál starala především o zobrazování. V tom největším rozlišení, 640 x 256, 2 barvy, si obrazovka zabrala 20 kB (z celkových 32). Kromě videa a správy dynamické paměti se starala i o zvuk, komunikaci &#8211; zkrátka o všechno, co nebyl procesor 6502A. ULA byla ve čtvercovém pouzdru 30 x 30 milimetrů a měla 68 vývodů. Mimochodem, Curry zmiňuje právě obvody ULA jako hlavní důvod, proč nevznikly pirátské kopie těchto počítačů, jako se to stalo například Applu.

<img loading="lazy" class="aligncenter size-medium wp-image-303" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/05/IMG_20140525_180702-650x365.jpg" alt="IMG_20140525_180702" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180702-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180702-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Uvedení počítače Acorn Electron na trh doprovázely problémy. Jednak cena byla vyšší než původně slibovaná, jednak kompatibilita nebyla stoprocentní, navíc byl počítač výrazně pomalejší než BBC Micro (důvodem byla už zmíněná organizace paměti). Přesto všechno zlé je, jak se říká, k něčemu dobré: Acorn měl problémy s dodanými obvody ULA, jeden z manažerů později uvedl, že &#8222;fungoval jeden z deseti&#8220;, což vedlo k jednání s firmou VLSI Technology o vytvoření CMOS ekvivalentu obvodu ULA.

Spolupráce Acornu s VLSI Tech byla zjevně plodná, protože později, když inženýři Acornu navrhli počítač Archimedes, tak jim VLSI Technology dodával speciální procesor, nazývaný tehdy Acorn RISC Machine &#8211; ve zkratce ARM. Ano, je vám to povědomé správně.

Ale Archimedes, to už je jiný příběh&#8230;

(Pro zájemce &#8211; [historie Electronu](https://www.theregister.co.uk/2013/08/23/acorn_electron_history_at_30/?page=1))

<img loading="lazy" class="aligncenter size-medium wp-image-297" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/05/IMG_20140525_181308-650x365.jpg" alt="Acorn Electron" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181308-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181308-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

## ORIC-1

Zatímco Acorn jsme tu aspoň trošku znali, jméno Tangerine Computer Systems znělo povědomě snad jen těm největším fandům. Možná někdo někde zahlédl název ORIC, nebo fotografii&#8230;

<img loading="lazy" class="aligncenter size-medium wp-image-291" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/05/IMG_20140512_175155-650x365.jpg" alt="IMG_20140512_175155" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140512_175155-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140512_175155-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Přitom první počítač této firmy, nazvaný ORIC-1 a uvedený v roce 1983, rozhodně měl šanci zaujmout. Dodával se ve verzi s 16 kB RAM nebo s 48 kB RAM, uvnitř ho poháněl osvědčený 6502A, taktovaný na 1 MHz. Na rozdíl od Spectra 48 obsahoval speciální zvukový čip, kterým nebyl nikdo jiný než známý AY-3-8912 (později použitý ve Spectru 128). Samozřejmostí je zabudovaný BASIC&#8230;

I v ORICu byl použit zákaznický obvod ULA, který se staral o zobrazování ve dvou grafických módech, LORES a HIRES. Grafika nepoužívala &#8222;atributy&#8220;, známé ze Spectra &#8211; byla svérázná jiným způsobem. Představte si, že máte textový displej 40 x 28, a jemu odpovídající oblast v paměti. Na každou pozici můžete zapsat buď znak v ASCII ($20-$7F), nebo jeho invertovanou podobu ($A0-$FF), anebo nějaký řídicí znak, například změnu barvy popředí ($00-$07), změnu barvy pozadí ($10-$17), změnu znakové sady ($08-$0F) nebo změnu grafického módu ($18-$1F). Což vede k mnoha zajímavým omezením, ale i k nečekaným možnostem (více viz [Oric Coding: Video display](https://www.defence-force.org/computing/oric/coding/part_7/index.htm)).<figure id="attachment\_293" aria-labelledby="figcaption\_attachment_293" class="wp-caption aligncenter" style="width: 660px">

<img loading="lazy" class="size-medium wp-image-293" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/05/IMG_20140525_181540-650x365.jpg" alt="Všimněte si obalu kazety dole - výrobce Durell. Pamatujete na úvodní hlášení v hrách téhle firmy?" width="650" height="365" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181540-650x365.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181540-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> <figcaption id="figcaption\_attachment\_293" class="wp-caption-text">Všimněte si obalu kazety dole &#8211; výrobce Durell. Pamatujete na úvodní hlášení v hrách téhle firmy?</figcaption></figure> 

Tangerine připravili vylepšené verze Atmos, Stratos a Telestratos, ale nakonec i je dostihl konec osmibitové éry. Jako zajímavý moment dodám, že firma vyvezla zhruba tisíc počítačů Atmos do tehdejší Jugoslávie, a že v Bulharsku vyráběli klon Atmosu pod označením Pravec 8D.

Další historie viz článek [Oric-1 is 30](https://www.theregister.co.uk/2013/01/28/the_oric_1_is_30_years_old/?page=1).

Každopádně jsem rád, že oba tyhle, v ČR poměrně unikátní, stroje mám.

<div id='gallery-5' class='gallery galleryid-307 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180648.jpg" title="Acorn Electron" class="highslide" onclick="return hs.expand(this,{captionId:'caption304'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180648-150x150.jpg" width="150" height="150" alt="Acorn Electron" /></a>
    </dt>
    
    <dd class="gallery-caption" id="caption304">
      <span class="imagecaption">Acorn Electron</span><br />
    </dd>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180702.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption303'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180702-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181204.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption298'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181204-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180711.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption302'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180711-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180753.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption301'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_180753-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181141.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption299'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181141-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181308.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption297'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181308-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140512_175155.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption291'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140512_175155-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140512_175200.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption290'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140512_175200-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140512_175210.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption292'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140512_175210-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181540.jpg" title="Všimněte si obalu kazety dole - výrobce Durell. Pamatujete na úvodní hlášení v hrách téhle firmy?" class="highslide" onclick="return hs.expand(this,{captionId:'caption293'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140525_181540-150x150.jpg" width="150" height="150" alt="Všimněte si obalu kazety dole - výrobce Durell. Pamatujete na úvodní hlášení v hrách téhle firmy?" /></a>
    </dt>
    
    <dd class="gallery-caption" id="caption293">
      <span class="imagecaption">Všimněte si obalu kazety dole - výrobce Durell. Pamatujete na úvodní hlášení v hrách téhle firmy?</span><br />
    </dd>
  </dl>
  
  <br style='clear: both' />
</div>