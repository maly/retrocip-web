---
id: 440
title: Nálož assemblerových novinek
date: 2014-11-09T14:53:56+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=440
permalink: /naloz-assemblerovych-novinek/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3207478579"
categories:
  - ASM80.com
tags:
  - ASM80
---
Dlouho jsem se nehlásil s novinkami ze světa ASM80.com. Tedy, hlavní důvod byl ten, že dlouho žádné novinky nebyly. Teď jich ale pár mám&#8230; Začneme od těch drobných a skončíme u těch největších, jo?

  * Nová direktiva **.align** umožňuje zarovnat kód na určitou adresu. Příklad: chcete, aby nějaká tabulka začínala na adrese, dělitelné osmi (protože na to někde v kódu spoléháte). Takže před tabulku napíšete direktivu **.align 8**. Je to jednodušší než počítat ORG a bez ohledu na to, kde je zbytek kódu umístěný, bude tahle tabulka vždycky na adrese xxx0h nebo xxx8h.
  * Další direktiva **.pragma html** způsobí, že listing nebude textový, ale bude ve formátu HTML. Tedy například odkazy na subrutiny budou fungovat jako odkazy atd.
  * Direktivu **.incbin** si vymohla další dnešní novinka, ke které se ještě dostanu. Takže teď už funguje vkládání binárních souborů napřímo pomocí incbin.
  * Díky Jirkovi jsem se rozhoupal a napsal **assembler pro procesor Motorola 6800**. No a když už jsem byl v tom, tak jsem napsal i debugger, a chystám se na emulátor HeathKitu, ve kterém je tento procesor použitý.
  * O [jednostránkových překladačích](https://retrocip.uelectronics.info/uplne-miniaturni-update-asm80/ "Úplně miniaturní update ASM80") jsem už psal. Jsou to HTML stránky se zabudovaným překladačem, které si můžete uložit na disk, otevřít v prohlížeči a překládat zdrojáky pomocí copy a paste.
  * [Zdrojový kód překladače](https://github.com/maly/asm80) je na GitHubu. Licencováno jako MIT, _source available, no whining_. Kromě toho tam mám i [pár dalších JS kousků,](https://github.com/maly?tab=repositories) jako emulaci procesorů nebo ACIA.
  * No a protože ne samým onlinem živ je člověk, připravil jsem jednak verzi překladače pro příkazovou řádku (zatím testuju na Windows), a pak [IDE80](https://www.ide80.com).

IDE80 je desktopová verze toho online prostředí. Zatím je to alfaverze pro Windows, jakmile doladím detaily a přidám ještě pár uvažovaných vylepšení, pustím to ven jako ostrou verzi. Už teď ale můžete [stahovat a testovat](https://www.ide80.com). (Pokud si ho stáhnete a nainstalujete, tak se může stát, že vám v nějaké situaci nahlásí chybu. Chyby a připomínky hlašte na _asm80@maly.cz_. Děkuji, pomůže mi to při ladění.)

IDE80 obsahuje i emulátory PMD-85, PMI-80, ZX Spectra a počítačů Granta Searla. Do budoucna by mělo IDE80 umět všechno to, co umí online verze, a především by měla být naportovaná i pro Mac OS X a Linux. _Stay tuned_.

A co chystám dál? Emulátor KIM-1 a HeathKitu, tedy dvou známých jednodeskáčů, do sbírky k PMI. Pokusím se i o C64, ale tam je to nejisté. Z kategorie &#8222;drobná vylepšení&#8220; to pak jsou generátory různých výstupních formátů &#8211; SNA, TAP, PRG apod.

* * *

_Pokud se vám ASM80 / IDE80 líbí a chcete ho podpořit, můžete jeho autorovi přispět na hosting, na vývoj, nebo třeba na kafe, aby mu to vyvíjelo rychleji. Nejjednodušší způsob je přes PayPal &#8211; stačí kliknout tady o kousek níž. Díky._

<div class="bitcoin-button" data-address="1C1WSFq8pvBu3ZqNgerqa1QZZy9Jp1jgwK" data-info="transactions" data-message="Poděkování Bitcoinem potěší">
</div>

<!-- Place this snippet somewhere after the last button -->