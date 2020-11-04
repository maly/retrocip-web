---
id: 532
title: 'ZX Spectrum a loadery &#8211; 1'
date: 2015-03-21T12:18:19+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/493-autosave-v1/
permalink: /493-autosave-v1/
---
Každý Spectrista jistě aspoň jednou viděl při nahrávání efekt, u něhož žasnul: Jak to dělají? Tak pojďte a poslyšte&#8230; Bude to dlouhé a možná se i něco dozvíte.

<!--more-->

ZX Spectrum nahrávalo programy na kazetu a z kazety čistě softwarově, nemělo na to žádný speciální obvod, jako třeba PMD. Na té nejvyšší úrovni se programy nahrávaly jako dvojice &#8222;hlavička + data&#8220;, kde v hlavičce byly uloženy informace o souboru (jméno, typ, délka apod.) a v datech vlastní obsah.

Hlavička má délku 17 bajtů a následující strukturu:

<table>
  <tr>
    <th>
      Pozice
    </th>
    
    <th>
      Délka
    </th>
    
    <th>
      Popis
    </th>
  </tr>
  
  <tr>
    <td>
    </td>
    
    <td>
      1
    </td>
    
    <td>
      Typ (0=program, 1=number array, 2=character array, 3=code)
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      10
    </td>
    
    <td>
      Jméno (zprava doplněno mezerami na 10 znaků)
    </td>
  </tr>
  
  <tr>
    <td>
      11
    </td>
    
    <td>
      2
    </td>
    
    <td>
      Délka datového bloku
    </td>
  </tr>
  
  <tr>
    <td>
      13
    </td>
    
    <td>
      2
    </td>
    
    <td>
      Parametr 1. Např. u programů je to parametr LINE, u code adresa počátku dat
    </td>
  </tr>
  
  <tr>
    <td>
      15
    </td>
    
    <td>
      2
    </td>
    
    <td>
      Parametr 2. Např. u programů je to začátek proměnných
    </td>
  </tr>
</table>

Data žádnou takovou strukturu nemají, jsou to prostě jen data.

Sestupme o úroveň níž:  každý blok byl uvozen jedním příznakovým bajtem ($00 pro hlavičku, $ff pro data) a ukončen kontrolním součtem (všechny bajty, včetně příznakového, XORovány dohromady). Hlavička tedy měla ve skutečnosti 19 bajtů &#8211; začínala 0 a končila kontrolním součtem, ovšem tyto bajty &#8222;zkonzumoval&#8220; systém a vám se do rukou nedostaly.

Tady končí ta úroveň, na kterou jste se mohli dostat voláním rutin z ROMky. Níž jsou už jen

## Jedničky a nuly

Data byla na pásek zapisována jako posloupnost pulsů. O jejich generování, včetně správného časování, se staral procesor.

Každý záznam byl uvozen takzvaným _pilotním tónem_. Ten byl tvořen obdélníkovým signálem, tj. pravidelně se střídaly stavy ON a OFF, a to tak, že 2168 T (T je takt procesoru) byl výstup MIC ve stavu ON, 2168T ve stavu OFF. Zaváděcí tón připravil vstupní obvody u kazeťáku na správnou úroveň hlasitosti (pokud tedy nějaké takové byly). Před hlavičkou trval 5 sekund, před datovým blokem dvě sekundy. O délce zaváděcího tónu rozhoduje příznakový bajt, všechny hodnoty menší než 128 mají &#8222;dlouhý&#8220;, 128 a větší mají krátký.

Po pilotním tónu následoval synchronizační puls. Ten sloužil k tomu, aby nahrávací rutina poznala, že končí pilotní tón a začala zpracovávat data. Synchronizační puls měl 667T ON a 735T OFF.

Po synchronizačním pulsu následovaly už bajty dat. Nejprve flag byte, pak obsah, nakonec kontrolní součet. Bajty byly posílány po bitech od nejvyššího. Každý bit byl představován pulsem (tedy přechodem do ON a přechodem do OFF), lišila se ale délka. U log. 0 to bylo 855T ON a 855T OFF, u log. 1 dvojnásobek, tedy 1710T ON a 1710T OFF.

Při čtení dat (a o to nám tu půjde) pak procesor měřil dobu, za jakou se vstupní signál změní (tj. čas mezi dvěma přechody, tzv. &#8222;hranami&#8220;). Záznam tak nebyl citlivý na polaritu (jako např. u zmíněného PMD v první verzi), zato procesor neměl už moc času na to dělat něco jiného, protože vlastně jen poslouchal vstup EAR, počítal průchody smyčkou, dokud se signál nezměnil, a k tomu kontroloval stisk klávesy SPACE (ten přerušil nahrávání, jistě se pamatujete).

## Čas pro assembler

Teď nastala ta chvíle, kdy přijde výpis ROMky (komentáře beru z českého vydání komentovaného výpisu ROM):

<pre class="lang:asm decode:true">;PODPROGRAM LD_BYTES
                  ;Tento podprogram je volán jako funkce 
                  ;LOAD nebo VERIFY hlavičky (z $076E) nebo dat (z $0802).
                  ;v DE je délka souboru, v IX adresa, kam se bude nahrávat
                  ;A=$00 - hlavička, A=$FF pro blok. CY=0 - VERIFY.
          .ORG    $0556 
LD_BYTES:         
          INC     D ; Z flag je vynulován (neboť D nemůže obsahovat $FF).
          EX      AF,AF' ; A=$00 - hlavička, A=$FF pro blok. CY=0 - VERIFY.
          DEC     D ; CY=1 - LOAD. Registr D zpět na původní hodnotu.
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

          LD      A,C ;BORDER na modrou a žlutou.
          XOR     $03 
          LD      C,A 
          LD      H,$00 ;Inicializace bajtu parity na 0.
          LD      B,$B0 ;Časovací konstanta pro bajt určující typ.
          JR      LD_MARKER ;Skok do smyčky čtení bajtu.

                  ;Smyčka čtení bajtu se používá k načtení vždy jednoho bajtu. 
                  ;První je typový, následován
                  ;datovými a na závěr bajt paritní.

LD_LOOP:          
          EX      AF,AF' ;Vyzvednutí indikátorů.
          JR      NZ,LD_FLAG ;Skok pro první (typový) bajt.
          JR      NC,LD_VERIFY ;Skok při VERIFY nahrávky.
          LD      (IX+00),L ;Uložení načteného bajtu na správnou adresu.
          JR      LD_NEXT ;Skok pro načtení dalšího bajtu.
LD_FLAG:          
          RL      C ;Dočasné uložení CY.
          XOR     L ;Návrat když se typový bajt liší od typového bajtu
          RET     NZ ;z pásky; CY=0.
          LD      A,C ;CY je obnoveno na původní hodnotu.
          RRA     
          LD      C,A 
          INC     DE ;Čítač je zvětšen, aby bylo kompenzováno jeho
          JR      LD_DEC ;zmenšení po odskoku.

                  ;Při VERIFYkaci je nově načtený bajt porovnán s původním:

LD_VERIFY:        
          LD      A,(IX+00) ;Zjištění hodnoty původního bajtu a
          XOR     L ;porovnání s právě načteným bajtem.
          RET     NZ ;Návrat, jestliže se oba bajty nerovnají.

                  ;Nový bajt bude načten po jednotlivých bitech.

LD_NEXT:          
          INC     IX ;Zvýšení adresy pro uložení dalšího bajtu.
LD_DEC:           
          DEC     DE ;Snížení čítače délky bloku.
          EX      AF,AF' ;Uložení indikátorů.
          LD      B,$B2 ;Časovací konstanta.
LD_MARKER:        
          LD      L,$01 ;Uložení značkového bitu.

                  ;Tato smyčka sestavuje načítaný bajt do registru L.

LD_8_BITS:        
          CALL    LD_EDGE_2 ;Nalezení délky pulsů jednotlivých bitů.
          RET     NC ;Návrat při nesprávné (větší) délce pulsu, (pak CY=0)
          LD      A,$CB ;Porovnání délky oproti asi 2400 T,
          CP      B ;kdy pro nulový bit je CY=0 a pro jedničkový bit je CY=1.
          RL      L ;Uložení nového bitu do registru L.
          LD      B,$B0 ;Časová konstanta pro další bit.
          JP      NC,LD_8_BITS ;Nejednalo-li se o poslední (osmý)bit 
          				;skok zpět do smyčky.
          LD      A,H ;Vyzvedni paritní bajt z reg.H.
          XOR     L ;a přidej nový bajt.
          LD      H,A ;Výsledek zpět do H.

                  ;Průchody se opakují do vynulování čítače DE, pak musí být 
                  ;i paritní bajt nulový.

          LD      A,D 
          OR      E 
          JR      NZ,LD_LOOP ;Je-li DE nenulový, skok zpět pro další bajt.
          LD      A,H 
          CP      $01 ;Test paritního bajtu.
          RET     ;Je-li paritní bajt nulový - návrat s CY=1, jinak CY=0.

                  ;PODPROGRAMY LD_EDGE_2 A LD_EDGE_1
                  ;Tyto dva podprogramy jsou nejdůležitější částí operací 
                  ;LOAD a VERIFY. Vstupuje se do nich s
                  ;časovou konstantou v registru B a barvou BORDERu i 
                  ;"typem hrany" v registru C.
                  ;Návrat z těchto podprogramů je s CY=1, když požadovaný 
                  ;počet hran byl nalezen v povolené
                  ;periodě - pak změna v registru B u CY=0 při chybě. 
                  ;Z flag=0 pokud byla stisknuta klávesa BREAK.
                  ;Zflag=1 znamená "bez nálezu" a provede se návrat. 
                  ;LD_EDGE_2 slouží pro nalezení délky kompletního
                  ;pulsu. LD_EDGE_1 slouží k určení času, který uplyne 
                  ;do nalezení hrany.

LD_EDGE_2:        
          CALL    LD_EDGE_1 ;Zde se v podstatě volá ještě jednou LD_EDGE_1 a
          RET     NC ;návrat pokud došlo k chybě.
LD_EDGE_1:        
          LD      A,$16 ;Čekání 358 T před vstupem do
LD_DELAY:         
          DEC     A ;vzorkovací smyčky.
          JR      NZ,LD_DELAY 
          AND     A 

                  ;Následuje vzorkovací smyčka. Obsah registru B je zvýšen 
                  ;při každém průchodu. Návrat "bez
                  ;nálezu" při dosažení 0 v reg. B.

LD_SAMPLE:        
          INC     B ;Čítání průchodů.
          RET     Z ;CY=0 & Z=1 "bez nálezu".
          LD      A,$7F ;Čtení z brány $7FFE (BREAK a EAR).
          IN      A,($FE) 
          RRA     
          RET     NC ;CY=0 & Z=0, když byla stisknuta klávesa BREAK.
          XOR     C ;Test bajtu s posledním typem hrany.
          AND     $20 
          JR      Z,LD_SAMPLE ;Skok zpět, když je beze změny.

                  ;Byla nalezena nová 'hrana' ve stanovené době k hledání. 
                  ;Takže následuje změna barvy a nastavení
                  ;CY flagu.
          LD      A,C ;Změna posledního "typu hrany"
          CPL     ;a barvy BORDERu.
          LD      C,A 
          AND     $07 ;Bere se jen barva BORDERu.
          OR      $08 ;Signál MIC-off.
          OUT     ($FE),A ;Změna barvy BORDERu červená/fialová 
          			;nebo modrá/žlutá.
          SCF     ;Signalizace úspěšného nalezení
          RET     ;před návratem.

                  ;Podprogram LD_EDGE_1 trvá 465T, 
                  ;plus 58T z každého průchodu vzorkovací smyčkou při 
                  ;neúspěšném hledání. Například při očekávání sync pulsu 
                  ;(viz. LD_SYNC na $058F) je povoleno 10 průchodů
                  ;vzorkovací smyčkou. Hledání hrany probíhá během cca 
                  ;1100T (465+10+58+přechody). Tak je zajištěno
                  ;zachycení části off pulsu sync, který přichází po 
                  ;dlouhých pulsech zaváděcího signálu.
</pre>

V tomto článku není prostor na podrobné probírání toho, jak algoritmus funguje. Rychle si ale projdeme některá fakta.

Rutina zakazuje přerušení. Je to logické, závisí totiž na přesném načasování. To je taky důvod, proč nebudou fungovat loadery uložené v pomalé RAM ($4000-$7FFF).

V registru C je uložen poslední stav vstupu EAR, ale posunutý o 1 bit doprava, a v nejnižších třech bitech barva borderu. U leaderu to je 02 (červená) / 05 (cyan), u dat pak 01 (modrá) / 06 (žlutá). Proč o 1 bit doprava? Při načítání je provedena sekvence LD A, $7F a IN A, ($FE). Hodnota 7F je vyslána na horních 8 bitů adresy, tj. při čtení klávesnice vybere řádek _B &#8211; N &#8211; M &#8211; Symbol Shift &#8211; Space_. Stav těchto kláves je v nejnižších pěti bitech (0-4), v bitu 6 je stav EAR. Pomocí instrukce RRC se nejnižší bit (=stav klávesy SPACE) posune do příznaku CY a EAR na pozici 5. bitu. Pokud je CY nulový, znamená to, že byl stisknut mezerník a nahrávání končí.

Rutina začíná detekcí signálu, který by odpovídal pilotnímu tónu (LD\_LEADER). Pokud je načteno 255 takových pulsů, má se zato, že je to pilotní tón a čeká se na kratší puls, synchronizační. Jakmile přijde (LD\_SYNC), můžeme načítat data.

LD_MARKER načte 1 bajt do registru L. Začíná s hodnotou 01, což nahrazuje počítadlo. Postupně se plní bity zprava instrukcí RL L, nejvyšší pak přechází do CY. Dokud je CY=0, načítají se další, jakmile je CY=1, znamená to, že byla načtena kompletní osmice.

Klíčové rutiny jsou LD\_EDGE\_n (kde n je 1 nebo 2). LD\_EDGE\_1 nejprve počká určitý časový úsek (465T) a pak zjišťuje, zda se změnila hodnota na vstupu EAR proti poslední uložené (v registru C, viz výše). Pokud ne, smyčka se opakuje. Při každém průběhu je zároveň zvýšena hodnota v registru B. Jakmile se dostane na nulu, znamená to &#8222;timeout&#8220;, tedy že nepřišla hrana v očekávaném časovém limitu.

Pokud byla hrana nalezena, neguje se obsah registru C. To má za následek jednak změnu uložené hodnoty EAR, ale také změnu barvy borderu.

LD\_EDGE\_2 vlastně provede dva LD\_EDGE\_1 za sebou.

Výstup LD_EDGE je tedy následující:

  * CY = 0, Z = 1 &#8211; V časovém intervalu nepřišla změna EAR (&#8222;timeout&#8220;)
  * CY = 0, Z = 0 &#8211; Stisknuta klávesa SPACE (BREAK)
  * CY = 1 &#8211; hrana byla nalezena, v B je aktuální hodnota počítadla.

Počítadlo v B čítá, jak jsem už psal, směrem nahoru. Při každém průchodu smyčkou, která zabere 58 taktů procesoru, je B zvýšen o jedničku. Příklad: při čtení bitu je volána rutina LD\_EDGE\_2, hledají se tedy dvě hrany. Počítadlo v B je nastaveno na $B0. To znamená, že timeout přijde po $4F průbězích cyklem ($FF-$B0). To představuje 2 x 465T čekací smyčky + 79 * 58T = 5512T. Po takové době tedy rutina zahlásí výpadek signálu.

Výsledná hodnota počítadla je porovnána s hodnotou $CB. Pokud je menší, je to vyhodnoceno jako dva krátké pulsy log. 0, pokud větší, je to log. 1. Hodnota $CB znamená, že smyčka proběhla 27x ($CB-$B0), tedy že dvě hrany přišly za čas menší než 2496T. Připomeňme si: U log. 0 trvají oba impulsy 1710T, u log. 1 je to 3420T. Hodnota mezi těmito dvěma časy je 2565, tedy plusmínus to, co vyšlo i mně. Je to míň, protože nějaké cykly zabere režie (volání podprogramu, vyhodnocování apod., viz LD\_8\_BITS).

## Hackujeme loader

Nebylo to složité, že ne? Tak, teď si řekneme, jak udělat nějaké ty triky&#8230;

Zaprvé: čtecí rutina není úplně &#8222;T-pünktlich&#8220;, takže pár T sem či tam nepředstavuje žádný větší problém. Pokud chceme jednoduchý efekt, můžeme ho přidat bez nějakých složitějších úprav.

### Efekty s borderem

Můžeme třeba obarvit pruhy, pokud se nám nelíbí dvoubarevné. Co třeba duha? Přepíšeme jednoduše konec rutiny LD_EDGE:

<pre class="lang:asm decode:true">LD      A,C 
          INC     A 
          XOR     $20 
          AND     $27 
          LD      C,A 
          AND     $07 
          OR      $08 
          OUT     ($FE),A 
          SCF     
          RET</pre>

Co se tu děje? Místo negace obsahu registru C zvyšujeme hodnotu o 1 a negujeme hodnotu bitu 5. Díky maskování s hodnotou $27 se nám nestane, že při přičítání &#8222;přeteče&#8220; hodnota a ovlivní bit EAR. Bude se stále měnit v rozsahu 0-7 a vytvoří v borderu duhový efekt.

**Poznámka**: Pokud si to budete zkoušet, nezapomeňte nahrávací rutinu umístit do horních 32kB RAM!

Když na konec, před instrukci SCF, přidám ještě dvojici instrukcí XOR A; OUT ($FE), A promění se pruhy v borderu na krátké čárky na černém pozadí.

Sem se vejdou všechny efekty, které nějak pracují s barvou borderu. Buď tím, že jej mění, nebo tím, že jej využívají. Co třeba efekt, který použil Busy u loaderu pro Song In Lines 3 (zakulacené rohy). Vypadá to efektně, co?



Přitom to není těžké&#8230; Čtyři čtverce v rozích obsahují jednoduchý vzorek (&#8222;zakulacení&#8220;). Pixely, které jsou rovny 1 (mají barvu INK) se budou tvářit jako by byly součástí borderu a budou zobrazovat pruhy. Nekoukal jsem se, jak to Busy dělá, ale vsadil bych se, že principiálně nějak takto:

<pre class="lang:asm decode:true">LD      A,C ;Změna posledního "typu hrany"
          CPL     ;a barvy BORDERu.
          LD      C,A 
          AND     $07 ;Bere se jen barva BORDERu.
          OR      $08 ;Signál MIC-off.
          OUT     ($FE),A ;Změna barvy BORDERu červená/fialová 
          			;nebo modrá/žlutá.
          OR      $30 ;v A je: 0 0 1 1 1 b b b (b = barva borderu)
                  ; tedy PAPER=7, INK=aktuální border, 
                  ; BRIGHT 0, FLASH 0
          LD      ($5800),A ;levý horní roh v paměti atributů
          LD      ($581F),A ;pravý horní roh v paměti atributů
          LD      ($5AE0),A ;levý dolní roh v paměti atributů
          LD      ($5AFF),A ;pravý dolní roh v paměti atributů

          SCF     ;Signalizace úspěšného nalezení
          RET     ;před návratem.</pre>

Efekty s borderem jsou většinou prosté a dostatečně rychlé, takže je sem můžeme napasovat a nestarat se o změnu časování. Většinou se vejdeme do tolerance.

### Jednoduché efekty s načteným obsahem

Během nahrávání stihneme určitě i jednodušší operace s načtenými daty, buď na úrovni bitů, nebo na úrovni bajtů. Loader u [dema Digisynth](https://www.youtube.com/watch?v=2DZNnLInGwo) dokázal v reálném čase provádět rozbalování dat pomocí Huffmanova dekompresního algoritmu (Huffman se k tomu hodí docela dobře, stačí mít uložený dekomprimační strom a podle načteného bitu jím procházet). Pro zájemce jsem připravil [rekonstrukci tohoto loaderu](http://retrocip.uelectronics.info/loaderove-intermezzo/ "Loaderové intermezzo"). My se ale podíváme na jiný případ, a tím bude známý [Mad Load](http://www.worldofspectrum.org/infoseekid.cgi?id=0008381) &#8211; rutina, která nahrává obrázky po čtverečcích v nějakém pořadí. Na videu je jeho vylepšená verze.



Mad Load používal velmi jednoduchý formát dat. Po příznakovém bajtu následovaly data pro jednotlivé čtverce. Každý zabral 11 bajtů &#8211; nižší a vyšší bajt adresy na obrazovce, kde má být čtverec uložen, pak 8 bajtů obrazové paměti a 1 bajt atributu. Poté následoval další čtverec&#8230;

František Fuka nahrávací rutinu mírně upravil &#8211; z LD\_8\_BITS udělal vlastně podprogram, který mu načte 1 bajt do registru L. Tento podprogram využívá v podprogramu pro načtení jednoho čtverce (MAD_SQUARE). Načítání čtverců je voláno stále dokola, dokud magnetofon dává data, a když přestane, vrátí se zpět. Nekontroluje se žádný součet, nic.

Zde je zdroják Mad Loaderu. Okomentoval jsem pouze části, které se od standardního liší.

<pre class="lang:asm decode:true ">LD      HL,MAD_RETURN 
          PUSH    HL 
          JP      MAD_LOAD 
          NOP     
          NOP     
          NOP     
          NOP     
MAD_RETURN:       
          EI      
          RET     

MAD_LOAD:         
          DI      
          IN      A,($FE) 
          RRA     
          AND     $20 
          LD      C,A 
          CP      A 
LD_BREAK:         
          RET     NZ 
LD_START:         
          CALL    LD_EDGE_1 
          JR      NC,LD_BREAK 
          LD      HL,$0415 
LD_WAIT:          
          DJNZ    LD_WAIT 
          DEC     HL 
          LD      A,H 
          OR      L 
          JR      NZ,LD_WAIT 
          CALL    LD_EDGE_2 
          JR      NC,LD_BREAK 

LD_LEADER:        
          LD      B,$9C 
          CALL    LD_EDGE_2 
          JR      NC,LD_BREAK 
          LD      A,$C6 
          CP      B 
          JR      NC,LD_START 
          INC     H 
          JR      NZ,LD_LEADER 

LD_SYNC:  LD      B,$C9 
          CALL    LD_EDGE_1 
          JR      NC,LD_BREAK 
          LD      A,B 
          CP      $D4 
          JR      NC,LD_SYNC 
          CALL    LD_EDGE_1 
          RET     NC 

                  ; Vlastní Mad Load
          CALL    LD_ONE_BYTE ; první je flag byte, a ten zahodíme
MAD_LOOP:         
          CALL    MAD_SQUARE 
          RET     NC 
          JR      MAD_LOOP 

MAD_SQUARE:       
          CALL    LD_ONE_BYTE ; první byte adresy (nižší)
          LD      A,L 
          EX      AF,AF' ; schovám do AF'
          CALL    LD_ONE_BYTE ; druhý byte adresy
          RET     NC 
          EX      AF,AF' 
          LD      H,L ; posunu do H
          LD      L,A ; a do L dám první načtený. V HL mám tedy adresu čtverce v obrazovce
          LD      B,$08 ;tolik bajtů obrazových dat jde do jednoho čtverce
MAD_SCRN:         
          PUSH    HL ;schovám adresu
          PUSH    BC ;a počítadlo
          CALL    LD_ONE_BYTE ; a načítám bajty
          POP     BC ;Obnovím si počítadlo
          LD      A,L ; načtený bajt zkopíruju do A
          POP     HL ; adresa místa, kde má být uložený
          RET     NC ; Pokud nastala chyba...
          LD      (HL),A ;Když ne, tak ukládám bajt
          INC     H ; a zvýším adresu o 256 (tedy o jeden mikrořádek)
          DJNZ    MAD_SCRN ; opakuju, dokud nemám všech 8 obrazových bajtů
          LD      A,H ; teď z adresy bitmapy udělám adresu atributu
          SUB     $08 ; nejprve odečtu těch 8, co se v předchozí smyčce postupně načetlo
          RRA     
          RRA     
          RRA     ; H div 8
          AND     $03 ; nejnižší tři bity
          OR      $58 ;$58, $59 nebo $5a - třetiny v atributové paměti
          LD      H,A ; v HL mám teď adresu odpovídajícího atributu, takže...
          PUSH    HL ; uschovám adresu
          CALL    LD_ONE_BYTE ; přečtu hodnotu atributu
          LD      A,L ; uložím do A
          POP     HL ; adresu atributu vrátím
          LD      (HL),A ; a umístím do paměti
          RET     ; Hotovo, jeden čtvereček načten



LD_ONE_BYTE:      
          LD      B,$B2 
          LD      L,$01 
LD_8_BITS:        
          CALL    LD_EDGE_2 
          RET     NC 
          LD      A,$CB 
          CP      B 
          RL      L 
          LD      B,$B0 
          JR      NC,LD_8_BITS 
          SCF     
          RET     

                  ;-----------------------------------

LD_EDGE_2:        
          CALL    LD_EDGE_1 
FF78:     RET     NC 
LD_EDGE_1:        
          LD      A,$16 
LD_DELAY:         
          DEC     A 
FF7C:     JR      NZ,LD_DELAY 
FF7E:     AND     A 
LD_SAMPLE:        
          INC     B 
          RET     Z 
          LD      A,$7F 
          IN      A,($FE) 
          RRA     
          XOR     C 
          AND     $20 
          JR      Z,LD_SAMPLE 
          LD      A,C 
          INC     A 
          XOR     $20 
          AND     $27 
          LD      C,A 
          NOP     
          NOP     
          AND     $07 
          OR      $08 
          OUT     ($FE),A 
          SCF     
          RET     
</pre>

&#8230; a v tomto bodě bych pro tentokrát skončil.

Příště nás čekají ty složitější efekty, třeba nejrůznější počítadla bajtů, zbývajícího času a další legrácky, pro které je potřeba víc taktů procesoru, a musíme tedy upravovat časovací smyčky a algoritmy rozsekat tak, aby se vešly do času, co máme k dispozici. Zatím přijměte tedy toto stručné &#8222;úvodní nakoukání do tajemství loaderů&#8220;, zkuste si zaexperimentovat sami a pokud nějaký hezký loader vytvoříte, pochlubte se!