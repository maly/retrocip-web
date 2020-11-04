---
id: 1080
title: 'Alpha: Sériové rozhraní'
date: 2018-05-10T15:53:13+01:00
author: Martin Maly
layout: revision
guid: https://retrocip.cz/1071-autosave-v1/
permalink: /1071-autosave-v1/
---
Gratuluji ke zprovoznění počítače. Jen je trošku hloupé, že počítač vlastně umí dělat jen to, co mu vypálíte do EEPROM, a nemůžete s ním nijak komunikovat. To teď napravíme. Přidáme první vstupně &#8211; výstupní obvod, totiž sériové rozhraní.

Použijeme obvod 6850, přesněji verzi 68B50 (verze B umí pracovat i na frekvenci 2 MHz). Jedná se o obvod,označovaný ACIA &#8211; Asynchronous Communications Interface Adapter. Pod slovy &#8222;asynchronní komunikační rozhraní&#8220;je trochu cudně skryto, že jde o standardní sériové rozhraní, jaké známe z &#8222;COM portů&#8220; a dalších sériových portů na počítači. My k jeho výstupům připojíme nějaký převodník USB-to-Serial, a tím získáme možnost komunikovat s Alphou přes terminálový program z počítače.

> Při testech se ukázalo, že i verze MC6850, tedy bez &#8222;B&#8220;, zvládne provoz touto rychlostí, ale může se to lišit kus odkusu&#8230;

## ACIA 6850<a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/05/MC6850-pinout.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1072" src="http://retrocip.cz/wp-content/uploads/sites/6/2018/05/MC6850-pinout-609x650.png" alt="" width="609" height="650" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/MC6850-pinout-609x650.png 609w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/MC6850-pinout-768x819.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/MC6850-pinout-960x1024.png 960w" sizes="(max-width: 609px) 100vw, 609px" /></a>

Obvod 6850 je sériový komunikační obvod. Jeho hlavním úkolem je převést zaslaný bajt na sériový signál (tj. správně odvysílat start bit, datové bity, případně paritní bit, a nakonec stop bit) a opačně, tj. načíst správně časovaný sériový signál a připravit ho k předání procesoru.

Asynchronní v popisu znamená, že spolu s daty není přenášen hodinový signál ani není činnost přesně časována –když přijde bajt, je vyslán, když přijdou vstupní sériová data, jsou načtena. Synchronizace, tj. to, že bude načteno opravdu to, co načteno být má, zajišťují právě start a stop bity – podle jejich správného průběhu pozná obvod, že jsou data v pořádku.

Vysílání dat po výstupu TXDATA (Tx = transmit) a příjem na vstupu RXDATA (Rx = receive) je časován pomocí systémových hodin. U našeho počítače je systémový kmitočet roven 1,8432 MHz. Tento kmitočet je v ACIA vnitřně dělen (dělitel je programově nastavitelný na hodnoty 1, 16 a 64). Použijeme dělení 16, což znamená, že komunikační rychlost bude 1843200 / 16 = 115200 bitů za sekundu (baud).

Obvod 6850 s procesorem komunikuje pomocí osmibitové datové sběrnice (D0-D7), několika CS vstupů (CS = ChipSelect), které určují, kdy se s obvodem komunikuje (CS0 = 1, CS1 = 1, /CS2 = 0 – ve všech ostatních případech je datová sběrnice odpojena), vstupu E (Enable, obvod komunikuje jen pokud je E=1), vstupu R/W, který udává, zda se z obvodu čte (1) nebo se do něj zapisuje (0) a vstupu RS, který udává, jestli se čtou/zapisují data (1), nebo řídicí hodnoty(0).

Kromě těchto signálů nabízí i signál /IRQ (Interrupt Request &#8211; požadavek na přerušení). Pomocí přerušení může ACIA dát vědět procesoru, že například přišla nějaká data po sériovém portu, nebo že data k odeslání byla odeslána apod. My přerušení nepoužijeme.

Z hlediska programátora se tedy obvod 6850 jeví jako dva registry (vybrané pomocí vstupu RS), z nichž jeden je datový, druhý „systémový“. Funkci osvětlí tabulka:

|    |     | VSTUPY       | REGISTRY                                     |
| -- | --- | ------------ | -------------------------------------------- |
| RS | R/W | TYP REGISTRU | FUNKCE                                       |
|    |     | Zápis        | Řídicí registr (Control Register, CR)        |
|    | 1   | Čtení        | Stavový registr (Status Register, SR)        |
| 1  |     | Zápis        | Data k vyslání (Transmit Data Register, TDR) |
| 1  | 1   | Čtení        | Přijatá data (Receive Data Register, RDR)    |

Všimněte si, že do řídicího registru je možné pouze zapisovat, nelze z něj číst, naopak stavový registr lze pouze číst,nelze do něj zapisovat. Není to totiž potřeba. Totéž s datovými registry – při čtení se čte to, co obvod přijal (a nezajímá nás to, co jsme odeslali). Při zápisu je jasné, že chceme data vyslat, nedávalo by smysl zapisovat do přijatých dat.

### Řídicí registr CR

Osmibitový registr CR řídí čtyři funkce obvodu:

  * Bity 0 a 1 nastavují dělicí poměr hodin, viz výše.  
    | CR1 | CR0 | DĚLITEL |
    | --- | --- | ------- |
    |     |     | 1       |
    |     | 1   | 16      |
    | 1   |     | 64      |
    | 1   | 1   | RESET   |
    
    Poslední kombinace nenastavuje dělitele, ale celý obvod resetuje do výchozího nastavení, tj. vyprázdní registry anuluje příznaky. My použijeme kombinaci 01, tj. dělení 16. Kdyby bylo potřeba rychlost snížit, použijeme kombinaci 10, tj. dělení 64, a obvod bude komunikovat rychlostí 28800 Bd.</li> 
    
      * Bity 2, 3 a 4 nastavují délku vysílaných a přijímaných dat (sedmibitové nebo osmibitové), zda se pracuje s paritou a kolik je stop bitů. Tyto informace najdete např. i v nastavení Hyperterminálu, když půjdete hledat detaily připojení. Pro naše účely použijeme kombinaci 101, tj. osmibitový přenos, bez parity, 1 stop bit.
      * Bity 5 a 6 určují, jestli vysílač po odvysílaném bajtu bude žádat o přerušení a v jakém stavu bude výstup RTS
      * Bit 7 určuje, jestli přijímač bude vyvolávat přerušení v případě chyby.</ul> 
    
    Pro bližší popis odkazuju zájemce opět k datasheetu, my použijeme hodnotu 15h (0 00 101 01 binárně), tj. osmibitový přenos, bez parity, přerušení zakázané a přenosová rychlost 115200 Baud (Baud je jednotka „bitů za sekundu“ –včetně start a stop bitů).
    
    ### Stavový registr SR
    
    Svět není dokonalý, život není fér a asynchronní sériové přenosy nejsou nijak sladěné s biorytmy našeho procesoru.Pokud pošlu bajt do 6850, začne ho obvod vysílat. Ovšem předtím je dobré podívat se, jestli už dokončil vysílání toho předchozího. Při příjmu je zase dobré se podívat, jestli nějaký bajt už načetl, a pokud ho načetl, tak ho zpracovat, aby se uvolnilo místo pro další bajt. Popřípadě získat informaci o tom, jestli nedošlo k chybě. Tyhle informace jako když najdete ve stavovém registru SR.
    
    Pokud načtete bajt ze SR, tak vás jednotlivé bity informují o následujícím:
    
      * Bit 0 – Receiver Data Register Full (RDRF). Pokud je nastaven na 1, znamená to, že obvod načetl bajt po sériové lince do přijímače a bylo by záhodno ho zpracovat. Jakmile procesor přečte stav datového registru, je bit RDRF nastaven na 0. Nula znamená, že žádný nový bajt nepřišel.
      * Bit 1 – Transmitter Data Register Empty (TDRE). Jakmile zapíšete do datového registru bajt, nastaví se TDRE na 0 a obvod začne bajt vysílat po sériové lince. Jakmile ho vyšle, nastaví tento bit na 1. Programátor by si měl před tím, než nějaký bajt pošle, zkontrolovat, že může – tedy že bit TDRE = 1.
      * Bity 2, 3 pracují s řídicími signály CTS, DCD, a já je tady s klidným svědomím opomenu.
      * Bit 4 – Framing Error (FE) znamená, že přijatá data byla špatně časována, např. že nepřišel požadovaný STOP bit.
      * Bit 5 – Receiver Overrun (OVRN). Overrun, neboli hezky česky _přeběh_, je stav, kdy přijímač přijal bajt, ale procesor ještě nezpracoval předchozí přijatý. Ten nově přijatý je tedy zahozený (protože jej není kam dát, žádný vnitřní buffer není) a nastaví se OVRN na 1, aby bylo jasné, že došlo k chybě.
      * Bit 6 – Parity Error (PE). Pokud využíváme přenos s paritou, zkontroluje 6850 paritní bit. Pokud byl chybný, nastaví příznak PE.
      * Bit 7 – Interrupt Request (IRQ) říká, že obvod požádal o přerušení z nějakého závažného důvodu. Buď byl přijat bajt a je povolené přerušení při přijetí dat, nebo byl odeslán bajt a je povoleno přerušení při odeslání, nebo vypadla nosná (DCD).
    
    ## 6850 a ALPHA
    
    Jak připojit tento obvod k naší konstrukci? Tak, je jasné, že datová sběrnice přijde na datovou sběrnici procesoru. Co se vstupy CS, R/W a RS?
    
    Vstup E povoluje obvodu komunikovat s procesorem (v logické 1). My budeme chtít tento obvod připojit jako periferii,tedy vstupně-výstupní obvod. Vývod IO/M shodou okolností říká právě to, jestli procesor chce pracovat s pamětí (0),nebo s periferií (1). Můžeme ho tedy přímo připojit na vstup E, tím se zajistí, že obvod ACIA bude reagovat jen na požadavky pro zápis a čtení do / z periferií a bude ignorovat operace s pamětí.
    
    R/W určuje, jestli se bude zapisovat (0) nebo číst (1). V našem systému má podobnou funkci signál /WR &#8211; je 0, pokud procesor hodlá zapisovat, jinak je 1, takže může sloužit jako signál čtení (i když není /RD aktivní). Je to v pořádku, stačí to?
    
    > Podívejme se na to: Pokud bude procesor chtít zapisovat, pošle správný signál, ve všech ostatních případech bude číst. Nevadí to něčemu? Nevadí, protože bude odpojený od sběrnice díky vstupům CS a E.
    > 
    > Jenže moment: Podívejte se pořádně na průběhy signálů &#8211; signál IO/M je nastaven dřív, než proběhne cyklus ALE,a /WR je přitom neaktivní (=1). Takže ACIA začne číst a posílat hodnoty po datové sběrnici, takže by teoreticky mohl ovlivnit latchování adresy. Teď oceníme oddělovač sběrnice ze začátku této kapitoly.
    > 
    > Odpověď tedy zní: Nestačí to! Měli bychom signál E držet neaktivní, pokud ALE=1, protože obvod nemá oddělené povolovací vstupy pro zápis a čtení (jako mají třeba paměti). Popřípadě použít /CS2 pro ALE. Ale díky oddělovači to není třeba řešit.
    
    Pracovat se s 6850 bude tedy pomocí instrukcí IN a OUT. Ty používají osmibitovou adresu, takže se teď ještě musíme postarat nějak o to, aby obvod správně z adresy poznal, že procesor komunikuje s ním. Navrhuju, pokud proti tomu nic nemáte, použít vstupy CS0, CS1 a /CS2 a připojit je na adresní vodiče A7, A6 a A5 takto:
    
    | A7  | A6  | A5   | A4      | A3      | A2      | A1      | A0 |
    | --- | --- | ---- | ------- | ------- | ------- | ------- | -- |
    | CS0 | CS1 | /CS2 | &#8211; | &#8211; | &#8211; | &#8211; | RS |
    
    Obvod bude vybraný pouze tehdy, pokud budou nejvyšší tři bity adresy rovny 110. Na stavu ostatních bitů nebude záležet. Jakákoli adresa, která splní masku &#8222;110x xxxx&#8220; vyhoví. Vyhovují tedy adresy C0h &#8211; DFh.
    
    Na nejnižší bit A0 jsem připojil vstup RS. Ten vybírá mezi datovým (1) a řídicím / stavovým registrem (0). Adresy, které budou mít v nejnižším bitu 1, budou přistupovat k datovému registru (C1h, C3h, C5h, &#8230; DDh, DFh). Ostatní adresy budou přistupovat k řídicímu či stavovému registru (C0h, C2h, C4h, &#8230; DCh, DEh). Programátor si může vybrat kteroukoli z nich, jsou ekvivalentní.
    
    Přesto doporučuju použít ty adresy, které mají v bitech A1-A4 logické 1. Do budoucna to ušetří případné mrzení s připojováním dalších periferií.
    
    Proto si zapište- **ACIA v počítači Alpha je zapojena takto:**
    
    | Adresa | Zápis           | Čtení           |
    | ------ | --------------- | --------------- |
    | 0DEh   | Řídicí registr  | Stavový registr |
    | 0DFh   | Data k odeslání | Přijatá data    |
    
    Musíme také přivést hodinový signál na vstupy RXCLK a TXCLK. Oba spojíme, protože chceme vysílat stejnou frekvencí jako přijímat. Připojíme je na vývod CLK z procesoru.Signál /RTS budeme ignorovat, vstupy /CTS a /DCD, sloužící k řízení přenosu, připojíme na zem.
    
    Vývody RXDATA (vstup) a TXDATA (výstup) jsou už to sériové rozhraní&#8230; připojte je k převodníku USB-to-UART, samosebou kříženě (TxD na vstup RXDATA, RxD na výstup TXDATA). Počítač je připraven komunikovat, stačí ho jen naprogramovat!
    
    <a href="http://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-acia.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1073" src="http://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-acia-650x393.png" alt="" width="650" height="393" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-acia-650x393.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-acia-768x465.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-acia-1024x619.png 1024w" sizes="(max-width: 650px) 100vw, 650px" /></a>
    
    ## Jak pracovat s 6850?
    
    Na začátku musíme nastavit řídicí registr tak, jak potřebujeme. Už jsme si řekli, že to bude hodnota 15h. Tím je obvod nastaven a připraven k přijímání a vysílání dat. Rozšíříme tedy naší inicializační sekci:
    
                    .ORG    0 
        
                            ; inicializace()
        
        COLD:       DI      
        
                    LXI     SP,0000h 
        
                            ; adresace sériového rozhraní
        ACIA        EQU     0DEh 
        ACIAC       EQU     ACIA 
        ACIAS       EQU     ACIA 
        ACIAD       EQU     ACIA+1 
        
                            ;inicializace ACIA
                    MVI     A,15h ; 115200 Bd, 8 bit, no parity, 1 stop bit, no IRQ
                    OUT     ACIAC
    
    Na začátku jsou pojmenované adresy ACIA (bázová adresa obvodu 6850 v našem počítači), ACIAC(ontrol), ACIAS(tatus) a ACIAD(ata). Vyhneme se tak programátorskému moru, „magickým konstantám“.
    
    Do registru A uložíme požadované řídicí slovo (15h) a instrukcí OUT ho zapíšete na adresu ACIAControl (tj. 0DEh).Vnější logika našeho počítače se postará o to, že data neskončí v paměti, ale tam, kde mají, tj. v obvodu 6850.
    
    Co dál? Obvod je nastaven, teď je zapotřebí vypsat ono obligátní HELLO WORLD. Někde v paměti tedy bude tenhle řetězec a my ho budeme bajt po bajtu procházet a vysílat na sériový výstup.
    
    Když se řekne „vysílat“, tak si na to uděláme podprogram. Bude se jmenovat třeba SEROUT (jako že SERial OUTput) ajeho funkce bude, že vyšle hodnotu v registru A. Předtím si ale zkontroluje, jestli je vysílač volný. Musí si tedy načíst hodnotu stavového registru SR a zkontrolovat bit 1 (TDRE). Pokud je nulový, musí počkat, až bude 1.
    
        ACIA_TDRE   EQU     02h
        SEROUT:             
                    PUSH    PSW 
        SO_WAIT:            
                    IN      ACIAS 
                    ANI     ACIA_TDRE ;bit TDRE - pokud lze vysílat, je =1
                    JZ      SO_WAIT 
                    POP     PSW 
                    OUT     ACIAD 
                    RET
    
    Na začátku si uložím obsah registru A. Mám v něm ten bajt, co chci vyslat, ale budu ten registr potřebovat, protože si do něj načtu hodnotu stavového registru. Takže si jeho hodnotu uložím na zásobník.
    
    Na dalším řádku načtu hodnotu stavového registru do registru A. Pak provedu logický součin (AND) s hodnotou 2.Hodnota 2 totiž binárně vypadá takto: 00000010 – jsou to tedy samé nuly, jen na pozici bitu 1, který potřebuju testovat, je jednička. Výsledkem logického součinu bude buď hodnota 2, pokud je bit 1 nastaven, nebo 0, pokud je nulový.
    
    Připomeňme si: pokud je bit 1 stavového registru nulový, znamená to, že obvod 6850 ještě vysílá předchozí data a my musíme počkat, dokud to nedokončí. Tedy pokud je (hodnota stavového registru AND 02) rovna nule, čekáme. A přesně to zajišťuje další instrukce JZ. Pokud je příznak Z=1 (tedy předchozí operace skončila s výsledkem 0), tak JZ skáče. Tady se skáče opět na načtení stavového bajtu a vše se opakuje, dokud není výsledek nenulový. V tu chvíli už víme, že má 6850 volno a můžeme vysílat.
    
    Pokud je tedy volno, přečteme si ze zásobníku zpět hodnotu, co byla původně v registru A a pomocí OUT ji zapíšeme do datového registru 6850 – ACIAData.
    
    Správná otázka je: Co se stane, když náhodou bude obvod 6850 vadný, nebo nebude zapojený správně a bude vracet pořád hodnotu 0? V takovém případě, ano, tušíte správně, jste právě vygenerovali nekonečnou smyčku, ve které se bude procesor točit do skonání věků – pardon, do vypnutí napájení, do RESETu nebo do přerušení.
    
    ## Výpis řetězce
    
    Už zbývá jen detail &#8211; dát to všechno dohromady. Máme inicializovaný obvod ACIA, máme někde podprogram, který umí poslat znak na sériové rozhraní, teď už jen stačí vytvořit smyčku, která bude posílat jednotlivé znaky nejznámějšího nápisu v historii programování.
    
    Buď můžeme zvolit postup mechanický, tedy postupně volat SEROUT s hodnotami pro znaky H, E, L, L, O&#8230; v registru A, nebo si ty znaky můžeme někam nějak zapsat a ve smyčce je postupně načítat.
    
    Nejjednodušší způsob je uložit si nápis někam do paměti jako posloupnost znaků. K tomu slouží direktiva DB. Na začátku programu si do registrů HL uložíte adresu prvního znaku nápisu. Pak stačí jen opakovat následující postup:
    
      1. Přečíst hodnotu z adresy, na kterou ukazuje HL, do registru A: MOV A, M
      2. Zavolat SEROUT: CALL SEROUT
      3. Zvýšit hodnotu HL o 1: INX H
      4. Opakovat od bodu 1: JMP 1
    
    Ale pozor &#8211; takto zapsáno to je nekonečná smyčka, která bude posílat stále dokola kompletní obsah paměti. Potřebujeme říct, kdy má přestat.
    
    Používají se v zásadě tři postupy. První je ten, že za poslední znak dáme ještě znak 0. Někdy se tomu říká také ASCIIZ (ASCII s ukončovací nulou &#8211; Zero), někdy se tomu říká CSTR (protože takový způsob ukládání řetězců volí jazyk C).Pokud v bodu 1 načteme hodnotu 0, tak je vypsaný celý řetězec a můžeme skončit. Nevýhoda: Součástí řetězce nesmí být samotný kód 0.
    
    Druhý způsob je ten, že jako první hodnotu řetězce dáme jeden byte, který bude obsahovat délku řetězce v bajtech.Tomuto způsobu se také někdy říká PSTR, protože takto ukládá řetězce Pascal. Algoritmus je o něco složitější:
    
      1. Přečíst hodnotu z adresy HL do registru C: MOV C, M
      2. Zvýšit hodnotu HL o 1: INX H
      3. Přečíst hodnotu z adresy, na kterou ukazuje HL, do registru A: MOV A, M
      4. Zavolat SEROUT: CALL SEROUT
      5. Zvýšit hodnotu HL o 1: INX H
      6. Snížit hodnotu C (Counter) o 1: DCR C
      7. Pokud je hodnota nenulová, skočit na bod 3: JNZ 3
    
    Ve skutečnosti zkušený programátor vynechá bod 5 a v bodě 7 skáče na bod 2 &#8211; tím ušetří jeden bajt. Je jedno, jestli se hodnota HL zvýší před skokem, nebo po skoku, efekt je stejný. Nevýhoda: řetězec může mít maximálně 256 znaků.
    
    Třetí způsob využívá toho, že se většinou u takových počítačů používá jen prostá znaková sada ASCII, která má definované znaky jen v rozsahu 00 &#8211; 7Fh. Tedy žádné speciální, semigrafické, přehlásky, &#8230; Pokud je tomu tak, tak se použije způsob, podobný tomu prvnímu, ale s tím rozdílem, že místo toho, abychom za poslední znak přidali 0, tak my zapíšeme poslední znak tak, že mu nastavíme nejvyšší bit na 1 (tedy ho posuneme do rozmezí 80h-FFh). Výhoda je, že řetězec nezabírá ani o jediný byte navíc. Nevýhoda je, kromě toho, že nelze použít speciální znaky, třeba i složitější obsluha, ale zas tak hrozné to není. Představme si podprogram STROUT, který tento postup implementuje:
    
        STROUT:
                MOV A,M
                ANI 7Fh
                CALL SEROUT
                MOV A,M
                ANI 80h
                RNZ
                INX H
                JMP STROUT
    
    Načtený bajt nejprve ošetříme a případný nastavený bit 7 znulujeme. Pak výsledek pošleme na výstup.
    
    Znovu si načteme stejný bajt, a tentokrát se podíváme na nejvyšší bit. Pokud je nenulový, máme vše za sebou a vracíme se pryč (RNZ). Jinak standardní postup: Přejít na další adresu a celé opakovat znovu!
    
    Všechny tři postupy mají svá pro a proti a své výhody a omezení. Který způsob použijete, to záleží na vás. V dobách osmibitů se hojně používal poslední (s negovaným bitem 7), ale pokud počítáte s tím, že například budete zobrazovat i jiné znaky než standardní sedmibitové ASCII, musíte zvolit jiný postup. Koncová nula je široce používaná dnes, právě díky tomu, že takto implementuje řetězce standardní Céčko. Takový řetězec může mít libovolnou délku, ale nesmí sám obsahovat nulu. Navíc to každý řetězec prodlouží o jeden byte.
    
    ## Hotový program
    
                    .ORG    0 
        
                            ; inicializace()
        
        COLD:       DI      
        
                    LXI     SP,0000h 
        
                            ; adresace sériového rozhraní
        ACIA        EQU     0DEh 
        ACIAC       EQU     ACIA 
        ACIAS       EQU     ACIA 
        ACIAD       EQU     ACIA+1 
        
        ACIA_TDRE   EQU     02h 
        
                            ;inicializace ACIA
                    MVI     A,15h ; 115200 Bd, 8 bit, no parity, 1 stop bit, no IRQ
                    OUT     ACIAC 
        
        WARM:               
                    LXI     H,HELLO 
                    CALL    STROUT 
        
                    JMP     WARM 
        
        SEROUT:             
                    PUSH    PSW 
        SO_WAIT:            
                    IN      ACIAS 
                    ANI     ACIA_TDRE ;bit TDRE - pokud lze vysílat, je =1
                    JZ      SO_WAIT 
                    POP     PSW 
                    OUT     ACIAD 
                    RET     
        
        STROUT:             
                    MOV     A,M 
                    ANI     7Fh 
                    CALL    SEROUT 
                    MOV     A,M 
                    ANI     80h 
                    RNZ     
                    INX     H 
                    JMP     STROUT 
        
        HELLO:              
                    DB      "HELLO WORLD",0Dh,0Ah+80h
    
    Na začátku proběhne inicializace &#8211; studený (cold) start. Nastaví se zásobník (bez něj by nefungovaly podprogramy) aobvod ACIA. Po inicializaci startuje vlastní obslužný program (teplý start). Tedy program: uloží do HL adresu řetězce,zavolá funkci STROUT a jede se znovu.
    
    Zvolil jsem nakonec ukládání řetězců &#8222;tradičně osmibitově&#8220;. Na posledním řádku vidíte znaky k výpisu. Znak 0Dh je řídicí znak pro terminál, konkrétně CR &#8211; návrat kurzoru na začátek řádku. 0Ah je přechod na nová řádek. A protože znak přechodu na nový řádek je poslední, musí být jeho hodnota zvýšena o 80h (což je ekvivalent nastavení bitu 7)
    
    > Assembler ASM80 s těmito triky počítá, tak nabízí pseudoinstrukce .pstr, .cstr a .istr &#8211; příklad:
    > 
    >         .PSTR "HELLO WORLD"
    >         .CSTR "HELLO WORLD"
    >         .ISTR "HELLO WORLD"
    > 
    > Všechny tři uloží do paměti řetězec HELLO WORLD, ale první ho uloží v &#8222;Pascal style&#8220;, tedy první byte bude délka a za ním znaky. Druhý ho uloží v &#8222;C style&#8220; s nulou na konci. Třetí ho uloží v &#8222;inverted style&#8220; &#8211; tedy poslední znak s nastaveným bitem 7.