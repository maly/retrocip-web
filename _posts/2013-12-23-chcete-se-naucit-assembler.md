---
id: 72
title: Chcete se naučit assembler?
date: 2013-12-23T12:13:59+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=72
permalink: /chcete-se-naucit-assembler/
dsq_thread_id:
  - "2068431722"
categories:
  - Stroják
tags:
  - "6502"
  - "8080"
  - Z80
---
Jediná správná odpověď na takovou otázku zní: Proč? Ano, proč bych se měl chtít učit něco, co vlastně dneska už skoro nikde nepoužiju, něco, co je strašně nepohodlné, něco, co skoro nikdo nevyžaduje&#8230; Zkusím na pár těchto otázek odpovědět.

<!--more-->

Proč se učit assembler? Je to znalost srovnatelná, řekněme, s uměním hry na harfu. Skoro nikde to neuplatníte, doma si můžete brkat tak pro sebe, na sraz to nevezmete a vaše hvězdná hodina přijde ve chvíli, kdy na koncertu symfonického orchestru hráč na harfu omdlí, zoufalý dirigent se obrátí do publika a zakřičí: &#8222;Není tu mezi přítomnými harfista?!&#8220;

Tedy, to jsem, uznávám, trošku přehnal. Assembler využijete častěji. Například pokud budete psát programy pro embedded zařízení, tam si pravděpodobně na assembler budete moci sáhnout (i když většina se toho píše v C a na assembler zbývá akorát tak ten zavaděč). S assemblerem se potkáte při vývoji velmi nízkoúrovňového softwaru, kde je kritická rychlost a velikost kódu (protože, marná sláva, nic rychlejšího než program ve strojovém kódu už procesor nezpracuje). Pokud jste Opravdový Odborník a píšete už třicet let bankovní systémy v Javě, tak assembler rozhodně nevyužijete (a ne, nenamlouvejte si, že tím, že znáte Javu, jste automaticky výborný programátor ve všech jazycích &#8211; assembler vaše sebevědomí srazí na kolena okamžitě). Pokud děláte weby, tak na assembler zapomeňte, ten je tak hluboko, hluboko pod vaší úrovní, že na něj ani nedohlédnete.

Assembler použijete taky v případě, že máte jako hobby staré počítače nebo tvorbu operačních systémů. Ale spíš ty staré počítače. Anebo jednočipové aplikace.

Pokud se ničím z toho nezabýváte, tak se stále ještě můžete naučit assembler ze dvou důvodů. Zaprvé &#8211; dokážete si, že nejste žádné máslo. Zadruhé &#8211; když se naučíte assembler, tak budete mnohem víc rozumět tomu, jak procesor vlastně pracuje. Což je dobré vědět nejen ze zvědavosti, ale navíc si budete moci představit za každým řádkem kódu, co napíšete, to neuvěřitelné množství věcí, které procesor dělá, no a některé optimalizace vám začnou dávat smysl.

Dobře, když tedy odpověď zní _Ano, naučím se assembler_, tak další logická otázka zní: Jaký? Ono totiž &#8211; co procesor, to jiná instrukční sada. Když se budete motat někde okolo Linuxu a PC, tak je vaší volbou assembler procesorů x86 a novějších. U embedded systémů záleží na tom, o jaké systémy jde. Můžete narazit na procesory, odvozené od architektury ARM, mohou to být následovníci Motoroly 68x, někde to bude x51, někde třeba embedded 6502 či Z80, někde AVR nebo PIC&#8230; Záleží na vás.

Já bych ale navrhoval začít procesorem 8080. Sice se s ním dneska už asi nepotkáte &#8211; tedy pokud doma neoprašujete nějaké to PMD, PMI nebo IQ-151 &#8211; ale pro učení má několik výhod.

Zaprvé &#8211; je jednoduchý. Tedy, taková 6502 je ještě jednodušší, ale ta je do jisté míry lehce &#8222;exotická&#8220; (ačkoli na ní běžely spousty krásných strojů).

Zadruhé &#8211; procesor 8080 je přímým předchůdcem procesoru Z80 a &#8211; a to především &#8211; rozšířením jeho architektury vznikly první intelské šestnáctibitové procesory 8086, se kterými, bohužel, žijeme dodnes.

Takže když se naučíte assembler 8080, máte jen krůček k Z80 (jen se naučíte jiné názvy instrukcí a doučíte se ty, co má Z80 navíc), nebo můžete ukročit na druhou stranu a vydat se cestou 8086, 80386, &#8230;

Tak, tolik asi k motivaci, a teď do práce. Pokud se chcete naučit assembler těchto starých veteránů (8080, Z80, 6502, ale pokud bude zájem, tak nevylučuju časem ani AVR nebo PIC), tak jste vítáni na stránce [Stroják.cz](http://strojak.cz) (neplést se Zdroják.cz, což je _magazín o programování 63 úrovní nad strojovým kódem_). Postupně píšu další kapitoly a rád bych vám nabídnul něco, co jsem já, když jsem se učil assembler, neměl.

Totiž, když jsem se já učil assembler (někdy v roce 1984&#8230; vidíte, napřesrok budu mít výročí!), měl jsem k tomu k dispozici tři, slovy tři materiály. Jednak instrukční sadu procesoru 8085, pak výpis monitoru počítače BOB-85 a pak výpis obsluhy klávesnice a displeje počítače JPR-1. O spoustě věcí (třeba fungování přerušení) se člověk nedozvěděl. O nějakou dobu později, už s reálným počítačem, jsem zkoušel, co se stane když&#8230; Na spoustu věcí jsem si musel přijít sám (víte třeba jak na procesoru, co umí akorát sčítat a odčítat, udělat násobení? Znáte aspoň tři způsoby?), ale většinu věcí jsem odhalil při nekonečném disassemblování cizích programů a zkoumání toho, jak to funguje. Když se mi dostal do ruky [Machine Code Tutor](http://www.worldofspectrum.org/infoseekid.cgi?id=0008031), už mě nic nového nenaučil.

Tehdy mi chyběly hlavně dvě věci: Možnost si naučené ihned vyzkoušet, a zjistit i něco víc než jen seznam instrukcí a jejich chování. Třeba právě to násobení. Nebo jak převést číslo na jeho desítkové vyjádření v ASCII kódech, a naopak.

A právě tohle bych chtěl ve svém kurzu napravit. Zatím tedy probírám instrukce jednu po druhé, ale v každé lekci máte embedovaný editor + assembler + debugger, kde si můžete kód přeložit a otestovat jeho chování, popřípadě si ho poupravit, podívat se, jak se chová upravený, &#8230; No a v samotném textu dělám odbočky k různým algoritmům. Zatím jen nesmělé (ve chvíli, kdy píšu tenhle článek, jsem s výkladem těsně za instrukcema skoků), ale jak bude narůstat počet známých instrukcí, poroste i počet ukázek a algoritmů.

Jste srdečně zváni na [Stroják.cz](http://strojak.cz)!

(Komentáře, prosím, používejte vždy ve chvíli, kdy vám bude něco nejasného &#8211; já se tak budu moct ke kapitole vrátit a doplnit ji tak, aby byla srozumitelná.)