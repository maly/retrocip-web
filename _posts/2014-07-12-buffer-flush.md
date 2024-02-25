---
id: 371
title: Buffer flush
date: 2014-07-12T15:09:29+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=371
permalink: /buffer-flush/
dsq_thread_id:
  - "2837897881"
categories:
  - Hardware
tags:
  - Archimedes
  - Atari
  - C128
  - C64
  - Commodore
---
To jsem zase nasbíral spoustu odkazů a je mi líto se o ně nepodělit&#8230;

<!--more-->

Nejprve upozornění &#8211; [článek o jedničkách a nulách](https://retrocip.uelectronics.info/exkurze-mezi-jednicky-a-nuly/ "Exkurze mezi jedničky a nuly") bude mít pokračování, tentokrát zaměřené na prostor mezi &#8222;mám tranzistory&#8220; a &#8222;mám hradlo&#8220;, který není Dave de Sadovi jasný. Můžete se těšit na animovaná CMOS hradla a krásné dvouemitorové TTL tranzistory, po kterých už bude všechno všem jasné. Ale to až budu mít trochu volna.

![](https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/05/IMG_20140503_092218-650x365.jpg) 

Dnes: vypouštění rybníka s odkazy.

U Atari 130XE jsem řešil, jako u všech svých retro strojů, jak nahrávat programy moderněji než z pásky, popřípadě z diskety. Pro většinu strojů existuje něco, co funguje s SD kartou. U Atari se to jmenuje [SDrive](https://raster.atariportal.cz/hw/sdrive/sdrive.htm). Existuje ještě jedno řešení [SIO2SD](https://www.lotharek.pl/product.php?pid=23), ale já zůstal u levnějšího, hezkého, českého SDrive, který jsem si pořídil v SMD verzi, která se dá zabudovat do krytu Atárka a tváří se, jako by tam byla odjakživa. Takže jsem zabudoval SDrive do Atari, připájel zevnitř na SIO port, a bylo.

Pro SDrive existuje vylepšená verze [SDrive-ng](https://www.kbrnet.de/projekte/sdrive-ng/index.html), která umí např. SDHC karty nebo FAT32.

Dokonce existuje i pandán k SIO2SD, jmenuje se [SIO2Arduino](https://www.whizzosoftware.com/sio2arduino/index.html). Funguje úplně stejně, tj. emuluje mechaniku Atari 1050. Pro případ připojení k PC máme i [SIO2PC](https://www.atarimax.com/sio2pc/documentation/).

Dobré informace o Atari najdete u [Jindroushe](https://www.atarimax.com/jindroush.atari.org/), včetně popisu [sériového rozhraní SIO](https://www.atarimax.com/jindroush.atari.org/asio.html). Dění mezi českými ataristy sleduju na [Atari portálu](https://www.atariportal.cz/), a pro software na úvodní naplnění SD karty jsem si šel [sem](https://atari.turiecfoto.sk/soft/) a [sem](https://mushca.com/f/atari/index.php?idx=2) a na ftp.pigwa.net.

Nový přírůstek Archimedes, inteligentní krasavec s procesorem RISC, si vyžádal taky nějaké to sbírání odkazů. Od stránek s technickými informacemi ([rozborka A3010](https://www.classicacorn.freeuk.com/32bit_computers/a3010base/a3010base.html), [historie Archimedů](https://www.retro-kit.co.uk/page.cfm/content/Acorn-BBC-A3000/), [manuály](https://chrisacorns.computinghistory.org.uk/Computers/A3010.html) a [dokumentace](https://www.chiark.greenend.org.uk/~theom/riscos/techdocs.html)) po [stránky o počítačích Acorn](https://www.classicacorn.freeuk.com/). A samozřejmě máme i hacky: [IDE pro A30x0](https://www.retro-kit.co.uk/page.cfm/content/Simtec-8bit-IDE-interface/), [hackování optické myši PS/2 pro Archimeda](https://www.ian-nic.pwp.blueyonder.co.uk/mouse/) ([pinout zde](https://www.chiark.greenend.org.uk/~theom/riscos/docs/mousepinout.txt)), [připojení ke standardním monitorům](https://www.riscos.org/legacy/monitors.html), [specifikace Archimedího videa](https://www.retro-kit.co.uk/user/custom/Acorn/32bit/documentation/Acorn_A3xA4xVideoSpec.pdf), a pro fajnšmekry [Hackimedes Raspimedes](https://www.riscosopen.org/wiki/documentation/show/Welcome%20to%20RISC%20OS%20Pi)&#8230; Jen náhradu mechaniky za SD kartu jsem neviděl. Ale zato mne inspiroval Martin svým [článkem](https://www.8bity.cz/2014/gotek-usb-floppy-emulator-pro-amigu/) o použití čínské &#8222;FDD mechaniky na USB Flash pohon&#8220; místo mechaniky v Amize, takže uvidím, třeba to zafunguje i u Archimeda.

S Commodorem jsem řešil totéž co s Atari. Jasně, existuje [IDE64](https://www.ide64.org/ide-faq.html), ale já bych rád tu SD kartu. Commodore 64 má IEC, což je bratranec Ataráckého SIO, tedy zase nějaké sériové rozhraní, a k němu existuje [SD2IEC](https://www.sd2iec.co.uk/id14.html), což je miniaturní krabička, do které zastrčíte SD kartu a pomocí dvou konektorů ji připojíte k sériovému výstupu a k rozhraní pro kazeťák. SD2IEC pak simuluje disk, dokonce umí dělat i CD a podobné věci ([i když ne úplně intuitivně](https://www.sd2iec.co.uk/id6.html)), jen je s tím trošičku opruz, protože to emuluje mechaniku 1541 naprosto věrně, tedy včetně její neskutečné pomalosti. Naštěstí existuje [zrychlovač a další triky](https://ilesj.wordpress.com/2010/10/04/tips-for-using-sd2iec/).  [Zdrojáky firmwaru SD2IEC najdete zde](https://www.sd2iec.de/). Zajímavé informace má i [Enhanced C64 DOS support](https://www.atarimagazines.com/compute/issue54/170_1_Enhanced_Commodore_64_DOS_Support.php).

Analogicky k Atari i u C64 existuje [Uno2IEC](https://github.com/Larswad/uno2iec/wiki/About-Uno2IEC,-the-Arduino-1541-emulator-Wiki-and-HowTo), což je emulátor disketové mechaniky postavený z Arduina.

A protože mám i pár kazet k C64, tak jsem si je &#8222;přepípal&#8220; do podoby PRG a T64 souborů pomocí utility [WAV-PRG](https://wav-prg.sourceforge.net/) (oproti té spektrácké jí nedělá problém jet on-the-fly a člověk nemusí laborovat s filtrama). Kdybych někdy chtěl, tak můžu [spojit C64 s PC po sériovém rozhraní](https://www.zimmers.net/anonftp/pub/cbm/crossplatform/transfer/C2N232/index.html). Případně použít alternativu [XU1541](https://www.trikaliotis.net/xu1541). K celému tomu cirkusu okolo IEC jsem našel i [dokumentaci](https://jderogee.tripod.com/projects/1541-III/files/IEC_disected-IEC_1541_info.pdf).

Až mě popadne pejcha a budu chtít používat GEOS, tak určitě využiju [stránku o GEOSu na C64](https://lyonlabs.org/commodore/onrequest/geos.html), [driver GEOS SD2IEC](https://www.lemon64.com/forum/viewtopic.php?t=43579&sid=e792496449c2ebb2cb9d00a1dcd41db7) i video &#8222;[jak přehazovat disketové obrazy na SD2IEC pod GEOSem](https://www.youtube.com/watch?v=1nmglNb3j-I)&#8222;. Ale zatím je pravděpodobnější, že dřív budu zkoušet starou dobrou CP/M na C128, a pak se mi bude hodit [tohle](https://www.lemon64.com/forum/viewtopic.php?t=47563&sid=33a8bd28dcbf7591718b1d3b6e7e6027).

Spousta perel se skrývá na webu [H2Obsession](https://sites.google.com/site/h2obsession/CBM). Od informací přes schémata až k obrazům disků. Literaturu pro Commodory jsem našel na [Bombjack.org](https://www.bombjack.org/commodore/) a naprosto neskutečnou [sbírku starých knížek o Commodorech v PDF na Scribdu](https://www.scribd.com/collections/3021875/Commodore-Computer-Books). Další informace pak na <a style="font-weight: inherit; font-style: inherit; color: #5eb2e5;" href="https://codebase64.org/">Codebase64.org</a>, diskuse na <a style="font-weight: inherit; font-style: inherit; color: #5eb2e5;" href="https://www.lemon64.com/">Lemon64.com</a>, novinky ze scény na <a style="font-weight: inherit; font-style: inherit; color: #5eb2e5;" href="https://csdb.dk/">CSDb</a> a na [Fridge](https://www.ffd2.com/fridge/).

Hezký víkend se starými počítači všem&#8230;