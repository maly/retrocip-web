---
id: 42
title: A80 roadmap
date: 2013-11-26T11:08:57+01:00
author: Martin Maly
layout: page
guid: http://retrocip.uelectronics.info/?page_id=42
dsq_thread_id:
  - "2000631083"
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Typy_procesoru"><span class="toc_number toc_depth_1">1</span> Typy procesorů</a>
    </li>
    <li>
      <a href="#Funkce"><span class="toc_number toc_depth_1">2</span> Funkce</a>
    </li>
    <li>
      <a href="#Sprite_editor"><span class="toc_number toc_depth_1">3</span> Sprite editor</a>
    </li>
    <li>
      <a href="#PMD_85_1_2A"><span class="toc_number toc_depth_1">4</span> PMD 85 (1, 2A)</a>
    </li>
    <li>
      <a href="#BOB-85"><span class="toc_number toc_depth_1">5</span> BOB-85</a>
    </li>
    <li>
      <a href="#PMI-80"><span class="toc_number toc_depth_1">6</span> PMI-80</a>
    </li>
    <li>
      <a href="#CPM"><span class="toc_number toc_depth_1">7</span> CP/M</a>
    </li>
  </ul>
</div>

Sem si poznamenávám věci, co chci v rámci asm80.com udělat a dodělat&#8230;

# Assembler

## <span id="Typy_procesoru">Typy procesorů</span>

  * <del>6809</del> &#8211; _**hotovo**_
  * SC/MP II
  * 1802

## <span id="Funkce">Funkce</span>

  * <del>Ukládání workspace na server</del> &#8211; _**hotovo**_
  * <del>&#8222;Source beautifier&#8220; na zkrášlení zdrojáku</del> _&#8211; **hotovo**_
  * Importy / exporty (např. možnost hodit jako zdroják výstup LST)
  * Emulátor 8080 doplnit o trasování <del>a breakpointy</del> _&#8211; **breakpointy** **hotové**_
  * Doplnit do výpisu volitelně takty procesoru

## <span id="Sprite_editor">Sprite editor</span>

# Emulátory

## <span id="PMD_85_1_2A">PMD 85 (1, 2A)</span>

  * <del>Ve Firefoxu extrémně pomalý, nefunguje audio &#8211; nepoužitelný! 🙁</del> _Opraveno_
  * <del>V Opeře funguje audio, nefunguje klávesa SHIFT (problém s jejím zachytáváním)</del> _Opraveno_
  * <del>V IE nefunguje! Nutnost fixnout clampedArrays!</del>  _&#8211; **hotovo**_
  * Dodělat 8251 a práci s páskou
  * <del>Dodělat LED (kosmetická vada)</del>**_ Hotovo_**
  * <del>Přidat možnost zastavit procesor a krokovat ho (monitor)</del> **_Hotovo_**

## <span id="BOB-85">BOB-85</span>

  * První verze otestována s procesorem 8080, klávesnice i displej fungují, monitor běží
  * Dodělat SIM/RIM a nedokumentované instrukce pro 8085
  * Dodělat práci s magnetofonem

## <span id="PMI-80">PMI-80</span>

  * Dodělat zvuk

## <span id="CPM">CP/M</span>