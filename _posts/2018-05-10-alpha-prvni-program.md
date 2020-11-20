---
id: 1069
title: 'Alpha: První program'
date: 2018-05-10T08:31:52+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1069
permalink: /alpha-prvni-program/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "6662597759"
image: https://retrocip.cz/wp-content/uploads/sites/6/2018/05/31403931_10155719197497496_8190272174116831232_n-960x198.jpg
categories:
  - ASM80.com
  - Hardware
  - Software
tags:
  - "8085"
  - ASM80
  - Assembler
  - OMEN Alpha
---
Nejjednodušší program bude obligátní &#8222;blikání LEDkou&#8220;. Po pravdě řečeno &#8211; zatím nemáme moc jiných způsobů, jak sepřesvědčit, že systém funguje, kromě zmíněného připojení rezistorů k datové sběrnici, simulace instrukce NOP a sledování změn na adresové sběrnici. Ale máme LED na výstupu SOD!

Zapojte tedy obě paměti k procesoru a zkusíme si naprogramovat první program. Bude to nějak takto:

    inicializace()
    while(true) {
      SOD = 1
      delay()
      SOD = 0
      delay()
    }

A máme to, rachota skončila, můžeme pokračovat&#8230;

Vlastně ne, teď to teprve začne. Musíme tohle všechno naprogramovat v assembleru. Začneme inicializací.

     .org 0
    
    ; inicializace()
    
    RESET: DI
    
      LXI SP, 0000h

Nejprve řekneme assembleru, že program bude začínat na adrese 0. Bývá dobrým zvykem si tento bod nějak pojmenovat &#8211; třeba návěštím RESET.

První instrukce je DI. Ta zakáže přerušení. Náš systém sice přerušení nepoužívá, ale patří k dobrému zvyku na začátku práce, než jsou nastavené všechny potřebné věci, zakázat přerušení. Kdyby nějaké přišlo dřív, než je nastavený například ukazatel zásobníku, systém by zhavaroval.

Další instrukce nastaví ukazatel zásobníku na konec paměti RAM. Ta u Alphy zabírá paměťový prostor 8000h &#8211; FFFFh. Takže nastavíme SP na hodnotu &#8222;o jedna vyšší než poslední volná adresa&#8220; &#8211; tedy FFFFh+1 = 0000H. Instrukce LXI SP poslouží výborně.

Po této inicializaci máme procesor připravený na práci. Nemusíme inicializovat nic víc &#8211; ostatně v našem systému nic víc není. Můžeme tedy napsat tu smyčku:

    LOOP: 
      MVI A, 11000000b
      SIM
      CALL DELAY
      MVI A, 01000000b
      SIM
      CALL DELAY
      JMP LOOP

Nejprve nastavujeme SOD na 1. K tomu slouží instrukce SIM. Připomeňme si: tato instrukce vezme hodnotu v registru A a podle ní nastaví SOD a masku přerušení (ta nás teď nezajímá). Pro nastavení musí být druhý nejvyšší bit (D6) roven 1. Pokud tomu tak je, procesor pošle hodnotu nejvyššího bitu (D7) na výstup SOD.

Následuje volání čekací smyčky. Tu zatím neřešíme, ta bude &#8222;někde, nějak, později&#8220;. Prostě jen skočíme na adresu,kde bude čekací rutina, a ta se pomocí instrukce RET zase vrátí zpátky.

Následuje analogicky nastavení SOD na hodnotu 0, opět volání čekací rutiny, a po ní skok zase na začátek celého procesu.

Tím je jádro programu hotové. Jak na tu čekací smyčku?

K čekání se mohou použít různé způsoby. Například instrukce NOP &#8211; zabere 4 takty, což u našeho systému představuje zhruba 2,17 mikrosekund. Pokud jich použijeme tisíc, bude to 2,17 milisekund, pokud sto tisíc, bude to 217 milisekund, tedy skoro čtvrt sekundy. To by šlo použít.

Teda, šlo, kdybychom mohli do paměti uložit sto tisíc instrukcí. Nemůžeme. 32768 je maximum. Co s tím?

Opět použijeme smyčku. Prázdnou, uvnitř se nebude dít nic. Jen budeme odečítat od zadané hodnoty k nule, a až dojdeme k té nule, smyčka skončí. Nějak takto:

    delay() {
      unsigned int d = KONSTANTA
      do {
        d--;
      } while (d!=0)
    }

Použijeme dvojici registrů D, E. Do ní si uložíme časovací konstantu, a pak budeme jen ve smyčce snižovat hodnotu DE o 1 a kontrolovat, jestli je výsledek 0. Pokud ano, tak skončíme, pokud ne, točíme se znovu.

    DELAY:              
                LXI     D, KONSTANTA 
    DLOOP:      DCX     D 
                MOV     A,D
                ORA     E
                JNZ     DLOOP
                RET

Smyčka DLOOP začíná snížením hodnoty v dvojici registrů D a E &#8211; DCX D. Teď musíme zkontrolovat, jestli je výsledek 0. Bohužel (šestnáctibitová) instrukce DCX nenastaví příznak zero (to dělají jen instrukce osmibitové). Použije se opěttrik, totiž logický součet (OR) hodnot v registrech D a E. Výsledek bude 0, pokud D i E budou nulové. A protože nemáme takto obecnou instrukci, musíme si nejprve hodnotu z jednoho registru zkopírovat do akumulátoru a pakudělat OR s druhým registrem. Instrukce ORA podle výsledku nastaví příznak Z a my tedy můžeme podle toho skákat &#8211; instrukcí JNZ, což je vlastně JUMP if NOT ZERO.

A teď otázka za sto bodů: Jak dlouho ta smyčka bude probíhat? Jasně, záleží to na té konstantě a bude to přímo úměrné. Ale kolik to bude? Dalo by se to spočítat?

Inu, dalo. Vezměte si k ruce [tabulku s instrukcemi a jejich trváním](https://ee.sharif.edu/~sakhtar3/articles/8080-8085/8085%20Instruction%20Set.pdf) a napište si, kolik která instrukce zabere taktů:

    DELAY:              
                LXI     D, KONSTANTA ; 10 T
    DLOOP:      DCX     D            ;  6 T
                MOV     A,D          ;  4 T
                ORA     E            ;  4 T
                JNZ     DLOOP        ; 10 T / 7 T - 10 pokud se skáče, 7 pokud se neskáče
                RET                  ; 10 T

Pokud bude konstanta = 1, proběhne tento cyklus přesně jednou. Zabere to tedy 10 + {6 + 4 + 4 + 7} + 10 = 41 T

Pokud bude konstanta = 2, proběhne tento cyklus takto:

10 + {6 + 4 + 4 + 10} + {6 + 4 + 4 + 7} + 10 = 65 T

Pokud bude konstanta = 3, poběží následujícím způsobem:

10 + {6 + 4 + 4 + 10} + {6 + 4 + 4 + 10} + {6 + 4 + 4 + 7} + 10 = 89 T

Je tedy vidět, že základní čekací doba je 41 T, a pokud zvýšíme konstantu o 1, zvýší to čekání o 24 T. Vzorec tedy je:

T = 41 + (N &#8211; 1) x 24

a z toho: N = (T &#8211; 41) / 24 + 1

Pokud bude konstanta rovna 100, bude čekání 41 + 99 x 24 = 2417 T.

Je to logické. První a poslední instrukce (LXI, RET) proběhnou vždy jen jednou (20T). Vnitřní smyčka jsou čtyři instrukce, a doba jejich provádění se liší podle toho, jestli už se dosáhlo nuly. Pokud ne, tak instrukce JNZ skáče a trvá 10T. Pokud ano, instrukce JNZ neskáče a trvá jen 7T. Průchod smyčkou tedy trvá 24T (6 + 4 + 4 + 10), pokud jde o poslední průchod, tak pouze 21T.

> Vzorec můžeme samozřejmě zapsat jako T = 24 * N + 17

Co se stane, když bude konstanta rovna 0? No, hned první DCX D odečte jedničku od 0000h, operace přeteče a výsledek bude FFFFh. Smyčka se tedy vykoná nikoli nula krát, ale 65536x!

Kolik vlastně taktů potřebujeme takto &#8222;propálit&#8220;, aby bylo zpoždění třeba půl sekundy? Víme, že procesor pracuje s taktem 1,8432 MHz, to znamená, že za sekundu vykoná 1 843 200 taktů. Na půlsekundové zpoždění tedy potřebujeme polovinu, ergo 921600 taktů. Zanedbáme takty, které zabere volání podprogramu instrukcí CALL, časování není zde úplně kritické.

Magická konstanta N bude tedy:

N = (921600 &#8211; 41) / 24 + 1 = 38399 (zaokrouhleně)

A co kdybychom chtěli blikat ještě pomaleji, třeba se sekundovými čekacími smyčkami? No, je to prosté, použijeme dvojnásobnou konstan&#8230; aha! Dvojnásobná hodnota je 76798, a to se nám do registrů nevejde. Tak zavoláme půlsekundové čekání dvakrát!

Anebooo &#8211; co když do smyčky mezi DCX D a MOV naházíme několik NOPů? Třeba pět:

    DELAY:              
                LXI     D, KONSTANTA ; 10 T
    DLOOP:      DCX     D            ;  6 T
                NOP                  ;  4 T
                NOP                  ;  4 T
                NOP                  ;  4 T
                NOP                  ;  4 T
                NOP                  ;  4 T
                MOV     A,D          ;  4 T
                ORA     E            ;  4 T
                JNZ     DLOOP        ; 10 T / 7 T - 10, pokud se skáče, 7 pokud se neskáče
                RET                  ; 10 T

Pět NOPů, každý po 4T, to máme 20 T. O tuto hodnotu se zvýší čas nutný k průchodu smyčkou. Vzorec se tedy změní na:

T = 61 + (N &#8211; 1) x 44

Půlsekundové zpoždění s touto rutinou bude vyžadovat konstantu 20945, sekundové pak 41890. Platíme za to tím, že program zabere v paměti víc místa.

Všimli jste si drobné nevýhody celé rutiny? Pracuje s registry D, E a A. Co když ale v A jsou nějaké informace, které chceme zachovat?

Lze samozřejmě použít PUSH PSW a POP PSW, tedy uložit registr A a opět ho obnovit. A asi i rovnou uložit DE. Ale jsme v assembleru: neděláme nic, co není nutné!

Můžeme rozložit dekrement dvojice registrů D a E na dvě vnořené smyčky. V té vnitřní se bude dekrementovat hodnota v registru E, v té vnější hodnota v registru D. Díky tomu, že instrukce dekrementace osmibitového registru správně nastavuje příznaky, můžeme rovnou testovat, zda je výsledek operace 0:

    DELAY2:             
                LXI     D, KONSTANTA2 ; 10 T
    DLOOP2:     DCR     E             ;  4 T
                JNZ     DLOOP2        ; 10 T / 7 T
                DCR     D             ;  4 T
                JNZ     DLOOP2        ; 10 T / 7 T
                RET                   ; 10 T

Vzorec se trošku zesložití &#8211; zachována zůstane konstanta 20 T na první a poslední instrukci. Nejvnitřnější smyčka zabere 14 T / 11 T (pro E nulové): 14 x (E &#8211; 1) + 11

Vnější smyčka zabere (počet taktů vnitřní smyčky) + 14 T / 11 T pro D = 0. Nesmíme ale zapomenout na to, že hodnota E se uvažuje pouze při prvním průběhu, při dalších už je E = 256 (ve skutečnosti 0, ale ta se chová jako 256).Pokud je E=256, zabere vnitřní smyčka 3581 T.

Tedy:

(14 x (E &#8211; 1) + 11) + 11 (to je první průběh smyčkou E + hodnota pro poslední průběh smyčkou D) plus

(D &#8211; 1) x (14 + 3581) (průběh plnou smyčkou E + vlastní smyčka D)

T = 20 + 14 x (E &#8211; 1) + 11 + 11 + (D &#8211; 1) x (14 + 3581) = 42 + 14 x (E &#8211; 1) + 3595 x (D &#8211; 1)

Zásadní rozdíl proti předchozí verzi je, že registry D a E jsou dekrementovány nezávisle, takže pokud nastavíte hodnotu DE = 0001h, bude se to ve skutečnosti chovat, jako by v registru D byla hodnota 256. Nejnižší možná hodnota, při níž proběhne rutina lineárně bez jakýchkoli skoků je tedy 0101h. Pak zabere rovných 42 T. Pokud bude hodnota 0000h, bude to jako D=256 a E=256, tedy provedení bude trvat 920337 T. Což je o něco méně než půl sekundy&#8230;

## Překlad a spuštění

Program máme napsaný, teď nastal čas ho přeložit. Spusťte si ASM80 (<https://www.asm80.com>) a kliknutím na New file založte nový soubor. Do něj napište výše uvedené příkazy. Uložte jej pod názvem test.a80 &#8211; přípona .a80 je důležitá, podle ní překladač pozná, že jde o program pro procesor 8080/8085). Klikněte na Compile.

V seznamu souborů vlevo se objeví dva nové soubory: test.a80.lst a test.a80.hex. První je výpis programu včetně operačních kódů a umístění v paměti, takzvaný listing:

    0000                          .ORG   0   
    0000                          ; inicializace()
    0000   F3           RESET:    DI   
    0001   31 FF FF               LXI   SP,0000h   
    0004                LOOP:     
    0004   3E C0                  MVI   A,11000000b   
    0006   30                     SIM   
    0007   CD 13 00               CALL   DELAY   
    000A   3E 40                  MVI   A,01000000b   
    000C   30                     SIM   
    000D   CD 13 00               CALL   DELAY   
    0010   C3 04 00               JMP   LOOP   
    0013                WAIT:     EQU   38399   
    0013                DELAY:    
    0013   11 FF 95               LXI   D,WAIT   
    0016   1B           DLOOP:    DCX   D   
    0017   7A                     MOV   A,D   
    0018   B3                     ORA   E   
    0019   C2 16 00               JNZ   DLOOP   
    001C   C9                     RET

Druhý (HEX) je kód, určený k naprogramování paměti. Stáhněte si jej (pravý klik na název, Download) a použijte k nahrání do paměti EEPROM. Konkrétní postup se liší podle zvoleného programátoru.

Paměť zapojte zpátky do systému a zapněte napájení. Pokud jste neudělali při zapojení nikde chybu, měla by dioda po chvilce blikat s frekvencí 1 Hz. Úvodní prodleva je dána velikostí RC článku na vstupu RESET.

Funguje? Gratuluji, právě jste si postavili vlastníma rukama osmibitový počítač a naprogramovali ho!

> Námět na cvičení: Tato úloha by šla přepsat tak, aby nevyžadovala paměť RAM. Konstrukci tak můžete oživit jen s třemi IO: procesor, adresový latch a paměť ROM. Stačí místo volání podprogramu (CALL) zkopírovat čekací rutinu na dané místo. Dvakrát.