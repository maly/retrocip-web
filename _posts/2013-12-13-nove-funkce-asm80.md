---
id: 64
title: Nové funkce ASM80
date: 2013-12-13T13:36:41+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=64
permalink: /nove-funkce-asm80/
dsq_thread_id:
  - "2047960178"
xyz_lnap:
  - "1"
categories:
  - ASM80.com
tags:
  - ASM80
---
Zahálel jsem? Nezahálel! Dovolte mi představit pár nových funkcí v ASM80&#8230;

<!--more-->

V posledních dnech jsem přidal pár funkcí a opravil několik chyb. Pojďme si je představit.

Pseudoinstrukce **.error** funguje tak, že zastaví překlad a vypíše chybu. Můžete ji použít v podmíněném bloku, nebo i jinak, je to na vás.

Opravil jsem chybu parsování argumentů, takže už funguje DB s řetězcem, ve kterém je středník.

V pravém sloupci je konverzní nástroj z binárních souborů na DB (což není databáze, ale instrukce DB). Fungování je prosté: Nastavte si v editoru kurzor do místa, kam chcete vložit binární data, a pak přetáhněte soubor z počítače do této oblasti.

[![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/12/asm1.jpg)](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/12/asm1.jpg)

&nbsp;

Soubor se převede na posloupnost DB a umístí do okna se zdrojovým kódem.

Přidal jsem pseudoinstrukci **.engine**. Touto direktivou řeknete IDE, pro jaký stroj je program určen (zatím jen &#8222;pmd&#8220; nebo &#8222;pmi&#8220;). Projeví se to po stisku F10 (Emulace). Pokud není direktiva zadaná, spustí se jen emulátor procesoru. Pokud direktivu zadáte, spustí se emulátor konkrétního stroje s nahraným programem. Po návratu do IDE se opět otevře soubor se zdrojovým kódem.

Když už je řeč o debugování &#8211; změnil jsem debugger tak, že při jeho použití se v okně se zdrojovým kódem neobjevuje čistý zdroják, ale soubor LST. Tedy přesně to, co bylo přeloženo, s rozbalenými makry a includy.

Nad tlačítkem Compile je nově tlačítko Format code, které zformátuje váš zdroják (udělá jednotné odsazení, převede názvy instrukcí na velká písmena apod.)

Přibyl zatím neoficiální emulátor BOB-85.

Přidal jsem uživatelské účty. Je to zatím nenastylovaná verze a práce s tím je poněkud spartánská. Najdete je vlevo v záložce &#8222;Workspaces online&#8220;. V tomto okně se můžete přihlásit, nebo zaregistrovat (&#8222;sign in&#8220;, ještě ten popis změním na nějaký jednoznačnější).

Jakmile se zaregistrujete a přihlásíte, máte možnost uložit celý svůj workspace, všechny soubory, s nimiž pracujete, na server pod svůj účet. Jsou privátní, nikdo k nim nemá přístup, jen vy. Použijte tlačítko &#8222;Save workspace as&#8230;&#8220;, tím zadáte i jméno, pod nímž má být daný workspace uložen. Je to taková obdoba &#8222;projektů&#8220;.

Ve chvíli, kdy máte uložené workspace, objeví se vám v téže záložce jejich seznam. Kliknutím přepnete na jiný (a předtím se aktuální uloží na server). Pokud chcete založit nový prázdný workspace, tak použijte &#8222;New workspace&#8220;. Aktuální se přitom uloží na server&#8230;

No a jako přihlášený uživatel můžete publikovat svůj zdroják jako knihovnu. Pokud třeba připravíte, řekněme, rutinu pro násobení dvou čísel, můžete ji nabídnout světu. Slouží k tomu tlačítko &#8222;Publish as library&#8220; Vyberete jméno, přidáte stručný popis, podrobný popis a odešlete. Do popisu nezapomeňte napsat, jaké registry váš program využívá a co potřebuje pro svůj běh! Navíc nezapomínejte, že assembler nemá oddělené jmenné prostory ani moduly, takže pokud v knihovně pojmenujete nějaké návěští třeba &#8222;LOOP&#8220;, tak je velmi pravděpodobné, že se pomlátí s jiným návěštím LOOP, takže prosím v knihovnách jména návěští důkladně prefixujte, abyste předešli konfliktu jmen. (Zvážím, jak to do budoucna dělat automaticky.)

Takto publikovanou knihovnu mohou ostatní použít ve svých zdrojových kódech pomocí speciální syntaxe .include. Dejme tomu, že budete chtít použít hlavičkový soubor s odkazy na rutiny monitoru PMD-85 (nic jiného než spousta EQU definic), které jsem zveřejnil já (uživatel &#8222;adent&#8220;) pod názvem &#8222;pmd-monitor&#8220;. Použijete tedy toto:

<p class="">
  <em>.include <adent/pmd-monitor></em>
</p>

Před lomítkem je jméno uživatele, za lomítkem název knihovny. Například takto:

<pre>.INCLUDE &lt;adent/pmd-monitor> 

          .ORG    1000H 
          .ENGINE pmd 

          CALL    PIIP 
          CALL    PIIP 
          CALL    PIIP 
          CALL    PIIP 
          RET     </pre>

Vhodnou knihovnu si můžete najít vlevo, v záložce &#8222;Libraries&#8220;.

(Ještě jednou upozorňuju, že je tahle funkcionalita v plenkách a nemusí stoprocentně fungovat.)

&#8212;

Další chystané novinky: zveřejňování celých programů (ne jen knihoven) a jejich import do vlastních workspaces. No a určitě přibude i nějaký nový emulátor českého počítače. O tom, jaký to bude, [můžete rozhodnout i vy](https://twtpoll.com/l53s2lh6wprq1i8). Zatím to vypadá, že nejvíc fanoušků má IQ-151. _Pámbu s námi! 🙂_