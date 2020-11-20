---
id: 765
title: Retročipení v březnu
date: 2016-03-28T13:18:28+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=765
permalink: /retrocipeni-v-breznu/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "4699548337"
image: https://retrocip.cz/wp-content/uploads/sites/6/2016/03/10407032_10153515386967496_8320520211423998695_n-960x198.jpg
categories:
  - Hardware
tags:
  - arduino
  - Commodore
  - Didaktik
  - displej
  - klávesnice
  - PS/2
  - Roboti
  - servo
  - Sinclair
---
Ruka, oko, noha&#8230; Tedy pardon: výstava, hraní s displejem a hraní s klávesnicí.

<!--more-->

V Praze se konala konference Devel a já byl požádán nejmenovaným operátorem O2, zdali bych pro ně mohl na tuto akci připravit něco, co by spojovalo starou a novou techniku. Chvilku jsme si ujasňovali, co by to asi tak mělo být, nápad pustit mezi geeky drona, řízeného starým Commodorem, jsem zavrhl (dovedete si představit ten žižkovský masakr roztočenou vrtulí?) a nakonec jsme se shodli na robotické ruce, řízené starým Didaktikem.

Jenže času nebylo nazbyt, ruka se stále ne a ne objevit, a pak se objevila. Přiložené Botboarduino bylo skvělé, ale bohužel nereagovalo na jakékoli pokusy o naprogramování. Čas se chýlil a chýlil&#8230; Narychlo jsem koupil servo shield od Adafruit a přidal vlastní Arduino. [sc:ebay item=&#8220;Adafruit 16-Channel 12-bit PWM Servo shield I2C&#8220;] 

Pak už se ruka hýbala. Bohužel jsem si neuvědomil, že při prvním spuštění, když se serva dostávají do definované polohy, sebou tak jako mocně škubnou, navíc v tom robotu jich je hned několik, a hodně silných, takže ta škuba byla opravdu velkolepá, a já dostal poprvé ve svém životě přes hubu od robota&#8230; Naštěstí opět zvítězil lidský intelekt nad hrubou silou, a pak už jen poslušně mávala a manipulovala.

Připojení k Didaktiku bylo pak docela prosté. Didaktik má vyvedený interface od 8255. Dva piny (PC4 a PC7, kdyby vás to zajímalo) jsem vyhradil pro komunikaci s Arduinem. Po jednom jde čas, po druhém data, a víceméně jsem simuloval něco jako SPI. A jelikož bylo málo času na rozchození, tak jsem to z Didaktika pouštěl v BASICu. Nahrubo. OUT, OUT, OUT&#8230; a jedem.

Šlo by to určitě i jinak. Třeba z toho Didaktika rovnou ovládat servo shield. A přepsat rutiny do strojáku. Jasně že by to všechno šlo, jenom ten čas se chýlil a chýlil a kvačil&#8230; Ale nakonec to dobře dopadlo.

Na akci jsem přivezl i několik starších kousků, zapojil jsem je k televizím a nechal jsem návštěvníky, ať si hrají.

<div id='gallery-10' class='gallery galleryid-765 gallery-columns-3 gallery-size-thumbnail gallery1'>
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/600684_10153515387422496_4756415373634951829_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption772'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/600684_10153515387422496_4756415373634951829_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/12523892_10153515387477496_7505419680903680049_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption773'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/12523892_10153515387477496_7505419680903680049_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/10387702_10153515387367496_7573605468496318499_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption771'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/10387702_10153515387367496_7573605468496318499_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/10407032_10153515386967496_8320520211423998695_n.jpg" title="Retročipařská výstavka na Devel 2016" class="highslide" onclick="return hs.expand(this,{captionId:'caption766'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/10407032_10153515386967496_8320520211423998695_n-150x150.jpg" width="150" height="150" alt="Retročipařská výstavka na Devel 2016" /></a>
    </dt>
    
    <dd class="gallery-caption" id="caption766">
      <span class="imagecaption">Retročipařská výstavka na Devel 2016</span><br />
    </dd>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/12189961_10153515387057496_515566159307672223_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption767'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/12189961_10153515387057496_515566159307672223_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/970469_10153515387212496_5793808113455511761_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption768'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/970469_10153515387212496_5793808113455511761_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/1924004_10153515387277496_7446025834420251948_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption769'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/1924004_10153515387277496_7446025834420251948_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/12143064_10153515387312496_340425830953014266_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption770'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/12143064_10153515387312496_340425830953014266_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/12107773_10153515387612496_8722114112772120592_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption774'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/12107773_10153515387612496_8722114112772120592_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" />
  
  <dl class="gallery-item">
    <dt class="gallery-icon">
      <a href="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/10612673_10153515387652496_502823875489423105_n.jpg" title="" class="highslide" onclick="return hs.expand(this,{captionId:'caption775'})"><img src="https://retrocip.cz/wp-content/uploads/sites/6/2016/03/10612673_10153515387652496_502823875489423105_n-150x150.jpg" width="150" height="150" alt="" /></a>
    </dt>
  </dl>
  
  <br style='clear: both' />
</div>

&#8212;

S Michalem Průšou jsme před časem řešili problém, jak předejít rušení na přívodech k displeji u 3D tiskárny. Displej je klasický 20&#215;4 s HD řadičem, a je řízený pomocí čtyřbitového rozhraní. [sc:ebay item=&#8220;20&#215;4 display module backlight&#8220;] 

Bohužel, ty čtyři bity (a další řídicí signály) jsou vedené plochým kabelem asi 30 cm na délku, a to je docela dost. Funguje to, ale ne stoprocentně. Lepší by bylo nějaké sériové vedení signálu&#8230; Michal navrhoval nějaký posuvný registr s latchem, třeba 74HC595, já navrhoval použít speciální řešení s PCF8574. [sc:ebay item=&#8220;display I2C 1602 2004 module&#8220;] 

Teď před víkendem přišel, a kupodivu fungoval na první zapojení. S drobnou výhradou teda &#8211; nejdřív jsem myslel, že nefunguje, ale stačilo poštelovat trimr, který řídí kontrast, a hned bylo dobře&#8230;

&#8212;

A když už jsem měl u Arduina ten hezký displej, tak jsem si vzpomněl na svou letitou ideu udělat nějaký retrohold starým kapesním počítačům SHARP PC, kde byla QWERTY klávesnice, dvou- nebo čtyřřádkový LCD displej a zadrátovaný BASIC, a hleděl jsem připojit PS/2 klávesnici k Arduinu. [sc:ebay item=&#8220;ps2 keyboard module&#8220;] 

Ne že by to nešlo. Jde to. Ne že by to nikdo nezkoušel. Zkoušeli to. Ale ta knihovna, kterou jsem našel a která se hojně používá, má takový jeden nešvar: Moc to neumí. Snaží se překódovat scankódy na ASCII, ale trošku to dře.

Totiž, pokud nevíte, jak taková PS/2 klávesnice komunikuje, tak vězte, že to není moc hezké. Připojené to je přes dva signály &#8211; hodiny a data. To je OK. Hodiny řídí klávesnice. To už není tak OK, protože to znamená, že buď musíte obětovat jeden vstup se schopností vyvolat přerušení při sestupné hraně, nebo musíte neustále sledovat dění na pinu a ztrácíte tak výkon&#8230; Jo a ten protokol, co vám jde, tak tam samozřejmě jdou jen scankódy, tedy vlastně jakási čísla kláves. Jeden scankód přijde, když je klávesa stisknutá, druhý, když ji pustíte. To, jaké znaky a významy jim přiřadíte, je už zcela na vás.

No a jestli si myslíte, že LEDky blikají díky magii a že třeba Caps Lock si klávesnice rozsvítí sama poté, co zmáčknete tlačítko Caps Lock, tak jste na omylu, protože LEDky řídí počítač pomocí příkazů, co do klávesnice posílá. Nejdřív si sebere oba signály pro sebe, pak pošle definovanou sekvenci, která řekne klávesnici, že následuje příkaz, a pak pustí hodiny (=nastaví opět na příjem). Klávesnice začne generovat hodinové pulsy, vy po datovém výstupu posíláte data, po devíti bitech (8+lichá parita) přepnete data na vstup, počkáte si na potvrzení, a pak vám klávesnice pošle na oplátku informaci o tom, že rozuměla.

Což skvěle fungovalo do soboty, 15:00. LEDky blikaly, klávesnice odpovídala jak má, tedy 0xFA, jako že rozuměla, a pak jsem zkusil poslat dvě změny LEDek po sobě. A najednou se to celé úplně&#8230; napsal bych rozbilo, ale to je málo pro to, co se stalo. Ono se to celé rozesralo! Příjem od klávesnice byl o bit posunutý, klávesnice najednou vracela úplně nesmyslné kódy, na příkazy reagovala statusem 0xFC, a tak jsem ladil a posouval a ošetřoval, kontroloval jsem, jestli nechodí nějaké falešné přerušení, nuloval jsem počítadla bitů, RESETy jsem posílal, a nic, přátelé, ani ťuk!

Takže nakonec mám [knihovnu](https://github.com/maly/PS2KeyboardPlus), která umí krásně číst scankódy, včetně těch rozšířených, umí poslat Arduinu zprávu onPress a onRelease, ale nebliká, přátelé, nebliká! 🙁

A to jsem se s tím crcal ještě půl neděle!

Ale co, aspoň ty scankódy mám, krok 2 pak může být jejich překlad na ASCII znaky. Tralala&#8230; Někdy příště!