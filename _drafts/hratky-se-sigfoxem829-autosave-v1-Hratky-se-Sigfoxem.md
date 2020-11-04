---
id: 836
title: Hrátky se Sigfoxem
date: 2016-07-19T11:51:19+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/829-autosave-v1/
permalink: /829-autosave-v1/
---
Minule jsem si hrál s deskou [SmartEverything](http://retrocip.uelectronics.info/liska-kontra-papousek/) (SME) a Sigfoxem &#8211; nechal jsem si posílat informace o teplotě, tlaku a vlhkosti. Teď mě napadlo, že si zkusím, jak je na tom GPS.

Nainstalovat si příklad &#8222;zjisti pozici z GPS a napiš mi ji&#8220; je triviální. Ale to se mi moc nechtělo, já chtěl něco onačejšího. Vzal jsem tedy pozici z GPS a posílal jsem ji přes Sigfox na server. Jako že takový sledovač GPS (_Gde Právě Su_), a zároveň trošku tester technologie Sigfox.

Přemýšlel jsem, jak posílat inteligentně souřadnice ve zprávách, co mají maximálně 12 bajtů. Napadlo mě omezování rozsahu, konverze float na bajty, a už už jsem psal union, a pak mě napadlo, že se podívám, jestli už to někdo neřešil&#8230; No jasně že [řešil](https://github.com/aboudou/SmartEverything_SigFox_GPS).

\[sc:ebay item=&#8220;gps module ublox 10hz&#8220;\]\[sc:ebay item=&#8220;gps module telit&#8220;\][sc:ebay item=&#8220;arm cortex board&#8220;]

Takže jsem použil hotové řešení, upravil jsem posílání tak, aby odesílal zprávu každou minutu, ChytréVšechno jsem dal do auta na nabíječku a vyrazil jsem. Na serveru jsem si předem připravil úplně jednoduché logování poslaných dat.

Jel jsem odpoledne po českých dálnicích v intervalu <2;0>. Tedy jako že po D2, pak po D1 a nakonec po D0. SME vesele poblikával a posílal data, na serveru se vše shromažďovalo, a já teď sednul a celé jsem to hodil do mapy.



Co bod, to zachycená zpráva. Na serveru se ukládaly kromě pozice i informace o RSSI a SNR &#8211; najdete je v infoboxech. Barva bodu odpovídá hodnotě RSSI.

Co z mapy vyčteme? Především dvě věci:

  * Pokrytí je už poměrně slušné
  * Při rychlostech nad 110 km/h zprávy nepřicházejí.

Podle toho, co mi říkali lidé ze SimpleCell, je Sigfox bohužel velmi citlivý na úhlovou rychlost vůči přijímači. Pokud se útlum způsobený tímto efektem potká s útlumem  Hezky to je vidět na Pražském okruhu od D1 směrem k D5. Prvních pár kilometrů se dá jet 130, pak je rychlost snížena na 100. Na D1 se objevily body v místech, kde je rozkopáno a zúženo, a D2 je, přátelé, rozkopaná téměř non-stop. 🙂

Na D1 to ale nezachytilo všechna místa, kde je zúžení a zpomalení. To zase přičítám horšímu pokrytí signálem na Vysočině a v Posázaví.

V experimentech budu pokračovat a SME s sebou asi budu vozit pravidelně.