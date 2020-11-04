---
id: 1181
title: Nad dotazy čtenářů
date: 2020-03-01T15:25:49+01:00
author: Martin Maly
layout: revision
guid: https://retrocip.cz/1178-revision-v1/
permalink: /1178-revision-v1/
---
Proč se nepoužíval obvod 8051 v domácích mikropočítačích? A proč nepůjde připojit ZX Spectrum k [modernímu zobrazovači](https://retrocip.cz/monitor-ke-starym-pocitacum-za-par-korun/) bez úprav?

Občas mi vy, čtenáři, píšete zajímavé dotazy a já si říkám, že je škoda odpovídat jen mailem jednomu, protože to může zajímat víc lidí. Zkusím tedy nepravidelný formát takovýchto veřejných odpovědí.

První dotaz poslal pan Tomáš:

<blockquote class="wp-block-quote">
  <p>
    zaujal mě váš článek <a href="https://retrocip.cz/monitor-ke-starym-pocitacum-za-par-korun/">https://retrocip.cz/monitor-ke-starym-pocitacum-za-par-korun/</a>, kde vysvitla nová naděje na připojení parkovacích kamerek k mému ZX81.
  </p>
  
  <p>
    Otázka je, zde by to fungovalo (obecně tyhle krámy typu TFT monitor s PAL vstupem) na <em>neupravených</em> ZX 80/81/Spectrum, tzn. tak jak jsou, bez předělávek na s-video apod.<br /><br />Předem díky za názor / radu, moc by mi to pomohlo.
  </p>
</blockquote><figure class="wp-block-image size-large">

<img loading="lazy" width="1024" height="576" src="http://retrocip.cz/wp-content/uploads/sites/6/2017/11/23550861_10155203516922496_8283049223022676107_o-1024x576.jpg" alt="" class="wp-image-997" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2017/11/23550861_10155203516922496_8283049223022676107_o-1024x576.jpg 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2017/11/23550861_10155203516922496_8283049223022676107_o-650x366.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2017/11/23550861_10155203516922496_8283049223022676107_o-768x432.jpg 768w, https://retrocip.cz/wp-content/uploads/sites/6/2017/11/23550861_10155203516922496_8283049223022676107_o.jpg 1440w" sizes="(max-width: 1024px) 100vw, 1024px" /> </figure> 

Odpověď je jednoduchá, ale nepotěší:

Tyhle monitory očekávají vstup jako standardní video v normě PAL. ZX Spectrum i ZX81 bez úpravy(!) pouštějí ven sice videosignál PAL, ale namodulovaný na televizní přenosovou frekvenci, aby simuloval signál, přijatý z antény. 

Takový signál se připojí k anténnímu vstupu, z něj jde do vstupního filtru, kde musí být naladěn kanál se stejnou přenosovou frekvencí, a pokud tomu tak je, tak projde dál, a tam z něj televize rekonstruuje zpátky PAL.

Což je taky důvod, proč obraz není tak kvalitní &#8211; protože se moduluje nejprve na 36. kanál UHF (603.25 &#8211; 607.75 MHz), s touto frekvencí se přenáší po kabelu do televize, a tam se zase demoduluje zpět na původní PAL, a právě tento proces způsobuje ztráty a rušení.

PAL monitor chce právě ten videosignál. Odstraňuje celý složitý řetězec modulace a demodulace, ale musíte si ze ZXS/ZX81 vyvést ten nemodulovaný signál. Složité to není, ale je to, přeci jen, nějaký zásah&#8230;

Pro zájemce: Zde je [návod na vyvedení videa ze ZX81](https://www.classic-computers.org.nz/blog/2016-01-03-composite-video-for-zx81.htm), popřípadě [zde, pokud máte ZX80 či Timex](https://www.bytedelight.com/?page_id=3560). Je to o něco náročnější úprava, protože je potřeba videosignál trochu zesílit tranzistorem.

U ZX Spectra se jednoduše odpojí modulátor a na anténní výstup se přivádí přímo videovýstup. Návod krok za krokem je třeba [tady (ZX Spectrum Composite Mod)](http://www.retrogamescollector.com/simple-zx-spectrum-composite-mod/), nebo [tady (ZXS 48k video)](https://spectrumforeveryone.com/technical/composite-mod-for-the-48k-range/). Sice je anténní vývod trochu jiný než standardní RCA konektor pro video (&#8222;CINCH&#8220;), ale použít ho lze. Nebo můžete druhý konektor prostě přidat:<figure class="wp-block-image size-large">

<img loading="lazy" width="342" height="210" src="https://retrocip.cz/wp-content/uploads/sites/6/2020/03/spectrum-video2.jpg" alt="" class="wp-image-1179" /> </figure> 

Druhý dotaz poslal pan Jan:

<blockquote class="wp-block-quote">
  <p>
    proč se do jednodeskových počítačů nepoužíval mikrokontroler 8051? Formálně se jedná o Hardvardskou architekturu, ale i v návodu se píše, že je možno použít jednu RAM na kód i data spojením signálů /PSEN a /RD. Domníval jsem se, že bych získal von Neumannovský procesor s obdobnou instrukční sadou a rychlostí jako zmiňovaná 8080, který má (může mít) navíc on-chip ROM pro monitor, serial port, časovače, a jeden volný I/O port. V podstatě celý Omen Alpha na jednom čipu (krom paměti). Přišel 8051 na jednodeskové osmibity příliš pozdě? Byl příliš drahý? Nebyly dostupné dostatečně rychlé nebo velké RAM paměti? Nebo je tam nějaká jiná zrada, která takové využití znemožňuje?
  </p>
</blockquote><figure class="wp-block-image size-large">

<img loading="lazy" width="1024" height="504" src="https://retrocip.cz/wp-content/uploads/sites/6/2020/03/KL_Intel_P8051-1024x504.png" alt="" class="wp-image-1180" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2020/03/KL_Intel_P8051-1024x504.png 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2020/03/KL_Intel_P8051-650x320.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2020/03/KL_Intel_P8051-768x378.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2020/03/KL_Intel_P8051.png 1197w" sizes="(max-width: 1024px) 100vw, 1024px" /> </figure> 

V první řadě přišel x51 příliš pozdě (až v roce 1980), takže v relevantní době už měli konstruktéři naučené postupy s obyčejnými CPU. V té době byly dostupné nejen 8080, 6502 nebo Z80, ale i 8086 (1978) nebo 8088 (1979). Ve stejném roce jako 8088 přišla i Motorola 68000. Takže podle mě nejpravděpodobnější důvod je ten, že řada x51 přišla příliš pozdě na to, aby zasáhla do dějin osmibitů.

Ne že by žádné mikropočítače takového typu neexistovaly &#8211; [existovaly, ale bylo jich výrazně méně](https://www.root.cz/clanky/mikroradice-a-jejich-aplikace-v-nbsp-jednoduchych-mikropocitacich-4/). Docela často se x51 používal třeba v herních konzolích.

Ale největší nevýhoda a hlavní důvod, proč se x51 nerozšířil v domácích počítačích, byl ten, že sice umožňoval práci s externí pamětí, ale nebyly instrukce, které by ji dokázaly využívat jako operační. U 6800 / 6502 / Z80 atd. byl jen jeden paměťový prostor. Mikrořadiče x51 pracovaly přímo jen s vnitřní RAM a s bohatou sadou registrů, ale pro přístup k externí paměti měly jen instrukci MOVX (v popsaném případě by šla použít i MOVC) s nepřímým adresováním, srovnatelným např. s tím, že byste u procesoru 8080 měli pro práci s obsahem paměti pouze instrukce MOV M,x a MOV x,M.