---
id: 33
title: Nahraj to znovu, Same!
date: 2013-11-25T20:57:21+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=33
permalink: /nahraj-to-znovu-same/
dsq_thread_id:
  - "1998989409"
categories:
  - ASM80.com
---
Konečně mám doopravováno a emulační jádro 8080 snad šlape jak má, takže byl čas i experimentovat.

<!--more-->

To mě takhle po ránu chytla marnivost, a když už mi PMD aspoň [nehavarovalo při &#8222;PRINT 1&#8220;](https://retrocip.uelectronics.info/testovaci-kod-pro-8080-a-peklo-s-daa/ "Testovací kód pro 8080 a peklo s DAA"), tak jsem si říkal, že bych se mohl pochlubit Martinu Bórikovi z RM Teamu, (spolu)autorovi známého emulátoru PMD 85. Pochválil, to mě potěšilo, a upozornil mě, že mi emulátor na PRINT 3*3 tiskne 9.04688.

Tak, v zásadě, první Pentium nepočítalo o moc líp, ale to není moc dobrá omluva. Co s tím?

Tak zaprvé jsem pořádně opravil DAA podle Bórikovic emulátoru. Výsledek byl stejný. Totiž &#8211; chybný.

Hm, hm, hm&#8230; Prověřil jsem pořádně RM verzi DAA, a chovala se chybně. Ha, problém bude asi někde jinde. Co takhle příznak AC? Je nastavený a nulovaný správně?

No, v zásadě byl, jen u logických instrukcí to bylo přesně obráceně, než jsem to měl.

Jupí, tak znovu: PRINT 3*3&#8230; 9.04688! Ďas aby to spral! Tam mi někde lítá jeden bit! Někde v té FP kalkulačce je nějaká instrukce, která očekává, že v CY je nula, a ona tam je jednička, a ta se mi tam přičte a tohle provede. Ale kde a jaká?

Šel jsem instrukci po instrukci. Všechno to sedělo. Sčítání, odčítání, logické instrukce, všechno šlape jak hodinky a nastavuje co nastavit má.

Chvíle zoufalství, až jsem si o tom tvítnul, a pak jsem ji našel. Bestii. Instrukce DAD, dvoubajtový součet, která po sobě nechává příznakový registr ve velmi razantním stavu! Ovšem MOJE dokumentace o tom cudně mlčela, tvářila se, že je všechno tak jak to má standardně být&#8230; Není to tak. S, Z, A a P &#8211; všechno nastavené na 1 bez ohledu na výsledek.

Upravil jsem, spustil &#8211; a 3*3 bylo náhle jen a pouhých 9! Ta úleva! Od radosti jsem si vypočítal mocniny čísel od 0 do 10. Krása, šlapalo to!

A pak jsem si šel ohřát večeři. Přemýšlel jsem přitom, jestli bych&#8230; teda&#8230; jak by šlo&#8230; vlastně&#8230; nojo! A napadlo mě, jak udělat zvuk. Nekecám, fakt věrnej zvuk, synchronizovanej, vše tak jak má být!

Večeře stydla a já programoval. Zase jsem v různých bídných kopiích a překreslených schématech, kde nevidíte, jestli to je PC0 nebo PC6, hledal přesné zapojení repráčku, a nakonec jsem našel. PMD totiž mělo generátor 4 kHz a 1 kHz tónu připojené na výstupy PC0 a PC1 systémového 8255, a ještě k tomu mělo přímý výstup PC2 na reproduktor.

Chvilku jsem počítal různé konstanty &#8211; např. &#8222;když samplovací frekvence je 48kHz a buffer má 1024 položek, kolik za tu dobu proběhne strojových cyklů při frekvenci 2MHz?&#8220; (42666.6 periodických) a &#8222;kolik T připadá na jeden sampl?&#8220; a &#8222;kolik samplů zabere 1kHz tón?&#8220; a do toho mi divě hvízdal počítač nepříjemnými frekvencemi.

Ale pak jsem vymyslel, jak převést dávkové zpracování instrukcí na posloupnost samplů, spustil jsem to a ozvalo se ono známé &#8211; však víte!

Přeložil jsem [Auto](https://pmd85.mysteria.cz/data/klasika/nat_auto.txt), nahrál ho do emulátoru, napsal JUMP 2000, a ono to ani nespadlo, ani neudělalo nějaké podivnosti, ale úplně normálně to fungovalo! Tady je důkaz:



A když už jsem byl v tom, tak jsem přeložil, zahrál a nahrál i [Cihly](https://www.youtube.com/watch?v=uSdXMtj1gkQ):