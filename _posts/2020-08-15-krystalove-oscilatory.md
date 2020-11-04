---
id: 1188
title: Krystalové oscilátory
date: 2020-08-15T10:30:14+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1188
permalink: /krystalove-oscilatory/
categories:
  - Hardware
  - Knihy
---
(Další kapitola, co se do Portů bajtů&#8230; nevešla)

Na to, že je přesný hodinový zdroj srdcem každého počítače, jsme se vlastními oscilátory moc nezabývali. Nemuseli jsme – všechny procesory, které jsme použili, měly zabudovaný oscilátor a stačilo jen připojit krystal a zátěžovou kapacitu (load capacity). Zátěžová kapacita má nejčastěji podobu jednoho nebo dvou kondenzátorů s&nbsp;kapacitou mezi 22 a 48 pF, a když nevíte, jakou přesně použít, podíváte se do datasheetu k&nbsp;procesoru, a tam to bývá popsané.

Důležité je jen dodržet pravidlo „velká zemnicí plocha kolem krystalu“. Při frekvencích, které používáme, nejsou kapacity, včetně těch parazitních, jako třeba kapacita plošného spoje, nijak výrazně kritické. U rychlejších obvodů by to byl problém, ale okolo 2 – 4 MHz, kde se pohybujeme my, není třeba přesně počítat kapacitu plošného spoje, stačí dodržet pravidla &#8222;velká zemnicí plocha okolo krystalu&#8220; a &#8222;netahat signálové vodiče poblíž&#8220;.

Když se do teorie krystalových oscilátorů ponoříte hlouběji, zjistíte, že možných zapojení je několik a že se rozlišují dva základní typy: paralelní a sériový. Krystaly mohou kmitat na dvou základních frekvencích, které se, aby se to nepletlo, rovněž nazývají sériová a paralelní. Sériová je o něco málo nižší než paralelní.

Obecně platí, že krystal se chová jako součástka, jejíž impedance (odpor) je nejnižší, pokud jí prochází střídavý proud s rezonanční frekvencí rovnou jeho mechanickým vlastnostem. Impedance krystalu je nulová ve dvou bodech – v _rezonančním_ bodu (sériová frekvence) a v _antirezonančním_ bodu (ideální paralelní frekvence).

U krystalů, které používáme my, tedy s&nbsp;kmitočty v&nbsp;řádu jednotek megahertzů, většinou není nutné řešit přesný rozdíl mezi sériovou a paralelní frekvencí. Stejně tak pro osmibitový hobby počítač není moc potřeba řešit teplotní kompenzaci a podobné věci, které musí řešit návrháři velmi přesných hodin.

Pokud si budete stavět samostatný krystalový oscilátor pro číslicová zařízení, pravděpodobně zvolíte zapojení s invertory. Ostatně i v těch mikroprocesorech, co mají &#8222;zabudovaný generátor hodin&#8220;, je použité stejné zapojení. Pomocí jednoho (paralelní) nebo dvou (sériové) invertorů vytvoříte první část oscilátoru, totiž zesilovač. O druhou část oscilátoru, o zpětnou vazbu, se postará právě krystal. 

## Sériové zapojení

První zapojení, sériové, používá dva invertory TTL LS/ALS, mezi nimiž je zapojen vazební kondenzátor C1 (někdy bývá vynechán) s kapacitou jednotek či desítek nF. Jeho úkolem je odfiltrovat stejnosměrný proud. Oba invertory mají zapojené zpětnovazební rezistory, které je udržují v oblasti lineárního zesílení. Jejich hodnota není kritická, pro frekvence 1 – 4 MHz se doporučuje okolo 2k2. Jiné zdroje doporučují R2 spočítat jako 3000/f, kde f je frekvence v MHz.<figure class="wp-block-image size-large">

<img loading="lazy" width="1024" height="552" src="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/serial-1024x552.png" alt="" class="wp-image-1189" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/serial-1024x552.png 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/serial-650x350.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/serial-768x414.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/serial-1536x828.png 1536w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/serial.png 1830w" sizes="(max-width: 1024px) 100vw, 1024px" /> </figure> 

Kondenzátor C2 je již zmíněný zátěžový – většina krystalů pro naše frekvence počítá se zátěžovou kapacitou mezi 22 – 33 pF, ale jak už jsem psal: hodnota není zcela kritická.

<blockquote class="wp-block-quote">
  <p>
    Raději to explicitně zmíním: Když píšu, že „hodnota není kritická“, tak tím mám na mysli, že není kritická pro uvažované použití, tedy generátor kmitů v řádu jednotek MHz bez nároku na vysokou přesnost.
  </p>
</blockquote>

Pomocí trimru C3 můžete kmitočet celého zapojení ještě jemně doladit, ale pokud nepotřebujete vysokou přesnost, můžete trimr vynechat. 

## Paralelní zapojení

Paralelní oscilátor používá pouze jeden jediný invertor TTL LS/ALS.<figure class="wp-block-image size-large">

<img loading="lazy" width="908" height="1024" src="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-ls-908x1024.png" alt="" class="wp-image-1191" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-ls-908x1024.png 908w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-ls-576x650.png 576w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-ls-768x866.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-ls.png 1140w" sizes="(max-width: 908px) 100vw, 908px" /> </figure> 

Rezistory R1, R2 a kondenzátor C4 fungují jako zpětná vazba pro udržení invertoru v oblasti lineárního zesílení. R1 a R2 mají odpor 1k, kondenzátor C4 kapacitu okolo 250 nF.

Zátěžové kondenzátory C1 a C2 mají opět kapacitu 22 – 33 pF, trimr C3 dolaďuje frekvenci, není nutný.

Pokud si postavíte tento oscilátor a použijete invertor 74HC04, 74HCT04 nebo jiné hradlo s technologií CMOS, zjistíte, že si ani nekmitnete. Oscilátor prostě nepoběží a nebude kmitat.

Někde se můžete setkat s názorem, že to je proto, že CMOS obvody jsou příliš rychlé, ale to není ten pravý důvod. Ony ve skutečnosti nejsou výrazně rychlejší než TTL LS. Pravý důvod je ten, že mají vyšší zisk a obrovský vstupní odpor. To znamená, že ve výše uvedeném zapojení je velmi těžké udržet je v lineární oblasti.

## Oscilátor s CMOS

Proto se používá modifikované zapojení s&nbsp;rezistorem sériově zapojeným u krystalu.<figure class="wp-block-image size-large">

<img loading="lazy" width="1024" height="881" src="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-1024x881.png" alt="" class="wp-image-1190" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-1024x881.png 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-650x559.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel-768x661.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2020/08/parallel.png 1140w" sizes="(max-width: 1024px) 100vw, 1024px" /> </figure> 

Rezistor R1, který udržuje invertor v&nbsp;pracovní oblasti, může mít řádově stovky kiloohmů. Typicky se používá hodnota 180 k, která je pro CMOS invertory více než dostatečná. Ale můžete použít téměř cokoli od 47 k&nbsp;po 1&nbsp;M.

Ladicí trimr C3 se opět používá k&nbsp;jemnému doladění, nebo se vynechává. Rezistor R2 pak omezuje proud krystalem, ale zároveň spolu s&nbsp;kondenzátorem C2 tvoří dolní propust, která brání krystalu kmitat na vyšších harmonických frekvencích.

Pokud v tomto zapojení použijete TTL LS/ALS hradlo, třeba 74LS04, také nebude kmitat. Velmi velký odpor ve zpětné vazbě jej nedokáže udržet v pracovní oblasti.

Výstup oscilátorů se většinou nepoužívá přímo. Ačkoli jsou invertory součástky číslicové, v těchto zapojeních z nich děláme trochu analogové zesilovače a nutíme je pracovat v oblastech _zakázaného pásma_. Kondenzátory navíc deformují strmost hran. Bývá proto dobrým zvykem na výstup připojit ještě jeden invertor (ne nutně se Schmittovým obvodem, ale pokud jej máte k dispozici, použijte jej), který ošetří strmost hran, posílí výstup a oddělí jej od zbytku obvodu.

K dalšímu studiu:

  * <https://www.electronics-tutorials.ws/oscillator/crystal.html>
  * <https://www.analog.com/media/en/technical-documentation/application-notes/an12fa.pdf>
  * <https://www.changpuak.ch/electronics/Oscillators.php>
  * <https://www.ecsxtal.com/store/pdf/Oscillation-Circut-Design-Considerations.pdf>
  * <https://www.eleccircuit.com/simple-crystal-oscillator-circuit/>
  * <https://www.eit.lth.se/fileadmin/eit/courses/edi021/PDF_files/oscillators.pdf>
  * <http://www.z80.info/uexosc.htm>