---
id: 453
title: Pár poznámek k ESP8266
date: 2014-11-23T12:34:59+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=453
permalink: /par-poznamek-k-esp8266/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3254586209"
categories:
  - Hardware
tags:
  - ESP8266
  - Ethernet
  - internet
  - LAN
  - wifi
---
Tuhle jsem tu psal o [ESP8266](https://esp8266.cz), což je &#8222;[WiFi za pár korun](https://retrocip.uelectronics.info/esp8266-wifi-za-par-korun-doslova/ "ESP8266 – wifi za pár korun (doslova)")&#8222;. Od té doby uplynulo už mnoho&#8230; kecám, je to teprv měsíc. Co se během té doby stalo?

Tak zaprvé &#8211; dostal jsem ty moduly do ruky a trošku jsem si s nimi pohrál. Na tomto místě bych chtěl apelovat: Nevěřte manuálům!

  * Rozhodně nestačí zapojit napájení a TxD, RxD, jak tvrdí někteří kolegové. Zapojte i ten zbytek vývodů na log. 1
  * Komunikace s běžným čínským &#8222;USB-to-serial&#8220; modulem není problém. Navíc tyhle moduly disponují 5V i 3.3V logikou.
  * Ne, neutáhnou napájení té wifiny a bude vám to spíš padat než pracovat, takže si připojte externí zdroj.
  * Když už komunikujete, tak dejte pozor, že pro test připojení k vaší domácí síti je potřeba přepnout to na STA, nikoli na AP. Informace _&#8222;1= Sta, 2= AP, 3=both, Sta is the default mode of router, AP is a normal mode for devices&#8220;_ (například [tady](https://www.electrodragon.com/w/Wi07c)) je zavádějící. Ne, ne zavádějící, to druhé slovo: Je _úplně nesmyslná_!
  * Když máte zařízení na AP, tak vám nebude fungovat příkaz AT+CWLAP (výpis wifi sítí), ale nenapíše to &#8222;&#8230; protože jsem sám AP&#8220;, ale jen ERROR.
  * Přijdete na to třeba tím, že uvidíte ESP8266 v seznamu dostupných wifi sítí na svém PC.
  * No a v neposlední řadě buďte připraveni, že AT+RST občas nedopadne dobře a pomůže jen vypnout/zapnout.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/11/IMG_20141123_121320-650x365.jpg) 

Tolik za mne. Co ostatní?

Tak například Dex měl [nějaké připomínky k vhodnosti](https://retrocip.cz/esp8266-wifi-za-par-korun-doslova/#comment-1654551081) těchto modulů pro osmibitové stroje. Rychlost komunikace se mu zkrátka zdá příliš vysoká. Vyjádřil jsem naději, že s dostupným SDK bude toto určitě brzy řešitelné, a měl jsem pravdu. Pár odkazů:

  * [Update firmware to change baud rate](https://www.rei-labs.net/esp8266-update-firmware-to-change-the-baudrate/)
  * [Další firmware](https://scargill.wordpress.com/2014/11/08/stubbornly-not-fixed-esp8266/)
  * Oukej, [ještě jeden firmware se zabudovaným engine pro Lua](https://scargill.wordpress.com/2014/11/13/lua-based-firmware-for-esp8266/)
  * a [firmware není nikdy dost](https://scargill.wordpress.com/2014/11/16/esp8266-alternative-firmware-more-updates/)
  * [ESP8266 jako nejlevnější počítač](https://scargill.wordpress.com/2014/11/20/the-worlds-cheapest-computer-esp8266lua-2/) (má to wifi, má to Lua)
  * [ESP8266 jako webserver](https://harizanov.com/2014/11/esp8266-powered-web-server-led-control-dht22-temperaturehumidity-sensor-reading/)
  * a není jediný, kdo [v ESP8266 implementoval celé zařízení](https://hackaday.io/project/3249-simple-native-esp8266-smartmeter)

Je to docela pěkné hackerské udělátko, co? [Shrnutí možností, co s ním můžete dělat](https://scargill.wordpress.com/2014/11/19/esp8266-options/), máte třeba [zde](https://scargill.wordpress.com/2014/11/19/esp8266-options/).

A pro ty, co mají rádi alternativy, je jedna tady: [USR-WIFI232-T](https://2xod.com/articles/Arduino_Wifi_With_Hi_Flying_HF-LPT100_or_USR-WIFI232/) (na [eBay opět k sehnání za 200-300 Kč](https://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5337592372&customid=&icep_uq=USR-WIFI232-T&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg))