---
id: 925
title: ASM80 z příkazového řádku
date: 2016-11-24T20:08:13+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=925
permalink: /asm80-z-prikazoveho-radku/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "5329772034"
categories:
  - ASM80.com
  - Stroják
tags:
  - "6502"
  - "8080"
  - ASM80
  - node
  - npm
  - Utility
  - Z80
---
Čtenáři jistě znají [ASM80](https://www.asm80.com) &#8211; online assembler / IDE pro osmibitové procesory. Udělal jsem i několik odvozených verzí, třeba single page překladače, embedded verzi překladače (použil jsem ji v tutoriálu [Stroják.cz](https://strojak.cz)), nebo třeba [stand alone IDE](https://www.ide80.com). Logickým pokračováním je tedy stará dobrá utilita pro překlad z příkazového řádku.

Předpokladem je mít funkční prostředí Node. Není to nic složitého, existuje pro všechny hlavní platformy, a stáhnout si ho můžete zde: [nodejs.org](https://nodejs.org/en/download/). Při instalaci se nainstaluje i správce balíčků, zvaný _npm_.

NPM slouží k instalaci balíčků a knihoven. Pro instalaci ASM80 stačí spustit příkazový řádek a napsat:

_npm i -g asm80_

Parametr _-g_ způsobí, že se asm80 nenainstaluje jako knihovna (to ani nedává smysl), ale jako systémový nástroj. Pak půjde spouštět jako klasická utilita pro příkazový řádek:

_asm80 test.a80_

spustí překlad souboru _test.a80_, a výsledkem budou dva soubory: test.hex s výstupem a test.lst s protokolem o překladu. Přípona _.a80_ napovídá, že překladač má použít instrukční sadu procesoru 8080.

Chování lze ovlivnit pomocí parametrů. Lze nastavit jméno výstupního souboru, potlačit generování .lst, nebo explicitně určit typ procesoru a formát výstupního souboru (kromě HEX a SREC lze generovat například i .COM soubory pro CP/M, .PRG pro emulátory C64, nebo SNA a TAP pro ZX Spectrum). Parametry jsou popsány [na stránce NPM balíčku ASM80](https://www.npmjs.com/package/asm80).

[Přehled syntaxe a direktiv naleznete na GitHub Pages](https://maly.github.io/asm80-node/).

Ještě mám pár námětů na vylepšení, chtěl bych znát váš názor&#8230;

  * Vytvořit systém knihoven, tak, jak ho mají klasické assemblery, co oddělují překlad a linkování. Tedy možnost udělat knihovnu subrutin, z níž by se includovaly pouze ty části kódu, které jsou potřebné pro správnou funkci. Mohli byste si pak udělat třeba něco jako &#8222;clib&#8220; pro svůj systém&#8230;
  * Mít možnost přilinkovat rovnou veřejně publikované knihovny, například na GitHubu.
  * &#8230; další procesory? Systémy?

Díky za tipy a návrhy. Můžete je rovnou posílat do [Issues](https://github.com/maly/asm80-node/issues) (a klidně i česky)