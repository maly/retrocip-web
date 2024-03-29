---
id: 719
title: Sublime Text v roli IDE pro Arduino
date: 2015-09-24T09:43:44+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=719
permalink: /sublime-text-v-roli-ide-pro-arduino/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "4160382771"
categories:
  - Hardware
  - Software
tags:
  - arduino
  - Sublime Text 2
---
Dneska v noci jsem ztratil s IDE pro Arduino trpělivost, a tak jsem udělal něco, co [přece nejde](https://retrocip.uelectronics.info/arduino-pro-naproste-debily/), a nahradil jsem originální IDE svým oblíbeným editorem Sublime Text 2.

<!--more-->

Mít v Sublime Text 2 správce balíčků je [téměř povinnost](https://economia.github.io/tunime-sublime/). Když to zkusíte, najdete teď (konec září 2015) dva pluginy se slovem Arduino. Jeden se jmenuje &#8222;Arduino&#8220; a dělá (snad) syntax highlight, a ten druhý se jmenuje Arduino-like IDE. Ten jsem použil, ale s nevalným výsledkem. Nakonec jsem použil [betaverzi pluginu Stino](https://github.com/Robot-Will/Stino) (jako že &#8222;Sublime Text arduINO&#8220;).

Instalace je prostá, jen je důležité předem odinstalovat &#8222;Arduino-like IDE&#8220;, pokud jste s ním experimentovali. A pak:

  1. Preferences &#8211; Package Control &#8211; Add Repository a přidejte adresu _https://github.com/gepd/Stino/tree/new-stino_
  2. Preferences &#8211; Package Control &#8211; Install Package a vyberte &#8222;Stino&#8220;.
  3. V menu se objeví položka Arduino. Nastavte Arduino &#8211; Preferences &#8211; Select Arduino App Folder a nasměrujte ho tam, kde máte nainstalované Arduino IDE (plugin použije překladače, co tam jsou)
  4. Ještě si nastavte Arduino &#8211; Preferences &#8211; Sketchbook folder na adresář s vašimi projekty

Zbytek je podobný Arduinu, tedy musíte nastavit typ desky a port. Typ programátoru je &#8222;AVRISP mkII&#8220;. Všechno běhá jak po másle. Nezískám tím žádné nové funkce, ale mohu zdrojáky editovat v Sublime, na který jsem zvyklý a které má například i funkci &#8222;Více kurzorů&#8220;, co u Arduino IDE postrádám velmi bolestně.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/09/stino-650x368.jpg) 

\[sc:ebay item=&#8220;arduino nano usb&#8220;\]\[sc:ebay item=&#8220;arduino uno USB&#8220;\]\[sc:ebay item=&#8220;arduino 2560 USB&#8220;\]\[sc:ebay item=&#8220;arduino due SAM3X8E&#8220;\]