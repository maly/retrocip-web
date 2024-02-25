---
id: 697
title: 'Elektronika a Mac, díl první: Arduino'
date: 2015-09-13T15:08:39+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=697
permalink: /elektronika-a-mac-dil-prvni-arduino/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "4125590988"
categories:
  - Hardware
tags:
  - Apple
  - arduino
  - Mac OS X
---
Lidé se mě občas ptají: A co ta tvoje knížka a ta elektronika, půjde mi to na Macu? A já teď už konečně mohu odpovědět: Ano!

<!--more-->

Díky štědrému dárci jsem se stal majitelem postaršího MacBooku (2008), který jsem si &#8222;vytunil&#8220; novým systémem 10.10.1 (Yosemite) a přidanou RAM (originál 2GB se docela vlekl). Samozřejmě mě zajímalo, jak si tento systém porozumí s elektronikou.

Začal jsem od Arduina. Připojuje se přes USB a tváří se jako standardní sériový port. OSX má ovladače, takže nebyl naprosto žádný problém.

IDE jsem stáhnul [zde](https://www.arduino.cc/en/Main/Software): [Arduino IDE Mac OS X 10.7 Lion or newer (verze 1.6.5)](https://www.arduino.cc/download_handler.php?f=/arduino-1.6.5-r5-macosx.zip)

Zapojil jsem Arduino do USB, chvilku počkal a v nabídce vybral možnost &#8222;cu.usbmodem621&#8220; (zkoušel jsem Arduino Due, na jiné verzi to může být jinak, netuším):

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/09/osx2.jpg) 

Překlad naprosto bez problémů, instalace knihoven taky, instalace jiných desek (v tomto případě Due) taky bez problémů.

Jediný problém nastal s čínským klonem Arduina, který používá jiný obvod pro USB. V naprosté většině jde o typ CH340G. Ale nic, co by chvilka googlení nevyřešila: [ovladač CH340 pro Mac OS X máte zde](https://retrocip.cz/files/ch34x-install-osx.zip)! V nových OSX si nejprve spusťte Terminál a zadejte &#8222;sudo nvram boot-args=&#8220;kext-dev-mode=1&#8220; &#8211; tím povolíte nepodepsané ovladače. Pak už jen stačí rozbalit, spustit, a dokonce ani není potřeba přepisovat jméno zařízení, jak radí některé manuály. Po připojení takového Arduina se v nabídce objevila jen trochu jiná položka:

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/09/osx3.jpg) 

Zvolil jsem &#8222;wchusbserial&#8220;, a zvolil jsem správně!

S Arduinem jsem na Macu neměl žádný problém, což je potěšující zpráva. Na další vývojová prostředí a elektronické kity se podíváme zase příště. [sc:ebay item=arduino%20UNO%20R3%20atmega328p%20board]