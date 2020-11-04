---
id: 875
title: ESP8266 + MQTT
date: 2016-09-26T09:42:48+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/872-autosave-v1/
permalink: /872-autosave-v1/
---
Trošku jsem si zaexperimentoval s [ESP8266](http://esp8266.cz). Abych byl přesný, tak s kitem [WeMos D1 mini](http://esp8266.cz/wemos-d1-mini/), což je takové miniArduino &#8211; jen USB převodník a procesor. V roli procesoru tam je modul ESP-12E.

Nejprve jsem laboroval s microPythonem i s JavaScriptem, a nakonec jsem sáhnul k Arduino IDE, které na téhle desce [funguje bez problémů](http://esp8266.cz/programovani/esp8266-a-arduino/).

Jako senzory jsem použil I2C tagy [BigClown](http://www.bigclown.com/). Jejich výhoda je, že jsou malé a obsahují senzor s potřebnou &#8222;bižuterií&#8220;. V mém případě jsem použil luxmetr (se snímačem OPT3001) a teploměr (TMP112).

Chvilku jsem řešil připojení I2C <> WeMos <> ESP8266, ale pak jsem to zvládl. I2C se totiž používá na pinech GPIO 4 a 5, které jsou mapovány na vývody D2 a D1. Udělal jsem si k tomu takovou [tabulku](http://esp8266.cz/wemos-d1-mini/wemos-d1-mapovani-vyvodu-ruznych-verzi/).

Jenže pak JavaScript nehodlal z I2C číst. MicroPython totéž. Nevím proč, pravděpodobně nějaké _quirks_, nebo jsem k čidlům přistupoval úplně blbě&#8230; Když už jsem podléhal beznaději, zkusil jsem výše zmíněné Arduino, a vše fungovalo. Hodně pomohla [knihovna pro OPT3001](https://github.com/closedcube/B060_OPT3001_PRO). O pár momentů později už mi lezly po sériové lince údaje z měření&#8230;

Pak jsem přihodil i ten teploměr, taky přes [knihovnu](https://github.com/Refikcanmalli/RCM_TMP112).

No a protože je tam ESP, tedy wifina &#8222;zdarma&#8220;, tak logický další krok bylo připojit se a data někam posílat. Použil jsem skvělý [WiFi Manager](https://github.com/tzapu/WiFiManager), aby to bylo přenositelné a aby v kódu nestrašilo heslo k mojí wifi. No a k tomu už jen MQTT client library.

Na serveru jsem spustil Mosquitto. V Debianích repozitářích byla zas nějaká stará verze, takže jsem na to šel takto:

  1. _wget http://repo.mosquitto.org/debian/mosquitto-repo.gpg.key_
  2. _sudo apt-key add mosquitto-repo.gpg.key_
  3. _cd /etc/apt/sources.list.d/_
  4. _sudo wget http://repo.mosquitto.org/debian/mosquitto-jessie.list_
  5. _apt-get updateapt-get install mosquitto mosquitto-clients_

Přidal jsem acl (Access Control List) podle [tohoto vzoru](https://troy.dack.com.au/mosquitto-mqtt/) a pustil jsem i MQTT over Websockets.

A bylo.

Jako poslední jsem udělal drobnou úpravu, při které modul změří, pošle data, a pak se uvede do deep sleep módu. Aby fungovalo probuzení, musel jsem propojit vstup RESET a GPIO16 (na WeMos D1 Mini je to pin D0). Na tomto výstupu totiž po uplynutí zadané doby přijde &#8222;budíček&#8220;. Chci použít Li-Ion baterii a zkusit, jak dlouho to bude takto fungovat. Zatím to napájím z powerbanky, a tam zatím úbytek napětí nepozoruju.

<figure id="attachment\_873" aria-labelledby="figcaption\_attachment_873" class="wp-caption aligncenter" style="width: 660px"><img loading="lazy" class="wp-image-873 size-medium" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/08/20160822_135317_HDR-650x366.jpg" alt="20160822_135317_HDR" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2016/08/20160822_135317_HDR-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2016/08/20160822_135317_HDR-768x432.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2016/08/20160822_135317_HDR.jpg 800w" sizes="(max-width: 650px) 100vw, 650px" /><figcaption id="figcaption\_attachment\_873" class="wp-caption-text">Ten žlutý vodič je zmiňované propojení RESETu, dokud tam nezapájím propojku 🙂</figcaption></figure>

<figure id="attachment\_877" aria-labelledby="figcaption\_attachment_877" class="wp-caption aligncenter" style="width: 660px"><img loading="lazy" class="size-medium wp-image-877" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/08/20160822_154605_HDR-650x366.jpg" alt="Ještě jsem dobastlil Li-Ion baterii 3.7V a modul na dobíjení přes MikroUSB" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2016/08/20160822_154605_HDR-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2016/08/20160822_154605_HDR-768x432.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2016/08/20160822_154605_HDR.jpg 800w" sizes="(max-width: 650px) 100vw, 650px" /><figcaption id="figcaption\_attachment\_877" class="wp-caption-text">Ještě jsem dobastlil Li-Ion baterii 3.7V a modul na dobíjení přes MikroUSB</figcaption></figure>

PS &#8211; poznámka víceméně pro mne&#8230;

MQTT a LetsEncrypt:

  * [Instalace LetsEncrypt](https://certbot.eff.org/#debianjessie-other)
  * [Přidání Backports](https://backports.debian.org/Instructions/)
  * [Vytvoření certifikátů pro Mosquitto](https://mosquitto.org/2015/12/using-lets-encrypt-certificates-with-mosquitto/)
  * [Nastavení bezpečné komunikace](https://troy.dack.com.au/mosquitto-mqtt/)
  * Přidat do CRONu /etc/cron.d/certbot aktualizaci certifikátů pro MQTT