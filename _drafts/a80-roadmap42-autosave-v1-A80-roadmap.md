---
id: 47
title: A80 roadmap
date: 2015-02-14T11:11:13+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/42-autosave-v1/
permalink: /42-autosave-v1/
---
Sem si poznamenávám věci, co chci v rámci asm80.com udělat a dodělat&#8230;

# Assembler

## Typy procesorů

  * <del>6809</del> &#8211; _**hotovo**_
  * SC/MP II
  * 1802

## Funkce

  * <del>Ukládání workspace na server</del> &#8211; _**hotovo**_
  * <del>&#8222;Source beautifier&#8220; na zkrášlení zdrojáku</del> _&#8211; **hotovo**_
  * Importy / exporty (např. možnost hodit jako zdroják výstup LST)
  * Emulátor 8080 doplnit o trasování <del>a breakpointy</del> _&#8211; **breakpointy** **hotové**_

&nbsp;

# Emulátory

## PMD 85 (1, 2A)

  * <del>Ve Firefoxu extrémně pomalý, nefunguje audio &#8211; nepoužitelný! 🙁</del> _Opraveno_
  * <del>V Opeře funguje audio, nefunguje klávesa SHIFT (problém s jejím zachytáváním)</del> _Opraveno_
  * <del>V IE nefunguje! Nutnost fixnout clampedArrays!</del>  _&#8211; **hotovo**_
  * Dodělat 8251 a práci s páskou
  * <del>Dodělat LED (kosmetická vada)</del>**_ Hotovo_**
  * <del>Přidat možnost zastavit procesor a krokovat ho (monitor)</del> **_Hotovo_**

## BOB-85

  * První verze otestována s procesorem 8080, klávesnice i displej fungují, monitor běží
  * Dodělat SIM/RIM a nedokumentované instrukce pro 8085
  * Dodělat práci s magnetofonem

## PMI-80

  * Dodělat zvuk

## CP/M