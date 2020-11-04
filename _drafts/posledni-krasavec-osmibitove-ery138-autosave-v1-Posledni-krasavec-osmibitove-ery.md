---
id: 142
title: Poslední krasavec osmibitové éry
date: 2014-11-23T14:10:23+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/138-autosave-v1/
permalink: /138-autosave-v1/
---
Éra osmibitových mikroprocesorů trvala, když ji budeme hodnotit velmi striktně, jen pár let. První osmibit od Intelu přišel na trh v roce 1972 (8008), první šestnáctibit představila stejná firma v roce 1978&#8230;

<!--more-->

Samozřejmě to nebylo takhle ostře ohraničené. Neznamenalo to, že v roce 1978 všichni zahodili osmibity a vrhli se na 16 bitů, ani že před rokem 1978 neexistovaly šestnáctibity. Encyklopedie uvádějí jako první šestnáctibitový mikroprocesor IPC-16A/500, zvaný PACE, který vyráběla společnost National Semiconductor od roku 1974.

Motorola představila svůj osmibit 6800 v roce 1974, šestnáctibitový 68000 přišel v roce 1979. Začínala éra domácích osmibitů, Spectrum vzniklo až za tři roky, Sinclair QL (s 68008, což je 68000 s osmibitovou sběrnicí) v roce 1984. IBM PC s procesorem 8088 byl představen v roce 1981. Tolik tedy k dobovému rámci.

Nejznámější osmibitové počítače &#8222;osmibitové éry&#8220; tedy vznikly až v době, kdy na trhu byly šestnáctibitové procesory &#8211; bohužel mnohem dražší. I architektura byla dražší, výroba desek taky&#8230; pro masové nasazení se tedy až do 90. let vyvíjely osmibitové systémy.

Zilog Z80 byl představen v roce 1976. Velmi vyspělý a komplexní mikroprocesor se stal základem spousty populárních počítačů. Mezi čtenáři není jistě mnoho těch, co by pochybovali o tom, že tahle přepracovaná a silně vylepšená &#8222;osmdesát osmdesátka&#8220; patří do pomyslné síně slávy.

Ale jak jsem už naznačil v [minulém článku o klonech](http://retrocip.uelectronics.info/klony-a-procesory/ "Klony a procesory"): Z80 mi v 80. letech učarovala. Byl to první procesor, se kterým jsem opravdu pracoval a byl jsem z něj nadšený (Slavo Labsky aka Busysoft v jednom svém demu pro ZX Spectrum dokonce Z80 opěvoval jako &#8222;vlastně šestnáctibitový procesor&#8220;).

Když jsem si pak pořídil Atari ST, musel jsem se naučit assembler procesoru 68000 a byl jsem unešen. Ortogonální instrukční sada, spousta adresních módů, prostě paráda! Ale měl jsem zato, že to je možné až u šestnáctibitů. Z mého omylu mě vyvedly dva procesory: 8086, který byl sice šestnáctibitový, ale ve srovnání s 68k to byla tragédie, a pak 6809.

[<img loading="lazy" class="aligncenter size-medium wp-image-139" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/03/6809-650x328.jpg" alt="6809" width="650" height="328" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/03/6809-650x328.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/03/6809-1024x518.jpg 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2014/03/6809.jpg 1476w" sizes="(max-width: 650px) 100vw, 650px" />](http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/03/6809.jpg)

## 6809

Motorola po 6800 samozřejmě neusnula na vavřínech a připravila několik variant téhož procesoru (s integrovanou pamětí, s omezenější sadou atd.) Zároveň se ale připravovala i na věci příští, a tak vyvíjela vhodného nástupce. Bylo jasné, že musí vyvinout šestnáctibit, ale zároveň nechtěla opustit osmibitový trh, protože pro osmibity bylo ještě mnoho let uplatnění. Proto začaly vznikat v laboratořích Motoroly dva procesory naráz. Jedním z nich byl 68000 &#8211; šestnáctibitový procesor, který byl ale navržen jako plně 32bitový s 16bitovou sběrnicí (model 68008 dokonce s osmibitovou &#8211; v té době znamenalo rozšíření datové sběrnice na dvojnásobek neúnosné zdražení celého systému). A druhým byl právě 6809.

V časopise Byte vyšel svého času [článek od dvou návrhářů 6809](http://retro.co.za/6809/documents/Byte_6809_Articles.pdf), takže dodneška víme, proč byl procesor navržen právě tak, jak byl navržen. Dnes je to cenný historický materiál, který popisuje tehdejší stav vývoje a mikroelektroniky. Pojďme se podívat na &#8222;design decisions&#8220;, které návrháři udělali.

V první řadě bylo jasné, že nelze zahodit kompatibilitu s existujícím software pro 6800. Nakonec padlo rozhodnutí obětovat binární kompatibilitu (tu má třeba Z80 s 8080), ale zachovat kompatibilitu na úrovni zdrojového kódu. Tedy tak, aby programy v assembleru 6800 šly přeložit pro nový procesor ve funkčně ekvivalentní podobě. Instrukce se tedy jmenovaly stejně, i když se překládaly na jiné operační kódy.

Zadruhé pro snížení nákladů bylo rozhodnuto, že návrháři použijí části, které se osvědčily v 6800 &#8211; například kombinační logika pro dekódování instrukcí (6809 nepoužíval mikrokód).

Vývojáři také podrobili existující zdrojové kódy pro procesor 6800 statické i dynamické analýze, aby zjistili, co vlastně programátoři používají a jak. Pro zajímavost &#8211; statická analýza, která nezohledňuje např. průběhy smyčkou a pouze počítá výskyty instrukcí, ukázala, že instrukcí LOAD (tedy přesuny z paměti do registrů) je v kódech 23,4 %, STORE (opačným směrem) 15,3 %, volání podprogramů a návratů je 13 procent, 11 procent tvoří podmíněné skoky atd., až ke třem procentům instrukcí sčítání a odčítání. Bylo tedy zřejmé, co je třeba posílit, jaké instrukce jsou často používané (a mají tedy zůstaty jednobajtové) a jaké je možné přesunout mezi instrukce s prefixem&#8230;

Další analýza se zaměřila na instrukce, které adresují pomocí indexu. Nejčastěji (53 % případů) byl index v rozmezí 1 &#8211; 31, ve 40 % případů byl index roven nule, 6 %  případů připadalo na index v rozmezí 64 &#8211; 255, no a poslední procento tvořily indexy v rozsahu 32 &#8211; 63. Tohle zjištění vedlo například k výraznému rozšíření adresních módů.

Vzhledem k tomu, jak časté byly instrukce INC a DEC ve spojení s adresováním, padlo rozhodnutí přidat adresní mód, který dokáže automaticky zvyšovat a snižovat hodnotu v indexovém registru.

No a v neposlední řadě se rozrostl počet registrů: K šestnáctibitovému indexovému registru X přibyl registr Y, k ukazateli zásobníku S přibyl ukazatel uživatelského zásobníku U, návrháři přidali osmibitový registr DP (direct page) a spojili registry A a B do šestnáctibitového registru D.

Registr DP souvisí s adresováním. Procesory Motorola (i odvozený MOS 6502) měly totiž možnost odkazovat buď plnou adresou (2 bajty), nebo zkrácenou, kdy adresu tvořil jen jeden bajt, ten nižší, a vyšší byl vždy 0. Tomuto přístupu se říkalo &#8222;zero page&#8220;, protože adresoval pouze buňky v rozsahu 0000h &#8211; 00ffh. Takové adresování bylo sice výhodné, ale na druhou stranu bylo v rozsahu nulté stránky brzo přeplněno. U procesoru 6809 existuje taky zkrácené adresování, ale vyšší bajt není automaticky 0, ale je roven právě obsahu registru DP. Programátor tedy může &#8222;nultou stránku&#8220; posunout kamkoli v paměti (resp. nastavit vyšší bajt adresy).

### Adresní módy a indexy

Adresní módy zůstaly tytéž, jaké [známe z procesoru 6800](http://strojak.cz/procesory-a-procesory/). Výrazně byly rozšířeny možnosti indexace. Instrukce, které používají indexovanou adresaci, obsahují (minimálně) o jeden bajt navíc. V tomto bajtu, zvaném &#8222;post byte&#8220;, je uložena informace o tom, jak se indexuje a s jakými registry.

Obecně lze pro indexování použít indexové registry X a Y, ukazatele zásobníku S a U, a s určitými omezeními i ukazatel PC. Co máme tedy na výběr?

  * Indexový registr (X, Y, S, U) + konstantní offset. Zde se rozlišuje několik možností podle toho, jak velký je offset. Pokud je 0, je to označeno v post byte a není zapotřebí další pameťová buňka. Druhá možnost je, že offset je v rozmezí -16 až +15. V takovém případě je offset zapsán rovněž v post byte. Zbývající dvě možnosti jsou &#8222;osmibitový index&#8220; a &#8222;šestnáctibitový index&#8220;.
  * K indexovému registru (X, Y, S, U) je přičtena hodnota registru A, B nebo D (D je šestnáctibitové spojení A a B).
  * Adresa je vzata přímo z indexového registru (X, Y, S, U) a poté je jeho hodnota zvýšena o 1 nebo 2. (Postinkrement)
  * Hodnota v indexovém registru (X, Y, S, U) je snížena o 1 nebo 2 a pak je vzata k adresaci. (Predekrement)
  * Adresa je dána programovým čítačem PC, k němuž je přičten osmibitový nebo šestnáctibitový offset.

Slušná sestava, že? A vynásobte si ji dvěma, protože všechny tyhle adresní módy (až na dvě výjimky) lze použít jako nepřímé (indirect), tj. adresa, kterou získáme indexací, ještě není ta cílová, ale ukazuje do paměti, ze které se přečtou dva bajty, a teprve ty udávají efektivní adresu, se kterou se bude pracovat.

Představme si, že od adresy 0800h máme uložené adresy nějakých dat ke zpracování a chceme je všechny projít. Není třeba žádných čachrů s mnoha registry, stačí třeba do X uložit adresu té tabulky adres (0800h) a pak načítat data přímo z cílových lokací pomocí LDA \[X++\] (hranaté závorky značí nepřímou adresaci, X++ je postinkrement o 2.)

Zmíněné výjimky u nepřímé adresace jsou právě postinkrement / predekrement o 1. Pokud používáte nepřímou adresaci, nelze ji zkombinovat s posunem o 1, vždy o 2!

### Relokace

Výrazné designérské rozhodnutí bylo posílení podpory relokace. Pokud jste pracovali s osmibitovými procesory a la 8080, byla adresa, na jakou má být program nahrán, zadána při překladu. Pokud se pak kód ocitl na jiných adresách, vedly skoky nazdařbůh a havárie nastala v řádu milisekund. Z80 nabídl relativní skok, ale ten není všespásný: jen 128 bajtů dozadu a dopředu, navíc volání podprogramu je opět s absolutní adresou. Pokud si pamatujete na programy MONS3 a GENS3, ty bylo možné nahrát kamkoli do paměti a odtamtud spouštět &#8211; ve skutečnosti byla na začátku rutina, která všechny absolutní adresy v kódu vlastního programu změnila.

Návrháři 6809 posílili &#8222;relokovatelnost&#8220; programů významnou měrou: všechny relativní skoky mohou mít osmibitový i šestnáctibitový offset, včetně instrukce volání podprogramu (JSR). Mohou využívat i indexované adresace, jak jsem psal výše.

### Dva zásobníky

Možná vás napadne FORTH, pro který je tento koncept jako stvořený. Ve skutečnosti návrháři měli na mysli jinou situaci: představte si, že podprogram potřebuje vrátit víc hodnot. Pokud je uloží někam do paměti na pevně danou adresu, nebude reentrantní. Pokud je předá na zásobníku, je potřeba nejprve vytáhnout návratovou adresu, tu si někam uložit (kam ale?), uložit data a pak vrátit návratovou adresu. Právě tenhle problém vývojáři obešli druhým zásobníkem U. Podprogram prostě PUSHuje data, která chce předat, do druhého zásobníku.

Instrukční sada 6809 obsahuje dvě instrukce PUSH &#8211; PSHS a PSHU. první ukládá na standardní zásobník, druhá na uživatelský. Ale co ukládá? Samozřejmě obsah registrů. Zde návrháři použili další trik: za operačním kódem PSHx je další bajt, který říká, které registry se mají na zásobník uložit. Jednou instrukcí tak můžeme uložit kompletní sadu registrů. Což je poměrně elegantní řešení, protože se instrukce pro ukládání obsahu registrů většinou vyskytují v sériích, navíc nemuseli pro každou kombinaci registrů a zásobníků dělat vlastní operační kód.

### Výtky

Ve výše zmíněném článku z Byte jsou sepsány i výhrady čtenářů a uživatelů, na které konstruktéři odpovídají. Například: Proč nejsou blokové operace? Nejsou. Něco bylo třeba obětovat, navíc podle vývojářů je programové řešení blokových operací díky mocným možnostem indexace poměrně jednoduché.

Nebo: Proč nejsou bitové operace? Odpověď: prodražilo by to celý procesor, tak jsme je, vzhledem k tomu, že nejsou často používané, oželeli. Proč je násobení, ale není dělení? Protože násobení je mnohem častější (třeba při přístupu k dvojrozměrným polím).

> Potěšila mě devátá výtka: &#8222;_Proč jste přidali všechny ty adresní módy? Já je nikdy nepoužiju!_&#8220; Jako bych si četl diskuse na Root.cz&#8230;

Na otázku &#8222;proč nemá registr DP vlastní instrukce LOAD a STORE?&#8220; odpověděli tvůrci logicky: jeho obsah je natolik důležitý, že by si programátor měl být vědom toho, že mění něco podstatného, tak to není úplně přímočaré.

### &#8230; a spousta drobností navrch

Procesor 6809 má třeba dvojici instrukcí TFR / EXG. TFR (transfer) přesouvá obsah z jednoho registru do druhého, EXG vzájemně vymění obsah dvou registrů. Platí to jak pro osmibitové (A, B, DP a CC &#8211; příznakový registr), tak pro šestnáctibitové (X, Y, S, U, SP a D).

Přerušení (hardwarové &#8211; IRQ i softwarové &#8211; instrukce SWI, SWI2 a SWI3) uloží automaticky na zásobník všechny registry. Instrukce návratu z přerušení pak zase všechny obnoví. Procesor má ale i vstup FIRQ (Fast IRQ), který vyvolá přerušení, ale registry neukládá.

Podmíněné skoky testují nejen čtyři základní příznakové bity, ale i jejich možné kombinace, takže můžete např. vyvolat skok, pokud je hodnota 1 větší nebo rovna hodnotě 2 v bezznaménkové aritmetice (BHS &#8211; Higher or Same), ale můžete kontrolovat totéž pro čísla se znaménkem (BGE &#8211; Greater or Equal).

Většina instrukcí je velmi rychlá, zabírá jen pár taktů. Na rozdíl od procesorů z rodiny 8080/Z80, které třeba pro načtení operačního kódu a jeho dekódování zaberou čtyři takty, u 6809 je vše mnohem rychlejší: NOP dva takty, podmíněný relativní skok 3 takty, aritmetické či logické operace 6 taktů, násobení 11 taktů (6809 totiž obsahuje hardwarovou násobičku 8&#215;8 bitů). Samozřejmě pokud je potřeba pracovat s indexovaným adresním módem a načítat post byte, prefix či offset, musíte si patřičný počet taktů přičíst&#8230;

[Další podrobnosti o instrukční sadě.](http://koti.mbnet.fi/~atjs/mc6809/Information/6809Instructions.txt)

### Kam s ním?

6809 přišel na trh na konci 70. let, čtyři roky po Z80. Mohly tyhle čtyři roky znamenat, že 6809 už nijak výrazně do osmibitové éry nepromluvil? Dost možná ano. I když všechna ta Spectra a Atari přišla až začátkem 80. let, měl 6809 smůlu. Pravděpodobným důvodem byla cena, a pak další náklady na přepsání software nebo učení nové instrukční sady.

A tak vlastně jediný &#8222;známější&#8220; počítač, který používal 6809, byl TRS-80 CoCo, neboli &#8222;Tandy/RadioShack Color Computer&#8220;, který přišel na trh v roce 1980. (Číslice 80 v označení TRS-80 odkazuje na předchozí model tohoto počítače, který používal Z80.) Postupně vznikly tři verze, v Británii se vyráběly klony tohoto počítače pod označením Dragon 32 / Dragon 64. Z dalších osmibitů používaly 6809 francouzské počítače Thomson MO5 / TO7 / TO8, japonské FM7 a FM8 od Fujitsu, nebo počítač [Aamber Pegasus](http://www.neoncluster.com/aamber_pegasus/aamber_pegasus.html) (1981), který svým minimalistickým návrhem připomínal trochu ZX80/ZX81. _Všimněte si, že počítače s 6809 byly na trhu dřív než třeba Sinclairovy geniální stroje se Z80. Na chvíli se zasním a představím si, jak by vypadalo Spectrum s 6809&#8230; Možná si na to napíšu emulátor. 😉_

Pro 6809 vznikl i zajímavý software. Kromě FORTHu nebo operačního systému OS-9 (později upraven jako [NitrOS-9](http://sourceforge.net/apps/mediawiki/nitros9/index.php?title=Main_Page)) to byl třeba i Microsoft BASIC &#8211; ten ještě v době, kdy Bill Gates psal sám programy.  Gates později v [rozhovoru](http://americanhistory.si.edu/comphist/gates.htm) označil 6809 za nejlepší osmibitový procesor vůbec.

A já v tomhle s Billem souhlasím.

Škoda jen, že přišel (asi) pozdě. Sice bylo možné pomocí transpileru přeložit zdrojové programy 6809 pro mikroprocesor 68000, ale to už byla spíš jen labutí píseň, stejně jako procesor Hitachi HD6309, [o kterém jsem se zmiňoval minule](http://retrocip.uelectronics.info/klony-a-procesory/ "Klony a procesory"). Ostatně 6309 si zaslouží ještě samostatný díl, protože jeho historie je taky zajímavá. Příště&#8230;

_Poznámka: Při pátrání v archivech mě zarazilo, jak obtížné je najít dnes informace o tom, kdy byl který procesor uveden na trh. V datasheetech tyto informace nejsou, a krom vyložených procesorových legend, u nichž jsou data notoricky známá, jsou informace zmatené a neúplné, u spousty z nich to je například &#8222;v 70. letech&#8220;, u procesoru HD6309 jsem rok uvedení na trh například nedohledal vůbec._