---
id: 682
title: FPMI-80
date: 2016-01-28T00:26:49+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/602-autosave-v1/
permalink: /602-autosave-v1/
---
Tím názvem si nejsem ještě moc jistý, ale nějak tak by to mohlo být. Je to zkrátka PMI-80 ve FPGA.

<!--more-->

Moje hraní s [FPGA kity](http://fpga.cz) a s jazykem [VHDL](http://vhdl.cz) nemohlo zůstat bez následků. Navíc jsem tu už psal o tom, že jsem [sehnal hezký bublinový displej](http://retrocip.uelectronics.info/jednodeskac-za-ktery-me-milovnici-jednodeskacu-prokleji/) a &#8222;vyntyč&#8220; klávesnici. Sice původní myšlenka byla využít ATMegu a nějak to spájet, ale když mám ten [levný kit](http://fpga.cz/altera-kity-ide-programator/)&#8230;

Výchozí situace byla takováto: Mám [klávesnici bez popisků](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=2&pub=5575085282&toolid=10001&campid=5337554641&customid=&icep_item=130541363513&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg), tak jsem si udělal sadu popisků. 0-9 a A-F zůstanou stejné, no a ten zbytek budu moct prohodit třeba za KIM-1 nebo TEMS nebo BOB nebo &#8211; prostě tak!

<img loading="lazy" class="aligncenter size-medium wp-image-604" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/07/IMG_20150725_132927-650x366.jpg" alt="IMG_20150725_132927" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_132927-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_132927-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Na eBay jsem sehnal za levno dva displeje HP 5082-7441, což je něco jako CQYP95 nebo NSA1198, zkrátka devítimístný LED displej. Originální VQD30 není, ale [i jiní nahrazují](http://www.nostalcomp.cz/pmi80_nahrkey.php)&#8230;

Klávesnici jsem chtěl původně spájet do matice 3&#215;8 plus jedno dedikované tlačítko pro RESET, ale pak jsem si řekl: Proč? Vždyť můžu nechat tu matici 5&#215;5 a takhle to přivést do FPGA! A dokonce i bez další bižuterie, protože pull-upy mi udělá FPGA.

Jak s displejem? No, originál to řeší tranzistorovými budiči a dekodérem, já to (zase) jako to prase prostě připájel 1:1 na vývody FPGA (dá se omezit proud, co jimi teče) a budím to přímo 3.3V logikou. _Poznámka pro zneklidněné čtenáře: To víte že tam jsou odpory!_

Chvíli jsem si pálil prsty pájkou (mentální poznámka: Musím si přivézt svěrák!) a pak to připojil ke kitu. Je pátek, čtyři hodiny odpoledne.

První test bylo jen blikání segmenty. Já vám řeknu, ta radost, když to šlapalo&#8230;

<img loading="lazy" class="aligncenter size-medium wp-image-615" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/07/IMG_20150725_095426-650x366.jpg" alt="IMG_20150725_095426" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_095426-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_095426-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Fajn, co dál? Šel jsem si [na Nostalcomp pro moudra](http://www.nostalcomp.cz/pmi80.php) (schéma, monitor atd.) a udělal postupně: RAMku, ROMku (napsal jsem si převodník z .bin do .mif) a CPU.

A tady začalo mé trápení. Použil jsem nějaké VHDL jádro 8080, co je mikroprogramované a malé. Perfektní, hurá, ale neseje to.

O krok dál: udělal jsem si modul ve VHDL, který má 3 vstupy: data1, data2 a adresa (8, 8 a 16 bitů) a výstup jako hexadecimální znaky na ten LED displej. Jo a podtaktoval jsem 8080 na 2Hz, abych to viděl prostým okem. Modul připojený na adresovou sběrnici, datovou sběrnici, na výstup z ROM, a jedeme. Syntéza, nahrání, sledovat čísla. Syntéza, nahrání, sledovat čísla. A furt dokola.

Nic. CPU na &#8222;test NOPem&#8220; běží. Bohužel, běží úplně stejně i s reálnými instrukcemi.

Sedmá večer. Zkouším použít jinou 8080. Pak ještě Z80. Pak zase zpátky tu původní, zkouším laborovat s nestandardním signálem VMA, a tu náhle&#8230; Instrukce skoku vyvolá změnu adresy, jupí! Takže jádro procesoru pracuje!

Devět večer. Už to nějak proběhne, takže implementuju 8255 a spouštím&#8230; a nic!

Znovu laboruju, kolem jedenácté zjišťuju, že problém je jinde. Totiž &#8211; nevrátí se mi procesor z podprogramu a místo toho někde bloumá. Podstrkuju mu vlastní testovací kód, a zjišťuju, že nějak nefunguje zápis do RAMky, nebo čtení z RAMky. Respektive funguje buď jedno, nebo druhé.

Dobře, místo modulu RAM používám svou implementaci polem. Výsledek je furt stejný. Le frustrace!

Zapojuju do celého obvodu skvělou Alteráckou funkci System-in Probe a čtu signály. Hm, hm, je to velmi divné. Simuluju jádro toho procesoru v ModelSIM. Aha, aha&#8230; čtení je posunuto proti datasheetu o jeden takt, a v tu chvíli už je na datové sběrnici něco jiného, protože to nedrží RD&#8230; ááááá&#8230;

Jdu spát. Ještě před usnutím si říkám, že to celé zahodím a postavím to na [podvozku od Granta Searla](http://searle.hostei.com/grant/Multicomp/index.html).

V sobotu v osm ráno zapínám Quartus, nahrávám Grantův Multicomp, nechávám v něm jádro Z80 a zkusím si spustit ten testovací kód. Něco je velmi, velmi blbě! Neběhá ani ten Grantův stroj. Nepomáhá nic. Ještě zkusím 6502 &#8211; a výsledek je tristní: Procesor se proměnil v šestnáctibitový čítač a čítá od nuly!

Strašný. V poledne opouštím tuhle neplodnou linii, vracím se zpátky ke včerejším experimentům a v návalu osvícení nahrazuju RAMku dvouportovou. Když není jádro procesoru schopné udržet adresu ještě ve chvíli, kdy čte, tak to musím vymyslet nějak jinak. A dvouportová s latchovanou adresovou sběrnicí pomohla. Ve 12:17 to vypadalo, že co do RAMky PUSHnu, to z ní taky POPnu!

Ve 12:25 se poprvé objevil známý nápis.  
<img loading="lazy" class="aligncenter size-medium wp-image-616" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/07/IMG_20150725_122339-650x366.jpg" alt="IMG_20150725_122339" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_122339-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_122339-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Jo, je to pozpátku, ale to je nejmenší detail! Stačí obrátit pořadí pozic.

Napsal jsem modul na obsluhu matice kláves. S kmitočtem 50 kHz scannuje řádky a výsledek ukládá do pole 25 bitů. Co klávesa, to jeden bit. No a za tenhle modul jsem si napsal modul, který těch 25 vodičů přemapoval na signál Reset, na signál Int a na tři výstupní bity (multiplexované z čtyř vstupních). Připojil jsem to na virtuální 8255 a &#8211; nic!

Nepřekvapilo mě to. A tak jsem zase experimentoval s hexadecimálním displejem (klávesnice se ukázala jako naprosto perfektní, šlapala na první dobrou), 2Hz procesorem, logickou virtuální sondou, a tak, viz výše. Když jsem vyloučil různé jiné příčiny (nefunkční 8255 a tak), tak jsem šel už najisto: Bylo to tak! Jednak vznikaly úplně divné PIOCS, jednak adresa držela, ale procesor poslal IORD (tak mu PIO dalo data), ale přečetl si je až v dalším taktu (a na něj si IORD shodil, takže na sběrnici bylo zase velké nulové).

Vyhodil jsem Grantův dekodér sběrnice (kombinační) a udělal ho podle sebe, procesem a s datovým latchem, spouštěným signálem RD. Test s logickou sondou ukázal, že data drží&#8230; takže&#8230; takže&#8230;?

Nahodil jsem plnou ROM, přepnul hodiny zpátky na plný kotel, přeložil, nahrál, objevilo se &#8222;PMI -80&#8220;, já stisknul klávesu &#8211; a byl tam! Otazníček maličký&#8230;

<img loading="lazy" class="aligncenter size-medium wp-image-606" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/07/IMG_20150725_160822-650x366.jpg" alt="IMG_20150725_160822" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160822-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160822-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Protože si pamatuju, jak jsem psal emulátor a jaké mrzení bylo s mapováním kláves (jo, to schéma, co je na internetech, není moc dobré a občas člověk netuší, jestli je to klávesa 6, 8 nebo B), tak jsem ani nedoufal, že by to snad mohlo fungovat napoprvé.

A víte vy co? Fungovalo!

<img loading="lazy" class="aligncenter size-medium wp-image-607" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/07/IMG_20150725_160830-650x366.jpg" alt="IMG_20150725_160830" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160830-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160830-1024x576.jpg 1024w" sizes="(max-width: 650px) 100vw, 650px" /> 

Bylo 16:16, a já si vzal do pravé ruky mobil s kamerou, levou jsem naťukal první program&#8230;



Trvalo to nějakých 15 hodin, ale mám funkční VHDL repliku PMIčka. Ještě bych mohl pořešit load a save (třeba sériovým portem a přes USB do PC, ne?), ale hardware funguje. Až to trochu dočistím a vyhezkám, tak hodím zdrojáky ven&#8230;

[sc:ebay item=&#8220;mini system CycloneII EP2C5T144&#8243;] [s

<div id='gallery-34' class='gallery galleryid-682 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_095426.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption615'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_095426-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_122339.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption616'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_122339-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_122539_edit.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption617'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_122539_edit-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160924.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption613'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160924-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160822.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption606'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160822-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160830.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption607'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160830-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160837.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption608'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160837-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160840.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption609'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160840-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160842.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption610'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160842-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160847.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption611'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160847-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160851.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption612'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160851-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160820.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption605'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_160820-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_132940.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption618'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_132940-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_132927.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption604'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2015/07/IMG_20150725_132927-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style='clear: both' />
</div>