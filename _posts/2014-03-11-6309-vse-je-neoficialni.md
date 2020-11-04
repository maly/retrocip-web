---
id: 145
title: '6309: Vše je neoficiální!'
date: 2014-03-11T13:50:09+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=145
permalink: /6309-vse-je-neoficialni/
dsq_thread_id:
  - "2407454465"
xyz_lnap:
  - "1"
image: http://retrocip.cz/wp-content/uploads/sites/6/2014/03/T2eC16VHJHFFmOoS6iBRzsj7cfw60_35-300x198.jpg
categories:
  - Hardware
tags:
  - "6309"
  - "6809"
  - Hitachi
---
[Minule](http://retrocip.uelectronics.info/posledni-krasavec-osmibitove-ery/ "Poslední krasavec osmibitové éry") jsem slíbil článek, věnovaný tomuto téměř mystickému procesoru. Rozhodně si jej zaslouží, protože krom toho, že jde o vylepšenou verzi [nejlepšího osmibitu](http://retrocip.uelectronics.info/posledni-krasavec-osmibitove-ery/ "Poslední krasavec osmibitové éry"), tak má rovněž, jak se dnes říká, &#8222;silný příběh&#8220;.

<!--more-->

V [článku o klonech a &#8222;druhých zdrojích&#8220;](http://retrocip.uelectronics.info/klony-a-procesory/ "Klony a procesory") jsem zmínil, že osmibitové procesory nevyráběla vždy jen ta firma, která je vyvinula, ale i jiné. Někdy legálně, na základě licence, někdy pololegálně, až ilegálně &#8211; tj. dotyčný výrobce vyvinul ekvivalentní obvod jen na základě reverzního inženýringu a dostupných materiálů.

Výrobci takových &#8222;kopií&#8220; a &#8222;klonů&#8220; často vylepšili původní proces, takže výsledný čip byl například rychlejší, měl nižší spotřebu, nebo se na křemík podařilo zaintegrovat i nějaké vylepšení. Zmiňoval jsem příklad Hitachi, který vyvinul vlastní vylepšený klon Z80 a licencoval jej zpět Zilogu, jenž ho opět vylepšil a prodával jako verzi Z180.

Stejný osud potkal i procesor 6809 od Motoroly, opět v rukou japonských vývojářů z Hitachi. Hitachi si licencoval od Motoroly výrobu 6809 a vyráběl ekvivalent s označením HD6809. Zároveň jej ale přepracovali (např. zavedli místo kombinační logiky mikroprogramy atd.) a přidali některé nové funkce. Takto upravený procesor ale licence neumožňovala prodávat pod označením 6809, takže jeho kód byl HD6309.

<a href="http://retrocip.uelectronics.info/6309-vyse-lepe-radostneji/t2ec16vhjhffmoos6ibrzsj7cfw60_35/" rel="attachment wp-att-146"><img loading="lazy" class="aligncenter size-full wp-image-146" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/03/T2eC16VHJHFFmOoS6iBRzsj7cfw60_35.jpg" alt="" width="300" height="225" /></a>

Hitachi jej inzerovalo jako &#8222;přímou náhradu 6809&#8220;, rychlejší a optimalizovanou. Když se podíváte do originálního [datasheetu 6309](http://www.datasheetarchive.com/dl/Scans-031/ScansU9X30544.pdf), popisuje jeho vnitřní organizaci stejně jako Motorola 6809. Tedy akumulátory A, B, indexy X, Y, ukazatele U a S, 16bitový registr D &#8211; viz [minulý článek](http://retrocip.uelectronics.info/posledni-krasavec-osmibitove-ery/ "Poslední krasavec osmibitové éry"). O jakémkoli vylepšení v tomto směru cudně mlčí.

Když náhrada, tak náhrada, takže někteří začali používat 6309 místo originálního 6809 ve svých počítačích. Po čase ale bystřejší hackeři, většinou z komunity tvůrců systému OS-9, začali objevovat drobné rozdíly v chování oproti 6809, zejména při provádění &#8222;nedokumentovaných&#8220; instrukcí, tj. operačních kódů, které neměly oficiálně přiřazené žádné instrukce.

Postupně odhalili, že 6309 má významná rozšíření proti svému vzoru. Informace se šířily mezi japonskými elektronickými nadšenci a v dubnu 1988 vyšel v japonském časopise [Oh!FM](http://ja.wikipedia.org/wiki/Oh!FM) článek, který shrnoval nedokumentované vlastnosti 6309. Do zbytku světa se tyto informace dostaly až v roce 1991, kdy [informace z článku](http://koti.mbnet.fi/~atjs/mc6809/Information/6309.txt) poslal Hirotsugu Kakugawa do konference comp.sys.m6809. Díky těmto informacím byl vyvinut minule zmíněný systém [NitrOS-9](http://sourceforge.net/projects/nitros9/), který využíval právě nedokumentované vlastnosti HD6309. (Až později byl NitrOS-9 přepsán tak, aby fungoval i s originálním 6809)

## Co má 6309 navíc?

HD6309 nabízí nové registry:

  * Dva osmibitové akumulátory, označené E a F (označení je vžité, ale neoficiální &#8211; nezapomínejme, že oficiální dokumentace o rozšíření mlčela). Tyto dva akumulátory dávají dohromady šestnáctibitový registr W (podobně jako A a B dávají D).
  * Šestnáctibitové virtuální registry D a W tvoří dohromady 32bitový registr Q
  * Šestnáctibitový registr V, který nelze použít jako plný akumulátor, pouze pro přesuny mezi registry (např. instrukcí TFR). Jeho zvláštností je, že jeho hodnota zůstane zachovaná i při RESETu.
  * Registr 0, použitelný pouze při přesunech mezi registry. Obsahuje vždy nulu a lze ho využít pro nulování jiných registrů.
  * Stavový a řídicí registr MD (8 bitů). Tento registr obsahuje dva příznaky: dělení nulou a provedení ilegální instrukce. Zároveň lze dva příznaky nastavit: jedním se určuje, jestli procesor běží v emulovaném módu nebo nativní, druhým lze zajistit, aby se FIRQ choval jako IRQ. Při resetu je nastaven tak, aby se choval jako 6809.

V emulovaném módu pracuje 6309 stejně jako původní 6809, se stejným časováním instrukcí. Samozřejmě lze použít rozšířené možnosti, nové registry apod. V nativním módu trvají některé instrukce kratší dobu a při přerušeníse na zásobník ukládají i nové registry E a F (takže v takovém případě je nutné přepsat obsluhu přerušení, pokud se nějak odkazuje na uložená data).

Procesor 6309 zavedl kromě přerušení i &#8222;vnitřní přerušení&#8220; (traps), které je vyvoláno v případě načtení ilegální instrukce nebo při dělení nulou. V takovém případě se uloží registry a je vyvolána obslužná rutina na adrese, uložené na FFF0h (u originálního 6809 rezervováno).

Největší inovace přinesl 6309 v oblasti instrukcí. Originální 6809 dovolovala například aritmetické a logické operace pouze mezi akumulátorem a pamětí. 6309 umí všechny tyto operace provést i mezi dvěma registry.

6309 přinesl i instrukce blokového přesunu. Rozšířil totiž původní instrukci TFR (Transfer) o čtyři nové módy, které pracují s pamětí adresovanou registrem: TFM r1+, r2+; TFM r1-, r2-; TFM r1+,r2 a TFM r1, r2+. Registry r1, r2 mohou být D, X, Y, U nebo S, registr W udává, kolik bajtů bude přeneseno. První dvě instrukce fungují jako prostý přesun se stoupajícími/klesajícími adresami (LDIR/LDDR pro znalce assembleru Z80). Druhé dva tvary přenášejí blok paměti do jednoho místa (vhodné např. pro blokový OUT), resp. z jednoho místa do celého bloku paměti (blokový IN).

Násobení 8&#215;8 bylo rozšířeno o možnost násobit 16&#215;16 bitů (Q = W * D). Přibyly instrukce dělení 16 / 8 bitů a 32 / 16 bitů.

Přibyly instrukce pro bitové manipulace s pamětí &#8211; AIM, EIM, OIM a TIM, po řadě AND in Memory, EOR in Memory, OR in Memory a TEST in Memory. Kromě nich se objevily i instrukce pro bitové operace mezi pamětí a registrem, které umožňují např. provést AND mezi negovaným třetím bitem nějakého paměťového místa a šestým bitem v registru apod. Tyto instrukce se jmenují BAND, BIAND, BOR, BIOR, BEOR, BIEOR, LDBT a STBT. Výše zmíněný příklad by se zapsal jako &#8222;BIAND A, 6, 3, 0x12&#8220; &#8211; Bit Inverted AND, registr A, 6. bit, s pamětí na adrese 0x12, třetí bit.

Kromě toho samozřejmě přibyly adekvátní instrukce pro uložení nových registrů na zásobník, pro operace s nimi atd.

6309 má vnitřně 16bitovou architekturu, takže například instrukce pro prohození obsahu registrů X a Y (16 bitů) interně pracuje s šestnáctibitovým TEMP registrem, takže pro výměnu jsou potřeba pouhé tři přesuny, nikoli šest jako u 6809.

Další materiály: [6309 tech ref](http://koti.mbnet.fi/~atjs/mc6809/Information/6309.techref), [6309 Book](http://cyberabi.ipower.com/Downloads/The_6309_Book.pdf).

Opět připomínám: **všechny tyhle informace jsou neoficiální**, Hitachi se k nim nikdy nevyjádřil, a jsou výsledkem mravenčí práce spousty vývojářů a hackerů, kteří zkoumali a zjišťovali, jak která instrukce funguje. Klobouk dolů!

Procesor 6309 se vyráběl ve verzích s kmitočtem 2MHz (63B09) i 3MHz (63C09) a nabízel vnitřní oscilátor i vnější oscilátor (verze měla na konci označení písmeno E).

Jak jsem se zmiňoval minule: u 6309 se mi nepodařilo zjistit, kdy šel do výroby. Našel jsem někde zmínku, že měl být použit v legendárním syntezátoru Yamaha DX-7, ale když se mi podařilo najít schéma DX-7, tak se tahle informace ukázala jako mylná. Je možné, že by se v japonských pramenech dalo dohledat víc informací, bohužel jazyková bariéra je pro mne příliš vysoká.

_To mi ale nezabránilo, abych si pár těchto kousků neobjednal a nezkusil nějaké experimenty. Tenhle procesor mě totiž po dlouhé době zase nadchnul&#8230;_ [sc:ebay item=HD63C09]