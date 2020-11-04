---
id: 463
title: Nového psa starým trikům nenaučíš
date: 2014-12-06T12:16:28+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=463
permalink: /noveho-psa-starym-trikum-nenaucis/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3296309151"
categories:
  - Software
tags:
  - CPU
  - osmibity
  - Sinclair
  - Spectrum
  - Z80
  - ZX
---
Ptal se mě tuhle jeden čtenář na to, jestli bych někde nevyštrachal svou knížku o osmibitových tricích, protože by ho to zajímalo. Hlavně jestli se něco z toho dá použít ještě dneska&#8230; Tak: nedá! Proč?

<!--more-->

[Tento článek vyšel před rokem [původně na mém zápisníku](http://www.misantrop.info/noveho-psa-starym-trikum-nenaucis/), ovšem zaměřením patří jednoznačně sem, takže jsem ho přenesl&#8230;]

Doba osmibitová byla plná hrdinských činů, kdy mužové s očima rozedřenýma do krve od mihotavého světla mizerných přesvícených televizí ohýbali křemík až na samé hranice jeho možností. Měl jsem v té době ZX Spectrum a rád jsem se hrabal ve zdrojácích, abych viděl, jak pracují ti lepší a přiučil se od nich. Jo, tehdy optimalizace ještě něco znamenala a ušetřit sto bajtů (zhruba tak prostor, který zabírá text perexu tohoto článku) znamenalo mít možnost do nich naprat novou funkci. Stejně tak ušetřit pár taktů uvnitř cyklu&#8230; A tak se používaly nejrůznější triky.

Na Spectru jste mohli napsat v BASICu třeba tohle:

<pre>LET a=31</pre>

a do paměti se uložilo mnoho bajtů. V jednom byl kód tokenu LET, v dalších pak znaky &#8222;a&#8220;, &#8222;=&#8220;, &#8222;3&#8220;, &#8222;1&#8220;, a za tím speciální šestibajtová sekvence, ve které bylo uloženo to číslo (0x1f). _Teď to píšu z hlavy, jsem líný to dohledávat, tak uvidíme, jak si to po dvaadvaceti letech pamatuju&#8230; _Dohromady tedy 11 bajtů. Gut. Šlo by to zkrátit? Šlo!

<pre>LET a=VAL "31"</pre>

První bajt byl token pro LET, další pak &#8222;a&#8220;, &#8222;=&#8220;, token pro VAL, uvozovka, &#8222;3&#8220;, &#8222;1&#8220; a uvozovka. Tedy osm bajtů. Tedy tři uspořené. Když jste takhle _zváleli_ všechny proměnné, ušetřili jste fakt slušný kus paměti. A některá čísla (0, 1, 3) šly ještě kratším zápisem: NOT PI, SIGN PI, INT PI &#8211; LET a=NOT PI zabralo 5 bajtů, LET a=0 deset. Rychlost? Prosímvás, v BASICu? 🙂

Když chcete rychlost, musíte do strojáku.

Tam jsme všichni uctívali velké T. Totiž takt procesoru. Nejkratší instrukce, třeba NOP, přesuny mezi registry nebo prostá aritmetika, zabraly čtyři T. Když jste potřebovali uložit číslo do paměti, mohli jste si vybrat:

<pre>LD ($4000), A ; 13 T
LD (HL), 123 ; 10T
LD (HL), A ; 7 T</pre>

A to jste si museli předtím připravit buď adresu, nebo obsah, nebo obojí do registrů. Když jste potřebovali smazat obrazovku, museli jste zapsat hodnotu 0 na adresy $4000-$57FF. Jedna možnost byla klasická s blokovou instrukcí LDIR:

<pre>LD HL, $4000
LD DE, $4001
LD BC, $17ff
LD (HL),L
LDIR</pre>

což bylo ukrutně pomalé, protože instrukce LDIR sežere na uložení jedné hodnoty 21T. Celá obrazovka se tak mazala skoro 130.000T, takže jste mohli takhle smazat obrazovku zhruba 27krát za sekundu. Nic moc výkon. Nebylo by něco rychlejšího? Co takhle to rozepsat do cyklu? LD (HL), A zabere přeci jen 7T&#8230; ale zase nějaký čas sežere režie smyčky.

Po nějakém čase, stráveném s dokumentací, jste přišli na to, že asi nejrychlejší bude použít instrukci PUSH, která během 11T uloží dva bajty a dekrementuje ukazatel zásobníku, a když trošku rozvinete cyklus, tak se dostanete na zhruba polovinu T proti tomu řešení s LDIR:

<pre>LD DE, 0
      LD B, 192
LOOP: PUSH DE
      PUSH DE
      (... celkem 32x)
      DJNZ LOOP</pre>

&#8230; a bylo to! (Uschování původní adresy SP jsem tu zanedbal)

Pokud vám nevadily změny příznakových registrů, tak jste nahradili LD A,0 (2 bajty, 7T) instrukcí XOR A (1 bajt, 4T)&#8230; atakdál. Náhodná čísla (spíš pseudonáhodná) jste mohli řešit třeba posloupností rotací a XORů s obsahem ROM (a třeba s registrem R). Algoritmus kreslení kruhu, který byl v BASICu řešen jako vykreslení obloukové úseče s úhlem 2PI, jste nahradili Bresenhamovým algoritmem, stejně tak čáru&#8230;

A jestli si pamatujete na Busysoftovo &#8222;MDA demo&#8220;, ve kterém běžel text v borderu, tak vězte, že byl vytvořený jako posloupnost instrukcí OUT (C),D a OUT (C), E &#8211; v jednom byla barva, ve druhém pozadí. Jedna čárka tak zabrala 12T, a aby bylo scrollování jemnější, byly před tím dvě instrukce NOP a podle fáze se skákalo buď na první (8 prázdných cyklů na začátku), druhý (4 prázdné cykly) nebo rovnou na OUT (0 prázdných cyklů). Změna obrazce se pak prováděla tak, že blok OUTů byl přepsán tak, aby se střídaly registry D a E podle znakové sady&#8230;

Takovéhle optimalizace byly fajn ještě do doby prvních šestnáctibitových procesorů. Pak přišel pipelining, který spoustu triků se samomodifikujícím se kódem &#8222;rozbil&#8220; &#8211; a už se to nikdy nespravilo.

Dneska podobné optimalizace umí dělat ti maníci, co píšou překladače. Ti znají procesor &#8222;do posledního hradla&#8220; a vědí, jak instrukce přeuspořádat tak, aby fungovaly co nejrychleji a aby pomohli procesoru v jeho spekulativních skocích a podobných skopičinách. Pokud nejste takovíhle master silicon nindžové, tak se rozlučte s myšlenkou, že ručně napíšete pro moderní procesor efektivnější kód, než jaký vytvoří optimalizující překladač.

Stejně tak se nevyplácí investovat moc času do optimalizací běžných aplikací, protože paměť i výkon jsou obrovské.

Jestli chcete ještě dneska někde hrát na bajty a cykly procesoru, musíte buď psát nízkoúrovňové ovladače, časově kritické úlohy, nebo embedded aplikace, tam se s podobnými výzvami ještě dneska setkáte.

Takže co zbylo z osmibitových triků pro dnešní použití? Vlastně nic moc, jen nějaké obecné techniky (double/triple buffering například), které nesouvisí až tak moc s procesorem.

Ale bohužel, pro vývoj webu, běžných aplikací, nedejbože Javovských aplikací, tyhle triky nepoužijete. Zato máte k dispozici jiné&#8230; Ale to je už jiná pohádka.