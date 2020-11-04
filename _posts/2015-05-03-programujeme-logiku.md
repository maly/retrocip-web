---
id: 561
title: 'Programujeme logiku&#8230;'
date: 2015-05-03T16:26:02+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=561
permalink: /programujeme-logiku/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3732809518"
image: http://retrocip.cz/wp-content/uploads/sites/6/2015/05/27320-A0-870x870-870x198.jpg
categories:
  - Hardware
tags:
  - FPGA
  - VHDL
---
Dr. Divnočip, aneb Jak jsem se naučil nedělat si starosti a mít rád FPGA.

<!--more-->

To bylo v dobách, kdy jsem ještě nevěděl, že diskusním fórům, plným _opravdových odborníků_, je lepší se vyhnout. Někdy poradí dobře, o tom žádná, ale pokud se někdo ptá trošku jinak nebo pokud přistupuje k věcem tak, jak oni nejsou z praxe zvyklí, tak jsou často velmi nepříjemní, leckdy až vulgární. A často to jsou stesky na úrovni mojí babičky, které se taky nelíbilo, že &#8222;ty dnešní mladý holky by nejradši jen strčily prádlo do automatický pračky a vytáhly ho vypraný a vyždímaný&#8220;, což považovala za symbol úpadku doby a mravů, protože pořádná ženská přeci pere a ždímá a stráví u toho celé sobotní dopoledne.

Těch keců, když jsem si jednou v jednom takovém &#8222;obecný pokec o elektronice&#8220; dovolil povzdychnout, že by se mi líbila malá frézka na plošné spoje, ale že pro domácí použití to je hrozně drahé&#8230; Těch keců, když jsem se zeptal, jestli si můžu u domácího plošňáku nějak nahradit prokovy! Jojo, všichni opravdoví odborníci, pracovali v různých elektrofirmách, tak co by brali vážně nějakého bastliče!

No a jednou se v takovém diskusním fóru objevil mladý nadšenec. Nejprve se, jak etiketa ve fórech plných opravdových odborníků velí, všem poníženě omluvil za svůj úplně pitomý dotaz, a pak napsal, že má od mládí sen, totiž navrhnout si vlastní mikroprocesor. A jestli tedy pánové nevědí, jak na to?

Pokud znáte mentalitu těchto fór, tak víte, co následovalo. Pokud ne, tak vám to prozradím: _&#8222;To potřebuješ do nějakého zapojení? Tak takové bych nekoupil!&#8220; &#8211; &#8222;Na co vlastní CPU?! Takový nesmysl! Kdyby ses, mladej, radši pořádně naučil AVR!&#8220; &#8211; &#8222;Ne, PIC!&#8220; &#8211; &#8222;Ne, ARM!&#8220; &#8211; &#8222;Ne, MSP430!&#8220; &#8211; &#8222;Ne, 86!&#8220; &#8211; &#8222;Idiote, co tu doporučuješ 86?!&#8220; _

Děkujeme za odpovědi.

Dlouho se nic nedělo a asi po půl roce přišel do toho vlákna člověk, který napsal, že ten nápad mu nepřijde, na rozdíl od _kolegů otporníků_, vůbec hloupý. Že sice navrhovat vlastní CPU a doufat, že se komerčně uchytí, je opravdu nesmysl, ale na druhou stranu že tuhle touhu chápe, a že ví, že se při takovém mentálním cvičení člověk naučí spoustu věcí o elektronice (asi jako při psaní vlastního programovacího jazyka a překladače pro něj, dodávám já). A že cest je několik, a jedna z nich je naučit se VHDL nebo Verilog, napsat si svoje CPU v něm a vypálit do FPGA.

Nevím, jestli si původní tazatel tuhle odpověď přečetl, nebo jestli se dnes smutně věnuje nějaké klidné činnosti a svůj mladický sen tiše pohřbil. Každopádně já si to zapamatoval a začal jsem (bylo to někdy okolo roku 2005) hledat, jak bych se dozvěděl víc. Postupně jsem si spájel programátor GALů, zkusil si naprogramovat je, pak jsem poexperimentoval s CPLD od Xilinxu, jenže pak na to postupně nebyla nálada, čas, peníze nebo sicflajš, zkrátka jsem si to tak jako odložil &#8222;na někdy&#8220;. Čas od času jsem vytáhl kit od Avnetu, zkusil si něco naprogramovat, zjistil jsem, že to je spíš utrpení než cokoli jiného, navíc to ISE od Xilinxu, kdo jste to kdy tahal a používal, tak mi dáte zapravdu&#8230;

Plynuly roky.

Pak jsem objevil zapojení Granta Searla a jeho [Multicomp](http://searle.hostei.com/grant/Multicomp/index.html). To je vám totiž taková stavebnice, kde si vyberete procesor, periferie, a poskládáte si osmibit, jaký se vám líbí, a v té úplně minimální variantě k tomu nemusíte připojovat vůbec nic, stačí jen převodník RS-232 / USB, a v terminálu uvidíte vlastní počítač s BASICem! Na nepájivém poli jsem měl zrovna rozposkládané nějaké zapojení s 6809, všude z toho trčely drátky, a tak jsem si říkal, že by mě to vlastně bavilo.

Nemám [retroducha](http://retrocip.uelectronics.info/retro-duch/). Kdybych ho měl, tak si poskládám vlastní CPU z TTL 74xx (a klobouk dolů před [těmi, co to udělali](http://www.nostalcomp.cz/cpu74_claudia1.php)) a tam, kde je napsáno &#8222;6502&#8220;, tam bude 40pinový šváb. Jenže já jsem takový barbar, a místo osmi 64Kbit DRAM, co jsou brutál retro, dám 64KB SRAM, a vůbec z toho nemám těžkou hlavu. Takže místo toho, abych sháněl třeba TMS9929 (což je TMS9918 ve verzi pro PAL) a navrhoval zapojení a leptal pro to plošňák, tak si to klidně seskládám do FPGA, neboť _radost z práce s novým osmibitem = (1-a) \* radost z toho, že to funguje + a \* radost z toho, že to je poskládané ze součástek z 80. let_. Přičemž parametr _a_ se u různých lidí liší. U mne je někde okolo 0.01, u lidí, co jsou ochotni připustit GAL v nové konstrukci to bývá tak okolo 0.5&#8230;

Ale zpátky k původnímu tématu: Okouzlily mě možnosti FPGA, od Granta jsem si nechal doporučit [vhodný kit](http://fpga.cz/ep2c5t144/), zjistil jsem, že je za pár korun a programátor taky, pak jsem si s tím pohrál, a ono to fakt fungovalo&#8230; Ou kej, pak přišly jiné starosti a radosti, jako třeba assembler [ASM80.com](http://www.asm80.com) a práce a tak, ale furt v hlavě vrtalo&#8230; V mezičase jsem našel [stránky pana ZZ](http://zz-indigo.mavipet.sk/?cat=14), který si taky z FPGA skládá všechno možné, tak jsem si říkal, že bych se měl to VHDL už fakt doučit, a znáte to, šlo to tak jako jo, ne, jo, ne, furt jako by chyběla motivace a spouštěč, a pak jednoho dne&#8230;

Jednoho dne jsem meditoval nad generováním videosignálu z procesoru. Nalomilo mě zkoumání [COSMAC ELFa](http://retrocip.uelectronics.info/cosmac-rca-1802/) a jeho videoobvodů, spojilo se to s neustálým opakováním Pavla Tišnovského, jak by bylo fajn zkusit dneska navrhnout videořadič, kde by nebyla bitmapa (třeba v tomto [článku o konzoli 2600](http://www.root.cz/clanky/historie-vyvoje-pocitacovych-her-7-cast-osmibitova-herni-konzole-atari-2600/)), ale něco jako TIA a vhodným časováním by se generoval řádek po řádku, zkoumal jsem generování obrazu u ZX80, přitom jsem se tak jako po očku koukal na ten kit, pak mě napadlo podívat se, jak by se generoval videosignál VGA (protože sehnat VGA monitor je snazší a levnější, než sehnat přijatelnou barevnou televizi s videovstupem), objevil jsem tutoriál, jak vytvořit VGA signál ve VHDL, koukal jsem se na to s neskutečným nadšením, protože to bylo fakt jednoduchý, ale jako FAKT JEDNODUCHÝ, a pak se to spustilo!

Fakt, nekecám, během tejdne jsem si ve volném čase prostudoval několik knih o VHDL a návrhu obvodů, konečně mi to celé začalo zapadat do sebe, dívám se na cizí kód a už mu rozumím, píšu vlastní kód a syntetizér to překládá napoprvé, a mám z toho fakt radost, protože to je zase po čase něco, co funguje.

Při učení čehokoliv se mi osvědčilo snažit se to, co jsem se naučil, vysvětlit někomu jinému. Ať už to byl pes, manželka, imaginární posluchač, virtuální posluchač, nebo skuteční studenti. Když se člověk snaží učit, tak při přípravě sám sobě klade otázky, které ho posunou dál. Nestačí studentovi říct &#8222;takhle to je&#8220;. Člověk se zamyslí a přemýšlí: Proč to tak je? Jak to udělat jinak? Proč to takhle jde a takhle ne? Navíc člověku naučené snáz přejde do krve.

Třeba takový [strojak.cz](http://strojak.cz) (pokud neznáte, tak je to kurz assembleru pro osmibity) vznikl v trošku jiné situaci &#8211; já assembler znám někdy od poloviny 80. let, takže tam jsem to víceméně _dával spatra_, ale taky jsem si některé věci osvěžil.

U VHDL jsem si říkal, že bych o tom mohl napsat sem, ale pak jsem objevil, že je volná doména vhdl.cz. A volná byla i fpga.cz! Tak dlouho jsem váhal, kterou z nich vzít, až jsem vzal obě, a připravil tam takový úvod pro lidi, co jsou v podobné situaci jako já, totiž doma si šmrdlají něco s elektronikou a rádi by do toho zapojili FPGA, jenže nevědí, kde začít, na fórech se bojí zeptat (a leckdy oprávněně, viz historka na začátku), ale hrozně je to láká.

Takže jsem na [fpga.cz](http://fpga.cz) dal takový přehled toho, co člověk potřebuje, když chce začít s FPGA pracovat. Jsou tam nějaké popisy kitů s low end FPGA Cyclone a Spartan, je tam popsáno, co budete potřebovat pro programování, jsou tam odkazy na to, kde to koupit (eBay je váš přítel!), popis toho, jak začít, co stáhnout, jak nainstalovat, jak si &#8222;nakreslit&#8220; zapojení&#8230; A snažil jsem se vybrat takové kity a programátory, aby se člověk vešel do tisícovky. Jako jo, seženete kity za čtyři tisíce, ale pro ten úplný začátek je to trošku _overkill_, neboli hezky česky přestřelené. Rovnou říkám, že pokud tam někdo přijde hledat _novinky a __informace_ _ze světa FPGA_, tak je na špatné adrese&#8230; O Stratixech se tam nedozví nic.

No a na [vhdl.cz](http://vhdl.cz) jsem sepsal takový úvodní kurz VHDL stylem &#8222;učte se se mnou&#8220;. Popisuju VHDL a věnuju se věcem, které mi v něm, coby člověku, zvyklému na programování, dělaly problém. Naopak přeskakuju věci, co jsou programátorům zřejmé &#8211; nebudu vysvětlovat, co jsou celočíselné typy, prostě očekávám, že tohle čtenář ví. Na rovinu ale přiznávám, že nejsem VHDL guru, jsem tak někde _lehce pokročilý. _Navíc vím, že se někde spletu a někde na to jdu prostě _blbě_ a nebojím se to přiznat. Ale pokud nejste _Opravdoví Odborníci a_ nehodláte mě kamenovat za to, že vykládám i o tom, kde jsem se spálil, a chcete si doma jako já bastlit s FPGA, jste vítáni na obou mých webech! Snad vám informace k něčemu budou.

PS: Na [VHDL.cz](http://vhdl.cz) stále pokračuju s novými články&#8230; V nejbližších dnech přijdou stavové automaty, a taky si dáme generování toho VGA signálu.

PPS: &#8222;_Možná by bylo zajímavé podobný grafický subsystém vytvořit s využitím soudobých komponent, protože dnešní mikroprocesory mají více než dostatečný výpočetní výkon pro programovou tvorbu grafiky a dobře navržený systém přerušení by mohl zajistit potřebnou kooperaci mezi podprogramem určeným pro vykreslování a zbylými aplikacemi&#8230;_&#8220; [píše Pavel Tišnovský](http://www.root.cz/clanky/graficke-subsystemy-pocitacu/), a já mu tímto neděkuju za brouka v hlavě.