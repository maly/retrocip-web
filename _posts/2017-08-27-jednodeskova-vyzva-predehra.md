---
id: 971
title: 'Jednodesková výzva: předehra'
date: 2017-08-27T13:21:28+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=971
permalink: /jednodeskova-vyzva-predehra/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "6099498894"
image: https://retrocip.cz/wp-content/uploads/sites/6/2013/11/image005-800x198.jpg
categories:
  - ASM80.com
  - Emulace
  - Hardware
tags:
  - "6502"
  - "8080"
  - Assembler
  - jednodeskáč
  - stroják
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#PMI-80"><span class="toc_number toc_depth_1">1</span> PMI-80</a><ul>
        <li>
          <a href="#Ovladani_PMI-80"><span class="toc_number toc_depth_2">1.1</span> Ovládání PMI-80</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#BOB-85"><span class="toc_number toc_depth_1">2</span> BOB-85</a>
    </li>
    <li>
      <a href="#KIM-1"><span class="toc_number toc_depth_1">3</span> KIM-1</a>
    </li>
    <li>
      <a href="#ET3400"><span class="toc_number toc_depth_1">4</span> ET3400</a>
    </li>
  </ul>
</div>

Dámy a pánové, vítám vás u Jednodeskové výzvy, určené všem programátorům, co si troufají ukázat světu, jak to válí s osmibitem. Troufnete si napsat vlastní firmware pro jednodeskový osmibitový počítač?

<!--more-->

V dalším článku si řekneme víc k zadání. Dnes mi dovolte udělat takové malé lehké představení &#8222;jednodeskáčů&#8220;.

**Jednodeskáč**, tedy celým jménem &#8222;jednodeskový mikropočítač&#8220;, byl v 70. a 80. letech malý počítač, co se vešel na jednu desku. Dnes se na jednu desku vejde leccos, ale tehdy jste si moc vyskakovat nemohli. Většinou se tam vešel procesor, trocha paměti (typicky 1 kB RAM, 1 kB PROM), klávesnice (měla většinou okolo 20-25 tlačítek: 16 znaků 0-9 a A-F pro zadávání hexadecimálních číslic a několik ovládacích), displej (ze sedmisegmentovek) a nějaké vstupně/výstupní porty, které byly buď dostupné pro další rozšiřování, nebo na nich byly různé LEDky, motorky a podobně. (Hezky [o jednodeskáčích píše Nostalcomp](https://www.nostalcomp.cz/jednodesky.php).)

V Československu se vyráběly dva známější jednodeskáče (a několik dalších, dnes už pozapomenutých): legendární PMI-80 a školní jednodeskový počítač TEMS 80-03A. Oba s procesorem 8080A a s plusmínus stejnou technickou výbavou. Několik jednodeskových počítačů vzniklo i péčí nadšenců (BOB85, SAVIA84), vznikaly i jako stavebnice (Petr)&#8230; Technicky vzato byl třeba i Ondra jednodeskový, ale pro nás budiž jednodeskáč počítač s omezenou klávesnicí a s displejem ze sedmisegmentovek.

Ve světě se prosadil asi nejvíc KIM-1, jednodeskový počítač od MOS Technology s procesorem 6502. Vznikla i &#8222;odlehčená&#8220; verze Junior Computer. Další zajímavý jednodeskáč byl třeba Cosmac Elf nebo Heathkit ET3400. (A samozřejmě [spousta dalších](https://www.nostalcomp.cz/slavni.php).)

Nechci vás nijak zatěžovat výčtem a podrobnostmi. Spíš bych rád demostroval &#8222;ducha doby&#8220; a &#8222;feeling&#8220; na několika jednodeskáčích.

## <span id="PMI-80">PMI-80</span>

Začnu **PMIčkem**. Je totiž velmi dobře zdokumentované, hlavně díky sérii článků v Amatérském Radiu ([výtah najdete zde](https://www.nostalcomp.cz/pdfka/pmi80_popis.pdf)). PMI-80 mělo 1 kB RAM, 1 kB ROM (a mohli jste si druhý kB přidat), klávesnici s 25 tlačítky, devítimístný displej z kalkulačky (obojí připojené přes periferní obvod 8255) a primitivní rozhraní pro magnetofon.

Já si kdysi udělal [online emulátor PMI-80](https://www.asm80.com/pmi80.html), a tak si můžete ověřit, jak to celé fungovalo.

ROM byla od adresy 0000 (logicky, po RESETu se spouštěl program od téhle adresy). RAM byla od adresy 1C00 do 1FFF. S periferiemi se komunikovalo na adresách F8, F9, FA a FB (klasická čtveřice registrů 8255 &#8211; PA, PB, PC, řídicí). Displej používal budič a dekodér 1-z-9 typu MH1082, připojený na port PC (=adresa FA), bity 0-3. Tímto bitem se vybírala pozice na displeji. Segmenty ovládal port PA (=adresa F8), a to bity 0-6 (bit 7 nebyl zapojený, desetinná tečka nefungovala).

Stejný budič používala i klávesnice, dokonce kvůli tomu byla zapojená do matice 3&#215;9 (tlačítka I a RE, Interrupt a Reset, byly mimo matici a šly přímo k procesoru). Informace z řádků klávesnice se četly na portu PC, piny 4-6.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/screenshot-www.nostalcomp.cz-2017-08-27-13-14-29.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2017/08/screenshot-www.nostalcomp.cz-2017-08-27-13-14-29-650x430.png)</a>

Displej tedy musel být pravidelně občerstvován procesorem; jakmile se program někde zasekl, zůstala svítit jediná pozice a klávesnice přestala reagovat.

Na magnetofon se vysílalo pomocí nosného kmitočtu na bitu 6 portu PA, klíčovaného bitem PA7. Z magnetofonu se data četla přes pin PC7.

### <span id="Ovladani_PMI-80">Ovládání PMI-80</span>

Displej byl, jako u většiny jednodeskáčů, rozdělen na část adresní (vlevo) a část datovou (vpravo). Rozdělení nebylo nijak dané, byla to čistě konvence. Klávesnice obsahovala 25 tlačítek, fyzicky rozmístěných do matice 5&#215;5:

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/screenshot-www.nostalcomp.cz-2017-08-27-13-19-29.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2017/08/screenshot-www.nostalcomp.cz-2017-08-27-13-19-29.png)</a>

Tlačítko RE celý systém zresetovalo, a na displeji se objevil nápis &#8222;PMI-80&#8220;. Po stisknutí libovolné klávesy (tedy kromě RE a I) se na displeji úplně vlevo objeví znak, co má být asi otazník, který říká, že PMI je připravené přijímat pokyny.

Nejzásadnější věc u jednodeskáče byla funkce prohlížení a měnění obsahu paměti. Po stisku tlačítka M (Memory) se vlevo objevilo &#8222;velké M&#8220; (píšu to v uvozovkách, protože na sedmisegmentovém displeji to vypadá spíš jako obrácené velké U nebo ruské P), pak mezera a čtyři pozice adresy. Třeba: &#8222;M 1C00&#8220; Pomocí hexadecimální klávesnice jste zadali adresu (pokud jste udělali chybu, prostě jste psali dál, systém bral jako platné pouze čtyři poslední znaky) a tlačítkem &#8222;=&#8220; jste ji potvrdili. Na displeji se objevil za adresou znak &#8222;=&#8220; a za ním dva znaky &#8211; obsah paměti na dané adrese. Třeba &#8222;M 1C00=00&#8220;. Teď můžete zadat nový obsah, zase pomocí hexadecimální klávesnice a zase se berou pouze poslední dva znaky, a tlačítkem &#8222;=&#8220; potvrdíte změnu a přesunete se k další adrese.

Konec zadávání můžete buď způsobit tím, že stisknete klávesu, kterou program nečeká (třeba EX), nebo resetujete stroj (RE). Tady reset neznamená vymazání paměti, takže to, co jste zadali, v ní zůstává.

Pomocí EX jste program spustili. Po stisku EX zase systém očekává zadání adresy, a po jejím potvrzení tlačítkem &#8222;=&#8220; spouští program.

Sofistikovanější podobu měla funkce Break (BR). Zde jste nejprve zadali adresu, na které se má program přerušit a skočit do monitoru, a v druhém kroku spouštěcí adresu. Jakmile program narazil na danou adresu, odskočil zpět do monitoru, a vy jste si mohli zkontrolovat, zda je vše tak, jak má být.

K tomu vám dopomáhala třeba i funkce kontroly registrů &#8211; klávesa R. Po jejím stisknutí jste si vybrali, od jaké dvojice chcete prohlížet (AF, BC, DE, HL, SP &#8211; viz klávesnice). Zase můžete pomocí klávesnice obsah měnit, tlačítkem &#8222;=&#8220; potvrzovat a přesunout se k další dvojici.

Poslední dvě funkce, S a L, fungovaly pro ukládání (save) a čtení (load) programů na pásek a z pásku.

Ve výše odkazovaném PDFku najdete i další informace, včetně některých užitečných rutin z ROMky. Další spoustu materiálů najdete na [stránkách PMI-80 na Nostalcompu](https://www.nostalcomp.cz/pmi80.php).

## <span id="BOB-85">BOB-85</span>

Tuhle konstrukci postavil ing. Kratochvíl s procesorem 8085 a [publikoval ji, jak jinak, v Amatérském Radiu](https://www.nostalcomp.cz/pdfka/bob_85.pdf), a to včetně dokumentace, výpisu monitoru, několika programů a zdrojových kódů. BOB-85 měl plusmínus stejné schopnosti jako PMI-80. Klávesnice měla 24 tlačítek (6&#215;4) a displej pouhých 6 pozic.

Displej nebyl multiplexovaný, ale každá pozice měla vlastní adresu (0A &#8211; 0F). ROM byla na adresách 0000 až 05FF, RAM od adresy 0600 do 09FF.

Pár zajímavých informací zase najdete na [Nostalcompu (BOB-85)](https://www.nostalcomp.cz/bob85.php), já jsem udělal [online emulátor](https://www.asm80.com/bob85.html), ale je v nějaké alfaverzi a ještě jsem ho nedodělal.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/screenshot-www.nostalcomp.cz-2017-08-27-13-52-15.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2017/08/screenshot-www.nostalcomp.cz-2017-08-27-13-52-15.png)</a>

Ovládání se od PMI-80 trochu lišilo. Nebyla tu možnost měnit obsah registrů nebo nastavovat breakpointy. Prohlížení paměti fungovalo podobně (jen místo klávesy M se jmenovala S MEM), místo klávesy &#8222;=&#8220; byla klávesa &#8222;NEXT&#8220; a při opravě dat nefungoval systém tak, že by obsah posouval, ale zadávali jste ho znovu. Navíc proti PMI tu je možnost postupovat pamětí nejen dopředu (NEXT), ale i dozadu (REC).

Klávesa EXEC vyvolala RESET stroje. Klávesa GO spouštěla program od dané adresy. VEK1, VEK2, a RST měly být nějaké přerušovací vektory do budoucna. A klávesa REC kromě už uvedené funkce vyvolávala ještě funkce pro zápis a čtení programů.

## <span id="KIM-1">KIM-1</span>

Pro nás trochu exotičtější stroj, s těmi jsme se v ČSSR moc nesetkávali. Koncepce opět stejná: procesor (6502), trocha pamětí, šest sedmisegmentovek, klávesnice 6&#215;4, ale hlavně: vyvedený rozšiřující konektor, díky kterému se dalo ke KIM-1 snadno připojit leccos.

[Manuál najdete zde](https://www.kim-1.com/usrman.htm), [online emulátor zde](https://www.asm80.com/kim.html).

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/kim1.jpg" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2017/08/kim1.jpg)</a>

Displej držel stejnou konvenci, 4 pozice adresa, 2 data, ale ovládání bylo trošičku jiné. Tlačítkem AD jste zvolili možnost &#8222;zadám adresu&#8220;, tlačítkem DA jste začali přistupovat k datům. Roli tlačítka &#8222;=&#8220;, respektive &#8222;NEXT&#8220;, tu má tlačítko &#8222;+&#8220;.

Tlačítko GO spustilo program od adresy, která byla na displeji (zadaná po stisknutí AD). Tlačítko PC vyvolalo hodnotu Program Counteru (PC) na displej. RS je RESET, ST je STOP.

Poslední tlačítko nebylo tlačítko, ale přepínač SST &#8211; pokud byl aktivní, fungoval KIM-1 v režimu &#8222;Single Step&#8220;, tedy po stisknutí tlačítka GO vykonal přesně jednu instrukci a opět se zastavil.

## <span id="ET3400">ET3400</span>

U tohoto jednodeskáče emulátor nemám. [Manuál ET3400](https://archive.org/details/HeathkitManualForTheEt-3400MicroprocessorTrainer) je k dispozici na Archive.org nebo jako [PDF](https://www.classiccmp.org/dunfield/heath/et3400.pdf).

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2017/08/heathet3400.jpg" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2017/08/heathet3400.jpg)</a>Tato počítačová stavebnice obsahovala procesor 6800 (ano, ten od Motoroly), a kromě klasických &#8222;jednodeskáčových&#8220; propriet i nepájivé kontaktní pole. Co mě na tomto stroji fascinuje je, že si vystačí s šestnácti tlačítky a RESETem. Každé tlačítko mělo dvě funkce &#8211; kromě hexadecimálního znaku to byl i nějaký povel, a co bylo aktivní, to záleželo na kontextu.

Mimochodem, ten manuál vřele doporučuju, stejně jako manuál ke KIM-1! Není to jen &#8222;jak se to ovládá&#8220;, ale představují i procesor, popisují jeho funkce, učí ho programovat, a ET3400 začíná dokonce konstrukčním návodem (IKEA faktor: 100!)

Čeho jsem chtěl touto exkurzí docílit? Chtěl jsem vás trošku vtáhnout do ducha jednodeskáčů a připravit vás na tu Jednodeskovou Výzvu, která brzy přijde. A nepůjde v ní o nic menšího, než &#8211; a teď poslouchejte &#8211; **navrhnout ovládání nového jednodeskáče a napsat pro něj monitor**. Máte na to pár tlačítek, osm míst na displeji, jedno kilo paměti &#8211; a ukažte se!

(A jestli ještě neumíte assembler 8080 a 6502, tak to [honem napravte](https://strojak.cz/)!)