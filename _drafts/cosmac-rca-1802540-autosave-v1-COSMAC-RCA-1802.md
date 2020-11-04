---
id: 547
title: COSMAC RCA 1802
date: 2015-04-18T18:31:11+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/540-autosave-v1/
permalink: /540-autosave-v1/
---
Tentokrát si dáme zase jednu exkurzi do historie &#8211; za výtvorem společnosti RCA, který rozhodně není zapomenutý.

<!--more-->

RCA, neboli Radio Corporation of America, byla americká společnost, která vyráběla elektroniku od roku 1919 až do roku 1985, kdy ji převzala zpět společnost General Electric (která ji kdysi na začátku 20. století spoluzakládala). Její historie je velmi zajímavá, dovolte tedy drobnou odbočku.

RCA koupila například v roce 1929 společnost Victor Talking Machine Company, výrobce fonografů a, mimo jiné, většinového vlastníka společnosti Victor Company of Japan (JVC). S touto společností přešla na RCA práva k užívání loga s Nipperem.

Že nevíte, kdo byl [Nipper](http://en.wikipedia.org/wiki/Nipper)? To je možné, ale tenhle obrázek určitě znáte.

<figure id="attachment\_541" aria-labelledby="figcaption\_attachment_541" class="wp-caption aligncenter" style="width: 649px"><img loading="lazy" class="size-full wp-image-541" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/04/OriginalNipper.jpg" alt="" width="639" height="430" /><figcaption id="figcaption\_attachment\_541" class="wp-caption-text">Nipper sleduje fonograf, Francis Barraud, 1895</figcaption></figure>

Nipper je samozřejmě ten pes. Victor Talking Machine použila podobný obrázek, jen místo fonografu dali gramofon, ve své reklamě i se sloganem &#8222;His Master&#8217;s Voice&#8220;. Ale zpět k RCA.

Tato firma stojí i za dalším ikonickým obrázkem:

<img loading="lazy" class="aligncenter size-full wp-image-542" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/04/640px-RCA_Indian_Head_test_pattern.jpg" alt="" width="640" height="480" /> 

Ano, první &#8222;monoskop&#8220; (televizní testovací obrazec). V roce 1939 totiž RCA na Světové výstavě představila první elektronický televizní systém a začala prodávat první televizory.

V laboratořích RCA vznikla spousta dalších významných inovací &#8211; barevná televize, technologie CMOS, LCD, videorekordér, &#8230; RCA později (do roku 1971) vyráběla například i počítače (Spectra 70). Svou výrobu domácích spotřebičů prodala Whirlpoolu, znát můžeme nahrávací společnost RCA/Ariola, později RCA Records. Zkrátka, není toho málo.

## RCA CDP1802

V roce 1976 uvedla RCA na trh mikroprocesor CDP1802. Byl vyrobený technologií CMOS, vyvinutou právě v RCA pod názvem COS-MOS (COmplementary Silicon / Metal-Oxid-Semiconductor). 1802 byl známý taky pod názvem COSMAC. Prý &#8222;Complementary Symmetry Monolithic Array Computer&#8220;, ale spíš bych si tipl, že se tam odráží ta technologie COS-MOS.

V letech 1970-1971 vyvinul Joseph Weisbecker vlastní architekturu mikroprocesoru. RCA vytvořila nejprve (v roce 1975) dva čipy 1801R a 1801U, a později je sloučila do jednoho obvodu 1802.

CDP1802 má několik vlastností, které jej výrazně odlišují od hlavních konkurentů té doby (6800, 6502, 8080, Z80). Například jádro CDP1802 je statické &#8211; znamená to tedy, že je možné zpomalit hodiny na minimum, dokonce je i zastavit, a procesor neztratí data z vnitřních registrů.

CDP1802 adresuje, podobně jako ostatní mikroprocesory té doby, 64 kB paměti. Uvnitř obsahuje šestnáct šestnáctibitových registrů (R0-R15), které jsou, až na dvě výjimky, plně ortogonální. Akumulátor, v němž se provádějí aritmetické operace, je osmibitový a je označený D. Procesor obsahuje i dva čtyřbitové registry P a X. Registr X udává, který z registrů R0-R15 funguje jako indexový registr pro přístup k datům, registr P pak říká, který z registrů R0-R15 funguje jako programový čítač. Ano, CDP1802 nemá dedikovaný registr PC (Program Counter), místo něj používá vždy jeden z registrů R0-R15.

Je to celé trošku divoké, a zvyknout si na to dá člověku, zvyklému na architekturu 8080/6800 a odvozené, trochu práce. Tak například procesor nemá zásobník. Na co taky, když nemá instrukci CALL. Zato má hned dvě instrukce RET &#8211; jedna při návratu povolí přerušení, druhá ho zakáže. Jak se volá podprogram? No, do nějakého registru si dáte jeho adresu, a pak řeknete: Ty teď budeš program counter! A na konci řeknete: Program counter je zase ten původní&#8230;

Samozřejmě, existují instrukce, které uloží &#8222;něco jako stav&#8220; &#8211; tedy registry P a X &#8211; na &#8222;něco jako zásobník&#8220;, tedy do paměti, adresované buď registrem RX (tedy registrem, jehož číslo je v registru X), nebo registrem R2. Tohle je právě ta výjimka z ortogonality, spolu s tím, že registr R0 je používán pro přenos DMA, registr R1 je program counter při přerušení a jediná instrukce, kterou by šlo považo.

Procesor CDP1802 má i jeden datový výstup (Q) a čtyři datové vstupy (EF1-EF4). Instrukční sada obsahuje instrukce Set Q (SEQ) a Reset Q (REQ). Pro testování vstupů pak jsou podmíněné skoky B1, B2, B3, B4, BN1, BN2, BN3, BN4 &#8211; skáče se, pokud je vstup = 1, resp. pokud je roven 0 (u varianty BN).

Skoky jsou osmibitové, ale nikoli relativní. Pouze se vezme osmibitová adresa a vloží se do nižšího byte registru PC (= registru R, jehož číslo je v registru P&#8230; Zvykejte si!) Hranici 256 byte normálním (krátkým) skokem nepřeskočíte. Naštěstí existují i instrukce dlouhého skoku s absolutní adresou.

Ke skokům připočtěme i instrukce &#8222;přeskoků&#8220;, včetně podmíněných &#8211; totiž instrukce, které umožňují přeskočit následující jeden nebo dva byty.

Když píšu &#8222;podmíněné&#8220;, tak si nepředstavujte nějakou podmínkovou bonanzu. Kdepak. Podmínky jsou buď podle datových vstupů EF, podle stavu výstupu Q, nebo podle toho, jestli je registr D roven nule. Jediný podmínkový příznak je DF, a ten odpovídá příznaku přenosu (CY). Testovat můžete stav příznaku povoleného přerušení (IE), a to instrukcí LSIE. Pokud je IE=1, přeskočí se následující dva bajty.

Existují instrukce pro přesun dat mezi pamětí a registrem D, adresované buď konkrétním registrem, nebo registrem, jehož číslo je v X. Jsou i varianty s postinkrementací a postdekrementací. Na druhou stranu nejsou instrukce pro přímou práci s registry R &#8211; pokud chcete do registru uložit nějakou hodnotu, musíte ji ukládat nadvakrát přes registr D. Instrukce PLO, PHI (Put Low / High) a GLO, GHI (Get Low / High) přenesou data mezi registrem D a dolní, resp. horní polovinou vybraného registru.

Procesor má navíc možnost přímo adresovat sedm vstupně / výstupních portů (instrukcemi INP 1 &#8211; INP 7 a OUT 1 &#8211; OUT 7), kdy probíhá přenos z / do paměti na adrese, která je v RX. Kromě toho oplývá i zabudovaným řadičem DMA: po spuštění přenáší byty z / do paměti, adresované registrem R0.

<figure id="attachment\_544" aria-labelledby="figcaption\_attachment_544" class="wp-caption aligncenter" style="width: 330px"><img loading="lazy" class="wp-image-544 size-full" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/04/320px-Ic-photo-RCA-CDP1802D-1802-CPU.jpg" alt="320px-Ic-photo-RCA--CDP1802D--(1802-CPU)" width="320" height="123" /><figcaption id="figcaption\_attachment\_544" class="wp-caption-text">CDP1802D (Autor: ZyMOS, Wikimedia)</figcaption></figure>

&nbsp;

## Použití

Říkáte si &#8222;Kdo by něco takového, proboha, použil?&#8220; Inu, tehdy ještě nebyla v lidech zafixovaná architektura 8080/6800, tak měli možná otevřenější přístup. Snad. Doufám. Po výše uvedeném popisu vás jistě nepřekvapí, že první vyšší programovací jazyk pro tento procesor byl FORTH. Na druhou stranu má CDP1802 některé výhody, především pak možnost zastavit hodiny procesoru a snížit tak jeho odběr na naprosté minimum. Jeho instrukční sada má, i přes všechny podivnosti, několik zajímavých rysů: Naprostá většina instrukcí trvá dva strojové cykly (každý 8 taktů hodin), až na vzácné výjimky, které trvají 3 cykly. Počítání času je tedy velmi jednoduché. No a navíc instrukční soubor obsahuje instrukci SEX.

Minimální odběr oceníte, když děláte třeba, hmmm&#8230; [vesmírnou sondu](http://retrocip.uelectronics.info/forth-na-komete/ "FORTH na kometě"). V mnoha sondách právě tento procesor je. Ostatně, od Intersilu si jej můžete objednat ve variantě se [zvýšenou odolností proti radiaci](http://www.intersil.com/en/products/space-and-harsh-environment/harsh-environment/microprocessors-and-peripherals/CDP1802A.html). Jen se připravte na to, že to není nejlevnější špás&#8230;

CDP1802 byl použit v legendárních počítačích COSMAC ELF a COSMAC VIP, v herní konzoli RCA System II, a využívaly ho i jugoslávské počítače Pecom 32 a Pecom 64.

RCA vyráběla vylepšenou variantu CDP1804 &#8211; tento procesor uměl víc instrukcí (třeba i CALL), obsahoval i časovač a měl integrovanou RAM a ROM. Verze CDP1805 byla bez ROM, verze CDP1806 i bez RAM.

_Jo a do [ASM80.com](http://www.asm80.com) jsem přidal podporu pro tento procesor. Můžete teď překládat zdrojové kódy a můžete je i debuggovat. Přípona souboru je .a18_