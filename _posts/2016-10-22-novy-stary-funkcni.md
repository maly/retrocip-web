---
id: 918
title: 'Nový starý&#8230; funkční!'
date: 2016-10-22T13:59:00+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=918
permalink: /novy-stary-funkcni/
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2016/10/20161022_144721-800x198.jpg
categories:
  - Hardware
tags:
  - "6809"
  - Jess
---
Přišly prototypové desky na [Nový Starý (krycí název Jess)](https://retrocip.uelectronics.info/jess/). Mám velkou radost, protože návrh obsahoval jen tři chybné spoje. Jasně, jsem blbec, namapoval jsem výstup na pin, který může být jen vstup. Ale co, můžu si za to sám. Ale jinak na první zapojení fungoval.

Vzal jsem [Multicomp od Granta Searla](https://searle.hostei.com/grant/Multicomp/index.html), [generátorem](https://www.uelectronics.info/multigen.html) jsem si vytvořil Microcomputer.vhd v sestavě 6809 / Full external SRAM / 10MHz / Serial, upravil jsem mapování pinů FPGA, nahrál výsledek do EPCS4, spustil, a na terminálu se objevilo to, co se objevit mělo. Půl hodinky od nažhavení pájky, dobrý výsledek! A tak jsem si i zaprogramoval:

<a href="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/10/jess1.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-920" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/10/jess1-650x456.png" alt="jess1" width="650" height="456" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2016/10/jess1-650x456.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2016/10/jess1-768x539.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2016/10/jess1.png 831w" sizes="(max-width: 650px) 100vw, 650px" /></a>

Krok 2: Zapájet DAC pro VGA (no, šest rezistorů) a ověřit klávesnici. Mezitím už se mi dělají opravené PCB. Na nich bude i místo pro konektory JTAG/AS (teď je deska přes ně a je to nepohodlné).

No a pak už je jen krok 3, totiž profit. Tedy pardon &#8211; krok 3 je samozřejmě úprava podle těch mých plánů, co jsem nastínil minule.

<a href="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/10/20161022_144721.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-919" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/10/20161022_144721-650x366.jpg" alt="20161022_144721" width="650" height="366" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2016/10/20161022_144721-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2016/10/20161022_144721-768x432.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2016/10/20161022_144721.jpg 800w" sizes="(max-width: 650px) 100vw, 650px" /></a>