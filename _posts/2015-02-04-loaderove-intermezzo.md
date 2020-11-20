---
id: 500
title: Loaderové intermezzo
date: 2015-02-04T10:46:22+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=500
permalink: /loaderove-intermezzo/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3484137593"
categories:
  - Software
  - Stroják
tags:
  - loadery
  - Sinclair
  - Spectrum
  - ZX
---
Trošku nečekaně se mi v mém plánu na články o loaderech objevil jeden hezký kousek, o který bych se s vámi rád podělil. Není moc efektní, jeho kouzlo je schované uvnitř, ale kdoví, třeba se bude někomu hodit.

<!--more-->

Zatímco se jinde často jde od teorie k praxi, zde to zkusím obráceně a začnu rovnou praxí. Tady máte [jeden .tzx soubor](https://retrocip.cz/zxs/games/loader.tzx), zkuste si ho v emulátoru spustit&#8230;

(nezbytná pauza na stažení, spuštění emulátoru, spuštění nahrávání&#8230; čekáme&#8230; aha&#8230; aha&#8230; hmm&#8230; a co to jako je, to je všechno?)

Nojo, to je všechno. Prostě se jen nahrávají obrazovky z pásku. Já vám to říkal, že to není moc na efekt. Ale zkuste se na ten TAP podívat. _Jasně, BASIC, jasně, loader, jednotlivé obrazovky, nojo, 2682 bajtů, 2189, 3340, 4522 bajtů, aha, jsou komprimované&#8230; aha, ale nahrávají se přímo&#8230; ten loader je musí dekomprimovat za běhu!_

No vidíte, že to šlo!

V [minulém článku](https://retrocip.uelectronics.info/zx-spectrum-a-loadery-1/ "ZX Spectrum a loadery – 1") jsem frajersky tvrdil, že Digisynth při nahrávání data rozbaloval. _Tyjo, opravdu? Nezdálo se mi to? Paměť mám děravou&#8230;_ Tak jsem stáhnul Digisynth, chvíli brejlil do kódu a pak jsem zahlédl povědomou pasáž: Ne, nezdálo! Dobrý, chtěl jsem to pustit z hlavy, ale znáte to&#8230; V metru mi to začalo vrtat: Vždyť nemůže být složité ten loader rekonstruovat a udělat k němu i packer&#8230; Huffmanova komprese je jednoduchá&#8230;

## Hufmannova komprese

je opravdu jednoduchá. Tedy ve chvíli, kdy už trošku kompresních metod znáte. Pokud ne, dovolte mi rychlý kurz:

Nejjednodušší kompresní metody jsou takové, které eliminují dlouhé sekvence bajtů o stejné hodnotě (RLE). Jsou rychlé a nenáročné, takže je lze použít i v loaderu a nasadit třeba do kopíráku (takže se do paměti vejde víc než by se tam vejít mělo, jelikož kopírák při nahrávání data kompresuje tímhle jednoduchým způsobem: &#8222;Následuje blok šedesáti nul!&#8220;

Lepší kompresní metody využívají toho, že se některé sekvence často opakují. Vytváří si proto slovník opakujících se sekvencí, a ty nahrazují kratším kódem. Proto se jim říká &#8222;slovníkové metody&#8220;. Vycházejí z pramáti slovníkových algoritmů, totiž LZ (Lempel-Ziv).

Jiný přístup zvolil pan Huffman a navrhl [kompresi](https://cs.wikipedia.org/wiki/Huffmanovo_k%C3%B3dov%C3%A1n%C3%AD), založenou nikoli na sekvencích, ale na četnosti výskytu určitých hodnot. Zkrátka vezme všechny hodnoty v souboru (třeba bajty, takže 0 až 255) a spočítá, kolikrát se jaká hodnota ve vstupním souboru vyskytne. Pomocí téhle informace vytvoří pro každou hodnotu kód (sekvenci bitů), který má tu vlastnost, že čím je hodnota častější, tím je sekvence kratší. Například u těch obrazovek se velmi často vyskytuje nula. Pokud je tam opravdu velmi často, je zakódována klidně jako dva bity. V extrémním případě i jako jeden. Ano, na druhou stranu ty málo používané hodnoty jsou delší, zaberou klidně dvanáct, patnáct bitů. Ale tuhle ztrátu bohatě vynahradí úspora u těch častých.

Pokud jsou ve vstupním souboru různé hodnoty, a všechny přibližně stejněkrát, tak se Huffmanova komprese stane neefektivní, ale pro běžné soubory má výsledky dobré. Často se také používá pro komprimaci hodnot ze slovníkové komprese LZ (LZHUF). Je použita i v algoritmu JPEG&#8230;

Do implementačních detailů nebudu zabíhat, stačí vědět, že si algoritmus vytvoří binární strom, který nakonec má přesně tu vlastnost, co jsem výše popisoval.

Nevýhoda je, že pro rozkódování potřebujeme předat nejdřív tenhle strom. Tuto nevýhodu obchází adaptivní Huffmanovo kódování, ale to je zase výpočetně náročnější při dekompresi. Na Spectru v loaderu nic moc&#8230;

Dekompresní strom tak, jak je implementovaný, je v zásadě velmi jednoduchá struktura. Každá položka má dvě hodnoty, jednu pro nulový bit, druhou pro jedničkový. Hodnota je buď odkaz na jinou položku (pokud kód pokračuje), nebo výsledné číslo.

### Názorně

Dejme tomu, že máme řetězec ABRAKADABRA. Strom je vidět [zde](https://atrey.karlin.mff.cuni.cz/~tnovak/compress/huffman/), a výsledný kód je:

A: 0  
B: 100  
R: 11  
K: 1011  
D: 1010

Dekompresní strom tedy bude vypadat takto:

<pre class="">0: [*, A | ., 1]
1: [., 2 | *, R]
2: [*, B | ., 3]
3: [*, D | *, K]</pre>

Nebojte, hned vysvětlím. Dekomprese začíná vždy u první položky. Pokud je první bit nulový, bere se levá část (*, A), pokud jedničkový, pravá (., 1). Hvězdička znamená &#8222;Už vím, co to je za znak, je to tenhle!&#8220;, tečka znamená &#8222;pokračuj daným záznamem&#8220;.

Dejme tomu, že budou chodit bity 0, 1, 0, 0, 1, 1, 0, &#8230; Co se bude dít?

  * Záznam číslo 0.
  * Bit=0: levá část oznamuje, že jsme našli znak A. Máme první bajt, začínáme znovu záznamem 0
  * Bit=1: pravá část říká, že máme pokračovat záznamem 1
  * Bit=0: levá část (.,2) říká, že máme pokračovat záznamem číslo 2
  * Bit=0: levá část záznamu 2 říká, že jsme našli znak B. Máme druhý, začínáme znovu záznamem 0
  * Bit=1: pravá část říká, že máme pokračovat záznamem 1
  * Bit=1: pravá část říká, že jsme našli znak R. Znovu od záznamu 0
  * Bit=0: levá část oznamuje, že jsme našli znak A.
  * &#8230; a tak dále.

## Implementace

Kompresní algoritmus jsem si napsal v JavaScriptu. [Můžete ho použít i vy](https://retrocip.cz/zxs/games/huffman.html), je to normální HTML stránka, kam pomocí drag a drop přenesete soubor, který chcete zkomprimovat, a stáhne se vám .tap soubor s výsledkem, vhodným k načtení loaderem. _Upozornění: Na vašem Pentiu MMX s prohlížečem IE3 to pravděpodobně fungovat nebude. Ani s novým IE to nefunguje. Použijte Chrome nebo Firefox, díky._

Výsledný soubor má následující formát:

  1. Příznakový bajt. Ignoruju ho
  2. Kontrolní součet. XOR všech hodnot vstupního souboru
  3. Délka dekompresní tabulky (počet záznamů)
  4. Data pro dekompresní tabulku. Každý záznam je uložen jako 2&#215;9 bitů. První bit je příznak, následujících 8 hodnota. Příznak určuje, jestli jde o cílovou hodnotu (1), nebo o odkaz (0). Prvních 9 bitů je pro nulový bit, zbývajících 9 pro jedničkový.
  5. Délka souboru v bajtech. 2 byty.
  6. Zkompresovaný soubor jako bitový tok
  7. Zarovnání počtu bitů na celou osmici, aby nebyly problémy při kopírování souboru

Vlastní loader na začátku kopíruje standardní ROMkový loader, jak byl popsán [minule](https://retrocip.uelectronics.info/zx-spectrum-a-loadery-1/ "ZX Spectrum a loadery – 1"). Pro načtení celého bajtu používám lehce upravenou rutinu LD\_8\_BITS, která neskládá výsledek do registru L, ale do registru E. LD_EDGE nejsou součástí rutiny, volám ty z ROMky (protože nedělám žádný speciální efekt, navíc s tím takto líp pracují emulátory a dovolí i různé zrychlené nahrávání).

Pro dekompresní tabulku je potřeba najít 1kB místa od adresy, která je zarovnaná na hodnotu $400. Já zvolil $FC00, ale můžete zvolit i jinou. Při ukládání je příznak hodnota/odkaz uložen do paměti jako celý byte (00/FF), testování je pak jednodušší (prostou rotací se zkopíruje do CY jednička nebo nula).

Rutina nekontroluje flag byte ani nebere délku dat, jediné, co potřebuje, je adresa pro uložení dat v registru IX.

V rutině nejsou žádné speciální triky, všechno je tak přímočaré, jak jsem výše popsal.

Jo a ještě: Pokud chcete, můžete ji použít pro svoje výtvory, je pod licencí CC-0 (Public Domain). Poděkování kdyžtak směřujte na autora původní rutiny z DigiSynthu&#8230;

Samozřejmě by šlo vylepšit kompresi, odstranit opakované sekvence, předkomprimovat, čímž by se výsledný kompresní poměr ještě zlepšil. Mým cílem nebylo představit nadupaný kompresor, ale ukázat, jak se dá do loaderu zakomponovat zajímavá funkcionalita.

PS: [Manic Miner v Huffmanově podání](https://retrocip.cz/zxs/games/hmanic.tzx)

<pre class="lang:default decode:true ">.ORG    $f000 ;61440
          .ENGINE zxs 

                  ;test loaderu
AGAIN:            
          LD      ix,$4000 
          CALL    LD_BYTES 
          JP      again 

                  ;---- Víceméně kopie ROMkových rutin - ignoruju flag byte a příznaky

LD_BYTES:         
          DI      ;Zákaz přerušení.
          LD      A,$0F ;Bílý BORDER.
          OUT     ($FE),A 
          LD      HL,$053F ;Adresa SA/LD_RET
          PUSH    HL ;na zásobník.
          IN      A,($FE) ;Test brány $FE.
          RRA     ;Rotace načteného
          AND     $20 ;bajtu, ale zvážení jen bitu EAR.
          OR      $02 ;Signál BORDER červený je uložen i do
          LD      C,A ;registru C. ($22 pro OFF a $02 pro ON stav vstupu EAR)
          CP      A ;Nulový indikátor je nastaven na 1.

                  ;Prvním úkolem při čtení dat z pásku je zjistit,
                  ;zda existuje nějaký pulsní signál (tedy hrany
                  ;on-off a off-on).

LD_BREAK:         
          RET     NZ ;Návrat při BREAKu.
LD_START:         
          CALL    LD_EDGE_1 ;Není-li přítomen signál během 1400 T
          JR      NC,LD_BREAK ;stavů, návrat s CY=1.
                  ;Jinak je BORDER nastaven na cyan.

                  ;Dále se čeká a zjišťuje, zda je signál stále přítomen.

          LD      HL,$0415 ;Délka čekání je téměř 1 sekunda.
LD_WAIT:          
          DJNZ    LD_WAIT 
          DEC     HL 
          LD      A,H 
          OR      L 
          JR      NZ,LD_WAIT ;Čekací smyčka.
          CALL    LD_EDGE_2 ;Pokračuj při zachycení dvou po sobě
          JR      NC,LD_BREAK ;jdoucích hran v dané periodě.

                  ;Nyní bude přijat jen zaváděcí signál.

LD_LEADER:        
          LD      B,$9C ;Časovací konstanta,
          CALL    LD_EDGE_2 ;Pokračuj při zachycení dvou po sobě
          JR      NC,LD_BREAK ;jdoucích hran v dané periodě.
          LD      A,$C6 ;Tyto hrany musí být zachyceny během
          CP      B ;3000 T.
          JR      NC,LD_START 
          INC     H ;Počet párů hran je ukládán do reg. H
          JR      NZ,LD_LEADER ;dokud jich není 256.

                  ;Po zaváděcím signálu přicházejí části off a on pulsu sync.

LD_SYNC:          
          LD      B,$C9 ;Časovací konstanta.
          CALL    LD_EDGE_1 ;Každá hrana je testována,
          JR      NC,LD_BREAK ;dokud nejsou nalezeny dvě hrany blízko sebe.
          LD      A,B ;(startovací puls sync).
          CP      $D4 
          JR      NC,LD_SYNC 
          CALL    LD_EDGE_1 ;Na konci musí být ještě konečná hrana části on
          RET     NC ;synchronizačního pulsu sync.

                  ;Teď už se mohou načítat bajty hlavičky nebo
                  ;programu (bloku dat) v operacích LOAD, VERIFY.
                  ;První bajt určuje typ.

          LD      A,C ;BORDER na zelenou / magenta
          XOR     $06 
          LD      C,A 

                  ;---------
                  ; tady začíná vlastní nahrávání.
                  ; Nejprve se vytváří dekompresní strom
                  ; Ukládá se od adresy FC00 (musí být dělitelná $400)
                  ; konstanta HUF_TABLE je tato adresa / $400

HUF_TABLE EQU     $3F 

          LD      hl,HUF_TABLE * $400 
          CALL    LD_byte ;flag byte -&gt; E
          RET     nc 
                  ;zahodim flag
          CALL    LD_byte ;checksum -&gt; E
          RET     nc 
                  ;ulozim si checksum do A'
          LD      a,e 
          EX      af,af' 
          CALL    LD_byte ; počet čtveřic v kompresním stromu
          RET     nc 
          LD      d,e 
                  ; D hodnot pro tabulku
HUF_DECODE:       
          LD      B,$B2 
          CALL    LD_EDGE_2 ;Nalezení délky pulsů jednotlivých bitů.
          RET     NC ;Návrat při nesprávné (větší) délce pulsu, (pak CY=0)
          LD      A,$CB ;Porovnání délky oproti asi 2400 T,
          CP      B ;kdy pro nulový bit je CY=0 a pro jedničkový bit je CY=1.
                  ; první bit záznamu udává příznak hodnota / odkaz
          SBC     a,a ; CY=0 -&gt; 00, CY=1 -&gt; FF
          LD      (hl),a ; ukládám do paměti příznak jako 00 nebo FF
          INC     hl ; a za něj první hodnotu / odkaz
          CALL    ld_byte ; načtu bajt
          LD      (hl),e ; a uložím.
          INC     hl ; Teď se bude obojí opakovat pro jedničkový bit
          LD      B,$AF 
          CALL    LD_EDGE_2 ;Nalezení délky pulsů jednotlivých bitů.
          RET     NC ;Návrat při nesprávné (větší) délce pulsu, (pak CY=0)
          LD      A,$CB ;Porovnání délky oproti asi 2400 T,
          CP      B ;kdy pro nulový bit je CY=0 a pro jedničkový bit je CY=1.
                  ; jeden bit příznaku
          SBC     a,a 
          LD      (hl),a 
          INC     hl 
          CALL    ld_byte 
          LD      (hl),e 
          INC     hl ; čtveřice hotova
          DEC     d ; už mám kompletní strom?
          JR      nz,huf_decode ; Ještě ne, tak čtu dál.

                  ; Tabulka je hotova, tak můžeme nahrávat data
                  ; Nejprve délku do registrů DE
          CALL    ld_byte 
          LD      h,e 
          CALL    ld_byte 
          LD      d,e 
          LD      e,h 

          LD      A,C ;BORDER na modrou a žlutou.
          XOR     $05 
          LD      C,A 

                  ; vlastní smyčka pro nahrávání
HUF_LOAD:         
          LD      b,$b2 
          LD      l,0 
HUF_BIT:          
          CALL    LD_EDGE_2 
          RET     nc 
          LD      a,b 
          CP      $cc ; lehce upravená konstanta
          LD      h,HUF_TABLE ; H je vyšší bajt adresy / 4
                  ; L nižší (ukazatel na záznam)
          CCF     
          ADC     hl,hl 
          ADD     hl,hl 

                  ; HL = adresa tabulky * 4 + 2 * CY
                  ; tedy pro CY=0 je to 4*HL, pro CY=1 je to 4*HL+2

          RRC     (hl) ; příznak hodnota/odkaz do CY
          INC     hl 
          LD      l,(hl) ; v L je teď hodnota nebo odkaz
          LD      b,$b1 ; připravíme časovací konstantu
          JR      nc,huf_bit ; pokud šlo o odkaz, tak pokračujeme s dalším bitem
          LD      (ix+0],l ; když ne, máme v L hodnotu k uložení
          INC     ix ; adresa++
          DEC     de ; počítadlo--
          EX      af,af' 
          XOR     l ; kontrolní součet v A'
          EX      af,af' 
          LD      a,d ; Už jsou všechny bajty načteny?
          OR      e 

          JR      nz,huf_load ; Ještě ne!

          EX      af,af' 
          CP      01 ; pokud je A' nenulový, bude CY=0 - tedy chyba

          RET     

                  ; načtení bajtu do reg. E

LD_BYTE:          
          LD      B,$B2 ;Časovací konstanta.
LD_MARKER:        
          LD      E,$01 ;Uložení značkového bitu.

                  ;Tato smyčka sestavuje načítaný bajt do registru E.

LD_8_BITS:        
          CALL    LD_EDGE_2 ;Nalezení délky pulsů jednotlivých bitů.
          RET     NC ;Návrat při nesprávné (větší) délce pulsu, (pak CY=0)
          LD      A,$CB ;Porovnání délky oproti asi 2400 T,
          CP      B ;kdy pro nulový bit je CY=0 a pro jedničkový bit je CY=1.
          RL      E ;Uložení nového bitu do registru E.
          LD      B,$B0 ;Časová konstanta pro další bit.
          JR      NC,LD_8_BITS ;Nejednalo-li se o poslední (osmý)bit
                  ;skok zpět do smyčky.
                  ; 
          RET     ;návrat s CY=1

LD_EDGE_2 EQU     $05e3 
LD_EDGE_1 EQU     $05e7 
</pre>

&nbsp;