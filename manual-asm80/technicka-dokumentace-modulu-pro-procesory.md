---
id: 410
title: Technická dokumentace modulů pro procesory
date: 2014-10-08T13:59:38+01:00
author: Martin Maly
layout: page
guid: http://retrocip.uelectronics.info/?page_id=410
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3095753249"
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Proste_instrukce"><span class="toc_number toc_depth_1">1</span> Prosté instrukce</a>
    </li>
    <li>
      <a href="#Instrukce_s_parametrem"><span class="toc_number toc_depth_1">2</span> Instrukce s parametrem</a>
    </li>
    <li>
      <a href="#Vicebajtove_operacni_kody"><span class="toc_number toc_depth_1">3</span> Vícebajtové operační kódy</a>
    </li>
    <li>
      <a href="#Relativni_skoky_kratkedlouhe_instrukce_atd"><span class="toc_number toc_depth_1">4</span> Relativní skoky, krátké/dlouhé instrukce atd.</a>
    </li>
  </ul>
</div>

Překladač se skládá z preprocesoru, parseru a vlastního jádra, které přeloží jednu konkrétní instrukci pro daný procesor. Tato jádra jsou napsána jako zásuvné moduly.

Objekt:

<pre class="lang:js decode:true">var M6809 = {
   "parseOpcode": function (s,vars) {
      ...
   },
   "endian": true
};
</pre>

Objekt má tedy jednu jedinou metodu _parseOpcode_ a má volitelný atribut _endian_ &#8211; pokud je true, jedná se o Big Endian (Motorola), jinak je procesor Little Endian (Intel, 6502).

Metoda _parseOpcode_ dostává dva parametry: první je _s_, což je rozparsovaná instrukce, viz dále, a druhý je _vars_, což jsou hodnoty proměnných, které jsou v dané chvíli známé (tj. v prvním průběhu ty, které jsou už definované, ve druhém všechny, včetně dopočítaných).

Metoda _parseOpcode_ vezme údaje z objektu _s_, identifikuje instrukci, doplní její operační kód a informace o celkové délce instrukce, a vrátí rozšířený objekt _s_.

V okamžiku volání metody má objekt _s_ tento formát:

<pre class="">{
 numline: 42,
 opcode: "PSHU", 
 params: Array[1], 
 addr: "0x100",
 lens: Array[0],
 bytes: 0
}</pre>

  * opcode je název instrukce (uppercase)
  * numline je číslo aktuálního řádku
  * addr je adresa, na které je instrukce
  * params je pole s parametry, které jsou zadány za instrukcí. Např. u instrukce &#8222;MOV A,B&#8220; bude mít params dvě položky, &#8222;A&#8220; a &#8222;B&#8220;
  * bytes, lens jsou položky, které musí metoda doplnit. bytes je celková délka instrukce a lens je pole, v němž jsou jednotlivé operační kódy a kódy parametrů.

Pokud není instrukce rozpoznána, musí metoda vrátit hodnotu _null._ Překladač pak_ _vyvolá chybový stav &#8222;nerozpoznaná instrukce&#8220;.

### <span id="Proste_instrukce">Prosté instrukce</span>

Jednoduché instrukce, jako &#8222;NOP&#8220;, které zabírají jediný bajt a nemají parametry, přeloží metoda tak, že do s.bytes uloží 1 a do s.lens[0] zapíše operační kód instrukce.

### <span id="Instrukce_s_parametrem">Instrukce s parametrem</span>

Důležité je myslet na to, že hodnotu parametru dopočítá překladač ve druhém průchodu, takže se o ni nestará překládací modul. Ten ale musí doplnit volání parsovací funkce. Příklad &#8211; instrukce MVI A, 123

V poli &#8222;params&#8220; budou hodnoty &#8222;A&#8220; a &#8222;123&#8220;. Operační kód se spočítá z názvu &#8222;MVI&#8220; a prvního parametru (params[0]). Do atributu s.bytes zadáme 2 a do pole lens[0] se zapíše operační kód (0x3F). Do pole lens[1] je potřeba zapsat funkci, která druhému průchodu řekne, že má spočítat aktuální hodnotu druhého parametru (params[1]). Takto:

<pre class="">lens[1] = function(vars){return Parser.evaluate(params[1],vars);}</pre>

Tedy jako hodnotu předáváme funkci s jedním parametrem (vars, známé pole proměnných) a funkce vrací výsledek výrazu Parser.evaluate(params[1],vars) &#8211; což je interní funkce překladače, která parsuje výrazy.

V případě dvoubajtového parametru (třeba LXI) se do lens[0] zapíše operační kód, do lens[1] výše uvedená funkce a do lens[2] hodnota null. Překladač pak doplní vypočítanou hodnotu na místa 1 a 2.

### <span id="Vicebajtove_operacni_kody">Vícebajtové operační kódy</span>

Není problém, stačí jen správně nastavit bytes a kódy do pole lens.

### <span id="Relativni_skoky_kratkedlouhe_instrukce_atd">Relativní skoky, krátké/dlouhé instrukce atd.</span>

Lze využít volání _Parser.evaluate(_výraz_, vars)_, který vrátí vypočítanou hodnotu výrazu. Pro spočítání relativního skoku lze použít pseudoproměnnou _vars._PC_, která obsahuje aktuální adresu překládané instrukce.

Problém může nastat u procesorů jako je 6502, kde lze zapsat instrukci LDA #10 a překladač se musí rozhodnout, jestli využije krátkou verzi se zero page, nebo dlouhou s absolutní adresou. V takovém případě je možné právě využít volání Parser.evaluate() a nechat si spočítat aktuální hodnotu výrazu. Pokud je známá, tj. nejsou v ní použité zatím nedefinované proměnné, dostanete výsledek, který můžete použít při rozhodování. Pokud je ve výrazu nějaká dopředná reference, nastane problém a Parser.evaluate vyhodí výjimku. Je potřeba si ji odchytit.

Výjimky

Pokud metoda narazí např. na podivnou kombinaci parametrů, musí vyhodit výjimku. Příklad:

<pre class="">throw "Bad addressing mode at line "+s.numline;</pre>