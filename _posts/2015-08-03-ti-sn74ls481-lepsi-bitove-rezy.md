---
id: 663
title: 'TI SN74LS481: Lepší bitové řezy'
date: 2015-08-03T17:39:11+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=663
permalink: /ti-sn74ls481-lepsi-bitove-rezy/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "3999477106"
image: https://retrocip.cz/wp-content/uploads/sites/6/2015/08/SN74LS481J-300x208-300x198.jpg
categories:
  - Hardware
tags:
  - řezy
  - Texas Instruments
---
Díky svolení kurátora _CPU Shack_ Musea mohu na Retročipu zveřejnit některé zajímavé články z historie mikroprocesorů. Dnes si vezmeme na paškál čtyřbitový řez od Texas Instruments.

<!--more-->

<img loading="lazy" class=" size-full wp-image-664 alignleft" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2015/08/SN74LS481J-300x208.jpg" alt="" width="300" height="208" /> Sedmdesátá léta byla doba, kdy vznikaly nové a inovativní procesory, rychlejší a s více bity. Většina z nich byly nové návrhy, některé naopak na čipu implementovaly starší architektury mainframů (např. TI9900 nebo Intersil 6100). V tu samou dobu probíhal souboj čtyřbitových řezových procesorů. Nejznámější je samozřejmě AMD AM2901, který je nesporným vítězem tohoto klání. Byli tu ale i jiní kandidáti, např. MMI6701 (s jehož výrobcem se později AMD spojili). Motorola přišla s procesorem MC10800. Intel navrhl svou, k neúspěchu odsouzenou (to, že měla jen dvoubitové řezy, mohlo rozhodnout) řadu 3000. TI udělali SBP0400 technologií I<sup>2</sup>L. Tato řada zaznamenala určitý úspěch, ale ten nestačil. Ve stejném roce, kdy vyšel SBP0400 (1976), vyšel i 6701, AM2901, a samotný TI vydal i procesor SN74S481. Šlo o čtyřbitový procesorový řez, vyrobený technologií Schottky TTL. K němu existoval i řadič SN74S482. Tento procesor se od svojí konkurence lehce lišil.

Nejpatrnější rozdíl je v pouzdru. Procesor měl pouzdro QIL48 (Quad In Line). Takto masivní pouzdro (pro pouhý čtyřbitový řez) stěží napomohl úspěchu. Tak velký počet pinů byl důsledkem toho, že procesor nabízel tři datové porty, adresový port a desetibitovou sběrnici pro výběr instrukce. Procesor nabízel téměř 25 tisíc (24.780) funkcí, které byly kompletně mikroprogramovatelné pomocí integrované PLA. Měl zabudované funkce pro násobení i dělaní, stejně jako algoritmus CRC. PLA mohla být naprogramována podle zákaznických požadavků tak, aby instrukční sada odpovídala plně požadavkům aplikace. Obvod 74S481 obsahoval i Program Counter (na rozdíl od 2901 nebo AMD29116). Řadič 74S482 byl potřeba pouze k obsluze přepínání kontextu (skoky, návraty, operace se zásobníkem apod.)

Zajímavý prvek tohoto čipu byl vývod, kterým poznal svou pozici ve víceprocesorovém systému. Pokud byl připojen na Vcc, choval se obvod jako MSP (nejvýznamnější část slova), pokud na GND, choval se jako LSP (nejméně významná část). Pokud jste ho nechali uprostřed (2.4V), bral to čip tak, že se nachází uprostřed slova. Takto se výrazně zjednodušil návrh vícebitových systémů: řezy &#8222;uprostřed&#8220; se chovaly stejně, ale nejvyšší a nejnižší část mohly správně ošetřit všechny stavy typu přetečení, přenosu apod.

TI inzeroval 481 s tím, že &#8222;dokáže efektivně emulovat existující systémy&#8220;. Idea procesoru 481 byla taková, že vezmete starší počítač nebo systém, a jeho jádro implementujete pomocí několika 481, podle bitové šířky. TI samotné využilo tento obvod v novějších verzích svého systému TI990, kde využili čtyři 481 pro sestavení šestnáctibitového procesoru.

Originální SN74S481 běžel na 10 MHz a jeho spotřeba byla 345mA. TI rychle vyvinul LS verzi, SN74LS481, která běžela na kmitočtu 8MHz a spotřebu měla 220mA. Vznikla i odolná verze v řadě 54 (54S481, resp. 54LS481). Nakonec vznikla i DIP48 verze SN74S481N, která měla snížit cenu a složitost konstrukce.

V důsledku konkurenčního boje a nedostatku &#8222;[druhých zdrojů](https://retrocip.uelectronics.info/klony-a-procesory/)&#8220; nezaznamenala 481 žádný větší úspěch mimo vlastní stroje TI. V roce 1986 jej TI přestali prodávat, ačkoli je ironické, že 74S482 ještě několik let prodávali, protože byl používaný s jinými řezovými procesory, nebo dokonce s ALU obvody 181.

&nbsp;

(Originál: [TI SN74LS481: A Better Bit-Slicer](https://www.cpushack.com/2015/07/16/ti-sn74ls481-a-better-bit-slicer/))