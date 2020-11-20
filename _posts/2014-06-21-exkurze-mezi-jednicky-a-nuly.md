---
id: 333
title: Exkurze mezi jedničky a nuly
date: 2014-06-21T13:50:18+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=333
permalink: /exkurze-mezi-jednicky-a-nuly/
dsq_thread_id:
  - "2783802966"
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2014/01/image003-600x198.jpg
categories:
  - Hardware
tags:
  - "8080"
  - logické obvody
  - mikroelektronika
  - Procesor
  - Z80
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Nahrubo8230"><span class="toc_number toc_depth_1">1</span> Nahrubo&#8230;</a>
    </li>
    <li>
      <a href="#8230_a_najemno"><span class="toc_number toc_depth_1">2</span> &#8230; a najemno</a><ul>
        <li>
          <a href="#Hradla"><span class="toc_number toc_depth_2">2.1</span> Hradla</a>
        </li>
        <li>
          <a href="#Kombinacni_obvody"><span class="toc_number toc_depth_2">2.2</span> Kombinační obvody</a>
        </li>
        <li>
          <a href="#Zpetna_vazba"><span class="toc_number toc_depth_2">2.3</span> Zpětná vazba</a>
        </li>
        <li>
          <a href="#Klopne_obvody"><span class="toc_number toc_depth_2">2.4</span> Klopné obvody</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Mikroprocesor"><span class="toc_number toc_depth_1">3</span> Mikroprocesor</a>
    </li>
    <li>
      <a href="#Jak_to_tedy_je_s_tim_programovanim"><span class="toc_number toc_depth_1">4</span> Jak to tedy je s tím programováním?</a>
    </li>
  </ul>
</div>

Pojďte se podívat, jak se stane, že procesor procesuje!

<!--more-->

Před nedávnem jsem dostal milý mail. Poslal ho Dave de Sade, známý [nespokojený hráč](https://angryplayer.blogspot.cz/), a psal v něm:

> (&#8230;) dříve jsem psal slušně v BASICu, hrál jsem si s assemblerem na Didaktiku, ale s příchodem QBasicu a modernějších jazyků mi totálně ujel vlak. Co ale vím je to, že programování je o analýze zadání a pak o rozkouskování na co nejjednodušší kroky, které se vyjádří programovacím jazykem. Jako laik vidím návaznost závislostí asi takto:
> 
>   * gamesa
>   * aplikační knihovny
>   * operační systém
>   * programovací jazyk
>   * assembler
>   * jedničky a nuly
>   * ?????
>   * tranzistory v procesoru
>   * elektřina
>   * bůh
>   * PROFIT
>   * opravdovej bůh
>   * JEŠTĚ VĚTŠÍ PROFIT
> 
> No a o ty otazníky mi právě jde. I když dokážu pochopit, jak složení primitivních příkazů vystaví z jedniček a nul komplikovanou gamesu či jiný kus softwaru, od malička mi hlava nebere, jak přesně jsou instrukce procesoru konstruované na úrovni HW. Nevím, jak si to mám vůbec představit. Jakože &#8211; co znamená &#8222;spustit program&#8220; z hlediska procesoru? To se jako vydá elektřina do nějakého drátku, který vede do jedné specifické nožičky procesoru, tam se přepne hradlo tranzistoru z nuly na jedna&#8230; a pak?

Jestli chcete vědět, co se skrývá za těmi otazníky, čistě ze zvědavosti, a nečekáte, že po přečtení článku budete schopni si postavit vlastní počítač z integráčů a ploché baterky, tak směle do čtení!

Na jedné straně, řekněme že nahoře, jsou ty Davem zmiňované jedničky a nuly, co vypadly z assembleru jako že strojový kód, na druhé straně, té spodní, jsou nějaké tranzistory v procesoru, poháněné elektřinou, a co je tedy mezi tím?

Nejdřív si to popíšeme nahrubo odshora dolů, a pak najemno odspodu nahoru, a budeme to brát v té úplně dřevní, osmibitové podobě, protože jinak bychom zabředli do prefetchí a cache a podobných věcí, které jsou sice fajn, ale základní princip spíš zamlží.

## <span id="Nahrubo8230">Nahrubo&#8230;</span>

Jedničky a nuly, co vznikly překladačem, jsou uložené do paměti. Paměť je jedna z částí počítače, a je to zase nějaký integrovaný obvod. Paměti jsou dvojího druhu: RAM a ROM, to podle toho, jestli se do nich dá i zapisovat (RAM, tedy správněji RWM &#8211; Read Write Memory &#8211; ale používá se nepřesné Random Access Memory), nebo jestli z nich lze jen číst (ROM). Kromě toho jsou paměti operační (tj. ta, ke kterým přímo leze procesor) a velkokapacitní (třeba disketa, magnetofon, děrná páska). Náš program je uložen, dejme tomu, v operační paměti typu ROM a jedná se o zavaděč, Monitor, BIOS, zadrátovaný BASIC, zkrátka o to, co se spustí úplně jako první, když zapnete počítač.

ROM je obvod se spoustou vývodů. Je tam napájení, jsou tam nějaké řídicí vstupy, a je tam třeba 14 adresních vstupů (já je označím A0 až A13, taková je konvence) a 8 datových výstupů (D0 až D7). Každý vstup nebo výstup si představte jako jeden vodič, jako jeden &#8222;drát&#8220;, kde buď nějaké napětí je (=logická 1), nebo není (=logická 0). Osm datových vodičů tedy dá dohromady osmibitové slovo &#8211; bajt. 14 adresních vstupů dává dohromady 16kB prostor, což je třeba tolik, kolik měla ROM v ZX Spectru.

Datové vodiče D0 až D7 tvoří dohromady osmibitovou _datovou sběrnici._ Vodiče s adresou A0 až A13 jsou připojeny na adresovou sběrnici. Ta měla u osmibitových procesorů většinou 16 bitů (tedy A0 až A15), takže mohla adresovat víc než těch 16 kB (čtyřikrát víc, tedy 64 kB).

Tak. Paměť máme, její adresová sběrnice A0-A13 je připojena na systémovou adresovou sběrnici A0-A15 tak, že A0 je připojeno na A0 atd. Bity A14 a A15 nejsou do paměti zavedené přímo, ale jsou zapojeny na adresní dekodér, který řekne třeba: Když je A14=0 a A15=0, tak procesor bude číst z paměti ROM, když to bude jinak, tak z jiné paměti (třeba RAM). Jednoduchý kombinační obvod tedy hlídá, jestli je na adresní sběrnici kombinace A14=A15=0, a pokud ano, pošle paměti ROM signál &#8222;Hele, asi budou chtít něco po tobě!&#8220;

Signál je zase naprosto jednoduchý: je to další vstup v paměti, další &#8222;drát&#8220;, který se jmenuje třeba CE (Chip Enable). Když je na něm logická 1 (tzn. je tam napětí), tak paměť vezme hodnoty z adresních vstupů, které dávají dohromady nějakou čtrnáctibitovou adresu, a podívá se, co má na té adrese uložené. Zatím to ale nikomu neříká.

Z druhé strany je na sběrnici připojený procesor. Ten si to celé diriguje a řídí. Ve chvíli, kdy baží po instrukci &#8211; a to je hned po RESETu nebo poté, co dokončil předchozí instrukci &#8211; udělá několik věcí. V první řadě na adresové výstupy A0-A15 nastaví logické nuly a jedničky tak, aby daly dohromady adresu, ze které se bude číst. Pro znalce procesorů: Je to stav registru PC, Program Counter. Taky přepne datové vývody D0-D7 do stavu &#8222;vstup&#8220;. Procesor totiž může po datových vodičích informace přijímat i vysílat. V téhle chvíli chce přečíst něco z paměti, tak si je nastaví jako vstup. No a úplně nakonec nastaví svoje řídicí výstupy, které říkají okolí, co má dělat, tak, že dá signál &#8222;Chci číst z paměti!&#8220; (Procesor 8080 měl výstup MR, Memory Read, u Z80 se skládal z RD &#8211; read &#8211; a MREQ &#8211; Memory Request)

Signál &#8222;Čteme z paměti!&#8220; běží po příslušném vodiči do všech paměťových obvodů. Pokud je adresa taková, že má dva nejvyšší bity 0, tak náš dekodér nastavil paměti ROM signál CE &#8211; jak jsem popsal výše. Takže naše paměť ROM je připravena na to, že se komunikuje s ní (chip enable), má připravená data z dané adresy, a teď přijde signál čtení. Ten je připojen na vstup, co se jmenuje např. OE &#8211; tedy Output Enable. V té chvíli paměť ROM pustí to, co má uložené, na datový výstup. Tedy zase: tam, kde je logická 1, tam nastaví příslušný výstup tak, že na něm je napětí, u log. 0 žádné napětí není.

No a protože je výstup paměti připojen na datovou sběrnici, tak se ta hodnota přenese na ni. A jelikož na druhém konci číhá procesor a má svoje datové vývody přepnuté na funkci vstup, tak se tímhle způsobem hodnota, co je uložená v ROM, tedy v našem případě operační kód, dostane do procesoru, a ten už si ho zpracuje.

Procesor hned potom vypne signál &#8222;Čteme z paměti&#8220;, tím se ROM odpojí. V elektronice se říká, že své datové výstupy nastaví do stavu _vysoké impedance_, což v podstatě znamená, že proud neteče z nich ani do nich, takže nijak neovlivňují to, co proudí po sběrnici. Díky tomu je možné na jednu datovou sběrnici připojit paměť ROM, paměť RAM, nějaké porty a další zařízení, aniž by všichni něco vysílali a na sběrnici se to tlouklo. Všichni mají své datové vstupy a výstupy odpojené a procesor jasně určuje, kdo se může kdy připojit a co má poslat, nebo naopak přečíst.

## <span id="8230_a_najemno">&#8230; a najemno</span>

Z druhé strany máme svět tranzistorů, odporů a drátků, tedy přesněji vedení pomocí plošných spojů a nožiček integrovaných obvodů. Jak to v něm vypadá?

Analogové počítače počítaly taknějak &#8222;napřímo&#8220;. Napětí 1V bylo třeba hodnota 1000, no a když jste chtěli sečíst 1000 a 500, tak jste vlastně sčítali napětí 1V a 0.5V, což dalo dohromady 1,5V &#8211; a bylo! Jenže vlivem nepřesností a úbytků napětí byla tahle technika velmi náročná a složitá, tak se zvolil technicky jednodušší, digitální způsob. Ten používá jedničky a nuly, a zjednodušeně řečeno: Je tam napětí? Je to 1, když není, tak 0.

S příchodem integrovaných obvodů a nástupem řady TTL se konvence ustálila na tom, že napětí 0 až 0.8V je logická 0, napětí 2 až 5V je logická 1. Obvody vysílají buď 0, nebo 5 voltů, ta tolerance tam je proto, aby zapojení bylo méně náchylné k rušení vlivem úbytku napětí.

### <span id="Hradla">Hradla</span>

Pomocí tranzistorů se skládají základní logické prvky, tedy invertor a hradlo. Tranzistor tu funguje jako spínač. Princip fungování TTL prvků už je příliš podrobný, stačí jen, když si řekneme, že několik tranzistorů dohromady dá zákaldní logický prvek. Invertor je prvek, co má jeden vstup a jeden výstup, a když je na vstupu 0, je na výstupu 1 a obráceně.<figure id="attachment\_335" aria-labelledby="figcaption\_attachment_335" class="wp-caption aligncenter" style="width: 130px">

<img loading="lazy" class="size-full wp-image-335" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/120px-NOT_ANSI_Labelled.svg_.png" alt="" width="120" height="50" /> <figcaption id="figcaption\_attachment\_335" class="wp-caption-text">Invertor</figcaption></figure> 

Další základní logické prvky jsou hradla AND, OR, NAND (=Not AND, negovaný AND) a NOR (=Not OR). Ve skutečnosti stačí jediný typ, buď NAND, nebo NOR, z něhož se dají všechny ostatní typy složit (např. OR uděláme z NANDu tak, že na jeho vstupy přivedeme signál přes invertory, invertor z NANDu uděláme tak, že signál přivedeme na jeden vstup a na druhý zapojíme log. 1), ale vyrábějí se pro jistotu všechny, v různém provedení. Základní typ je dvouvstupové hradlo, ale existují třívstupová, čtyřvstupová, osmivstupová a ještě divnější&#8230;<figure id="attachment\_336" aria-labelledby="figcaption\_attachment_336" class="wp-caption aligncenter" style="width: 130px">

<img loading="lazy" class="wp-image-336 size-full" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/120px-AND_ANSI_Labelled.svg_.png" alt="" width="120" height="50" /> <figcaption id="figcaption\_attachment\_336" class="wp-caption-text">Hradlo AND</figcaption></figure> <figure id="attachment\_337" aria-labelledby="figcaption\_attachment_337" class="wp-caption aligncenter" style="width: 130px"><img loading="lazy" class="size-full wp-image-337" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/120px-OR_ANSI_Labelled.svg_.png" alt="" width="120" height="50" /><figcaption id="figcaption\_attachment\_337" class="wp-caption-text">Hradlo OR</figcaption></figure> <figure id="attachment\_338" aria-labelledby="figcaption\_attachment_338" class="wp-caption aligncenter" style="width: 130px"><img loading="lazy" class="size-full wp-image-338" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/120px-NAND_ANSI_Labelled.svg_.png" alt="" width="120" height="50" /><figcaption id="figcaption\_attachment\_338" class="wp-caption-text">Hradlo NAND</figcaption></figure> <figure id="attachment\_339" aria-labelledby="figcaption\_attachment_339" class="wp-caption aligncenter" style="width: 130px"><img loading="lazy" class="size-full wp-image-339" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/120px-NOR_ANSI_Labelled.svg_.png" alt="" width="120" height="50" /><figcaption id="figcaption\_attachment\_339" class="wp-caption-text">Hradlo NOR</figcaption></figure> 

Schematické značky, co jsem tu použil, jsou podle normy ANSI. V ČSSR se koncem osmdesátých let začala prosazovat norma IEC, kde všechny vypadají jako obdélníčky se symbolem, setkáte se s oběma typy. Jedno mají společné: Negovaný vstup nebo výstup se značí kroužkem. Příklad: Hradlo AND má na výstupu logickou 1, pokud jsou oba vstupy v log. 1. Schematická značka hradla NAND vypadá úplně stejně, až na ten kroužek, a ten právě znamená, že je výstup negovaný: hradlo NAND má na výstupu logickou 0, pokud jsou oba vstupy v log. 1.

### <span id="Kombinacni_obvody">Kombinační obvody</span>

Pomocí kombinace hradel se sestavují základní kombinační obvody &#8211; nejrůznější dekodéry, multiplexery, demultiplexery, sčítačky, zkrátka obvody, kde je stav výstupů jasně daný stavem vstupů a nezávisí na tom, jaký byl stav před chvílí. Stav výstupů kombinačních obvodů lze popsat pravdivostní tabulkou nebo logickým výrazem. Kombinační obvody se používají v počítačích například v roli dekodérů adres, o kterém jsem psal výše. Jeden z hojně používaných obvodů osmibitové éry měl tři vstupy a osm výstupů a podle toho, jaké binární číslo bylo na vstupech (000, 001, &#8230;, 110, 111) byl aktivní právě jeden z osmi výstupů.

Svým způsobem bychom mohli paměť ROM považovat za velmi, velmi komplikovaný kombinační obvod (&#8222;Když jsou vstupy nastavené na 00000000000000, tak nastav výstupy na 11110011&#8220;). V osmibitových systémech se ale na místech, kde bylo potřeba vyhodnotit opravdu složitý logický výraz, použila někdy právě paměť ROM (PROM), která výrazně zjednodušila návrh.

### <span id="Zpetna_vazba">Zpětná vazba</span>

S kombinačními obvody není moc velká legrace. Na vstupy přivedete data, na výstupech se o něco později objeví nějaká jiná data, a tím to končí. Jenže trošku složitější systém by si měl něco pamatovat &#8211; a paměť (a nejen ta) vznikne zavedením zpětné vazby. A zpětná vazba v tomto případě neznamená, že vám někdo řekne, co si o tom myslí, ale to, že nějaký výstup běžného kombinačního obvodu přivedeme na vstup. Pak se začnou dít věci&#8230;

Představte si takovéhle zapojení:

<img loading="lazy" class="aligncenter size-full wp-image-340" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/R-S_mk2.gif" alt="" width="500" height="365" /> 

Jedná se o dvě hradla NOR. Jeden vstup je vždy zvenčí (R, S), druhý vstup je vytažen z výstupu protilehlého hradla. No a celé tohle zapojení má taky dva výstupy, Q a /Q (tak se přepisuje ono Q s čárou nahoře, které neznamená nic jiného než NOT Q, tedy negované Q).

To, co je na obrázku, se nazývá _klopný obvod_, konkrétně typu RS. Jedná se o sekvenční obvod, tedy o takový obvod, u kterého je stav výstupu závislý kromě stavu vstupů i na předchozím vnitřním stavu.

Dejme tomu, že na začátku je Q ve stavu log. 1, /Q v log. 0 a oba vstupy R a S jsou taky v log. 0. Na horním hradle jsou tedy vstupy 0 (R) a 0 (/Q), výstup (=Q) bude 1. Na dolním hradle bude jeden vstup 1 (Q), druhý 0 (S), výstup (=/Q) bude tedy 0. Situace je stabilní, takhle to prostě drží &#8222;samo o sobě&#8220;, dokud jsou oba vstupy R a S nulové.

Když na vstup R teď přivedu jedničku, co se stane? Na horním hradle budou vstupy 1 (R) a 0 (/Q), výstup (=Q) se tedy změní na 0. Na dolním hradle bude jeden vstup 0 (Q), druhý 0 (S), výstup (=/Q) bude tedy 1. Čímž se ovšem změní poměr na horním hradle: teď tam rázem budou vstupy 1 a 1! Na výstupu ale zůstane 0 a zůstane tam, i když se vstup R vrátí zpátky do 0. Co se stalo? Impulsem na vstupu R jsme dostali obvod do druhého stabilního stavu (proto se tomu říká bistabilní klopný obvod), kdy je Q=0 a /Q=1.

Přivedením jedničky na S se to celé zase překlopí obráceně. Jupí, máme nejjednodušší obvod, který si pamatuje, jak byl nastavený. Na celý obvod se totiž můžeme dívat jako na paměť, která si pamatuje přesně jeden bit. Jeho hodnota záleží na tom, jestli byl obvod naposledy nastaven (vstupem S &#8211; Set), nebo vynulován (vstupem R &#8211; Reset).

Když náhodou přivedu 1 na oba vstupy, obvod se rozkmitá na velmi vysoké frekvenci a dostane se do takzvaného &#8222;hazardního stavu&#8220;, kdy není stav výstupů garantován.

### <span id="Klopne_obvody">Klopné obvody</span>

Když na vstupy R a S připojíme hradla AND následujícím způsobem, dostaneme synchronní klopný obvod RS, tedy takový, který je možné přepínat pouze tehdy, když je to povoleno (E = enable).<figure id="attachment\_343" aria-labelledby="figcaption\_attachment_343" class="wp-caption aligncenter" style="width: 310px">

<img loading="lazy" class="size-full wp-image-343" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/300px-SR_Clocked_Flip-flop_Diagram.svg_.png" alt="Klopný obvod RS s povolovacím vstupem" width="300" height="145" /> <figcaption id="figcaption\_attachment\_343" class="wp-caption-text">Klopný obvod RS s povolovacím vstupem</figcaption></figure> 

Pokud je E=1, tak funguje jako standardní RS obvod, viz výše. Pokud je E=0, tak jsou oba vnitřní vstupy R a S stále ve stavu 0, tedy stav obvodu se nezmění, ať už na vstupy R a S přivedeme cokoli.

Ale pořád tam jsou ty hazardní stavy, když je S i R=1. Co takhle přivést jen jeden vstup, připojit ho napřímo na S, a přes invertor na R? Tím bude na vstupech RS buď kombinace 01, nebo 10, takže nedojde k hazardu&#8230;<figure id="attachment\_345" aria-labelledby="figcaption\_attachment_345" class="wp-caption aligncenter" style="width: 310px">

<img loading="lazy" class="size-full wp-image-345" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/300px-D-type_Transparent_Latch_NOR.svg_.png" alt="Klopný obvod D" width="300" height="150" /> <figcaption id="figcaption\_attachment\_345" class="wp-caption-text">Klopný obvod D</figcaption></figure> 

Výsledkem je další klopný obvod, kterému se tentokrát říká D. Má vstup D (Data), na který se přivádí hodnota 0 nebo 1, a povolovací vstup E (Enable), který se častěji označuje C (Clock). Když je E ve stavu 0, je na výstupech Q a /Q stále to, co tam bylo předtím. Pokud přivedeme na vstup E logickou 1, bude se na výstup Q propisovat hodnota ze vstupu D (a na /Q bude jeho negace). Jakmile se E přepne zpátky do 0, zůstane klopný obvod v tom stavu, v jakém byl naposled.

Proč takhle zevrubný úvod? Protože jsme si ukázali, jak se od tranzistorů vytvářejí hradla, z hradel klopné obvody, no a teď je na místě říct, že z klopných obvodů typu D a z hradel můžeme poskládat posuvné registry, čítače, paměťové buňky&#8230; tedy vlastně celý procesor!

## <span id="Mikroprocesor">Mikroprocesor</span>

Mikroprocesor se skládá z profláklých částí &#8211; z registrů, aritmeticko-logické jednotky a z řadiče.

Registry, to jsou vlastně klopné obvody typu D. Takový registr (třeba akumulátor), to je vlastně osm takových klopných obvodů vedle sebe se spojenými vstupy E. Na vstupy D (D0-D7) přivedeme hodnotu, pak vstup E přehodíme do logické 1 a pak do logické 0, no a na výstupech Q (Q0-Q7) budeme mít tu hodnotu, co si registr zapamatoval.

Aritmeticko-logická jednotka je zase složitý kombinační obvod, který dokáže vzít dva údaje (třeba osmibitová čísla) a udělat s nimi požadovanou operaci &#8211; má tedy 2 x 8 datových vstupů a např. 4 řídicí vstupy, kterými se určuje, jakou operaci je třeba provést.

Řadič se skládá ze spousty kombinační logiky, registrů a pamětí, které dohromady řídí všechny základní operace. Někde se používá čistě kombinační logika, někde mikrokód, což je rozložení instrukce do posloupnosti elementárních operací, jako &#8222;načti číslo z paměti&#8220;, &#8222;ulož číslo do paměti&#8220;, &#8222;přesuň hodnotu z registru do sčítačky&#8220; apod.

Celý tenhle cirkus musí mít nějaký takt, aby se dalo říct, že něco má proběhnout až poté, co se stane něco jiného. Třeba že nejdřív má procesor poslat adresu, pak přečíst instrukční kód, pak ho dekódovat, pak (třeba) přečíst hodnotu z paměti, přičíst ji k akumulátoru atd. K tomu slouží hodinový vstup.

Aniž bych tedy zabíhal do podrobností, tak můžu říct, že z těch kombinačních a klopných obvodů je sestaven celý procesor. Jeho práce je založená na tom, že si stále dokola čte z paměti instrukční kódy, ty dekóduje a provádí podle toho všechno potřebné. A ano, na jedné straně to jsou posloupnosti nějakých čísel, uložené v paměti, na druhé straně pak tranzistory, které buď proud vedou, nebo nevedou, a tak vlastně dělají ty elementární logické operace.

## <span id="Jak_to_tedy_je_s_tim_programovanim">Jak to tedy je s tím programováním?</span>

Když tedy autorovi mailu odpovím: Ano, v zásadě je to tak, jak píšeš. Procesor, což je vlastně jen velká struktura z tranzistorů, po zapnutí napájecího napětí udělá velký RESET, nastaví si registr PC na hodnotu 0 (a teď už víš, že registr PC je vlastně v principu jen 16 klopných obvodů &#8211; když si to hodně zjednodušíme) a spustí se stroj, který je řízen pravidelným hodinovým cyklem, něco jako &#8222;sání &#8211; komprese &#8211; výbuch &#8211; výfuk&#8220;. V rámci každého cyklu procesor načte z paměti operační kód (a teď už víš, že to je tak, že nastaví 16 drátů do nějakého stavu &#8222;je napětí / není napětí&#8220;, obvod zvaný &#8222;paměť&#8220;, který je na tyhle dráty připojený, na to odpoví tak, že nastaví zase datové dráty do nějakého stavu, a to zase umí procesor &#8222;přečíst&#8220;), uloží si ho do instrukčního registru (zase to budou jen důmyslně uspořádané klopné obvody) a podle toho něco udělá &#8211; a to &#8222;něco udělá&#8220; je vlastně zase jen to, že v určitém pořadí připojuje určité části k vnitřním sběrnicím a posílá signály &#8222;ulož&#8220;, &#8222;zpracuj&#8220;, &#8222;pošli výsledek&#8220;&#8230;

Není to nic magického a záhadného, princip je poměrně jednoduchý, ale suma potřebných znalostí není zas tak úplně malá. Každopádně je to fascinující kus techniky a já doufám, že alespoň část z té fascinace jste zažili i vy! (A pokud budete mít zájem, můžeme se k technice na téhle úrovni ještě někdy vrátit, takže se neostýchejte zeptat.)

PS: Podrobněji tuto oblast probral Pavel Tišnovský na [začátku seriálu o počítačích](https://www.root.cz/serialy/co-se-deje-v-pocitaci/?pi=6) na Root.cz. A jestli chcete míň vědecké, zato ale poutavější předvedení, tak si pusťte [Visual 6502](https://www.visual6502.org/JSSim/index.html) &#8211; vizualizaci toho, jak běhají elektrony v procesoru 6502.

PPS: Obrázky pocházejí z Wikipedie a jsou pod licencí Public Domain; [animace RS obvodu](https://en.wikipedia.org/wiki/File:R-S_mk2.gif) je pod CC-BY, autorem je Napalm Llama.

_Aktualizace_: Dan v komentářích doporučuje knihu [Code: The Hidden Language of Computer Hardware](https://www.amazon.com/gp/product/0735611319/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0735611319&linkCode=as2&tag=dein-20&linkId=OB2TQEVYR5I62567) (odkaz vede na Amazon), takže pokud vládnete angličtinou, podívejte se do ní. Já si pamatuju na podobnou knížku v češtině &#8211; tedy ta nešla úplně k procesorům, ale z tranzistorů skládala hradla a z hradel různé konstrukce&#8230; pamatuju si jen příjmení autora: &#8222;Drozen&#8220;.