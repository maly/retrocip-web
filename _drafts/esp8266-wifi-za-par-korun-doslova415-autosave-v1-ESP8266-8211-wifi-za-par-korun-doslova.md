---
id: 418
title: 'ESP8266 &#8211; wifi za pár korun (doslova)'
date: 2015-08-17T16:38:41+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/415-autosave-v1/
permalink: /415-autosave-v1/
---
Tyhle moduly se po eBay rozšířily jako houby po dešti. Podle popisu je <a href="http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5337592372&customid=&icep_uq=ESP8266&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=4661&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg" target="_self">ESP8266</a><img style="text-decoration: none; border: 0; padding: 0; margin: 0;" src="http://rover.ebay.com/roverimp/1/711-53200-19255-0/1?ff3=9&pub=5575085282&toolid=10001&campid=5337592372&customid=&uq=ESP8266&mpt=[CACHEBUSTER]" alt="" /> levný WiFi modul se sériovým rozhraním (že nekecám, [tady ho máte za 90 Kč, poštovné zdarma](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=2&pub=5575085282&toolid=10001&campid=5337592372&customid=&icep_item=181524343824&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg)).

<img loading="lazy" class="aligncenter wp-image-416 size-full" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/10/ESP8266-e1414314282119.jpg" alt="" width="344" height="202" /> 

Jen to má trošku háček. Jak jinak, je to z Číny. Největší háček, tedy spíš hák, představovalo to, že dokumentace byla dostupná jen v čínštině.

Ale nakonec se podařilo téměř nemožné, a my dneska máme:

  * [Schéma a přeložený datasheet](https://nurdspace.nl/ESP8266)
  * [Zdokumentované AT příkazy](http://www.electrodragon.com/w/Wi07c)
  * [Knihovnu pro Arduino](https://hackaday.io/project/2879-ESP8266-WiFi-Module-Library)
  * [Diskusní fórum](http://www.esp8266.com/index.php)
  * [Nějaké](http://rayshobby.net/?p=9734) [články](http://www.cnx-software.com/2014/08/28/esp8266-wifi-serial-module-costs-just-5/) a [tutoriál](https://www.zybuluo.com/kfihihc/note/31135)
  * [SDK pro ESP8266 včetně překladače](http://bbs.espressif.com/viewforum.php?f=12) (pokud si chcete udělat vlastní firmware)

Jako bych vás slyšel: &#8222;Takže ty chceš říct, že [za 90 Kč](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=2&pub=5575085282&toolid=10001&campid=5337592372&customid=&icep_item=181524343824&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg) dostanu něco, co připojím čtyřma drátama (Vcc, GND, TxD a RxD), dám pozor na to, že ta logika je 3.3V, ne 5V, a pak tam vesele pouštím 115200 Bd komunikaci (u starších verzí 57600 Bd) a pomocí toho zjistím seznam wifi sítí, připojím se a přenesu data? A že to šlape v b/g/n? A že to umí WPA/WPA2 a pracovat jako Station i jako AP, jako server i jako klient?&#8220; Jo, tohle všechno říkám.

Představujete si, jak ke svému jednočipovému projektu (nebo třeba Spectru, Atari nebo Commodore, i když tam to nebude úplně přímočaré, viz Dexova poznámka v komentářích) připojujete nějakým sériovým rozhraním tohle za pár pětek, a najednou jste na síti, ba co dím &#8211; bezdrátově na síti?! Krásné, že? Modulů je [více druhů](http://tminusarduino.blogspot.cz/2014/09/experimenting-with-esp8266-5-wifi-module.html), takže klidně najdete i úplného mrňouse s konektorem pro externí anténu ([zde za 100 Kč](http://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=2&pub=5575085282&toolid=10001&campid=5337592372&customid=&icep_item=181528782672&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg)).

Taky se nemůžete dočkat, až vám dorazí? (Už mám dva týdny objednáno a tenhle článek je vlastně takové těšení na druhou. Jakmile přijdou, otestuju a zase napíšu!)

[sc:ebay item=ESP-05]