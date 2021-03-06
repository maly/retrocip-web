---
id: 514
title: 'ZX Spectrum a loadery &#8211; 2'
date: 2015-02-08T12:58:33+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=514
permalink: /zx-spectrum-a-loadery-2/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3496414331"
categories:
  - Software
  - Stroják
tags:
  - loadery
  - Spectrum
  - Z80
  - ZX
---
Pokračování článku o loaderech na ZX Spectru. Držte si klobouky a vytáhněte kalkulačky, pojedeme z kopce!

<!--more-->

[Minule](https://retrocip.uelectronics.info/zx-spectrum-a-loadery-1/ "ZX Spectrum a loadery – 1") jsem skončil s tím, že příště bude potřeba počítat a vážit každý takt. Naším spojencem nejbližším bude tentokrát čekací smyčka v rutině LD\_EDGE\_1. Připomeňme si ji:

<pre class="lang:default decode:true">LD_EDGE_2:        
          CALL    LD_EDGE_1 ;Zde se v podstatě volá ještě jednou LD_EDGE_1 a
          RET     NC ;návrat pokud došlo k chybě.
LD_EDGE_1:        
          LD      A,$16 ;7T
LD_DELAY:         
          DEC     A ;4T
          JR      NZ,LD_DELAY ;12T / 7T
          AND     A ;4T
 
LD_SAMPLE:       
          ......</pre>

Vidíte ji tam u LD_DELAY? Ano? Tak to je ona! Zabere 7T + 21*(4T+12T) + (4T+7T) = 354T, což je docela dost taktů na to, aby se v nich dalo něco rozumně dělat. Co třeba? No, nejčastěji nějaké počítadlo toho, kolik ještě zbývá nahrát.

## Grafický ukazatel

U textovky Poradce jsem použil, kromě děsivého blikání, taky jakýsi &#8222;teploměr&#8220;, který ukazoval, kolik ještě zbývá nahrát. Je to takové to vlevo&#8230;



Tenhle efekt je jednodušší, než byste možná čekali. Délka sloupečku je téměř přesně &#8222;délka souboru / 512&#8220; (píšu &#8222;téměř přesně&#8220;, protože jsem ho tehdy namaloval o kousek kratší&#8230;) Takže si vezmu vyšší bajt zbývajícího počtu bajtů (ten je ve dvojici registrů DE), vydělím dvěma, přičtu offset odspodu obrazovky, převedu si souřadnici na adresu v obrazovce a daný bajt maskuju s hodnotou %11100111 &#8211; tedy vše zůstává, jen dva body uprostřed, které jsou shodou okolností zrovna ten &#8222;teploměr&#8220;, se umáznou. K přepočtu použiju dokonce rutinu PIXEL-ADD ($22AA) z ROMky Spectra, která vezme souřadnice v B a C (stejný souřadný systém jako má PLOT) a z nich spočítá adresu na obrazovce (HL) a masku bodu (A).

<pre class="lang:default decode:true">LD_EDGE_2:        
          CALL    LD_EDGE_1 
          RET     NC 
LD_EDGE_1:        
          JP      LD_OWN 
LD_BACK:          
          LD      A,03 ;7T
LD_DELAY:         
          DEC     A ;4T
          JR      NZ,LD_DELAY ;12T/7T
                  ;7T + 2*(4+12) + (4+7) = 50T
          AND     A 
LD_SAMPLE:        
          INC     B 
          RET     Z 
          LD      A,7f 
          IN      A,(fe) 
          RRA     
          RET     NC 
          XOR     C 
          AND     20 
          JR      Z,LD_SAMPLE 
          LD      A,C 
          CPL     
          LD      C,A 
          LD      A,06 
          RRCA    
          AND     07 
          OR      08 
          OUT     (fe),A 
          SCF     
          RET     

LD_OWN:           
          PUSH    DE ;11T
          PUSH    HL ;11T
          PUSH    BC ;11T
          PUSH    AF ;11T
          LD      B,D ;4T
          SRL     B ;8T
          NOP     ;4T
          INC     B ;4T
          INC     B ;4T
          LD      C,00 ;7T
          CALL    22aa ;17T + 132T rutina
          LD      A,(HL) ;7T
          AND     e7 ;7T
          LD      (HL),A ;7T
          POP     AF ;10T
          POP     BC ;10T
          POP     HL ;10T
          POP     DE ;10T
          JP      LD_BACK ;10T
</pre>

Jak vidíte, zasáhnul jsem do časovací smyčky tak, že na začátek jsem si odskočil do vlastní rutiny pro obsluhu onoho efektu (i s odskokem to je 305T), a po návratu mám ještě stále dost času, takže si počkám dalších 50T, což dává výsledných 355T, a to je skoro přesně to naše Magické Číslo!

## Číselný ukazatel

Pokud chcete ukazovat místo nějakého ubývajícího sloupečku raději číslo, bude to náročnější. Každá číslice se totiž musí vykreslit na obrazovku, což je osm zápisů na každou číslici, při trojmístném počítadle to je už docela hodně, to se do 354T nevejde. Musíte proto rozsekat algoritmus na části, které se do tohoto časového limitu vejdou, a postupně je volat. Neocenitelným pomocníkem vám bude druhá sada registrů a indexregistr IY.

Velmi pěkná počítadla měly hry od Hewson a české programy od Universum (tuším), používaly vlastní font a měly i efekt přetáčení číslic. Já jsem u další textovky měl jen prosté počítání &#8222;počtu bajtů / 64&#8220;, což se mi pro 48kB blok vejde stále do tří číslic. Na začátku jsem si požadované číslice připravil (desítkově!) do registrů D a E (resp. do těch zrcadlových):

<pre class="lang:default decode:true">PUSH    DE 
          EXX     
          POP     HL 
          LD      DE,00 
          LD      BC,$40
LD_DIV:          
          AND     A 
          SBC     HL,BC 
          JR      C,b0be 
          LD      A,E 
          ADD     A,01 
          DAA     
          LD      E,A 
          LD      A,D 
          ADC     A,00 
          DAA     
          LD      D,A 
          JR      LD_DIV 
LD_DIV2:          
          LD      H,3d 
          LD      BC,4001 
          EXX     
          LD      IY, LD_CHARS ; bude dal...

</pre>

Efekt neprobíhal přímo v LD\_EDGE, ale v LD\_8\_BITS. I když LD\_EDGE bylo upravené tak, aby používalo při načítání bajtů výrazně jiné časovací hodnoty:

<pre class="lang:default decode:true">LD      B,b0 
          LD      L,01 
LD_8_BITS:        
          CALL    LD_EDGE_2M
          RET     NC 
          LD      A,d4 
          CP      B 
          RL      L 
          LD      B,b0 
          JP      NC,LD_LONGWAY 
          LD      A,H 
          XOR     L 
          LD      H,A 
;... zbytek normálně...

;LD_EDGE_2M : 468 + 58 * B
;zkraceno o 462T
LD_EDGE_2M:       
          CALL    LD_EDGE_1M 
          RET     NC 
          LD      A,10 
          DEC     A 
          JR      NZ,b157 ;  254T
LD_EDGE_1M:       
          AND     A 
          INC     B 
          RET     Z 
          LD      A,7f 
          IN      A,(fe) 
          RRA     
          XOR     C 
          AND     20 
          JR      Z,b15b 
          LD      A,C 
          CPL     
          LD      C,A 
          AND     04 
          OR      08 
          OUT     (fe),A 
          SCF     
          RET     


</pre>

Samotný efekt se pak odehrával takto:

<pre class="lang:default decode:true">LD_LONGWAY:       
          EXX     
          DEC     C 
          JP      Z,LD_SUB1 
          JP      (IY) 

LD_RETHERE:       
          POP     IY 
          EXX     
          JP      LD_8_BITS 

LD_SUB1:          
          LD      C,07 
          DEC     B 
          JP      Z,LD_SUB2 
          LD      A,04 
          DEC     A 
          JR      NZ,b1a9 ;wait 59T
          EXX     
          JP      LD_8_BITS 

LD_SUB2:          
          LD      A,E 
          SUB     01 
          DAA     
          LD      E,A 
          LD      A,D 
          SBC     A,B ;v B je 0
          DAA     
          LD      D,A 
          LD      B,$40 
          LD      IY,LD_CHARS 
          LD      HL,$3d00 
          EXX     
          JP      LD_8_BITS 

                  ;1. cislice
LD_CHARS:         
          LD      A,E 
          ADD     A,A 
          ADD     A,A 
          ADD     A,A 
          OR      $81 
          LD      L,A 
          LD      A,(HL) 
          LD      (50fd),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (51fd),A 
          INC     L 
          LD      A,(HL) 
          LD      (52fd),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (53fd),A 
          INC     L 
          LD      A,(HL) 
          LD      (54fd),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (55fd),A 
          INC     L 
          LD      A,(HL) 
          LD      (56fd),A 
          CALL    LD_RETHERE 

                  ;2. cislo
          LD      A,E 
          AND     $f0 
          RRA     
          OR      $81 
          LD      L,A 
          LD      A,(HL) 
          LD      (50fc),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (51fc),A 
          INC     L 
          LD      A,(HL) 
          LD      (52fc),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (53fc),A 
          INC     L 
          LD      A,(HL) 
          LD      (54fc),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (55fc),A 
          INC     L 
          LD      A,(HL) 
          LD      (56fc),A 
          CALL    LD_RETHERE 

                  ;3. cislo
          LD      A,D 
          ADD     A,A 
          ADD     A,A 
          ADD     A,A 
          OR      $81 
          LD      L,A 
          LD      A,(HL) 
          LD      (50fb),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (51fb),A 
          INC     L 
          LD      A,(HL) 
          LD      (52fb),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (53fb),A 
          INC     L 
          LD      A,(HL) 
          LD      (54fb),A 
          CALL    LD_RETHERE 
          INC     L 
          LD      A,(HL) 
          LD      (55fb),A 
          INC     L 
          LD      A,(HL) 
          LD      (56fb),A 
          LD      (56fb),A 
          NOP     
          LD      IY,LD_CHARS 
          EXX     
          JP      LD_8_BITS 
</pre>

&nbsp;

K tomu samozřejmě pár vysvětlivek:

V registrech DE je vlastní počítadlo, uložené v BCD kódování. V registru C je počítadlo bitů, v registru B počítadlo bajtů. Jakmile dopočítají do nuly, proběhne snížení počítadla o 1, úprava výsledku instrukcí DAA a příprava na samotný tisk znaků. Do HL je uložena hodnota $3D00, což je shodou okolností adresa ROM, kde jsou uloženy číslice, a do IY je uložena adresa rutiny LD_CHARS, která číslice vykresluje.

Když rozsekáte rutinu na kousky, máte dvě možnosti. Buď upravujete kód a přepisujete adresu, kam se má příště skočit, nebo si adresu uchováváte nějak jinak. Tady to je v registru IY. Jednoduchým trikem CALL LD\_RETHERE (Jako že RET HERE, vrať se sem) skočíte zpátky do LD\_8\_BITS, a zároveň se návratová adresa dostane do IY, takže při příštím volání bude JP (IY) směřovat na další kousek. Všimněte si, že tisk znaků je právě voláním LD\_RETHERE rozsekán na malé kousky.

## Ukazatel času a další

Když počítáte čas, je to složitější než počítat bajty. Musíte totiž buď počítat každý bit jinak (jedničkový je dvakrát tak delší než nulový), anebo využít přerušení (ano, přerušení v loaderu!)

Hledal jsem ukázku toho prvního přístupu, ale než jsem ji stihnul okomentovat, ozval se mi Busy, že našel zdroják svého loaderu k [Overscanu](https://busy.speccy.cz/tvorba/overscan.htm), a jestli bych ho nechtěl publikovat. Takže s Busyho laskavým svolením: Overscan loader!

(Počítání času i grafické efekty jsou v ceně!)



A tady je zdroják i s Busyho komentáři (Díky!) Protože to je dlouhé, tak se s vámi rozloučím už zde a popřeju hodně zdaru při vlastním experimentování.

<pre class="lang:asm decode:true ">5b00            *a
5b00            *s
5b00            ;==============================================================;
5b00            ;== Verzia 16 == Loader pre Overscan == 12.08.1991 Busy soft ==;
5b00            ;==============================================================;
5b00            znaky  =    #8800	Modifikovany znakovy font
5b00                   org  #8200,0
8200 f3         p      di
8201 310082            ld   sp,p
8204 fd217f00          ld   iy,#7f	Inicializacia casovacov pre cas a auto kitt
8208 210080            ld   hl,#8000	Inicializacia IM2 vektora
820b 110180            ld   de,#8001
820e 011001            ld   bc,#0110
8211 3681              ld   (hl),#81
8213 edb0              ldir
8215 212183            ld   hl,rut	Presun obsluznej rutinky IM2
8218 118181            ld   de,#8181
821b 010800            ld   bc,load-rut
821e edb0              ldir
8220 067f       i      ld   b,#7f
8222 216711            ld   hl,#1167
8225 11d685            ld   de,k
8228 d5                push de
8229 7e         ll14   ld   a,(hl)	Zaplacanie pameti blbostami
822a ad                xor  l		(pre oklamanie nepriatela)
822b 12                ld   (de),a
822c 02                ld   (bc),a
822d a9                xor  c
822e 0b                dec  bc
822f 02                ld   (bc),a
8230 0b                dec  bc
8231 13                inc  de
8232 2b                dec  hl
8233 7c                ld   a,h
8234 b5                or   l
8235 20f2              jr   nz,ll14
8237 e1                pop  hl
8238 012a7a            ld   bc,-k
823b edb0              ldir
823d 3e80       rst    ld   a,#80	Spustenie IM2
823f ed47              ld   i,a
8241 ed5e              im2
8243 fb                ei
8244 cd0c85            call zn		Konverzia znakoveho fontu
8247 3e48              ld   a,#48
8249 32b784            ld   (n20+2),a
824c 210040            ld   hl,#4000	Priprava obrazovky
824f 110140            ld   de,#4001
8252 010018            ld   bc,#1800
8255 71                ld   (hl),c
8256 edb0              ldir
8258 010603            ld   bc,#0306
825b 71                ld   (hl),c
825c edb0              ldir
825e 21a341     o      ld   hl,#41a3	Stvorceky pod kitt efekt
8261 0e06              ld   c,#06
8263 061a       ll11   ld   b,#1a
8265 e5                push hl
8266 367e       ll10   ld   (hl),#7e
8268 2c                inc  l
8269 10fb              djnz ll10
826b e1                pop  hl
826c 24                inc  h
826d 0d                dec  c
826e 20f3              jr   nz,ll11
8270 210059            ld   hl,#5900	Cerveny ramik uprostred
8273 011208            ld   bc,#0812
8276 5d                ld   e,l
8277 73         ll12   ld   (hl),e
8278 2c                inc  l
8279 71                ld   (hl),c
827a 7d                ld   a,l
827b c61d              add  a,#1d
827d 6f                ld   l,a
827e 71                ld   (hl),c
827f 2c                inc  l
8280 73                ld   (hl),e
8281 2c                inc  l
8282 10f3              djnz ll12
8284 21e15a            ld   hl,#5ae1
8287 1e44              ld   e,#44
8289 cd0083            call sap
828c 21e158            ld   hl,#58e1
828f cdfe82            call pas
8292 21015a            ld   hl,#5a01
8295 cdfe82            call pas
8298 215f85            ld   hl,firma	Vypis textov
829b cd0783            call text
829e 217685            ld   hl,demo
82a1 cd0783            call text
82a4 219185            ld   hl,loatim
82a7 cd0783            call text
82aa cd2983            call load		Zavolanie loadera
82ad 08                ex   af,af
82ae af                xor  a
82af cdf884            call out		Border 0
82b2 08                ex   af,af
82b3 3839              jr   c,ok		Ak sa blok nahral dobre tak skok
82b5 af         error  xor  a		Ak nastala chyba pri loade
82b6 210048            ld   hl,#4800	tak vypis oznam
82b9 77         ll13   ld   (hl),a
82ba 23                inc  hl
82bb cb64              bit  4,h
82bd 28fa              jr   z,ll13
82bf 112548            ld   de,#4825
82c2 212515            ld   hl,#1525
82c5 cd0b83            call txt
82c8 21d285            ld   hl,krik
82cb cd0b83            call txt
82ce 21a585            ld   hl,rew
82d1 cd0783            call text
82d4 21b785            ld   hl,reload
82d7 cd0783            call text
82da af                xor  a
82db 32b784            ld   (n20+2),a
82de 06ff       press  ld   b,#ff	Cakanie na stlacenie klavesy
82e0 cdcf83            call mmm		Pocas cakania volame loader
82e3 af                xor  a		aby efekty stale bezali
82e4 dbfe              in   a,(#fe)
82e6 f6e0              or   #e0
82e8 3c                inc  a
82e9 28f3              jr   z,press
82eb c33d82            jp   rst
82ee             
82ee 210058     ok     ld   hl,#5800	Ak sa blok nahral poriadku
82f1 110158            ld   de,#5801	zmazeme vsetky atributy
82f4 0603              ld   b,#03
82f6 edb0              ldir
82f8 cde6c3            call 50150        Dekomprimacia dema
82fb c376e9            jp   59766	Spustenie dema
82fe             
82fe 1e12       pas    ld   e,#12	Nejake pomocne podprogramy
8300 061e       sap    ld   b,#1e	pre kreslenie ramikov
8302 73         pp1    ld   (hl),e
8303 2c                inc  l
8304 10fc              djnz pp1
8306 c9                ret
8307             
8307 5e         text   ld   e,(hl)	Vypis textu
8308 23                inc  hl		HL = adresa textu
8309 56                ld   d,(hl)	DE = pozicia textu na obrazovke
830a 23                inc  hl
830b e5         txt    push hl
830c d5                push de
830d 6e                ld   l,(hl)
830e 2688              ld   h,&gt;znaky
8310 0608              ld   b,#08
8312 7e         xtx    ld   a,(hl)	Vypis jedneho znaku
8313 12                ld   (de),a
8314 14                inc  d
8315 24                inc  h
8316 10fa              djnz xtx
8318 d1                pop  de
8319 e1                pop  hl
831a 1c                inc  e
831b cb7e              bit  7,(hl)	Text konci znakom s nastavenym bit7=1
831d 23                inc  hl
831e 28eb              jr   z,txt
8320 c9                ret
8321             
8321 08         rut    ex   af,af	Rutinka beziaca z prerusenia
8322 fd24              inc  yh		YH = casovac pre kitt efekt
8324 fd2c              inc  yl		YL = casovac pre pocitanie a vypis casu nahravania
8326 08                ex   af,af
8327 fb                ei
8328 c9                ret
8329             
8329 af         load   xor  a		POZOR, tu zacina loader !!
832a cdf884            call out
832d 211f40     rohy   ld   hl,#401f	Vykreslenie useknutych rohov
8330 11ff57            ld   de,#57ff
8333 7b                ld   a,e
8334 77                ld   (hl),a
8335 12                ld   (de),a
8336 d9                exx
8337 010207            ld   bc,#0702
833a 210040            ld   hl,#4000
833d 11e057            ld   de,#57e0
8340 77                ld   (hl),a
8341 12                ld   (de),a
8342 7e         rr1    ld   a,(hl)
8343 24                inc  h
8344 15                dec  d
8345 cb27              sla  a
8347 77                ld   (hl),a
8348 12                ld   (de),a
8349 d9                exx
834a 7e                ld   a,(hl)
834b 24                inc  h
834c 15                dec  d
834d cb3f              srl  a
834f 77                ld   (hl),a
8350 12                ld   (de),a
8351 d9                exx
8352 10ee              djnz rr1
8354            nnn
8354 3ef8       rrr    ld   a,#f8	Chytanie uvodneho tonu
8356 32f484            ld   (xor+1),a
8359 2600              ld   h,#00
835b 06ff       djnz   ld   b,#ff
835d cdcf83            call mmm
8360 25                dec  h
8361 20f8              jr   nz,djnz
8363 cdcf83            call mmm
8366 30ec              jr   nc,nnn
8368 cdcb83            call ppp
836b 30e7              jr   nc,nnn
836d 069c       sss    ld   b,#9c
836f cdcb83            call ppp
8372 30e0              jr   nc,nnn
8374 3ec6              ld   a,#c6
8376 b8                cp   b
8377 30db              jr   nc,rrr
8379 24                inc  h
837a 20f1              jr   nz,sss
837c 3efc              ld   a,#fc
837e 32f484            ld   (xor+1),a
8381 06c9       ttt    ld   b,#c9
8383 cdcf83            call mmm
8386 30cc              jr   nc,nnn
8388 78                ld   a,b
8389 fed4              cp   #d4
838b 30f4              jr   nc,ttt
838d cdcf83            call mmm
8390 d0                ret  nc
8391 79                ld   a,c		Uvodny ton a sync-pulz OK a mozeme loadovat
8392 ee03              xor  #03
8394 4f                ld   c,a
8395 cdb583            call byte         Load flagbajtu
8398 d0                ret  nc
8399             
8399 dd21e6c3   loa    ld   ix,50150	Samotna slucka loadujuca bajty dema
839d 116a24            ld   de,9322
83a0            ;      ld   ix,#4000	Pri testovani efektov v loaderi
83a0            ;      ld   de,#1b00	sa blok nahraval do obrazovky
83a0 cdb583     loa1   call byte
83a3 d0                ret  nc
83a4 dd7500            ld   (ix+#00),l
83a7 dd23              inc  ix
83a9 1b                dec  de
83aa 7a                ld   a,d
83ab b3                or   e
83ac 20f2              jr   nz,loa1
83ae cdb583            call byte          load parity
83b1 d0                ret  nc
83b2 fe01              cp   #01
83b4 c9                ret
83b5             
83b5 06b2       byte   ld   b,#b2	Loadnutie jedneho bajtu
83b7 2e01              ld   l,#01
83b9 cdcb83     loa9   call ppp
83bc d0                ret  nc
83bd 3ecb       kk     ld   a,#cb
83bf b8                cp   b
83c0 cb15              rl   l
83c2 06b0              ld   b,#b0
83c4 30f3              jr   nc,loa9
83c6 7c                ld   a,h
83c7 ad                xor  l
83c8 67                ld   h,a
83c9 37                scf
83ca c9                ret
83cb             
83cb cdcf83     ppp    call mmm		Loadnutie jedneho bitu
83ce d0                ret  nc
83cf d9         mmm    exx		Loadnutie jednej polperiody
83d0 c3                db   #c3		Namiesto cakacej slucky su tu moje speci-rutinky
83d1 ec83       skok   dw   n1		Skok na cast kodu ktora sa prave ma vykonat
83d3             
83d3 7e         n1sub  ld   a,(hl)              /0
83d4 81                add  a,c		Prepocet casoveho udaja
83d5 fe0a              cp   #0a
83d7 0e00              ld   c,#00
83d9 3802              jr   c,n1aa
83db 0c                inc  c
83dc af                xor  a
83dd 77         n1aa   ld   (hl),a
83de 23                inc  hl                  \50
83df 7e                ld   a,(hl)
83e0 81                add  a,c
83e1 fe06              cp   #06
83e3 0e00              ld   c,#00
83e5 3802              jr   c,n1bb
83e7 0c                inc  c
83e8 af                xor  a
83e9 77         n1bb   ld   (hl),a
83ea 23                inc  hl                  \100
83eb c9                ret                      \110+call=117
83ec             
83ec fd7d       n1     ld   a,yl
83ee fe33              cp   51                  \29
83f0 3866              jr   c,n10	Test, ci uz uplynula sekunda
83f2 0e01              ld   c,#01	aby sme mohli vypisat dalsi cas
83f4 fd7d              ld   a,yl
83f6 d632              sub  50
83f8 fd6f              ld   yl,a
83fa 215a85            ld   hl,cas              \76
83fd cdd383            call n1sub
8400 3e04              ld   a,#04
8402 110784            ld   de,n3
8405 181a              jr   jpde                \222 +/-
8407             
8407 23         n3     inc  hl		Kazdu sekundu sa tiez zmeni
8408 cdd383            call n1sub	efektik v strednej tretinke
840b ed5f              ld   a,r
840d 32b984            ld   (udaj+1),a
8410 3a5b85            ld   a,(cas+1)
8413 0f                rrca
8414 9f                sbc  a,a
8415 e60f              and  #0f
8417 f607              or   #07
8419 32cc84            ld   (rot),a
841c 3e05              ld   a,#05
841e 112a84            ld   de,n2
8421 ed53d183   jpde   ld   (skok),de
8425 1ef9              ld   e,#f9
8427 c3e084            jp   wait                \222 +/-
842a             
842a 2b         n2     dec  hl
842b cb7e              bit  7,(hl)              \32
842d 2808              jr   z,n2aa	Priprava na vypis jedneho znaku
842f 3e0e              ld   a,14
8431 01ec83            ld   bc,n1
8434 c3dc84            jp   buduci
8437 e5         n2aa   push hl
8438 6e                ld   l,(hl)
8439 2688              ld   h,&gt;znaky
843b 1650              ld   d,#50
843d 0602              ld   b,#02               /2*88=202
843f 7e         dd1    ld   a,(hl)	Samotne vyprintenie jedneho znaku
8440 12                ld   (de),a
8441 24                inc  h
8442 14                inc  d
8443 7e                ld   a,(hl)
8444 12                ld   (de),a
8445 24                inc  h
8446 14                inc  d
8447 7e                ld   a,(hl)
8448 12                ld   (de),a
8449 24                inc  h
844a 14                inc  d
844b 7e                ld   a,(hl)
844c 12                ld   (de),a
844d 24                inc  h
844e 14                inc  d
844f 10ee              djnz dd1
8451 1c                inc  e
8452 e1                pop  hl
8453 3e01              ld   a,#01               \299
8455 c3e084            jp   wait
8458             
8458 fd7c       n10    ld   a,yh		Auto-Kitt efekt
845a fe02              cp   #02                 \56
845c 3857              jr   c,n20	Test ci uz mame robit dalsiu fazu
845e c3                db   #c3
845f 9584       jump   dw   n12                 \73
8461             
8461 21a358     n11    ld   hl,#58a3
8464 3647              ld   (hl),#47
8466 2c         incdec inc  l
8467 7d                ld   a,l
8468 326284            ld   (n11+1),a
846b 216684            ld   hl,incdec           \124
846e febc              cp   #bc
8470 3f                ccf
8471 3802              jr   c,iidd
8473 fea4              cp   #a4
8475 9f         iidd   sbc  a,a                 \151
8476 e601              and  #01
8478 ae                xor  (hl)
8479 77                ld   (hl),a
847a 218484            ld   hl,jr+1
847d 3e03              ld   a,#03
847f ae                xor  (hl)
8480 77                ld   (hl),a
8481 3ea2              ld   a,#a2               \210
8483 1803       jr     jr   #03
8485 329684            ld   (n12+1),a
8488 fd2600            ld   yh,#00
848b 3e02              ld   a,#02
848d 219584            ld   hl,n12
8490 225f84     buduce ld   (jump),hl
8493 184b              jr   wait                \267
8495             
8495 216458     n12    ld   hl,#5864            \73
8498 7d                ld   a,l
8499 febc              cp   #bc                 \94
849b 3011              jr   nc,n12bb
849d 014104            ld   bc,#0441            \111
84a0 7e         n12aa  ld   a,(hl)              /47
84a1 b9                cp   c
84a2 3801              jr   c,#01
84a4 3d                dec  a
84a5 77                ld   (hl),a
84a6 2c                inc  l
84a7 10f7              djnz n12aa               \4*47=188
84a9 229684            ld   (n12+1),hl
84ac 1835              jr   end
84ae             
84ae 3e08       n12bb  ld   a,#08
84b0 216184            ld   hl,n11
84b3 18db              jr   buduce              /\174
84b5             
84b5 210048     n20    ld   hl,#4800	Zaplacanie strednej tretiny obrazovky
84b8 3e33       udaj   ld   a,#33               \82
84ba 772c772c          dw   #2c77,#2c77	8*[INC C : LD (HL),A]
84be 772c772c          dw   #2c77,#2c77
84c2 772c772c          dw   #2c77,#2c77         8*11=88
84c6 772c772c          dw   #2c77,#2c77         \
84ca 2002              jr   nz,n21              /
84cc 0f         rot    rrca
84cd 24                inc  h
84ce cba4       n21    res  4,h
84d0 cbdc              set  3,h
84d2 22b684            ld   (n20+1),hl
84d5 32b984            ld   (udaj+1),a
84d8 3e03              ld   a,3
84da 1804              jr   wait                \68
84dc             
84dc ed43d183   buduci ld   (skok),bc	Nastavenie ktora cast kodu sa ma volat nabuduce
84e0 3d         wait   dec  a
84e1 20fd              jr   nz,wait
84e3 d9         end    exx		Vsetky casti kodu trvaju 200 cyklov (+/- autobus)
84e4 a7         zzz    and  a		Koniec mojich speci-rutiniek v loaderi
84e5 04         l222   inc  b
84e6 c8                ret  z
84e7 3eff              ld   a,#ff	#FF namiesto #7F nereaguje na medzeru
84e9 dbfe              in   a,(#fe)
84eb 1f                rra
84ec d0                ret  nc
84ed a9                xor  c
84ee e620              and  #20
84f0 28f3              jr   z,l222
84f2 79                ld   a,c
84f3 eefc       xor    xor  #fc		#FC namiesto CPL zabezpeci krajsie farby
84f5 4f                ld   c,a		uvodny ton cerveno-zlty a bajty modro-modre
84f6 e607              and  #07
84f8 320058     out    ld   (#5800),a           +52
84fb 321f58            ld   (#581f),a	Nastavenie rohovych atributov
84fe f608              or   #08
8500 d3fe              out  (#fe),a
8502 e607              and  #07
8504 32e05a            ld   (#5ae0),a
8507 32ff5a            ld   (#5aff),a
850a 37                scf
850b c9                ret
850c             
850c 110088     zn     ld   de,znaky	Konverzia znakoveho fontu
850f d5         ll1    push de		Z romkoveho fontu vytvori kontury
8510 7b                ld   a,e		a tiez font preorganizuje tak aby
8511 fe20              cp   ' '		print rutinky bezali rychlejsie
8513 3002              jr   nc,#02
8515 c630              add  a,'0'
8517 87                add  a,a
8518 6f                ld   l,a
8519 260f              ld   h,#0f
851b 29                add  hl,hl
851c 29                add  hl,hl
851d cd4985            call read
8520 4f                ld   c,a
8521 23                inc  hl
8522 cd5085            call write
8525 0606              ld   b,#06
8527 cd4985     ll2    call read
852a 4f                ld   c,a
852b 2b                dec  hl
852c cd4985            call read
852f b1                or   c
8530 4f                ld   c,a
8531 23                inc  hl
8532 23                inc  hl
8533 cd5085            call write
8536 10ef              djnz ll2
8538 cd4985            call read
853b 4f                ld   c,a
853c 2b                dec  hl
853d cd4985            call read
8540 23                inc  hl
8541 cd5485            call rit
8544 d1                pop  de
8545 1c                inc  e
8546 20c7              jr   nz,ll1
8548 c9                ret
8549             
8549 7e         read   ld   a,(hl)
854a 0f                rrca
854b 0f                rrca
854c b6                or   (hl)
854d 07                rlca
854e b6                or   (hl)
854f c9                ret
8550             
8550 cd4985     write  call read
8553 2b                dec  hl
8554 b1         rit    or   c
8555 ae                xor  (hl)
8556 12                ld   (de),a
8557 23                inc  hl
8558 14                inc  d
8559 c9                ret
855a 00000a00   cas    db   0,0,10,0,0	Buffer pre casovy udaj
            855f 4550       firma  dw   #5045
8561 42757379          db   'Busy software '
            6    
            2    
            856f 70726573          db   'presen','t'+#80
            8576 8350       demo   dw   #5083
8578 3e3e3e20          db   '&gt;&gt;&gt; THE OVER'
            0    
            8584 5343414e          db   'SCAN DEMO &lt;&lt;'
            d    
            8590 bc                db   '&lt;'+#80
8591 e250       loatim dw   #50e2
8593 4c6f6164          db   'Loading'
            859a 20636361          db   ' cca 1 min'
            d    
            85a4 ae                db   '.'+#80
85a5 8848       rew    dw   #4888
85a7 52657769          db   'Rewing the tape'
            4    
            4    
            85b6 ac                db   ','+#80
85b7 c448       reload dw   #48c4
85b9 70726573          db   'press any key '
            e    
            5    
            85c7 616e6420          db   'and reload'
            f    
            85d1 ae                db   '.'+#80
85d2 202121a1   krik   db   ' !!','!'+#80
85d6            k
85d6            l      =    k-p		Navestie "l" bude celkova dlzka kodu
85d6             
85d6                   org  #6000,0	Dalsie rutinky nemaju ziadny ucel
6000 f3         mrs    di		sluzili ako pomoc pri odladovani
6001 ed56              im1
6003 af                xor  a
6004 ed47              ld   i,a
6006 c3b3f4            jp   #f4b3	Skok do MRS
6009             
6009 cd0c85     zs     call zn		Test konverzie znakoveho fontu
600c 210088            ld   hl,znaky	Zobrazenie vsetkych znakov
600f 110040            ld   de,#4000	na obrazovke
6012 010008            ld   bc,#0800
6015 edb0              ldir
6017 c9                ret
6018             
6018 3e0c       poke   ld   a,12		Nastavi testovaci IM2 vektor do romky
601a d317              out  (23),a	(Outy su pre zapis do romky na MB01)
601c 218181            ld   hl,#8181
601f 22ff3a            ld   (#3aff),hl
6022 3e04              ld   a,4
6024 d317              out  (23),a
6026 c9                ret
6027                   end
</pre>

&nbsp;