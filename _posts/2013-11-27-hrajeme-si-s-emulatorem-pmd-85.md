---
id: 49
title: Hrajeme si s emulátorem PMD-85
date: 2013-11-27T13:08:40+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=49
permalink: /hrajeme-si-s-emulatorem-pmd-85/
dsq_thread_id:
  - "2004132803"
xyz_lnap:
  - "1"
categories:
  - Emulace
tags:
  - ASM80
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Spusteni_emulatoru"><span class="toc_number toc_depth_1">1</span> Spuštění emulátoru</a>
    </li>
    <li>
      <a href="#Pouzivani_emulatoru"><span class="toc_number toc_depth_1">2</span> Používání emulátoru</a>
    </li>
    <li>
      <a href="#Zaklady_MONITORu"><span class="toc_number toc_depth_1">3</span> Základy MONITORu</a>
    </li>
    <li>
      <a href="#Nahrani_demonstracni_hry"><span class="toc_number toc_depth_1">4</span> Nahrání demonstrační hry</a>
    </li>
    <li>
      <a href="#Basic_G"><span class="toc_number toc_depth_1">5</span> Basic G</a>
    </li>
    <li>
      <a href="#Co_dal"><span class="toc_number toc_depth_1">6</span> Co dál?</a>
    </li>
  </ul>
</div>

Už jsem to tu naznačoval, a teď přišla ta chvíle, kdy je webový emulátor legendárního PMD 85 připravený k veřejnému alfatestu. Zde je opět průvodce&#8230;

<!--more-->

Na úvod předesílám, že <del>je zatím problém v prohlížeči Firefox, tam emulátor moc nefunguje.</del> Funguje v Opeře, Chromu i Firefoxu (v nových verzích i se zvukem) a v nových IE (bez zvuku).

_Aktualizace: problém ve Firefoxu se mi snad podařilo vyřešit._

O PMD naleznete na webu spoustu informací, například u [Schottka](schotek.cz/pmd/index.htm), u [Libora Lasoty](http://pmd85.mysteria.cz/), [Petera Chrenka](http://pmd85.topindex.sk/) nebo u [RM týmu](http://pmd85.borik.net/wiki/PMD_85) (Martinu Bórikovi z RM týmu patří můj dík za cennou pomoc a rady při tvorbě tohoto emulátoru).

[Emulátor PMD 85](http://www.asm80.com/pmd85.html) je opět součástí mého  [osmibitového IDE ASM80](http://www.asm80.com/). Až jej odladím alespoň do betaverze, udělám z něj asi i samostatný projekt, zatím je primárně určen k ladění assemblerových pokusů s PMD, tedy v rámci ASM80.

## <span id="Spusteni_emulatoru">Spuštění emulátoru</span>

Prosté &#8211; klikněte na odkaz [Emulátor PMD 85](http://www.asm80.com/pmd85.html). Vlevo je sloupec se soubory, které máte uložené v prohlížeči. Uprostřed je samotný emulátor. Vpravo pak ovládání “virtuální magnetofonové pásky”.

## <span id="Pouzivani_emulatoru">Používání emulátoru</span>

Ovládá se normální klávesnicí PC. Většina kláves je stejná (znaky, čísla, mezera), levý EOL je namapován na ENTER. Tlačítko Esc funguje jako STOP, pokud chcete reset, stiskněte Shift a Scroll Lock. Bohužel rozložení znaků na klávesnici PMD 85 bylo trošku jiné &#8211; koukněte na [layout klávesnice PMD](http://pmd85.borik.net/w/images/1/13/Pmd85-kbd-layout.gif). Snažil jsem se nechat klávesy na těch místech, kde jsou. K0 až K11 se proměnily v klávesy F1 &#8211; F12, což přineslo jeden problém: stránku nelze reloadovat pomocí F5. Ale to ještě pořeším.

## <span id="Zaklady_MONITORu">Základy MONITORu</span>

Doporučuju příručky, které jsou ke stažení na výše zmíněných odkazech. Pro základní práci postačí tyto příkazy:

MGLD xx &#8211; nahrání souboru z pásku do paměti. XX je hexadecimální číslo souboru &#8211; musí odpovídat tomu, s jakým číslem byl záznam na pásku nahrán. Po nahrání se program buď spustí sám, nebo je ho potřeba spustit pomocí JUMP.

JUMP xxxx &#8211; skok na startovací adresu programu. Adresu XXXX musíte znát, u demonstračních pásků, které jsem připravil, se objeví v okně.

BASIC G &#8211; Spustí interpret BASICu. Nezapomeňte na mezeru před písmenem G!

## <span id="Nahrani_demonstracni_hry">Nahrání demonstrační hry</span>

Ukážeme si nahrání a spuštění hry AUTO. Po resetu klikneme na demonstrační pásku Auto:

[<img loading="lazy" class="aligncenter size-medium wp-image-50" alt="pmd1" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmd1-650x374.jpg" width="650" height="374" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2013/11/pmd1-650x374.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2013/11/pmd1.jpg 921w" sizes="(max-width: 650px) 100vw, 650px" />](http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmd1.jpg)Tím se připraví páska s nahrávkou programu AUTO. Na displeji se objeví informace o programu:

[<img loading="lazy" class="aligncenter size-full wp-image-51" alt="pmd2" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmd2.png" width="346" height="187" />](http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmd2.png)

&nbsp;

Je tedy jasné, že program se bude nahrávat z monitoru příkazem MGLD 00. Po nahrání ho budeme spouštět pomocí JUMP 2000.

Napíšeme tedy MGLD 00, odešleme ENTERem a klikneme na &#8222;Tape start &#8211; playback&#8220;.

[<img loading="lazy" class="aligncenter size-full wp-image-23" alt="pmi4" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi4.png" width="204" height="147" />](http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi4.png)

&nbsp;

Na displeji se objeví  &#8222;00/? AUTO&#8220; &#8211; tedy číslo nahrávky, typ a název. Po nahrání nápis zmizí a my můžeme zadat kýžený JUMP 2000.

S páskou se pracuje [stejně jako v emulátoru PMI-80](http://retrocip.cz/hrajeme-si-s-emulatorem-pmi-80/#Ukldme_program_na_psku).

## <span id="Basic_G">Basic G</span>

K vyvolání interpreteru BASIC slouží příkaz BASIC G. V demonstračních páskách je i hra Bombardér, která je právě v BASICu. Nahrává se pomocí LOAD 02 (samozřejmě až z BASICu, ne z MONITORu) a spustí se klasickým RUN.

## <span id="Co_dal">Co dál?</span>

Rád bych svůj emulátor (pokud se nemýlím, tak je to první javascriptový emulátor PMD vůbec) vylepšil, dopiloval, rozšířil a otestoval &#8211; viz [Roadmap](http://retrocip.cz/a80-roadmap/). Zatím budu rád, když mi nahlásíte chyby nebo náměty pomocí tlačítka Usertvoice (ta bublina s otazníkem v pravém horním rohu).

A teď &#8211; _happy retro gaming!_

&nbsp;