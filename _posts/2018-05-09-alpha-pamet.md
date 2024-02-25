---
id: 1061
title: 'Alpha: paměť'
date: 2018-05-09T08:24:16+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1061
permalink: /alpha-pamet/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "6660519856"
image: https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-driver-1140x198.png
categories:
  - Hardware
tags:
  - "8085"
  - EEPROM
  - OMEN Alpha
  - paměť
  - RAM
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Budic_sbernice"><span class="toc_number toc_depth_1">1</span> Budič sběrnice</a>
    </li>
    <li>
      <a href="#EEPROM"><span class="toc_number toc_depth_1">2</span> (EEP)ROM</a>
    </li>
    <li>
      <a href="#RAM"><span class="toc_number toc_depth_1">3</span> RAM</a>
    </li>
    <li>
      <a href="#Pametovy_subsystem_a_adresace"><span class="toc_number toc_depth_1">4</span> Paměťový subsystém a adresace</a>
    </li>
  </ul>
</div>

Samotný procesorový obvod, jak jsme si ho navrhli v minulé kapitole, k ničemu není. Respektive &#8211; můžete ho zapojit,připojit datovou sběrnici přes rezistory (třeba 10k) na zem a sledovat, jak se na nejvyšším bitu adresy (A15) mění 1 a 0, protože procesor stále dokola čte celou paměť od 0000 do FFFFh, všude najde operační kód 00 (to jsou ty rezistory připojené k zemi), ten znamená &#8222;nedělej nic a načti další instrukci&#8220;, takže se obsah registru PC (Program Counter) zvýší o 1 a celý cyklus se opakuje. Když si k A15 připojíte LEDku přes rezistor, měla by bliknout cca 7x za sekundu. To znamená, že celý paměťový rozsah zvládne na dané frekvenci projít procesor za sekundu sedmkrát.

Právě jste stvořili úžasný 16bitový čítač. Ale to asi nebude to, co chceme. Musíme zajistit, aby procesor měl covykonávat. Musíme mu dát paměť, kde bude program a data.

Procesor po spuštění nemá žádný mechanismus, jak třeba vzít program odněkud odjinud a nahrát ho do paměti. O tose musíte postarat sami. A nejčastější způsob je ten, že použijete paměť ROM (spíš EPROM či EEPROM / FLASH), vníž je uložený nějaký základní ovládací program.

> Tím programem byl nejdřív takzvaný bootstrap, tedy krátký program (512 byte třeba), který dokázal načíst datatřeba z pásky, ze čtečky karet, z pevného disku, zkrátka odněkud, nahrát je do paměti a spustit. S podobnýmpostupem se můžeme setkat třeba u Arduina: zde je v procesoru uložený bootstrap loader, který čeká, jestlináhodou počítač neposílá nějaká (přesně daná) data. Pokud ano, přepne se do programovacího módu a nahrajepřicházející data do paměti.
> 
> Později se z bootstrapu stal Monitor. Monitor byl už o něco větší program, který už komunikoval s obsluhou a uměl provést jednoduché příkazy, např. pro prohlížení paměti, její změnu, načtení programu z externích pamětí a jeho spuštění. Pokud pamatujete na počítač PMD-85 v první verzi, tak se po zapnutí ohlásil právě monitor. Vy jste pomocí MGLD mohli nahrát do paměti něco z magnetofonu, spustit to pomocí JUMP, nebo z externího rompacku stáhnout a spustit BASIC G
> 
> U některých počítačů, většinou domácích, už se monitor ani nepoužíval, už startoval rovnou vyšší jazyk (BASIC).
> 
> Profesionální počítače, určené třeba pro práci s CP/M, se zase pomalu vrátily k malým obslužným programům. Neříkalo se jim bootstrap, na to byly příliš velké, a ani Monitor, protože nekomunikovaly s obsluhou. Ujala se zkratka BIOS podle jedné ze základních částí CP/M (Basic Input Output System). BIOS je spíš kolekce rutin aprogramů, která ovládá hardware konkrétního počítače. Systém nemusí řešit, kde přesně a jak je připojen disk, to ví právě rutina v BIOSu. Po startu se spustí CP/M loader právě z BIOSu.
> 
> U jednodeskových počítačů se používal model &#8222;Monitor&#8220;. Takový monitor se vešel i do 1 kB ROM. K tomu nabídl jednodeskáč třeba 1 kB RAM, a bylo!

Chtěl jsem dodržet ducha doby, ale na rovinu přiznávám, že tehdejší paměti, třeba 2kB EPROM a 1 kB SRAM, jsou už dnes dílem nesehnatelné, dílem sice sehnatelné, ale zato se s nimi mizerně pracuje&#8230; Opravdu vás nechci nutit shánět ultrafialovou mazačku EPROM!

Proto nepoužijeme EPROM, použijeme EEPROM, tu můžeme vymazat jednodušeji. I naprogramovat&#8230;

## <span id="Budic_sbernice">Budič sběrnice</span>

Ale ještě předtím by bylo fajn udělat jednu úpravu. Totiž posílit a oddělit datovou sběrnici. Procesory obecně nemají příliš silné výstupní obvody, a když na datovou sběrnici připojíte víc obvodů, přetížíte ji. Se dvěma paměťovými obvody, obvodem ACIA a bufferem na adresu budeme už na hraně. Proto před tím, než budeme cokoli přidávat, připojíme posilovač.

> Mimochodem: Pravidlo pro nízkou spotřebu a malé zatížení sběrnice zní: Používat CMOS kde to jde. Vyhněte seobvodům TTL (74, 74LS, 74ALS apod.) a hledejte CMOS verze (74HC, 74HCT, 74ACT). Totéž platí i pro periferní obvody &#8211; například dále probíraný obvod PIO 8255 lze sehnat i v CMOS verzi 82C55, která má nižší spotřebu a téměř nezatěžuje sběrnici. CMOS verze bývají většinou plně kompatibilní se svojí předlohou, občas bývají mírně vylepšené&#8230;

Použijeme obvod 74HCT245, což je osmice obousměrných bufferů a budičů. Vstupem DIR se určuje směr toku dat (0= z B do A, 1= z A do B), vstupem G se přepíná obvod do stavu vysoké impedance (pokud je vstup G=1, je obvod odpojen a výstupy A i B ve stavu vysoké impedance).

Spojil jsem vstup G se signálem ALE, díky tomu je ve chvíli, kdy procesor na sběrnici posílá spodní část adresy, obvod odpojen a neruší. Takže kdyby nějaký periferní obvod chtěl posílat data ve chvíli, kdy procesor řeší latchování nižší části adresy (ALE=1), je datová sběrnice oddělená.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-driver.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-driver-650x318.png)</a>

Díky tomuto zapojení jsou k datové sběrnici připojeny maximálně dva obvody, a díky signálu ALE jsou zapojeny &#8222;proti sobě&#8220;, tedy vždy je aktivní jen jeden z nich. Celá &#8222;procesorová&#8220; část vypadá tedy takto:

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-cpu-1.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-cpu-1-650x483.png)</a>

## <span id="EEPROM">(EEP)ROM</span>

Já vím, není to nic úplně jednoduchého, co by se vám válelo doma, ale doporučuji buď koupit programátor pamětí EEPROM, nebo si ho postavit z Arduino MEGA. Není potřeba vymýšlet kolo, návody už existují a většinou stačí jen propojit vývody z MEGY s vývody paměti: <https://danceswithferrets.org/geekblog/?page_id=903> &#8211; já to dělám zrovna tak, použiju nepájivé kontaktní pole, to propojím s Arduinem Mega, do pole pak zasouvám paměti a programuju jako po másle.

Použijeme EEPROM o velikosti 32 kB. Tyto paměti se vyrábějí pod označením 28C256 (podle výrobce např.AT28C256). Jsou dobře dostupné a mají i vhodné pouzdro DIP.

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/AT28C256-pinout.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2018/05/AT28C256-pinout-527x650.png)</a>

Datové vývody D0-D7 není třeba představovat. A0 až A14 je naše známá sběrnice s adresou (15 bitů = 32 kB) a tři signály slouží k ovládání paměti. /WE povoluje zápis (my ho v systému připojíme natvrdo k log. 1 a tím zápis zakážeme &#8211; paměť naprogramujeme v programátoru.) /CE (Chip Enable) říká paměti, že má reagovat na pokyny -pokud je 0, paměť reaguje, pokud 1, paměť se odpojí. A konečně /OE (Output Enable) povoluje čtení z paměti, tzn. žena výstupech IO0-IO7 je vystavena hodnota, uložená v paměti na adrese dané adresovými vstupy.

No jo, ale co když budeme potřebovat třeba jen 8 kB ROM? V takovém případě zkrátka připojíte vstupy A13 a A14 na log. 0 (zem), a v paměti budou tři čtvrtiny prostoru nevyužité. Takový způsob zapojení se někdy používá i záměrně &#8211; v jedné paměti máte např. čtyři alternativní firmwary, a buď pomocí mechanických přepínačů, nebo pomocí nějaké vnitřní logiky, si můžete volit, který firmware chcete použít. (I některé počítače to tak měly &#8211; vzpomínáte na ZXSpectrum 128 a jeho dva BASICy?)

## <span id="RAM">RAM</span>

Paměť RAM použijeme typu 62256. Její vývody jsou rozložené takto:

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/62256-pinout.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2018/05/62256-pinout-527x650.png)</a>

Letmým pohledem zjistíme, že jsou obě paměti zapojeny naprosto stejně! Až na určité rozdíly ve značení. Místo /CE(Chip Enable) je signál /CS (Chip Select), ale význam je stejný.

## <span id="Pametovy_subsystem_a_adresace">Paměťový subsystém a adresace</span>

Máme tedy dva kusy paměti, každý s velikostí 32 kB, a jeden procesor s adresním prostorem 64 kB. Úplně se nabízí rozdělit dostupný prostor na dvě poloviny, jedna bude od 0 do 32767, druhá od 32768 do 65535, v jedné bude ROM, ve druhé RAM. Otázka pro bystré hlavy: Kde bude která paměť?

Odpověď se skrývá v předchozích odstavcích: procesor po RESETU začne provádět program od adresy 0000h, takže na téhle adrese by měla být ROM. Ergo ROM bude na adresách 0000h &#8211; 7FFFh, RAM na adresách 8000h &#8211; FFFFh.

A teď: jak budeme připojovat? Oba obvody připojíme naprosto stejně: datovou sběrnici na datovou sběrnici procesoru, adresovací signály A0 až A14 na odpovídající vývody procesoru. Vývod A15 z procesoru necháme zatím stranou, ten použijeme později.

Signál /OE (Output Enable) připojíme na signál /RD. Připomeňme si: Signálem /RD říká procesor, že bude číst. Vstup/OE u paměti slouží k tomu, že povolí výstup uložených informací na datovou sběrnici. To je vlastně přesně to, co potřebujeme: když chce procesor číst, pošle signál /RD, a ten &#8222;otevře&#8220; výstup naší paměti.

Analogicky připojíme vstup /WE (Write Enable) u paměti RAM k signálu /WR u procesoru. Jen u RAM, u EEPROM ne. Asi nechceme, aby náhodná chyba v programu přepisovala obsah paměti EEPROM&#8230;

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-justmem.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-justmem-650x394.png)</a>

Poslední vstup, který zbývá, je Chip Select (/CS, u RAM značen jako /CE, ale funkce je stejná). To je &#8222;univerzální povolovací vstup&#8220;. Pokud je neaktivní (log. 1), tak paměť ignoruje vše, co se na ostatních vývodech děje. Pokud je aktivní (log. 0), vykonává operace čtení a zápisu podle výše popsaných signálů.

Protože jsme spojili paralelně datové výstupy obou pamětí, bylo by fajn nějak zajistit, aby v jeden okamžik posílala svoje informace jen jedna paměť. Která? No to záleží na stavu vodiče A15.

Vodič A15 je v log. 0 tehdy, když procesor přistupuje na adresy 0000h &#8211; 7FFFH, v log. 1 pokud přistupuje na horní polovinu prostoru. Pokud je tedy A15 = 0, měla by být vybrána paměť EEPROM, pokud je A15 = 1, měla by být vybrána paměť RAM.

Ovšem ne vždy! Jen tehdy, když procesor přistupuje k paměti! Vzpomeňte si &#8211; k tomu slouží signál IO/M. Pokud procesor chce přistupovat k paměti, je tento signál v log. 0.

Tedy shrnuto:

| Stav /CE                                       | &#8230;pokud je            |
| ---------------------------------------------- | -------------------------- |
| /CE u paměti EEPROM _(__/ROMCS__)_ aktivní (0) | A15 = 0 a zároveň IO/M = 0 |
| /CE u paměti RAM _(__/RAMCS__)_ aktivní (0)    | A15 = 1 a zároveň IO/M = 0 |

Pro paměť EEPROM jde tedy o logickou funkci **A15 OR IO/M**, pro paměť RAM to je **(****NOT A15****)** **OR IO/M**. Abychom zbytečně nepoužívali hradla OR a NOT, použijeme jedno pouzdro 74HCT00 &#8211; tedy 4 x NAND, a to takto:

/ROMCS = (NOT A15) NAND (NOT IO/M)

/RAMCS = A15 NAND (NOT IO/M)

Jedno hradlo bude invertovat IO/M, druhé bude invertovat A15, třetí vytvoří signál /ROMCS, čtvrté /RAMCS.

A máme to! Kompletní schéma je zde:

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-mem.png" rel="lightbox">![](https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-mem-650x650.png)</a>Můžete si zkusit vytvořit první program, nahrát si ho do EEPROM a nechat blikat LEDku na výstupu SOD.