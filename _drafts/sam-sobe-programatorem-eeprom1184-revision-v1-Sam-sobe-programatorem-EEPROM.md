---
id: 1193
title: Sám sobě programátorem EEPROM
date: 2020-08-15T10:31:40+01:00
author: Martin Maly
layout: revision
guid: https://retrocip.cz/1184-revision-v1/
permalink: /1184-revision-v1/
---
(Kapitola, co se do Portů bajtů&#8230; nevešla)

U všech konstrukcí [OMEN](https://omenmicro.eu/) jsem v zapojení paměti EEPROM navrhl jumper / přepínač, většinou nazvaný WREN – WRite ENable, a zároveň jsem psal, že při práci by měl být zapojený tak, aby na vstupu /WE byla stále logická 1.

Tak proč tam tedy je? Proč jsem ho nepřipojil přímo na +5 voltů?

Když je vstup /WE u paměti EEPROM připojený na logickou 1, je zakázaný zápis. A to je dobře, tak to má být a tak to chceme. Většinou chceme z&nbsp;paměti (EEP)ROM jen číst. Pokud do ní nějaký kód zapisuje, je to většinou omylem, a kdyby se zapisovat mohlo, přineslo by to spíš mrzení, než užitek.

<div class="wp-block-image">
  <figure class="alignright size-large is-resized"><img loading="lazy" src="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/pinheader3.jpg" alt="" class="wp-image-1185" width="244" height="244" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/pinheader3.jpg 360w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/pinheader3-150x150.jpg 150w" sizes="(max-width: 244px) 100vw, 244px" /></figure>
</div>

Ale existují i situace, kdy je dobré zápis povolit. Například tehdy, kdy chcete aktualizovat firmware, obslužný program, nebo do EEPROM přidat nějaké rozšíření. Což jsou věci, které čas od času mohou být k&nbsp;užitku, proto je dobré je nějakým způsobem umožnit, ale zároveň je člověk nepoužívá denně a rutinně, tak nevadí, když jejich povolení je trochu netriviální. Pokud možno tak složité, aby k&nbsp;němu nemohlo dojít náhodou. Ideální je proto použít takzvaný pin header, neboli tři kovové kolíčky, a věc, které se říká „shunt“ nebo též „jumper“, a která vodivě spojí vývody 1-2, nebo 2-3.

Na této ukázce ze schématu OMEN Alpha je zmíněný přepínač zakreslený s&nbsp;označením JP5. Pokud jsou spojené vývody 1 a 2, je na vstup /WE přivedeno napájecí napětí. Pokud jsou spojené vývody 2 a 3, je na tento vstup přiveden signál /WR od procesoru. Podobné zapojení je i u Brava a Echa, protože princip práce je stále stejný.<figure class="wp-block-image size-large">

<img loading="lazy" width="972" height="1024" src="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/wren-972x1024.png" alt="" class="wp-image-1186" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/wren-972x1024.png 972w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/wren-617x650.png 617w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/wren-768x809.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/wren-1457x1536.png 1457w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/wren.png 1557w" sizes="(max-width: 972px) 100vw, 972px" /> </figure> 

Asi tušíte, že v&nbsp;zápisu do paměti EEPROM bude nějaký háček, a tušíte správně. Asi ten hlavní háček, přímo hák, je ten, že se něco nepovede. Data se zapíšou špatně, program bude mít chyby, přepíšete si základní programové vybavení, a po restartu budete mít zařízení ve stavu „brick“, tedy funkční obdobu cihly.

Druhý háček vyplývá ze samotného principu zápisu dat do paměti EEPROM. Z&nbsp;hlediska připojení se taková paměť neliší od RAM: přivedete data na datovou sběrnici, adresu na adresní, aktivujete signály /CS a /WE, a je to! Jenže EEPROM je přeci jen primárně ROM, tedy paměť pouze ke čtení, a ačkoli máme možnost přepsat data, má to své záludnosti. Třeba to, že zápis trvá docela dlouho, a po tu dobu by paměť neměla být rušena, a už vůbec by po ní neměl někdo chtít nějaká data.

Většina pamětí EEPROM, a platí to i pro oba typy, co v&nbsp;konstrukcích používáme, tedy AT28C64 a AT28C256, umí zapsat data buď po jednotlivých bajtech, nebo po celých blocích, ale poté, co zapíšeme data, respektive celý blok, musíme čekat. U těchto pamětí je čekací doba až 10 milisekund; u jejich rychlejších variant, třeba 28C256F, to jsou 3 milisekundy.

<blockquote class="wp-block-quote">
  <p>
    To fakt není žádná super rychlost. Kdybychom zapisovali bajt po bajtu do paměti OMEN Alpha, což je 32 kB, tak to bude trvat 32768&nbsp;x 10 ms = 327,68 s, což je něco přes pět minut. Pokud využijeme zápisu po blocích 64 bytů, zvládneme to za pět sekund.
  </p>
</blockquote>

Důsledkem tohoto omezení je, že program, který zapisuje do EEPROM, nemůže být v&nbsp;této EEPROM zároveň spuštěný. Po zápisu bajtu by procesor přistupoval k&nbsp;téže paměti. Paměť by to brala jako znamení, že má začít zapisovat, na 10 milisekund by se odmlčela, ale procesor by během těch deseti milisekund z&nbsp;téže paměti četl instrukce. Jenže po dobu zápisu paměť posílá na datovou sběrnici informace o tom, že zapisuje, resp. že už zápis skončil, takže by se místo instrukcí četly nesmysly, program by okamžitě zhavaroval, a _brick_ by nastal dřív, než stačíte říct „RESET!“

Takže dvě zlatá pravidla:

<ol type="1">
  <li>
    Program, který zapisuje do EEPROM, musí celý kompletně běžet v&nbsp;RAM a musí zajistit, že během zápisového cyklu se na sběrnici neobjeví žádná adresa, která by aktivovala paměť EEPROM.
  </li>
  <li>
    Program, který přepisuje EEPROM, musí být trojnásob opatrný než běžný program. Všechna data zkontrolujte dřív, než je prohlásíte za zapsaná, raději pomaleji, ale bezpečně, nezapomeňte zakázat všechna přerušení a modlete se, aby odněkud nepřišlo nemaskovatelné přerušení.
  </li>
</ol>

## Jak zapsat do EEPROM?

Samotný zápis je úplně stejný jako při zápisu do RAM. Použijte jakoukoli instrukci, která zapisuje do paměti, a pokud bude adresa odpovídat EEPROM a předtím povolíte zápis do EEPROM (hardwarovým přepínačem, viz výše), paměť začne ukládat zapsaný bajt na zadanou adresu. Během zápisu z&nbsp;paměti nelze číst data, namísto toho načítáte stavové informace.

Blokový zápis umožňuje zapsat až 64 po sobě jdoucích bajtů najednou. Pro zápis bloku není potřeba žádná speciální instrukce, prostě zapisujete na adresu a, a+1, a+2, &#8230; Stačí dodržet dvě prostá pravidla:

  * Mezi zapsanými daty nesmí uplynout víc než 150 mikrosekund, což není většinou problém; u Alphy to vychází na 276 hodinových taktů, během té doby stačíme spolehlivě připravit nový bajt a zapsat ho.
  * Všechny adresy v&nbsp;bloku musejí mít stejnou hodnotu v&nbsp;bitech A6 – A14 (u 28C256; u paměti 28C64 to jsou bity A6 – A12). Při zápisu bloků můžete měnit pouze bity A0 – A5, tedy nejnižších 6 bitů adresy.

Jakmile tato pravidla nedodržíte, tj. nastane větší časová prodleva nebo další adresa překročí hranici bloku 64 bajtů, paměť další data zahodí a spustí interní zápisový cyklus.

Po spuštění interního zápisového cyklu paměť další požadavky na zápis ignoruje. Tento cyklus může trvat až 10 milisekund, jak jsme si už řekli. Během této doby paměť informuje o tom, že se zapisují data, prostřednictvím dvou mechanismů, /DATA Polling a Toggle bit.

/DATA Polling spočívá v&nbsp;tom, že program čte data ze stejné adresy, kam naposledy zapisoval. Dokud trvá zápis, je v&nbsp;nejvyšším bitu D7 invertovaná hodnota zapisované hodnoty. Tedy pokud jste jako poslední hodnotu zapsali například 0x55 (což je binárně 01010101) na adresu 0xAAA, budete při čtení z&nbsp;téže adresy načítat hodnotu, která má v&nbsp;nejvyšším bitu 1. Jakmile cyklus zápisu skončí, bude načtená hodnota odpovídat tomu, co jste do paměti zapsali.

Druhá metoda kontroly je bit toggling – během cyklu zápisu se při čtení bude hodnota bitu D6 pravidelně měnit 0 – 1 – 0 – 1 atd. Když cyklus zápisu skončí, hodnota D6 se měnit přestane.

Obě tyto metody můžete použít k&nbsp;určení, zda je bezpečné zapisovat další data. Alternativní postup je prostě vyčkat deklarovaných 10 milisekund. Ale reálná kontrola stavu pomocí technik /DATA polling nebo toggle bit je nejjistější.

Můžete se inspirovat malým ukázkovým programem (pro OMEN Alpha), kde je použita právě technika /DATA polling: <https://8bt.cz/eepw>