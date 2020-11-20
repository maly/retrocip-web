---
id: 92
title: Hrajeme si s emulátorem JPR-1
date: 2014-01-12T10:57:45+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=92
permalink: /hrajeme-si-s-emulatorem-jpr-1/
dsq_thread_id:
  - "2111843551"
xyz_lnap:
  - "1"
categories:
  - ASM80.com
tags:
  - "8080"
  - ASM80
  - Intel
  - Tesla
---
K tomuto počítači mám značně osobní vztah, a proto jeho emulátor nesmí ve sbírce chybět.

<!--more-->

Začnu, s dovolením, osobní vzpomínkou. Když v roce 1983 vyšlo &#8222;legendární&#8220; Amatérské Radio řady B (modré) čísla 1 a 2, bylo mi deset let. Nerozuměl jsem ani za mák všem těm schématům a popisům činnosti, ale na konci druhého čísla byl &#8222;popis programového vybavení JPR-1&#8220;, kde ing. Tomáš Smutný (bratr konstruktéra JPR-1 Eduarda Smutného) popisoval programovací jazyk MIKRO BASIC. Popis příkazů, co dělají, jak fungují&#8230; A to bylo moje první setkání s opravdovým programováním, navíc takové, kde bylo i vysvětlené, co se vlastně děje.

Zanedlouho byl sešit popsaný &#8222;programy&#8220; na sčítání čísel, na výpočty objemů a obsahů a podobnými nikdy nevyzkoušenými perlami. První mikropočítač jsem na vlastní oči viděl až o dva roky později (a nebylo to JPR-1, tou dobou přejmenované už na SAPI), bylo to PMD. Ale JPR zůstalo jednoznačně prvním počítačem, na kterém jsem se naučil minimální základy programování, ačkoli jsem u něj nikdy neseděl.

Další emulátor pro [ASM80.com](https://www.asm80.com/) je tedy [emulátor JPR-1](https://www.asm80.com/jpr.html) (nebo SAPI-1, jak chcete) právě s MIKRO BASICem. Je to alfaverze, neřešil jsem zrcadlení pamětí ani perfektní emulaci generátoru znaků (místo toho jsem použil nějaký obstojný font 5&#215;7). Chováním víceméně odpovídá téhle sestavě:

  * [JPR-1 ](https://www.sapi.cz/sapi/jpr-1.php)
  * [AND-1](https://www.sapi.cz/sapi/and-1.php)
  * [ANK-1](https://www.sapi.cz/sapi/ank-1.php)
  * [REM-1](https://www.sapi.cz/sapi/rem-1.php)
  * [DSM-1](https://www.sapi.cz/sapi/dsm-1.php) (jen dirty hack pro ukládání na pásku)

V emulované EPROM jsou uložené programy [MIKRO BASIC](https://www.sapi.cz/sapi/mikrobasic.php) a [HELP](https://www.sapi.cz/sapi/help.php).

Bohužel nevím o tom, že by se dochovalo nějaké programové vybavení pro tuto sestavu (ani pro další s MIKOSem), takže to máte jen jako Opravdu Nostalgický Počítač k hraní a zkoumání.

_Děkuji autorovi (snad jediných) stránek o [SAPI-1](https://www.sapi.cz/sapi/sapi.php) s přezdívkou EC1045 za to, že uchovává a dává veřejně k dispozici dokumentaci starých českých osmibitových počítačů. Z jeho stránek jsem při tvorbě tohoto emulátoru intenzivně čerpal. Díky._