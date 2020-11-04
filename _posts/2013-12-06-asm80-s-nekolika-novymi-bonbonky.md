---
id: 58
title: ASM80 s několika novými bonbónky
date: 2013-12-06T21:53:52+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=58
permalink: /asm80-s-nekolika-novymi-bonbonky/
dsq_thread_id:
  - "2030994659"
xyz_lnap:
  - "1"
categories:
  - ASM80.com
tags:
  - ASM80
---
Pár drobností jsem do [IDE](http://www.asm80.com) přidal, ať si nedělám ostudu, až vyjde článek na Zdrojáku 🙂

<!--more-->

Nejnovější build (3600) přináší oficiálně několik jednoduchých vylepšení:

1. Pomocí pseudoinstrukce .ENGINE můžete určit, pro jaký stroj je kód určen. Například &#8222;.engine pmi&#8220; Při kliknutí na tlačítko emulátoru se pak spustí přímo emulátor daného stroje s přednahraným souborem v paměti.

2. Líbí se vám výstup ve formátu LST a původní zdroják vám připadá neladný? Klikněte na &#8222;Format code&#8220;. Kód musí být syntakticky správný, jinak se nestane nic. Pokud bude správný, budete mít zdroják zarovnaný a instrukce + návěští převedené na velká písmena.

3. Trasování v &#8222;obecném emulátoru&#8220; zvýrazňovalo řádek s instrukcí. Když byl použit include nebo makro, nefungovalo to dobře. Teď se trasuje nikoli ve zdrojáku, ale v listingu, což je o hodně přehlednější (za tip díky Martinu Bórikovi)