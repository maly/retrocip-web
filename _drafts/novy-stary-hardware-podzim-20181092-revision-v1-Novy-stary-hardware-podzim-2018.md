---
id: 1111
title: Nový starý hardware, podzim 2018
date: 2018-10-28T11:27:23+01:00
author: Martin Maly
layout: revision
guid: https://retrocip.cz/1092-revision-v1/
permalink: /1092-revision-v1/
---
Práce na knize jde zdárně do finále, a já k tomu připravuju a testuju hardware. Po [OMEN Alpha](https://retrocip.cz/tag/omen-alpha/) přišlo na řadu Bravo, Charlie i Kilo. A k tomu taky nějaké ty periferie&#8230;

<!--more-->

První verze Brava měla EEPROM 32 kB a sériový port. Konečně dorazila nová verze, Issue 3, kterou jsem rozšířil o obvod VIA 6522. VIA obsahuje dva osmibitové paralelní porty, dva časovače a posuvný registr pro sériové rozhraní. Jenže Issue 3 nepremával. Vypadlo mi propojení &#8222;země na zem&#8220; a v tom blbém Eaglu jsem to přehlédnul jak šíré lány.

Když jsem napravil propojení, zjistil jsem, že nepracuje. Nekmitá. Sakra. Zapojení, co doporučovali po internetech, se u čtyřmegového 65C02P4 ukázalo nefunkčním. Musel jsem přidat jeden kondenzátor a rezistor 180k, a najednou to kmitá a šlape na první dobrou. Posuďte sami:



Opravil jsem chyby, vynechal výběr banky  (Issue 3 a 4 už používají menší EEPROM 28C64) a zadal jsem výrobu desek pro novou verzi. Ale přemýšlím, že bych zájemcům nechal i verzi 3, s nějakou slevou za to, že si zapájí drátek a dvě součástky&#8230; Škoda to vyhodit, ne?

<div id='gallery-44' class='gallery galleryid-1111 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103913-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1097'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103913-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_104048-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1105'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_104048-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103935-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1096'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103935-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_104029-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1093'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_104029-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_104015-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1094'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_104015-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_104004-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1095'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_104004-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
</div>

**OMEN Kilo** je takový chudák otloukánek. Moc jsem se na něj těšil, plošný spoj funguje na první dobrou, ale jinak to byl Problém Sám. Nejprve vadné procesory 63B09P. Nekmitaly, nehrály, mrtvé. Tak jsem koupil 63C09P, ty už fungují. A taky pár 6809, co taky fungují.

Jenže jsem musel vyměnit krystal, protože ten, co jsem tam dal, byl co? Správně, mrtvý!

Pak nefungoval obvod ACIA 6850. Čert aby to spral.

A když už to všechno mělo fungovat, tak se ukázalo, že ty přepínače, co jsem tam jako frajer dal místo pinů a switchů, mají nějaký divný zkrat, takže v jedné pozici povolovaly zápis do EEPROM (a ta se vesele přepisovala), v druhé blokovaly zpápis do RAM. Domluvil jsem jim kombinačkama a v příští verzi už nebudou!

A pak už to všechno fungovalo. Dokonce jsem si napsal jednoduchý monitor&#8230;

> _Vlastně jediné, co mi teď chybí, je monitor pro Bravo. Používám c&#8217;mon, ale moc nadšený z něj nejsem. Nechcete někdo napsat lepší? Nehlásí se támhle pan Šlajs? 🙂_

Ale jinak mám Kilo moc rád a do budoucna se stane základem většího systému.

Pokud vám vrtá hlavou, k čemu že tam mám ten systémový konektor, tak vězte, že k připojování dalších periferií. Zatím jsou připravené následující kousky:

  * Porty (s ATMegou)
  * OMEN PIA s obvodem 6821 (jako výše zmíněná VIA, ale bez posuvného registru)
  * OMEN CF IDE pro připojení Compact Flash karty
  * OMEN Terminal s 20 tlačítky a sedmisegmentovým displejem (ale připojuje se na paralelní porty)

<div id='gallery-45' class='gallery galleryid-1111 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103813-1280-1.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1107'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103813-1280-1-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103724-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1102'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103724-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103731-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1101'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103731-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103835-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1099'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103835-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103843-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1098'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103843-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103653-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1104'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103653-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103705-1280.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption1103'})"><img src="http://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103705-1280-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style='clear: both' />
</div>

Ještě mám v plánu přidat zvukové rozhraní, jednodušší mono se SN76489 a sofistikovanější se SAA1099, ale možná si to ještě rozmyslím a sáhnu rovnou po OPL. No a pak věc, co mi teď asi chybí nejvíc, a to jsou microdrive&#8230;

Cože?

Ne, dělám si legraci. Microdrive nechci, ale mám tu spoustu SPI FLASH pamětí a ty by se hodily jako hezké médium pro ukládání software. Musím to ještě nějak vymyslet. Nechce se mi dělat interface s vlastním procesorem, tak to možná nechám jen na paralelním portu&#8230;

No a poslední úprava, co jsem dělal, je úprava monitoru MON85 pro OMEN Alpha. Ukázalo se totiž, že ACIA má takovou zajímavou vlastnost, a to tu, že příznak &#8222;jsou připravena načtená data&#8220; se vynuluje i při zápisu. Takže během posílání dat, třeba u funkcí D a M, kdy monitor chrlí data a zastaví ho až mezera nebo Esc, nemá moc šanci tu mezeru nebo ESC chytit, protože tím, jak data posílá, zároveň zahazuje přijatá. Dodělal jsem hotfix, ale není to úplně to ono.