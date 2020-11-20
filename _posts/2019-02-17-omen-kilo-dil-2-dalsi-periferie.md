---
id: 1141
title: 'OMEN Kilo, díl 2 &#8211; další periferie'
date: 2019-02-17T12:06:31+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1141
permalink: /omen-kilo-dil-2-dalsi-periferie/
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2018/10/20181028_103724-1280-1140x198.jpg
categories:
  - Hardware
tags:
  - "6309"
  - "6809"
  - "6821"
  - MC6809
  - Motorola
  - OMEN
  - OMEN Kilo
  - PIA
---
_Další ukázka z připravované knihy, pokračování_ [_předchozího článku_](https://retrocip.cz/omen-kilo-zapojeni-procesoru/)_._

<!--more-->

Na zapojení procesoru není nic, co bychom už neviděli. Vstup DMA zůstane nepoužitý, je proto připojen na napájecí napětí, výstupy Q a BS jsou ponechány nezapojené.<figure class="wp-block-image">

<img loading="lazy" width="2834" height="1514" src="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-decoder.png" alt="" class="wp-image-1142" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-decoder.png 2834w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-decoder-650x347.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-decoder-768x410.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-decoder-1024x547.png 1024w" sizes="(max-width: 2834px) 100vw, 2834px" /> </figure> 

Dekodér pamětí a signálů /RD, /WR funguje naprosto stejně jako u počítače Bravo. Opět stačí jeden obvod 7400. Jen místo výstupu PHI2 je použit výstup E se stejnou funkcí.<figure class="wp-block-image">

<img loading="lazy" width="1957" height="3600" src="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-memory.png" alt="" class="wp-image-1143" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-memory.png 1957w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-memory-353x650.png 353w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-memory-768x1413.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-memory-557x1024.png 557w" sizes="(max-width: 1957px) 100vw, 1957px" /> </figure> 

Paměti jsou vyřešené doslova stejně jako u předchozích počítačů. Na místě EEPROM jsem použil obvod 28C64 s&nbsp;kapacitou 8 kB, ale schéma je připravené i pro 28C256 – pak je potřeba přepínačem BANK zvolit vybranou polovinu paměti. <figure class="wp-block-image">

<img loading="lazy" width="1528" height="1049" src="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-io.png" alt="" class="wp-image-1144" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-io.png 1528w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-io-650x446.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-io-768x527.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-io-1024x703.png 1024w" sizes="(max-width: 1528px) 100vw, 1528px" /> </figure> 

Dekodér pro periferie je zapojen tak, aby využíval adresy, kde je A15 = 1 a A14 = A13 = 0, tedy 8000h až 9FFFh. Pro každou periferii je vyhrazen prostor 1 kB, a to takto:

<table class="wp-block-table">
  <tr>
    <td>
      <strong>Periferie</strong>
    </td>
    
    <td>
      <strong>Adresa</strong>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>IO0</strong>
    </td>
    
    <td>
      8000h – 83FFh
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>IO1</strong>
    </td>
    
    <td>
      9000h – 93FFh
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>IO2</strong>
    </td>
    
    <td>
      8800h – 8BFFh
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>IO3</strong>
    </td>
    
    <td>
      9800h – 9BFFh
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>IO4</strong>
    </td>
    
    <td>
      8400h – 87FFh
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>IO5</strong>
    </td>
    
    <td>
      9400h – 97FFh
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>IO6</strong>
    </td>
    
    <td>
      8C00h – 8FFFh
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>IO7</strong>
    </td>
    
    <td>
      9C00h – 9FFFh
    </td>
  </tr>
</table>

Periferii IO0 věnujeme, podobně jako u Brava, sériovému portu:<figure class="wp-block-image">

<img loading="lazy" width="1700" height="1993" src="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-serial.png" alt="" class="wp-image-1145" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-serial.png 1700w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-serial-554x650.png 554w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-serial-768x900.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/kilo-serial-873x1024.png 873w" sizes="(max-width: 1700px) 100vw, 1700px" /> </figure> 

Vstup RS, výběr registru, připojíme opět k A0, takže se sériový port objeví jako dvojice registrů na adresách 8000h – 83FFh. Na sudých to bude řídicí registr, na lichých datový. Opět doporučuju použít takové adresy, které mají v nedekódovaných bitech 1, tedy využít adresy 83FEh (řídicí) a 83FFh (datový). 

Poslední část je připojení na sběrnici. Nevyvádím kompletní systémové sběrnice, pouze datovou a z adresní jen tři nejnižší bity. Kromě těchto signálů na sběrnici vyvádím i signály /WR, /RD, E, RESET, vstupy /WAIT (jiný název pro MRDY), /BUSRQ (jiný název pro HALT), /IRQ a /FIRQ.

## <a>Paralelní port PIA (6821)</a>

Použili jsme už obvod PPI 8255 (u OMEN Alpha) i VIA 6522 (u Brava). Obvod PIA 6821 (v rodině 65xx má analogický obvod 6521 – liší se pouze označením některých vývodů, např. místo E je PHI2) je jednodušší verze obvodu VIA 6822 / 6522. Obsahuje dva osmibitové paralelní porty s&nbsp;možností nastavit každý bit každého portu nezávisle jako vstup nebo výstup.<figure class="wp-block-image">

<img loading="lazy" width="2163" height="5000" src="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/MC6821-pinout.png" alt="" class="wp-image-1146" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2019/02/MC6821-pinout.png 2163w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/MC6821-pinout-281x650.png 281w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/MC6821-pinout-768x1775.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2019/02/MC6821-pinout-443x1024.png 443w" sizes="(max-width: 2163px) 100vw, 2163px" /> </figure> 

Opět vidíme ze strany procesoru standardní rozhraní: datovou sběrnici se signály D0 – D7, dva vstupy pro výběr registrů (RS0 a RS1 – připojte je k&nbsp;adresní sběrnici, třeba na A0 a A1), signály R/W a E, které odpovídají signálům u procesoru 6809, vstup /RESET, tři vybavovací vstupy CS0, CS1 a /CS2 (obvod reaguje, pokud CS0 = CS1 = 1 a /CS2 = 0) a dva přerušovací výstupy /IRQA a /IRQB. &nbsp;Tyto výstupy jsou v&nbsp;provedení „s otevřeným kolektorem“, tak je můžete bez problémů spojit dohromady a připojit oba ke vstupu /IRQ, /FIRQ nebo /NMI (nezapomeňte na pull up rezistor).

Ze strany portů jsou k dispozici port A (PA0 – PA7), port B (PB0 – PB7) a dva řídicí signály pro každý port (CA1, CA2, CB1, CB2).

Řídicí signály CA1, CB1 slouží jako zdroj požadavků na přerušení. Lze nastavit, jestli reagují na vzestupnou, nebo sestupnou hranu signálu, případně lze přerušení od těchto vstupů zamaskovat.

Řídicí signály CA2, CB2 mohou být naprogramovány buď jako zdroj přerušení, stejně jako v&nbsp;předchozím případě, nebo jako datový výstup. Výstup můžete řídit buď přímo, nebo můžete určit, že se výstup chová jako signalizace „data na portu připravena“. Signály Cx2 mají zapojený interní pull-up rezistor.

Porty A a B nejsou identické. Port A je vybavený pull-up rezistory a při čtení vrací vždy hodnotu, která je na vstupním pinu.

Port B má namísto toho klasický třístavový výstup, kompatibilní s&nbsp;úrovněmi TTL, a může budit vyšší zátěž. Při čtení se chová různě podle toho, jestli je nastaven jako vstup, nebo jako výstup. Pokud je nastaven jako vstup, čte se hodnota z&nbsp;pinu. Když je nastaven na výstup, při čtení z&nbsp;tohoto portu získáte data, která jste si uložili do registru.

Obvod PIA má šest interních registrů, ale přímo adresovatelné jsou jen čtyři (máme dva adresní vstupy RS0 a RS1). Jedná se o řídicí registr (CR), datový registr (PORT) a registr směru toku dat (DDR), všechny ve variantách pro port A a port B: CRA, CRB, PORTA, PORTB, DDRA, DDRB.

Jak návrháři vyřešili problém přístupu k&nbsp;šesti registrům, když jsou adresovatelné jen čtyři? Je to prosté: Na adresách 1 a 3 jsou řídicí registry CRA a CRB, na adresách 0 a 2 jsou datové a směrové registry (PORT/DDR). Bit 2 příslušného řídicího registru (CRA2, CRB2) udává, jestli na adrese 0, resp. 2, je k&nbsp;dispozici datový registr (PORTA, PORTB – když bit 2 = 1), nebo registr řízení směru (DDRA, DDRB – bit 2 = 0).

<table class="wp-block-table">
  <tr>
    <td>
      <strong>RS1</strong>
    </td>
    
    <td>
      <strong>RS0</strong>
    </td>
    
    <td>
      <strong>CRA2</strong>
    </td>
    
    <td>
      <strong>CRB2</strong>
    </td>
    
    <td>
      <strong>Registr</strong>
    </td>
  </tr>
  
  <tr>
    <td>
      <strong></strong>
    </td>
    
    <td>
      <strong></strong>
    </td>
    
    <td>
      1
    </td>
    
    <td>
      X
    </td>
    
    <td>
      PORTA
    </td>
  </tr>
  
  <tr>
    <td>
      <strong></strong>
    </td>
    
    <td>
      <strong></strong>
    </td>
    
    <td>
      0
    </td>
    
    <td>
      X
    </td>
    
    <td>
      DDRA
    </td>
  </tr>
  
  <tr>
    <td>
      <strong></strong>
    </td>
    
    <td>
      <strong>1</strong>
    </td>
    
    <td>
      X
    </td>
    
    <td>
      X
    </td>
    
    <td>
      CRA
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>1</strong>
    </td>
    
    <td>
      <strong></strong>
    </td>
    
    <td>
      X
    </td>
    
    <td>
      1
    </td>
    
    <td>
      PORTB
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>1</strong>
    </td>
    
    <td>
      <strong></strong>
    </td>
    
    <td>
      X
    </td>
    
    <td>
      0
    </td>
    
    <td>
      DDRB
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>1</strong>
    </td>
    
    <td>
      <strong>1</strong>
    </td>
    
    <td>
      X
    </td>
    
    <td>
      X
    </td>
    
    <td>
      CRB
    </td>
  </tr>
</table>

Registry PORT nastavují hodnoty, které jsou poslané na výstupní piny. Při čtení z&nbsp;těchto registrů získáte buď hodnoty na pinech (port A, port B v&nbsp;režimu vstup), nebo hodnoty předtím uložené do registru PORT (port B v&nbsp;režimu výstup).

Každý jednotlivý pin lze nastavit individuálně jako vstupní, nebo jako výstupní, a to zápisem hodnoty 0 nebo 1 do příslušného bitu registru DDR. Hodnota 0 znamená, že se příslušný pin chová jako vstup, hodnota 1 znamená výstup.

Řídicí registr CRx ovládá, kromě už zmíněného přepínání mezi datovým a směrovým registrem, hlavně chování vývodů Cx1, Cx2. Zároveň udržuje dva příznaky pro přerušení.

<table class="wp-block-table">
  <tr>
    <td>
      Bit 7
    </td>
    
    <td>
      6
    </td>
    
    <td>
      5
    </td>
    
    <td>
      4
    </td>
    
    <td>
      3
    </td>
    
    <td>
      2
    </td>
    
    <td>
      1
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      IRQx1F
    </td>
    
    <td>
      IRQx2F
    </td>
    
    <td>
      Cx2DIR
    </td>
    
    <td>
      Cx2CT1
    </td>
    
    <td>
      Cx2CT0
    </td>
    
    <td>
      DDR
    </td>
    
    <td>
      Cx1TR
    </td>
    
    <td>
      CX1IE
    </td>
  </tr>
</table>

**Bit 0** udává, jestli je povolené přerušení vstupem Cx1. Pokud je 0, je přerušení zakázané.

**Bit 1** říká, jestli je přerušení vyvolané sestupnou (0), nebo vzestupnou (1) hranou na vstupu Cx1.

**Bit 2** přepíná, jak už bylo zmíněno, přístup k&nbsp;registru DDR (0) nebo k&nbsp;PORT (1).

**Bit 5** nastavuje chování vývodu Cx2. Pokud je 0, je Cx2 vstup, pokud je 1, je Cx2 výstup.

**Bit 6** oznamuje, že došlo k&nbsp;přerušení od vstupu Cx2. Je automaticky vynulovaný při čtení datového registru, nebo RESETem. Pokud je Cx2 nakonfigurovaný jako výstup, je tento bit 0.

**Bit 7** oznamuje, že došlo k&nbsp;přerušení od vstupu Cx1. Vynulovaný je při čtení datového registru a při RESETu.

Vynechal jsem bity 3 a 4. Tyto bity ovládají vývody Cx2 a jejich role se liší podle toho, jestli je tento vývod nakonfigurovaný jako vstup (bit 5 = 0), nebo jako výstup (bit 5 = 1). 

Pokud je bit 5 = 0 (Cx2 je vstup), fungují analogicky jako bity 0 a 1. Tedy bit 3 zakazuje (0) nebo povoluje (1) přerušení od vstupu Cx2, bit 4 aktivuje přerušení od sestupné (0) nebo vzestupné (1) hrany.

Pokud je bit 5 = 1, tak můžeme buď přímo ovládat úroveň na tomto výstupu (pokud je bit 4 = 1, tak na výstup Cx2 je zapsána hodnota z&nbsp;bitu 3), nebo nastavit Cx2 jako strobovací signál (bit 4 = 0).

Strobovací mód je asi nejsložitější, navíc se liší u portu A a portu B.

U portu A funguje jako příznak „procesor přečetl data“. Jakmile přečte data z&nbsp;registru PORTA, nastaví se při sestupné hraně vstupu E pin CA2 do log. 0. Zpátky do log. 1 je nastaven podle toho, co je v&nbsp;bitu 3 řídicího registru CRA. Pokud 0, čeká se na aktivní přechod na vstupu CA1 (který přechod je aktivní určuje bit 1 řídicího registru). Pokud je tam 1, čeká se na první sestupnou hranu signálu E.

U portu B funguje jako příznak „data připravena ke čtení“. Jakmile procesor zapíše data do&nbsp;registru PORTB, nastaví se při následující vzestupné hraně vstupu E pin CB2 do log. 0. Zpátky do log. 1 je nastaven podle toho, co je v&nbsp;bitu 3 řídicího registru CRB. Pokud 0, čeká se na aktivní přechod na vstupu CB1. Pokud je tam 1, čeká se na první vzestupnou hranu signálu E.

Já jsem si udělal malý modul, který obsahuje pouze tento obvod a pinové lišty. Pomocí switche přepínám mezi adresami IO1 – IO7. Tento obvod lze docela dobře připojit ke všem dosud probíraným konstrukcím. Doporučuju variantu 68B21, která zvládá vyšší rychlosti (verze 6821 do 1 MHz, 68A21 do 1,5 MHz, 68B21 do 2 MHz).

Díky tomuto modulu tak získávám velmi užitečné rozhraní, na které mohu pohlížet i jako na 16 konfigurovatelných vývodů. Díky nim mohu připojovat moderní periferie s&nbsp;rozhraním SPI (4 vývody) a I<sup>2</sup>C (3 vývody)&#8230;

## <a>Moderní periferie (SPI)</a>

Několik jsem jich zmínil v&nbsp;Hradlech, voltech. Spousta dalších je ale použitelná i s „novými starými osmibity“. Jako příklad mě napadá třeba SD karta nebo některé displeje s&nbsp;rozhraním SPI.

Staré osmibity bohužel nemají přímo rozhraní SPI, to vzniklo až mnohem později. Dokonce nejsou ani široce dostupné obvody, které by člověk mohl k&nbsp;těmto procesorům připojit a ony by fungovaly jako řadiče sběrnice SPI. 

Existuje řešení, implementované v&nbsp;obvodech CPLD (Xilinx XC9532 apod.), popřípadě přímo obvodově, pomocí posuvných registrů a čítačů. Výhodou takového řešení je vysoká přenosová rychlost, klidně i vyšší, než je pracovní rychlost procesoru. 

Jednodušší řešení nechává většinu námahy na procesoru a programátorovi. Nejčastěji se použije paralelní port, buď PPI 8255, nebo PIA/VIA, popsané v&nbsp;minulé kapitole, a jeho piny se využijí na vstup a výstup dat. U PIA/VIA je to o něco snazší, u 8255 musíme myslet na to, že jako vstup či výstup může být nastavena vždy celá brána A, B, případně polovina brány C.

Potřebné průběhy signálů pak musíte generovat programem, a to včetně hodinových pulsů SCK. Není to nijak složité, jen to zabere nějaký čas a paměťový prostor.

Zkusím hrubý odhad: na vyslání jednoho bitu bude u procesoru 6809 potřeba cca 16 – 20 taktů hodin. Je potřeba nejprve připravit datový bit a hodinový puls do registru, jeho obsah poslat do PIA, negovat bit s&nbsp;hodinami, opět poslat obsah do PIA a rotovat data. Vyslání jednoho bajtu tak bude trvat 128 – 160 taktů. Počítejme 150. Teoreticky tedy stihneme za jednu sekundu poslat 12 kilobajtů. Čtení bude ještě o něco pomalejší.

Pokud by rozhraní SPI běželo na kmitočtu, rovném hodinovému, stihli byste za sekundu poslat 225 kilobajtů. Je to málo? Je to moc? Pokud budete přes SPI připojovat třeba externí paměť, tak asi není problém počkat pár sekund na načtení celých 32 kB do paměti RAM. Pokud ale zvažujete připojit přes SPI například displej, tak na nějaké akční divoké hrátky s&nbsp;obrazem spíš zapomeňte – nebo sáhněte po obvodovém řešení.