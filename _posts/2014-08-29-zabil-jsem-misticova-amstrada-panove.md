---
id: 392
title: Zabil jsem Misticova Amstrada, pánové!
date: 2014-08-29T19:42:53+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=392
permalink: /zabil-jsem-misticova-amstrada-panove/
dsq_thread_id:
  - "2970535291"
image: https://retrocip.cz/wp-content/uploads/sites/6/2014/05/IMG_20140503_092822-1140x198.jpg
categories:
  - Hardware
tags:
  - Amstrad
  - CPC
  - GOTEK
  - Z80
---
Vůbec jsem se s tím nemazal a prostě jsem to tam nacpal!

<!--more-->

Když jsem si před pár měsíci jel pro CPC6128, zjistil jsem, že člověk, od kterého ho mám, má přezdívku Mistic JOE a je [v komunitě známý](https://textovky.panprase.cz/index.php?topic=183.0).

Jeho, tedy teď už moje, CPC6128 je upravené. Má zabudovaný ParaDOS1.1 a místo původní 3&#8243; mechaniky má 3.5&#8243; standardní PC. Nojo, jenže! Komu by se chtělo crcat s disketama?! Hledal jsem tedy něco na SD nebo tak, a shodou okolností jsem narazil na [článek](https://www.8bity.cz/2014/gotek-usb-floppy-emulator-pro-amigu/) o emulátorech GOTEK.

Od slov nejdu daleko k činům. Emulátor jsem pořídil, chvilku si s ním hrál a přemýšlel, do kterého počítače ho vrznu. Nakonec vyhrál Amstrad. Chvíle laborování, hm, hm, hm, to nepůjde, _vymontuj to a udělej ACH JO&#8230;_ Ach jo!

Ale po chvíli hledání jsem objevil, že [to jde](https://www.cpcwiki.eu/forum/amstrad-cpc-hardware/gotek-usb-in-a-cpc6128/), a je k tomu i [video](https://www.youtube.com/watch?v=XRTNlZ76nJg). V zásadě ten největší problém je, že musíte mít Gotek ve verzi 720k. Tedy [SFRM72-FU-DL, k zakoupení zde](https://rover.ebay.com/rover/1/711-53200-19255-0/1?icep_ff3=9&pub=5575085282&toolid=10001&campid=5337563928&customid=&icep_uq=sfrm72-fu-dl&icep_sellerId=&icep_ex_kw=&icep_sortBy=12&icep_catId=&icep_minPrice=&icep_maxPrice=&ipn=psmain&icep_vectorid=229466&kwid=902099&mtid=824&kw=lg). Pak už je to prosté. Teda, podle návodu&#8230;

Přišel Gotek 720, já ho zapojil místo Misticovy třiapůlky, a nic. Postup je následující: Pomocí utility [DSKIMA](https://mega.co.nz/#!bQVFzCAL!OH_5ERMYTXALRupPjD1RCXmIykB8gOl4JxUSBZ2ZZ3M) ([mirror](https://download.hellshare.cz/dskima-zip/24398073/)) si převedete .dsk soubory na soubory .IMG a ty si pojmenujete 000.IMG, 001.IMG, 002.IMG atakdál, až 099.IMG. Na flashce (FAT32, max 2GB) si vytvoříte adresář IMG720 a do něj si tyhle images nahrajete. Na GOTEKu pak mačkáte obě tlačítka najednou tak dlouho, dokud se na displeji neobjeví &#8222;b00&#8220;. Pak si levým a pravým vyberete číslo image (levé dělá desítky, pravé jednotky). Dejme tomu &#8222;b05&#8220;. Pak zasunete flashku, stisknete pravé tlačítko, ono to chvilku pracuje, a voila, máte simulovanou disketu!

Jenže já dal &#8222;cat&#8220;, a nic. Respektive &#8222;retry, ignore, cancel&#8220;. Takže jsem dál experimentoval, laboroval, a zjistil, že jumpery musí být nastaveny na JA a S1. Pak už to hlásilo aspoň existující disketu, ale ve výpise byla horda nesmyslů.

A tak jsem experimentoval dál, protože mi bylo jasné, že když to jde jednomu, půjde to i mně. Všiml jsem si, že používá ParaDOSí utilitu (|drive) a nastavuje formát disku na IBM. Pak stroj resetuje, a potom mu to jde. Hm, hm&#8230; Zkusil jsem to, a ta utilita po nastavení parametrů dokázala načíst obsah emulovaného disku.

Teď už by to chtělo jen RESET a &#8230; ale ten já nemám! [Ten až někdy](https://c1.staticflickr.com/7/6114/6237699864_ec28ce740b_z.jpg). Jak tedy zařídit, aby si CPC pamatovalo to nastavení? (A až po napsání tohodle článku jsem přišel na to, že by měla fungovat kombinace \[CTRL\]\[SHIFT\][ESC] &#8211; a to jsem to předtím hledal!)

Long story short &#8211; po chvíli pátrání jsem objevil [užitečné vlákno](https://www.cpcwiki.eu/forum/applications/parados-question!/), a z něho vyextrahoval myšlenku. Pak jsem zaexperimentoval, a zjistil jsem, že&#8230;

&#8230; stačí udělat &#8222;poke&bafe,4&#8220; a &#8222;poke&baff,3&#8220;, a pak cat funguje jak má!

Někdy to vypálím do té ROMky (na adresy &1c00 a &1c03), ale to má čas. Teď je zapotřebí natahat všechny diskety na tu FLASHku a udělat si pořádný seznam, kde bude napsáno, jaké číslo má jaká disketa!