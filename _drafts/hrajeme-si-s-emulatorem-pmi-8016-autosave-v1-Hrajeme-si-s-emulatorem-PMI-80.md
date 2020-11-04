---
id: 25
title: Hrajeme si s (emulátorem) PMI-80
date: 2013-11-23T15:02:22+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/16-autosave-v1/
permalink: /16-autosave-v1/
---
Pokud si chcete vyzkoušet, jak funguje můj emulátor PMI-80, nevadí vám, že to je alfaverze a potřebujete trošku postrčit, tak tu pro vás mám takového menšího průvodce&#8230;

<!--more-->

[Emulátor PMI-80](http://www.asm80.com/pmi80.html) je součást mého [osmibitového IDE ASM80](http://www.asm80.com/). Je zatím v první veřejné alfaverzi, ale už je aspoň trošku funkční, tak pro největší nedočkavce zveřejňuju návod, jak si zkusit něco málo s PMI.

Pokud hledáte informace o PMI-80, mohu doporučit stránku [PMI-80 na NOSTALCOMP.cz](http://www.nostalcomp.cz/pmi80.php).

## 1. Spuštění emulátoru

Není co řešit &#8211; klikněte na odkaz [Emulátor PMI-80](http://www.asm80.com/pmi80.html). Je to webový emulátor, takže stačí mít nový prohlížeč (na starých nefunguje, protože používá některé moderní vymoženosti) a myš.

_Intermezzo: Ano, asi by šlo vše upravit tak, aby to fungovalo i s IE6, ale &#8222;poměr cena-výkon&#8220; je velmi nepříznivý. Ztratil bych tím tolik času, že by mě to pravděpodobně přestalo bavit ještě před dokončením, takže jsem upřednostnil řešení &#8222;mít hotovo pro 97 % internetové populace&#8220; před řešením &#8222;nikdy to nedodělat pro 100 %&#8220;_

[<img loading="lazy" class="aligncenter size-medium wp-image-18" alt="pmi1" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi11-650x321.png" width="650" height="321" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2013/11/pmi11-650x321.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2013/11/pmi11-1024x506.png 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2013/11/pmi11.png 1135w" sizes="(max-width: 650px) 100vw, 650px" />](http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi11.png)

&nbsp;

Vlevo je sloupec se soubory, které máte uložené v prohlížeči. Uprostřed je samotný emulátor. Vpravo pak ovládání &#8222;virtuální magnetofonové pásky&#8220; a tabulku s nápovědou, která říká, jak jsou přiřazené klávesy. Nemusíte klikat myší na virtuální klávesnici (i když to můžete taky), můžete použít klávesnici svého PC.

## 2. První krok

Stisknutím jakéhokoli tlačítka (kromě RE, což je RESET, a I, které vyvolá přerušení) se dostanete do režimu, kdy PMI čeká na příkaz &#8211; poznáte to podle toho, že se vlevo objeví &#8222;něco jako otazník&#8220;:

[<img loading="lazy" class="aligncenter size-full wp-image-19" alt="pmi2" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi2.png" width="280" height="78" />](http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi2.png)Co teď? Teď tam naťukáme první program. Kde ho vezmeme?

### 2.1. První program

Já použiju program z knihy _Mikropočítač PMI-80. Co s ním?_ od Jaroslava Vlacha (naleznete ji na výše uvedeném odkazu na webu NOSTALCOMP). Použiju program číslo 3 &#8211; stopky. Jeho zdrojový kód je následující:

<pre class="lang:asm decode:true">1C00                          .ORG   1c00h   
1C00                CLEAR:    EQU   00abh   
1C00                OUTDA:    EQU   00f2h   
1C00                DISP:     EQU   0140h   
1C00   3E 19                  MVI   a,19h   
1C02   CD AB 00               CALL   clear   
1C05   3E 00                  MVI   a,0   
1C07   32 FA 1F               STA   1ffah   
1C0A   16 20        L1:       MVI   d,20h   
1C0C   00           L2:       NOP   
1C0D   CD F2 00               CALL   outda   
1C10   CD 40 01               CALL   disp   
1C13   15                     DCR   d   
1C14   C2 0C 1C               JNZ   l2   
1C17   3A FA 1F               LDA   1ffah   
1C1A   3C                     INR   a   
1C1B   00                     NOP   
1C1C   32 FA 1F               STA   1ffah   
1C1F   C3 0A 1C               JMP   l1</pre>

Jistě znáte výpis assembleru &#8211; úplně vlevo adresa, pak operační kód, pak instrukce. Teď je potřeba operační kódy naťukat do PMI.

## 3. Vkládání dat do PMI-80

Na displeji máme otazník. Stiskneme M (pro práci s pamětí) a na displeji se objeví &#8222;M 0000&#8220; (samosebou limitované sedmisegmentovým displejem, takže M je spíš takové obrácené U). Zadáme adresu (1c00) a stiskneme [=] (na PC klávesnici je to ENTER). V pravé části se objeví &#8222;00&#8220;. Teď zadáváme data. Na adrese 1C00 je to 3E, takže zadáme 3E, mačkáme ENTER ([=]), adresa se o jedničku zvýší, zadáme obsah na adrese 1C01, tedy 19&#8230; a pokračujeme, dokud nenaťukáme všechno.

[<img loading="lazy" class="aligncenter size-full wp-image-21" alt="pmi3" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi3.png" width="275" height="75" />](http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi3.png)

Když dojdeme k poslední adrese a poslední hodnotě, tak stiskneme třeba \[EX\] (na klávesnici je to X). Na displeji se objeví &#8222;Err:data&#8220;, po dalším stisknutí se dostaneme zpátky na otazník.

## 4. Spouštíme program

Na displeji je otazník, PMI čeká na pokyny. Mačkáme tedy [EX]. Displej vlevo zobrazí G a čtyři znaky adresy. Zadáme 1C00 a mačkáme [=]. Tím se program spustí od adresy 1C00.

Krása, že?

Když se ho nabažíme, stiskneme [RE]. Tím se PMI resetuje, ale nebojte &#8211; o program v paměti nepřijdete, zůstane tam uložený.

## 5. Ukládáme program na pásku

Po náročném ťukání by bylo dobré svou práci uložit, ať se s tím příště nemusíte ťukat zas. PMI k tomu má rozhraní pro práci s páskou, a můj emulátor má zase &#8222;virtuální pásky&#8220;. Jak na to?

  * Z pohotovostního režimu (s otazníkem) stiskneme [S]
  * Adresu zadáme 1C00 a potvrdíme [=]
  * Číslo bloku necháme 00 a potvrdíme [=]
  * Displej ukazuje &#8222;MG run&#8220;&#8230;
  * Klikneme na &#8222;Tape start &#8211; record&#8220;, tím spustíme nahrávání. Rozhraní signalizuje &#8222;RECORDING&#8220;
  * Stiskneme opět \[=\] (na PMI), čímž se spustí záznam. Nějakou dobu to potrvá. Během té doby se úplně vlevo objeví znak &#8222;t&#8220; a počítadlo odpočítává.
  * Po skončení záznamu displej ukáže &#8222;MG STOP&#8220;.
  * Klikněte na tlačítko &#8222;Tape stop&#8220;, čímž se ukončí nahrávání na virtuální pásku.
  * &#8222;Tape length: 4626&#8220; je správná délka záznamu.
  * Stiskneme naposledy [=] a tím se ocitneme zase v pohotovostním režimu.

Virtuální páska je uložena v prohlížeči, takže když se na stránku s emulátorem vrátíte po čase, budete ji mít k dispozici.

## 6. Čteme program z pásky

Postup je obdobný ukládání.

  * Stiskneme [L] v pohotovostním režimu
  * Zadáme adresu, od které chceme data nahrát (1C00) a potvrdíme [=]
  * Zadáme číslo bloku (00) a potvrdíme [=]
  * Displej vyzve &#8222;MG Start&#8220;
  * Nejprve potvrdíme [=]&#8230;
  * &#8230; a pak klikneme na Tape start &#8211; playback
  * Data se začnou nahrávat, ukazatel odpočítává
  * Po nahrání displej zobrazí &#8222;MG Stop&#8220;
  * Stiskem [=] se zase dostaneme do pohotovostního režimu.

[<img loading="lazy" class="aligncenter size-full wp-image-23" alt="pmi4" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi4.png" width="204" height="147" />](http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2013/11/pmi4.png)

## 7. Ukládáme si pásku pro pozdější použití

Máme pásku nahranou. Bohužel se na ni vejde vždy jen jeden záznam. Ale to nevadí, můžeme si ji uložit pro pozdější použití. Stačí kliknout na &#8222;Save tape&#8220; a zadat vhodné jméno (&#8222;Stopky&#8220; třeba). Soubor se uloží do souborového systému v prohlížeči a objeví se v levém sloupci s příponou &#8222;.pmitape&#8220;. Můžete si jich uložit kolik chcete. Kliknutím na něj jej zase &#8222;přesunete&#8220; do &#8222;virtuálního magnetofonu&#8220;.

## 8. Použijeme assembler, protože jsme líní to celé překládat ručně a ťukat do klávesnice.

To samosebou lze. V horním pravém rohu je tlačítko &#8222;Back to IDE&#8220;. Klikněte na něj a ocitnete se v IDE ASM80. Vlevo je seznam souborů, vpravo ovládání, uprostřed okno editoru. Klikněte na &#8222;New file&#8220;, jako jméno zadejte &#8222;stopky.a80&#8220; (přípona A80 řekne překladači, že má použít modul pro procesor 8080) a napište program. Nebo si ho zkopírujte odsud:

<pre class="lang:asm decode:true crayon-selected">org 1c00h

clear 	equ 00abh
outda 	equ 00f2h
disp 	equ 0140h

	mvi a,19h
	call clear
	mvi a,0
	sta 1ffah
l1: mvi d, 20h
l2: nop
	call outda
	call disp
	dcr d
	jnz l2
	lda 1ffah
	inr a
	nop
	sta 1ffah
	jmp l1</pre>

Klikněte na &#8222;Save file&#8220; a pak na &#8222;Compile&#8220; (nebo použijte F9). Program se přeloží a v levém sloupci, mezi soubory, se kromě &#8222;stopky.a80&#8220; objeví i další dva soubory: &#8222;stopky.a80.hex&#8220;, který obsahuje HEX formát binárních dat, a &#8222;stopky.a80.lst&#8220;, ve kterém je výpis přeloženého kódu (to je ten první výpis v tomto článku).

A protože je to celé propojené (cizím slovem &#8222;integrované&#8220;), tak se vraťte do emulátoru PMI-80 (stačí kliknout na tlačítko vpravo nahoře s nápisem PMI-80, nebo ručně zadat adresu&#8230;) V emulátoru teď vidíte v levém sloupci soubor &#8222;stopky.a80.hex&#8220;. Kliknutím na něj ho natáhnete do paměti. Tralala, žádné ťukání do kláves. Můžete si vyzkoušet &#8211; pomocí klávesy M si projděte paměť od adresy 1C00 a uvidíte, že tam je to, co čekáte&#8230;

## 9. Co dál?

Vezměte si k ruce tu knihu, experimentujte, hrajte si, ať se vám líbí! Kdybyste objevili jakoukoli chybu, použijte Uservoice (to je taková ta bublina s otazníkem vpravo nahoře) a nahlašte mi ji, děkuju.

No a pro zvědavce prozradím, že mi funguje prealfa verze emulátoru BOB-85. Musím dodělat přesné časování instrukcí procesoru 8085 a napojit jej na virtuální magnetofon. Až bude hotovo, dám vědět opět tady&#8230;

&nbsp;