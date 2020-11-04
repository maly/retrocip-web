---
id: 286
title: Jak fungují nedokumentované instrukce 6502?
date: 2014-05-12T08:09:00+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/283-autosave-v1/
permalink: /283-autosave-v1/
---
Snad každý procesor osmibitové éry v sobě ukrýval nějaké překvapení, o kterém se v manuálu nepsalo. Nejčastěji to byly instrukce s kódy, které byly v oficiálním seznamu instrukcí označeny cudným &#8222;- &#8211; -&#8222;.

<!--more-->

Procesor 8085 měl několik šikovných instrukcí, o nichž se nemluví, stejně jako Z80 &#8211; tam se před člověkem otevřel velmi široký svět, ve kterém šlo pracovat s polovinami indexových registrů, posouvat obsah apod.

Naprostým přeborníkem v nedokumentovaných instrukcích je ale procesor 6502. Zatímco výše zmíněné procesory působí dojmem, že &#8222;nedokumentované&#8220; někdo opravdu navrhl, zabudoval, ale pak se rozhodli, že o nich nikomu neřeknou, tak u 6502 působí nedokumentované (&#8222;ilegální&#8220;) operační kódy jako směs instrukcí s naprosto nahodilými efekty, od použitelných přes obskurní až k naprosto ujetým.

Použitelná by mohla být (s trochou fantazie) třeba instrukce ANC #imm, která provede operaci AND mezi obsahem registru A a přímým operandem a nastaví hodnotu příznaku C podle nejvyššího bitu výsledku.  Trošku divočejší instrukce jsou třeba LAX (která funguje jako LDA a LDX najednou, tj. vloží operand do registrů A a X) nebo XAA (přenese obsah registru X do registru A a pak provede logický součin [AND] registru A s operandem). Naprosté obskurity jsou instrukce jako TAS nebo SAY.

Kupříkladu taková instrukce TAS. Ta má tvar TAS $nnnn, Y &#8211; tedy operační kód a dva bajty adresy. Jako příklad dejme TAS $1234, Y:

  1. Instrukce udělá logický součin (AND) obsahu registrů A a X (oba registry ponechá nezměněné) a uloží výsledek do ukazatele zásobníku S.
  2. S výsledkem udělá logický součin s hodnotou $13 (tj. vyšší byte adresy, zvýšený o 1) a výsledek uloží do paměti na adresu $1234

Vzbuzuje to několik otázek. Například: Jak na to, proboha, někdo mohl přijít? Nebo: K čemu to je? Nebo: Proč?

Na tu poslední si odpovíme právě v tomto článku. Budu vycházet z famózního [How MOS 6502 Illegal Opcodes really work](http://www.pagetable.com/?p=39) a přidám drobné úpravy.

## Jak 6502 dekóduje instrukce?

Na rozdíl od procesorů s mikrokódem, ve kterých je každá instrukce přeložena interně do posloupnosti jednoduchých operací, které jsou provedeny jakýmsi &#8222;vnitřním procesorem&#8220;, používá 6502 kombinační logiku, jejímž srdcem je PLA &#8211; dekódovací paměť. PLA je organizována jako 130 řádků (údajů), každý po 21 bitech. Každý řádek udává operaci (či operace), co se pro daný operační kód (či sadu operačních kódů) provedou v určitém taktu.

Každý údaj lze rozdělit na tři části: ON bity, OFF bity a časování. Zjednodušeně si je můžeme představit takto:

<table border="1" width="500px">
  <tr>
    <td colspan="8" align="center">
      <b>ON bity</b>
    </td>
  </tr>
  
  <tr>
    <td>
      <i>7</i>
    </td>
    
    <td>
      <i>6</i>
    </td>
    
    <td>
      <i>5</i>
    </td>
    
    <td>
      <i>4</i>
    </td>
    
    <td>
      <i>3</i>
    </td>
    
    <td>
      <i>2</i>
    </td>
    
    <td>
      <i>1</i>
    </td>
    
    <td>
      <i></i>
    </td>
  </tr>
  
  <tr>
    <td colspan="8" align="center">
      <b>OFF bity</b>
    </td>
  </tr>
  
  <tr>
    <td>
      <i>7</i>
    </td>
    
    <td>
      <i>6</i>
    </td>
    
    <td>
      <i>5</i>
    </td>
    
    <td>
      <i>4</i>
    </td>
    
    <td>
      <i>3</i>
    </td>
    
    <td>
      <i>2</i>
    </td>
    
    <td>
      <i>1</i>
    </td>
    
    <td>
      <i></i>
    </td>
  </tr>
  
  <tr>
    <td colspan="6" align="center">
      <strong>časování</strong>
    </td>
  </tr>
  
  <tr>
    <td>
      <i>T6</i>
    </td>
    
    <td>
      <i>T5</i>
    </td>
    
    <td>
      <i>T4</i>
    </td>
    
    <td>
      <i>T3</i>
    </td>
    
    <td>
      <i>T2</i>
    </td>
    
    <td>
      <i>T1</i>
    </td>
    
    <td>
      &nbsp;
    </td>
    
    <td>
      &nbsp;
    </td>
  </tr>
</table>

Pozornému čtenáři neuniklo, že nesedí počet bitů (22) s deklarovaným. To je proto, že ve skutečnosti vypadá řádek o něco složitěji &#8211; ale k tomu se hned dostanu.

Části ON a OFF určují, jak má vypadat instrukční kód. Pokud má mít nastaven bit 4, bude nastaven bit 4 v části ON. Pokud má mít bity 3 a 2 nulové, budou nastaveny bity 3 a 2 v části OFF. Na hodnotě bitů, které nejsou nastaveny ani v ON, ani v OFF, nezáleží a může být jakákoli. Poslední, co se kontroluje, je takt. V prvním taktu instrukce (po jejím vyzvednutí) je procesor v taktu T1 a s každým dalším se zvětšuje o 1, tj. posouvá doleva v poli &#8222;časování&#8220;.

Pokud všechny bity, na kterých záleží, odpovídají dané hodnotě, a pokud sedí i číslo taktu, je daný řádek &#8222;platný&#8220; a předává se kombinační logice, která podle toho vykoná nějaké jednoduché úkony. Pro jednu instrukci může být platných řádků i víc najednou.

## Skládání instrukcí

Ve skutečnosti je v PLA uloženo pouze šest ON bitů a šest OFF bitů pro bity 2-7. Hodnoty bitů 0 a 1 se kódují trochu jinak, pomocí trojice signálů G1, G2 a G3. G1 platí, pokud je nastavený bit 0, G2 pro nastavený bit 1 a G3 pro oba bity nulové. Vidíte, že logika není úplná, že chybí ošetření stavu pro oba bity nastavené. A je to opravdu tak, při pohledu do [tabulky instrukcí](http://www.oxyron.de/html/opcodes02.html) vidíme, že instrukce, které mají nastavené oba nejnižší bity (tj. končí na x3, x7, xB a xF) jsou &#8222;ilegální&#8220;. Ve skutečnosti takové instrukce spustí řádky s G1 i s G2 a fungují tak jako kombinace dvou předchozích instrukcí.

(Když se ponoříte do [výpisu PLA](https://code.google.com/p/breaks/source/browse/trunk/Docs/6502/PLA.txt?spec=svn2&r=2), zjistíte, že jsou bity v naprosto odlišném pořadí, proto opakuju: výše zmíněné je pouze ilustrační!)

Zjednodušeně řečeno: pokud má instrukce oba nejnižší bity nastavené, chová se, díky neúplnému dekodéru, jako dvě instrukce, G1 a G2. Například instrukce s kódem $AF se chová jako instrukce s kódem $AE &#8211; LDX i s kódem $AD &#8211; LDA (nikoli $AC, ta má oba bity nulové a je správně dekódována jako G3). U prvních dvou taktů to nevadí, v nich se pouze čte absolutní adresa, ve třetím taktu se obsah této adresy čte do registru. Do kterého? To se nastavuje v prvním taktu. Pro G1 se aktivuje signál, uvolňující zápis do registru A, pro G2 zápis do registru X. V našem případě se tedy uvolní oba, a hodnota se zapíše do obou.

Obdobně kombinace STA a STX vytvoří složenou instrukci SAX, která do paměti uloží hodnoty obou registrů A a X najednou &#8211; totiž výsledek AND obou hodnot (tipuju, že jsou interně použity otevřené kolektory, které vytvoří &#8222;montážní AND&#8220;).

Takto lze vystopovat mnoho &#8222;ilegálních instrukcí&#8220;. Dokonce i TAS&#8230;

## Kill &#8218;em!

V PLA je zakódováno nejen to, kolik taktů která instrukce trvá a kde bere operandy, ale i načtení kódu další instrukce v posledním taktu. Což s sebou nese další zajímavý jev.

Některé kódy nedělají nic (NOP). Některé NOPy ještě předtím načtou jeden nebo dva bajty. No a některé instrukce prostě zaseknou celý procesor (většina kódů, které končí dvojkou). Nejpravděpodobnější vysvětlení je, že se u těchto kódů v PLA nenajde právě to načtení další instrukce, které mj. vynuluje počítadlo taktů, a instrukce se dostane do smrtícího &#8222;osmého taktu&#8220;, pro který už neexistuje žádný záznam v PLA. A protože se požadavky na přerušení řeší až na konci instrukce, tj. v okamžiku, kdy se nuluje počítadlo taktů, tak ani přerušení nenastane. Procesor se zkrátka &#8222;ocitne mimo šachovnici&#8220; a vzpamatuje ho až RESET.

K dalšímu studiu doporučuju:

  * [Decode ROM](http://visual6502.org/wiki/index.php?title=6507_Decode_ROM)
  * [How MOS 6502 Illegal Opcodes really work](http://www.pagetable.com/?p=39)
  * [PLA dump](https://code.google.com/p/breaks/source/browse/trunk/Docs/6502/PLA.txt?spec=svn2&r=2)
  * [6502 opcodes](http://www.oxyron.de/html/opcodes02.html)
  * [6502 extra opcodes](http://www.ffd2.com/fridge/docs/6502-NMOS.extra.opcodes)