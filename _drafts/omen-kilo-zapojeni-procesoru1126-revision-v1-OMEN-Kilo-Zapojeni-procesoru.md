---
id: 1136
title: 'OMEN Kilo: Zapojení procesoru'
date: 2018-12-07T23:13:34+01:00
author: Martin Maly
layout: revision
guid: https://retrocip.cz/1126-revision-v1/
permalink: /1126-revision-v1/
---
Čím jiným začít, než fyzickým zapojením procesoru 6809&#8230;<figure class="wp-block-image">

<img loading="lazy" width="7536" height="3000" src="http://retrocip.cz/wp-content/uploads/sites/6/2018/12/MC6809-pinout-e1543756426392.png" alt="" class="wp-image-1127" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/12/MC6809-pinout-e1543756426392.png 7536w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/MC6809-pinout-e1543756426392-650x259.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/MC6809-pinout-e1543756426392-768x306.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/MC6809-pinout-e1543756426392-1024x408.png 1024w" sizes="(max-width: 7536px) 100vw, 7536px" /> </figure> 

Možná nepřekvapí jistá podobnost s&nbsp;procesorem 6502. Je to logické, protože, jak jsme si už několikrát řekli, 6502 i 6809 vycházejí ideově z&nbsp;téhož předchůdce, 6800.

### Popis vývodů

Procesor má vyvedené obě hlavní sběrnice, jak datovou (D0 – D7), tak adresní (A0 – A15). Tady nás nečeká žádné překvapení. Signály nejsou multiplexované a jsou plně k&nbsp;dispozici.

Vstup /RESET slouží k&nbsp;inicializaci procesoru do výchozího stavu. Na vstupu je Schmittův klopný obvod, takže stačí jednoduché zapojení s&nbsp;kondenzátorem a rezistorem, podobně jako u předchozích počítačů. Po RESETu se načte adresa (2 bajty) z&nbsp;adres FFFEh a FFFFh a získaná hodnota se použije k&nbsp;nastavení programového čítače PC. _Nezapomeňte, že 6809 používá ukládání ve stylu Big Endian, tedy nejprve vyšší část adresy, poté nižší. Vyšší část tedy bude na adrese FFFEh, nižší na FFFFh._

Výstup R/W informuje okolí o tom, jestli procesor hodlá číst (=1), nebo zapisovat (=0). Opět, podobně jako u 6502, se nerozlišují periferie a paměť, pro procesor je vše „paměť“.

Pro připojení krystalu se používají vývody XTAL a EXTAL. Pokud použijete externí generátor časovacích pulsů, přiveďte signál na vstup EXTAL a XTAL nechte nezapojený.

Uvnitř poběží procesor na frekvenci, která je rovna čtvrtině frekvence krystalu (popřípadě přivedené frekvenci). U OMEN Kilo jsem tedy použil frekvenci 7,3728 MHz, která po podělení čtyřmi dává naši oblíbenou pracovní frekvenci 1,8432 MHz.

<blockquote class="wp-block-quote">
  <p>
    Existují tři varianty tohoto procesoru, které seliší podle maximální pracovní frekvence: 6809, 68A09 a 68B09. Varianta bezpísmene pracuje na frekvenci 1 MHz, varianta A používá 1,5 MHz a varianta Bmůže pracovat až s&nbsp;frekvencí 2 MHz (to znamená, že externí krystal můžemít frekvenci 4, 6, resp. 8 MHz).
  </p>
</blockquote>

Pro řízení vnějších obvodů slouží hodinové signály E a Q. E má stejnou funkci, jakou má u 6502 výstup PHI2. Sestupnou hranou na výstupu E oznamuje procesor, že začíná operační cyklus. Poté přijde vzestupná hrana na výstupu Q a oznámí, že na adresní sběrnici je platná adresa. Následuje vzestupná hrana signálu E, sestupná signálu Q (bez speciálního významu) a celý cyklus ukončuje (a zároveň zahajuje nový) sestupná hrana signálu E, při níž přečte procesor data ze sběrnice.

<figure class="wp-block-image">

<img loading="lazy" width="2000" height="500" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/12/6809time-1.png" alt="" class="wp-image-1134" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/12/6809time-1.png 2000w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/6809time-1-650x163.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/6809time-1-768x192.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/6809time-1-1024x256.png 1024w" sizes="(max-width: 2000px) 100vw, 2000px" /> </figure> 

Pro většinu jednoduchých případů můžeme signál Q (Quadrature time) ignorovat a pracovat pouze se signálem E, stejně jako u 6502.

<blockquote class="wp-block-quote">
  <p>
    Pozor na variantu 6809E! Tato varianta nemá interní oscilátor, vyžaduje externí generátor dvoufázových hodin a zapojení vývodů je lehce odlišné.
  </p>
</blockquote>

Výstupy BA a BS informují o tom, zda je procesor odpojený od sběrnice (ve stavu HALT / DMA) či zda reaguje na požadavek na přerušení apod. Za normálního chodu jsou oba v&nbsp;0 a pokud nepotřebujete nějak speciálně reagovat na výjimečné stavy, můžete oba ignorovat.

Vstup MRDY slouží k&nbsp;témuž, k&nbsp;čemu sloužily vstupy READY, /WAIT či RDY – informuje o tom, že externí obvody nezpracovaly požadavek a že potřebují, aby procesor počkal. Pokud je tento vstup v&nbsp;log. 0, hodiny se zastaví ve stavu E=1, Q=0 a čeká se, až bude MRDY opět 1. Teprve pak procesor dokončí čtení dat, popřípadě přestane posílat data k&nbsp;zápisu, a pokračuje se dál.

Vstup /DMA/BREQ slouží k&nbsp;přerušení práce procesoru a jeho odpojení od sběrnice. Pokud je na tomto vstupu na konci aktuálního cyklu 0, procesor se uvede do stavu DMA a uvolní sběrnici. Uvolnění sběrnice signalizuje nastavením BS = BA = 1. Okolní zařízení teď má až 15 cyklů na to, aby si udělalo, co je potřeba. Po 15 cyklech si procesor vezme dva cykly pro vnitřní refresh a na tu dobu nastaví BS = BA = 0. Pokud stále trvá požadavek na DMA, celý postup se opakuje.

Vstup /HALT zastaví procesor podobně jako předchozí vstup. Do stavu HALT se procesor přepne poté, co dokončí aktuálně prováděnou instrukci. Opět se odpojí od datové i adresní sběrnice, odpojí i výstup R/W, a nastaví BS = BA = 1.

### Přerušení

Poslední tři vstupy, o nichž jsem se ještě nezmínil, jsou vstupy /NMI, /FIRQ a /IRQ. Jejich stav je vyhodnocován ve chvíli sestupné hrany Q (E=1).

/NMI je dobře známé „nemaskovatelné přerušení“. Logická 0 na tomto vstupu vyvolá přerušení, které programátor nemůže programově zamaskovat. Jediná výjimka je těsně po RESETu, dokud není nastaven obsah v&nbsp;registru ukazatele zásobníku (S). Po detekování požadavku na NMI je na zásobník automaticky uložen obsah registrů PC, U, Y, X, DP, A, B a CC a procesor si z&nbsp;adres FFFCh a FFFDh načte adresu obsluhy nemaskovatelného přerušení. Toto přerušení má zároveň nejvyšší prioritu.

Téměř identicky se chová požadavek /IRQ. Dva rozdíly tu ale jsou. Zaprvé: adresa obsluhy přerušení se bere z&nbsp;adres FFF8h a FFF9h. A zadruhé: pokud je bit I v&nbsp;registru CC roven 1, přerušení se nevykoná.

Vstup /FIRQ (Fast IRQ) funguje obdobně jako IRQ. I tento vstup vyvolá přerušení (adresa obsluhy je v&nbsp;paměti na FFF6h a FFF7h), i toto přerušení lze zamaskovat bitem F v&nbsp;registru CC, ale hlavní rozdíl je, že neukládá na zásobník žádné jiné registry, jen programový čítač PC a registr příznaků CC.

## OMEN Kilo CPU

Základní deska, která vychází z&nbsp;koncepce OMEN Bravo. Obsahuje jen procesor, 32 kB RAM, 8 kB EEPROM, dekodér signálů a nezbytné obvody okolo (RESET, krystal). Nakonec jsem k „nezbytným obvodům“ přidal i nám důvěrně známý 68B50 pro sériovou komunikaci. Díky tomu může být OMEN Kilo použit jako jednodeskový počítač, bez jakýchkoli dalších obvodů.<figure class="wp-block-image">

<img loading="lazy" width="3420" height="2750" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/12/kilo-cpu.png" alt="" class="wp-image-1129" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/12/kilo-cpu.png 3420w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/kilo-cpu-650x523.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/kilo-cpu-768x618.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/12/kilo-cpu-1024x823.png 1024w" sizes="(max-width: 3420px) 100vw, 3420px" /> </figure> 

_Pokračování příště&#8230;_