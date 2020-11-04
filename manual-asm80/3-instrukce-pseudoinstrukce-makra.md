---
id: 101
title: 3. Instrukce, pseudoinstrukce, makra
date: 2014-01-15T10:55:02+01:00
author: Martin Maly
layout: page
guid: http://retrocip.uelectronics.info/?page_id=101
dsq_thread_id:
  - "2131770165"
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Preprocesor"><span class="toc_number toc_depth_1">1</span> Preprocesor</a><ul>
        <li>
          <a href="#Makra"><span class="toc_number toc_depth_2">1.1</span> Makra</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Pseudoinstrukce"><span class="toc_number toc_depth_1">2</span> Pseudoinstrukce</a>
    </li>
    <li>
      <a href="#4_Vyrazy_a_cisla"><span class="toc_number toc_depth_1">3</span>  4. Výrazy a čísla</a>
    </li>
  </ul>
</div>

## <span id="Preprocesor">Preprocesor</span>

Instrukce pro preprocesor začínají vždy tečkou. Provedou se ještě před parsováním zdrojového textu.

| Pseudoinstrukce     | Význam                                                                                                                                                |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| .include _filename_ | Vloží obsah jiného souboru do tohoto místa.                                                                                                           |
| .incbin _filename_  | Vloží obsah jiného souboru do tohoto místa binárně, tj. nepovažuje ho za zdrojový kód pro assembler, ale za sérii dat, která bude vložena tak jak je. |
| .macro _macro_name_ | Označuje začátek definice makra. Tyto definice nelze vnořovat.                                                                                        |
| .rept _počet_       | Zopakuje instrukce od tohoto místa až po .endm tolikrát, kolik říká parametr                                                                          |
| .endm               | Konec definice makra / smyčky REPT                                                                                                                    |

### <span id="Makra">Makra</span>

Makro se definuje pomocí pseudoinstrukcí .macro a .endm. Parametry jsou číslovány od 1 a odkazovány pomocí zápisu %%1, %%2, %%3, …

<pre>.macro decadd
   adi %%1
   daa
.endm
; Ukázka použití
 decadd $22</pre>

Výsledný kód je:

<pre>0000 ; Ukázka použití 
 **MACRO UNROLL - DECADD
0000 87 22 ADI $22 
0001 27    DAA</pre>

## <span id="Pseudoinstrukce">Pseudoinstrukce</span>

Slouží k ovlivnění běhu překladače nebo pro vložení obsahu do výsledného kódu. Některé z nich lze z důvodu zpětné kompatibility používat bez úvodní tečky, ty jsou v následující tabulce uvedené bez tečky (ale lze použít i tečkovanou verzi).

| Pseudoinstrukce              | Význam                                                                                                                                                                                                                                                                                  |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| db (aliasy: defb, fcb)       | Vložení bajtů. Umožňuje DB čísel i řetězců. Lze použít syntaxi 10 DUP (123) – N krát zopakuje to, co je v závorce.                                                                                                                                                                      |
| dw (aliasy: defw, fdb)       | Vložení slov. Lze použít syntaxi 10 DUP (123) – N krát zopakuje to, co je v závorce.                                                                                                                                                                                                    |
| ds (aliasy: defm, defs, rmb) | Vložení prázdného místa                                                                                                                                                                                                                                                                 |
| fill _value_, _count_        | Vyplní paměť (count byte) hodnotou value.                                                                                                                                                                                                                                               |
| bsz (alias: zmb)             | Vložení určeného počtu nulových bajtů                                                                                                                                                                                                                                                   |
| org _addr_                   | Nastavení počáteční adresy překladu                                                                                                                                                                                                                                                     |
| .phase _addr_                | Kód bude vygenerován na tom místě, kde je zapsán (tedy dle příslušného ORG), ale bude vygenerován tak, jako by byl od adresy addr. Hodí se k psaní rutin, co budou později v paměti přeneseny na jiné místo                                                                             |
| .dephase                     | Konec bloku .phase                                                                                                                                                                                                                                                                      |
| .align _N_                   | Další instrukce bude na adrese, která je dělitelná N. Volné místo je volné a ve výsledném HEX není nijak ovlivněné.                                                                                                                                                                     |
| .ent _addr_                  | Nastavení adresy, od které se kód spustí v emulátoru. Použití např.: **.ent $**                                                                                                                                                                                                         |
| .engine _název_              | Nastavení emulátoru, ve kterém bude spouštěn kód při emulaci. Dostupné jsou: pmi, pmd, bob, jpr, sbcz80, sbc6502, sbc09 a zxs                                                                                                                                                           |
| equ (alias: =)               | Přiřazení hodnoty návěští. Např. **VIDRAM equ $4000**                                                                                                                                                                                                                                   |
| .if _cond_                   | Podmínka – blok instrukcí až po .endif se vloží pouze tehdy, pokud podmínka platí (výraz je nenulový)                                                                                                                                                                                   |
| .ifn _cond_                  | Podmínka – blok instrukcí až po .endif se vloží pouze tehdy, pokud podmínka neplatí (výraz je nulový)                                                                                                                                                                                   |
| .endif                       | Ukončuje blok s podmínkou. Pozor! Podmínky nelze vnořovat!                                                                                                                                                                                                                              |
| .block                       | Zahajuje blok lokálních proměnných. Všechna návěští v tomto bloku jsou lokální, tzn. mimo blok na ně nelze odkazovat. Pokud chcete definovat takové návěští,  
které bude vidět i zvenčí bloku, napište před jeho jméno znak @. Možné využití je u knihoven, vkládaných pomocí include. |
| .endblock                    | Ukončuje blok lokálních proměnných                                                                                                                                                                                                                                                      |

## <span id="4_Vyrazy_a_cisla"> 4. Výrazy a čísla</span>

Assembler z důvodu kompatibility umožňuje zápis v různých formátech. Decimální čísla se zapisují bez jakéhokoli prefixu či postfixu (10,42,-5). Hexadecimální v několika různých tvarech – s prefixy 0x nebo $ či s postfixem h: 0x12AB, $12AB nebo 12ABh jsou ekvivaletní zápisy. Binární konstanty se zapisují pomocí prefixu „%“ nebo postfixem „b“: 01001b / %01001

Parser v assembleru zvládne i složitější matematické výrazy se závorkami apod. Lze použít operátory +, -, *, /, % (modulo), # (modulo – alias k % kvůli komaptibilitě) a další. Ve výrazech lze použít návěští v roli proměnných. Lze použít i pseudoproměnnou $, která obsahuje aktuální hodnotu čítače adres.