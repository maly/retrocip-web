---
id: 987
title: 'Jednodesková výzva: druhé kolo'
date: 2017-10-02T18:53:55+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=987
permalink: /jednodeskova-vyzva-druhe-kolo/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "6186230211"
categories:
  - ASM80.com
  - Emulace
  - Hardware
  - Stroják
tags:
  - jednodeskáč
  - Jednodesková výzva
---
Tak co, zkusili jste si jednoduchý obslužný program pro ovládání jednodeskového počítače přes sériovou linku?

Já ano. Zkusil jsem napsat [obslužný program pro model s procesorem Z80](https://gist.github.com/maly/542524230b4805976b32bf45ac2971dc). Ovládání je jednoduché &#8211; máte pouze dva příkazy M a G. Příkaz M slouží ke změně obsahu paměti. Po jeho zadání zadáte adresu (2 bajty, tedy 4 hexadecimální znaky), a pak vám systém vypíše adresu, stávající obsah a čeká na zadání. Pokud nezadáte nový obsah a stisknete enter, pokračuje se na další adresu beze změny. Pokud zadáte nový obsah, uloží se. Zadávání ukončíte stiskem mezery.

Příkaz G si jen vezme adresu a skočí na ni (JP). Nic víc.

Při zadávání hexadecimálních hodnot bere systém poslední dvě (popř. čtyři) hodnoty, takže když chcete zadat třeba 1234 a překlepnete se, tak neřešte žádné mazání, nic takového, prostě zadejte znovu: 12331234 &#8211; a systém vezme jen poslední zadané.

V programu jsem použil pár triků. Například pro velmi často používanou rutinu “pošli na terminál znak” jsem umístil příkaz skoku na adresu 8. Proč? No protože Z80 má osm instrukcí RST, které zabírají jen jediný bajt, a skáčou (podobně jako call) na některou z adres 0, 8, 10, 18, 20, 28, 30, 38 (hexadecimálně). Úspora svou bajtů pro každé volání.

Některé úspory jsou na první pohled méně patrné. Třeba rutina pro výpis dvoubajtové hodnoty, uložené ve dvojici registrů HL (PRINTADDR), obsahuje pouhé tři instrukce: přesune obsah registru H do registru A, zavolá rutinu “vypiš jednobajtovou hodnotu z registru A” (PRINTHEX), pak přesune do registru A hodnotu z registru L &#8211; a dál? No, dál pokračuje rutina PRINTHEX, protože je umístěná šikovně hned za PRINTADDR… Hezký trik, který ušetří jeden skok, ale je potřeba na to myslet, protože jakmile mezi PRINTADDR a PRINTHEX vložíte ještě něco, tak se PRINTADDR rozbije.

Taky je dobrý zvyk nedělat konstrukce typu CALL něco, RET, a místo toho použít rovnou JP něco &#8211; ušetříte tím nejen jednu instrukci, ale i dva bajty na zásobníku.

Ošidil jsem kontrolu hexadecimálních znaků, takže mi jako hexadecimální projde i to, co je v ASCII mezi 9 a A, třeba dvojtečka. To je ještě potřeba pořešit. Ještě bych udělal pár optimalizací, ale dostal jsem se do 256 bajtů, a to je snesitelné.

_PS: Komentáře jsem psal třikrát, a třikrát mi spadnul prohlížeč. Tak tam nakonec nejsou. Pardon, to je velká chyba, já je tam dopíšu. 🙁_

Tak. Pojďme na samotné **druhé kolo**. Přidáme si starou dobrou tlačítkovou klávesnici a sedmisegmentový displej.

Způsobů řešení displeje je mnoho, nejčastěji se používá multiplexované řízení, kdy se pravidelně posílá informace o tom, jaká pozice se adresuje a jaký obsah má mít. Já zvolil jednodušší řešení: každá pozice má vlastní registr a zapisuje se do ní přímo. Konkrétní adresy jsou 0x40, 0x41 atd… Zvolil jsem displej s osmi pozicemi, takže u Z80 to je 0x40 až 0x47 (zleva doprava). U 6502 je zase mapování do prostoru paměti, 0xA040 až 0xA047.

Klávesnice je řešená jako matice tlačítek. Minimálně jich je potřeba 16, pro každou hexadecimální hodnotu jedno tlačítko, ale pohodlnější práce je s maticemi 5&#215;4 nebo 6&#215;4. PMI-80 mělo matici 5&#215;5 (ovšem mapovanou z obvodových důvodů na 3&#215;9). Pro jednoduchost jsem zvolil matici 5&#215;4, kam můžete namapovat 16 kláves s hodnotami a 4 řídicí, resp. funkční tlačítka.

Adresaci jsem zvolil (u Z80) trochu zvláštní: řádky se čtou z různých adres tak, že adresa je vždy 0x80 + 16*číslo řádku. Tedy 0x80 pro první řádek, 0x90 pro druhý řádek, … Pokud není žádné tlačítko stisknuté, je přečtená hodnota 0xFF, pokud je, jsou na pozici bitů 0 až 3 nuly tam, kde jsou stisknutá tlačítka. (Nepřekvapí, že u klávesnice je to 0xA800, 0xA900, 0xAA00, 0xAB00, 0xAC00&#8230;)

Já si navrhl čtyři funkční tlačítka M, G, EX a =, ale vy si klidně navrhněte jiné podle funkcí, které chcete nabídnout. Ale než se k tomu dostaneme&#8230;

Jestli můžeme &#8211; pojďme si nejprve procvičit práci s klávesnicí a displejem úplně jednoduchým příkladem: Udělejme hexadecimální kalkulačku &#8211; přesněji sčítačku, která bude sčítat zadaná čísla. Budete potřebovat jen dvě funkční klávesy: CLR a + Čísla si můžeme omezit, ať se to líp počítá, na dvoubajtové hodnoty. Funkce bude jednoduchá: CLR vynuluje součet, zadáním čísla a + se tato hodnota přičte k mezisoučtu, a ten se zobrazí. 1234 + 21 + 52 + 75 + 3A + 2BFF + …

Ještě [moje EMU soubory](https://gist.github.com/maly/5a67c663f88040213a6efabed9be34c9) pro ASM80 jako gist.

Ať se daří