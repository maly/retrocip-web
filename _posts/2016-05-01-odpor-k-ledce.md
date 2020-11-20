---
id: 800
title: Odpor k LEDce
date: 2016-05-01T19:44:12+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=800
permalink: /odpor-k-ledce/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "4792031999"
categories:
  - Hardware
tags:
  - dogma
  - LED
  - rezistor
  - shield
---
Mám odpor k LEDce. Vy ne? To byste měli mít, protože prostě&#8230; víte proč, že?

Jsou dva typy elektroinžinýrů. Jedni vědí, že &#8222;u LEDky musí být rezistor&#8220;, kouknou do schématu, vidí LEDku bez rezistoru, a je jim jasné: To navrhoval patlač, klikač, amatér, bastlič, Satanáš a debil, protože všichni vědí, že u LEDky **musí** být rezistor a bez něj to může zapojit jen prase.

No a pak je ta, kupodivu menší, skupina lidí, kteří vědí, že &#8222;LEDka vydrží jen určitý proud&#8220; (plus samozřejmě že určitý proud potřebuje k tomu, aby vůbec svítila, ale tím se teď tady nezatěžujme). A že nejjednodušší je zapojit sériově rezistor, který proud omezí, ale &#8211; a to je důležité &#8211; není to jediné správné řešení.

Totiž,on ten rezistor má dvě zásadní nevýhody. Zaprvé: topíte jím pánubohu do oken. Není to nic podstatného, ale přeci jen &#8211; topíte. Zadruhé: je to další součástka v obvodu, a když držíte náklady nebo prostor na minimu, tak se každý rezistor počítá.

Tady je dobré vědět, že k LEDce není &#8222;povinný rezistor&#8220;, ale &#8222;maximální proud&#8220;. A pokud se vám LEDka ocitne v místě, kde je jaksi z podstaty proud, který je nižší než maximální povolený, tak se nemusíte už s rezistorem babrat. Však proč taky &#8211; **o proud jde**, ne o to, že máte rezistor u LEDky!

Maximální proud je kupodivu na spoustě různých míst. Například na výstupech a vstupech jednočipů. Nebo tam, kde se budí pomocí PWM. Například si představte takový displej, složený ze čtyř sedmisegmentovek, a jeho buzení pomocí jednočipu. Na porty připojím rovnou anody a katody, protože vím, že budím postupně jednu pozici po druhé, tedy každá LED je vlastně buzena PWM se střídou 1/4. Navíc vím, že jednočip má omezený proud per pin, takže nehrozí nebezpečí, že by tekly velké proudy. A proto vím, že v tomto případě není nutné použít rezistor.

Ale obávám se, že do finální verze [shieldu](https://retrocip.uelectronics.info/vyukovy-shield-pro-arduino-dil-druhy/) jich asi budu muset osm strčit. Sice jsou ty LEDky připojený mezi vývody ATtiny2313 a jsou buzené přes PWM, ale asi bych neměl na to vysvětlovat stále dokola to, co jsem napsal tady, každému, kdo se naučil mantru o LEDkách v té blbé verzi (tedy s rezistorem místo té verze s proudem)!

Já to na jednu stranu chápu: Začátečníkovi je lepší říct, že &#8222;k LEDce patří rezistor&#8220;, tím začátečník nic nezkazí&#8230; Jenže pak je taky dobré mu říct, proč to dělá, a dbát na to, aby to pochopil. Jinak se to naučí jak Ohmův zákon a bude to naprosto mechanicky aplikovat pořád, všude, a co hůř: bude se chodit hádat do diskusí&#8230;

PS: Tady máte [trošku detailnější počtení o LEDkách a rezistoru a jednočipu](https://tinkerlog.com/2009/04/05/driving-an-led-with-or-without-a-resistor/), ale je to v angličtině.