---
id: 376
title: Symfonie na jednom bitu
date: 2014-08-10T17:20:40+01:00
author: Martin Maly
layout: post
guid: https://retrocip.uelectronics.info/?p=376
permalink: /symfonie-na-jednom-bitu/
dsq_thread_id:
  - "2915801443"
xyz_lnap:
  - "1"
image: https://retrocip.cz/wp-content/uploads/sites/6/2014/08/557px-Waveforms.svg_-557x198.png
categories:
  - Hardware
tags:
  - Spectrum
  - Z80
  - zvuk
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Trocha_teorie_na_uvod8230_Jo_je_to_nezbytne"><span class="toc_number toc_depth_1">1</span> Trocha teorie na úvod&#8230; Jo, je to nezbytné!</a>
    </li>
    <li>
      <a href="#Obdelnik_vladne_vsem"><span class="toc_number toc_depth_1">2</span> Obdélník vládne všem</a>
    </li>
    <li>
      <a href="#Delime_frekvence"><span class="toc_number toc_depth_1">3</span> Dělíme frekvence</a>
    </li>
    <li>
      <a href="#Vicehlas"><span class="toc_number toc_depth_1">4</span> Vícehlas</a>
    </li>
    <li>
      <a href="#Priste"><span class="toc_number toc_depth_1">5</span> Příště</a>
    </li>
    <li>
      <a href="#Ke_hrani"><span class="toc_number toc_depth_1">6</span> Ke hraní</a>
    </li>
    <li>
      <a href="#Ke_cteni_a_inspiraci"><span class="toc_number toc_depth_1">7</span> Ke čtení a inspiraci</a>
    </li>
  </ul>
</div>

Kdo by neznal jednobitovou muziku! Tedy vlastně, hm, všichni Ataristi a Commodoristi, jejichž počítače měly mnohakanálové zvukové generátory. To my na Spectru jsme si museli vystačit s jedním drátem, kde byla buď 0, nebo 1, a podle toho, jak rychle se s ním (programově) vrklalo, tak takový zvuk to dělalo&#8230;

<!--more-->

Tak samozřejmě nebyla to žádná extra sláva, ale šlo to, a když se tomu trošku dalo, tak byly výsledky opravdu zajímavé. Jak se tomu _dalo dát_?

## <span id="Trocha_teorie_na_uvod8230_Jo_je_to_nezbytne">Trocha teorie na úvod&#8230; Jo, je to nezbytné!</span>

**Zvuk** je, jak známo, vlnění, přenášené vzduchem nebo jiným médiem. Jako základní typ vlnění se bere sinusoida.

**Tón** je základní stavební jednotkou hudby. Jeho nejdůležitějším parametrem je frekvence, udávaná v hertzech, neboli &#8222;počet cyklů za sekundu&#8220;.

**Komorní A** je tón, který má většinou frekvenci 440 Hz. (Proč většinou? Protože [jsou ladění](https://jlswbs.wordpress.com/2009/01/15/komorni-a-jeho-ladeni/), které mají komorní A někde jinde, např. na 442 Hz, Händel používal třeba i 409 Hz, La Scala v 18. století zase měla komorní A na frekvenci 451 Hz&#8230;)

**Oktáva** je podle klasických hudebních teoretiků interval osmi tónů. [Podle Franty Fuky](https://www.fffilm.name/2012/02/hudebnim-skladatelem-snadno-rychle.html) to je 12 půltónů a vřele doporučuju si jeho článek přečíst. Pokud to z jakéhokoli důvodu neuděláte, tak aspoň vězte, že &#8222;o oktávu vyšší tón&#8220; má dvojnásobnou frekvenci v Hz. Pokud má komorní A frekvenci 440 Hz, tak o oktávu vyšší A bude mít 880 Hz. Vzhledem k tomu, že frekvence vibrace je nepřímo úměrná délce vibrujícího předmětu, tak by mohlo platit, že struna délky L vydává nějaký tón a struna délky L/2 vydává tón o oktávu vyšší. Máte-li kytaru, račte si přeměřit vzdálenosti od pražce ke kobylce a ověřit, jestli pražec v poloviční vzdálenosti vyvolá o oktávu vyšší tón.

**Samplování**_ _je postup, při kterém ze spojitého průběhu, třeba té sinusovky, uděláme nespojitý. Děje se to tak, že v pravidelných intervalech bereme vzorek (_sample_) aktuální hodnoty jako číslo a ukládáme si ho. Pokud pak ve stejných intervalech nastavíme výstup na tuto hodnotu, dostaneme průběh, který s určitou mírou tolerance odpovídá původnímu.

**Samplovací frekvence** je frekvence, s jakou vzorkujeme. Pro nás u jednobitové muziky to bude frekvence, s jakou jsme schopni změnit hodnotu na výstupu. Z čehož vyplývá, že pokud budeme s touto rychlostí měnit jedničky a nuly, dostaneme obdélníkový průběh o poloviční frekvenci &#8211; a to bude zároveň maximální dosažitelná frekvence hraní. (Čímž jsme si, mimo jiné, hezky odvodili [Shannonův, též Nyquistův, teorém](https://cs.wikipedia.org/wiki/Shannon%C5%AFv_teor%C3%A9m)).

**Sinusoida** je základní zvukový průběh. Naštěstí žádný nástroj nemá zvuk, který by byl přesně sinusový, což je dobře, protože sinusoida je velmi plochý, tupý a bezbarvý zvuk.

**Amplituda** je pro nás totéž co hlasitost.Čím větší jsou výchylky vln od neutrální hodnoty, tím hlasitější zvuk vnímáme. Ucho vnímá jen absolutní odchylku, nerozlišuje, jakým je (ta odchylka) směrem, jestli kladným, nebo záporným (v případě přenosu zvuku vzduchem: nerozlišuje, jestli přišlo zředění nebo zhuštění, vnímá jen rozdíl oproti normálu).

**Barva zvuku** je dána v reálném světě stavbou nástroje, materiálem, ozvučnicí &#8211; každý z těchto faktorů přidá k základnímu tónu nějakou další složku, jiné chvění s jinou intenzitou, čímž dodá zvuku charakteristické zabarvení. U složitých hudebních nástrojů rezonují různé součástky na různých frekvencích a kombinace těchto rezonančních efektů je unikátní a pro daný nástroj charakteristická. Jednoduché nástroje mají jednoduchý zvuk &#8211; například nejrůznější píšťaly. Složité nástroje, třeba takové, kde vzniká tón například drnkáním, mají zvuk složený z nejrůznějších frekvencí, které v čase mění svou amplitudu&#8230;

**Pila** (sawtooth) je průběh zvuku, který vypadá &#8211; inu, jako pila. Je zvláštní, že pilu můžeme snadno sestrojit ze sinusovek &#8211; vezmeme tu základní, třeba 440, k ní přidáme dvakrát rychlejší (880) s poloviční amplitudou, k tomu třikrát rychlejší (1320) s třetinovou amplitudou&#8230; A když to uděláme donekonečna, dostaneme pilový průběh.

**Harmonická** frekvence je taková, jejíž velikost je nějakým celočíselným násobkem základní frekvence. První harmonická je rovna základní frekvenci, druhá harmonická dvojnásobné, třetí trojnásobné&#8230; Předchozí definici pily bychom mohli zjednodušit tak, že to je součet všech harmonických (první, druhé, třetí, &#8230;) s lineárně klesající amplitudou. Viz též [ladění](https://cs.wikipedia.org/wiki/Lad%C4%9Bn%C3%AD).

**Trojúhelník** je další oblíbený průběh, který uvádím vlastně jen tak do počtu. Stejně jako pilu lze zkonstruovat i trojúhelník součtem harmonických &#8211; tentokrát lichých harmonických (1., 3., 5., &#8230;) s exponenciálně klesající hlasitostí.

**Obdélník** je poslední ze svaté čtveřice základních průběhů. Namísto plynulého klesání nebo stoupání má pouze dvě hodnoty &#8211; maximum a minimum. Konstruuje se stejně jako trojúhelník, tedy součtem lichých harmonických, ale hlasitost tentokrát neklesá exponeniálně, tedy s druhou mocninou, ale lineárně.

**Šum** je náhodný průběh.<figure id="attachment\_379" aria-labelledby="figcaption\_attachment_379" class="wp-caption aligncenter" style="width: 567px">

<img loading="lazy" class="wp-image-379 size-full" src="https://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/08/557px-Waveforms.svg_.png" alt="557px-Waveforms.svg" width="557" height="480" /> <figcaption id="figcaption\_attachment\_379" class="wp-caption-text">Zdroj: https://en.wikipedia.org/wiki/File:Waveforms.svg</figcaption></figure> 

To rozkládání základních periodických funkcí na součty sinusovek je samo o sobě velmi zajímavá část matematiky, a pokud by vás to zajímalo víc, tak klíčová slova jsou [Fourierova transformace](https://cs.wikipedia.org/wiki/Fourierova_transformace) a Fourierovy řady.<figure class="wp-caption aligncenter" style="width: 510px">

<img loading="lazy" src="https://upload.wikimedia.org/wikipedia/commons/0/0a/Synthesis_square.gif" alt="" width="500" height="250" /> <figcaption class="wp-caption-text">Zdroj: https://cs.wikibooks.org/wiki/Soubor:Synthesis_square.gif</figcaption></figure> 

## <span id="Obdelnik_vladne_vsem">Obdélník vládne všem</span>

Jednobitová hudba je generována, jak buď ze zkušenosti víte nebo si domyslíte, páč jste bystří, tím, že měníme hodnotu jediného bitu &#8211; buď je 0, nebo 1. Výsledkem je tedy, když se zamyslíme, obdélníkový průběh. On nakonec nebude úplně přesně obdélníkový, protože na cestě od procesoru k zesilovači jsou ještě různé obvody, které ten obdélník trošičku zdeformují, ale to není teď pro nás podstatné. Podstatné je, že můžeme zapomenout na sinusoidy a trojúhelníky, protože vygenerujeme vždy a pouze obdélník.

Když chceme zahrát třeba tón o frekvenci 1000 Hz, bude to znamenat, že tisíckrát za sekundu (=každou milisekundu) musí proběhnout oba cykly, tedy 1 a 0. Nejjednodušší tedy bude, když necháme půl milisekundy logickou 1, další půl milisekundy logickou 0, dohromady to dá jednu milisekundu, a když to budeme opakovat, dostaneme tisícihertzový obdélník jako víno!

Když je polovinu doby 1 a polovinu 0, říkáme tomu, že signál má střídu 1:1 (též 50%). Stejnou dobu je zapnuto, jako vypnuto. Spectrum 48 nám takový signál vygeneruje na požádání &#8211; ano, je to to, co zahraje BEEP.

Co když ale zmenšíme dobu, po kterou máme tu logickou 1, o polovinu? Výsledek bude tedy mít střídu 1:3 (25%, tj. čtvrtinu doby 1, tři čtvrtiny 0) a zvuk bude znít jinak, bude ostřejší &#8211; i když ho stále budeme vnímat jako &#8222;stejně vysoký&#8220;. S šířkou střídy pracuje například _hradlo_ Jonathana Smitha, použité v [Ping Pongu](https://www.youtube.com/watch?v=Qq4Uq6Ae4tc).

Zvláštní, takovou _bzučivou_, hudbu měl třeba [Heartland](https://www.youtube.com/watch?v=mRINk7kTnp8) nebo hry od Special FX ([Firefly](https://www.youtube.com/watch?v=C_hG8ot3Zn4)). BTW, tu rutinu pro Special FX napsal opět Jonathan Smith (před několika lety zemřel) a použitá byla i v známém editoru [Orfeus](https://cs.wikipedia.org/wiki/Orfeus_Music_Assembler). Má střídu 1:N &#8211; tedy pouze na začátku cyklu na malou chvíli zapneme log. 1 a pak je logická 0 až do konce požadované doby. (Ta _malá chvíle_ je dána samplovací frekvencí, tedy rychlostí, s jakou jsme schopni v naší rutině přepínat 1 a 0.)

## <span id="Delime_frekvence">Dělíme frekvence</span>

Jak tedy budeme hrát jednotlivé tóny? Když chci zahrát 440 Hz, co to přesně znamená?

Tak dejme tomu, že máme Spectrum 48, jehož základní taktovací frekvencí je 3,5 MHz. Tedy tři a půl milionu hertzů. Známé T &#8211; doba jednoho cyklu hodin &#8211; je tedy 0,285714 mikrosekundy.

Frekvence tónu _f_ je 440. Za jednu sekundu proběhne tedy 440 cyklů, to znamená, že na jeden cyklus vyjde&#8230; 3500000/_f_ = 7954,545454 cyklů T.

A protože jste programátoři assembleroví, tak si tu nebudeme vysvětlovat, jak se počítá, kolik která instrukce zabere T, stačí vědět, že když každých zhruba 3977 T změníme výstup z log. 0 na log. 1 (resp. obráceně), bude nám Spectrum hrát krásné obdélníkové komorní A.

Jenže my nemůžeme měnit výstup s frekvencí hodinového signálu &#8211; nejrychlejší instrukce má 4 takty, navíc my potřebujeme OUT, a ten má aspoň 11 taktů, k tomu nějaké počítání&#8230; No, dejme tomu, že jeden průběh smyčkou našeho hracího programu, tedy tou částí, kde se rozhodne, jestli teď 0 nebo 1, zabere třeba 120 T. Prostým podělením 3500000/120 zjistíme, že tahle smyčka proběhne zhruba 29166x za sekundu, což nám dává samplovací frekvenci cca 29kHz. Když budeme pravidelně střídat 1 a 0, vygenerujeme tón o frekvenci 14,5kHz &#8211; to je docela vysoké pištění. Ultrazvuk to není, ale je to hodně vysoké a člověk by to měl slyšet (horní hranice slyšitelné frekvence je okolo 20 kHz, v mládí víc, k stáru se zhoršuje).

Hrací smyčka funguje v zásadě &#8211; a hrubě zjednodušeně &#8211; tak, že počítá průběhy, tj. kolikrát proběhla. Když je dosaženo hodnoty D (dělitel), jede se zase od nuly, protože to znamená nový průběh. Během té doby je tedy potřeba změnit výstup podle požadovaného průběhu. Příklad: pro &#8222;heartlandovský&#8220; průběh 1:N bude na výstupu log. 1 pouze když je počítadlo rovno 0, pak bude na výstupu log. 0. Logická jednička bude na výstupu tedy po dobu jednoho průběhu, 120T.

Kolik je tedy dělitel? Je to v zásadě převrácená hodnota frekvence (1/_f_) a vzorec je: **D = M / _f_ / Tcyc**. M je frekvence hodin (3500000), _f_ je požadovaná frekvence tónu, Tcyc je trvání smyčky v taktech hodin (T), a výsledkem je číslo, které udává, kolik průchodů smyčkou připadá na jeden cyklus výsledného tónu. Pro komorní A nám to vychází 66,287878&#8230;, a protože jsme na Spectru a máme registry celočíselné, tak 66. Tón o oktávu vyšší bude mít dělitele 33 (čím menší dělitel, tím vyšší tón), o oktávu nižší pak 133 (zaokrouhleně).

Jaká je maximální a minimální frekvence našeho hypotetického hradla? Maximální frekvence je taková, která se ozve s dělitelem 2 (pokud by byl 1, hodnota na výstupu by se neměnila) &#8211; po dosazení do vzorce vychází 14,6 kHz. Minimální frekvence bude, při osmibitovém počitadle, 114 Hz.

Teď už zbývá jen spočítat hodnoty pro jednotlivé půltóny. Podíl frekvencí dvou sousedních tónů je dvanáctá odmocnina ze dvou (neptejte se mě proč a [přečtěte si toho Fuku](https://www.fffilm.name/2012/02/hudebnim-skladatelem-snadno-rychle.html)) &#8211; naštěstí si to nemusíte počítat a můžete využít [tabulky](https://www.phy.mtu.edu/~suits/notefreqs.html). (Z ní se taky dozvíme, že naše nejnižší frekvence je cca tón B2, nejvyšší pak zhruba A9.) Dostaneme tabulku dělitelů &#8211; nějak [takhle](https://docs.google.com/spreadsheets/d/1fSbAVHtOm1pXRo90QyolqM66l7d_hT51omESPoz3Ack/pubhtml).

Vzhledem k tomu, že používáme celočíselné operace, tak nebudou frekvence přesné a v místech, kde máme hodně tónů na málo hodnot (tedy u vysokých tónů) bude už velmi slyšitelné rozladění. Ostatně už v sedmé oktávě připadá na jednoho dělitele několik tónů a šestá bude taky slyšitelně rozladěná.

## <span id="Vicehlas">Vícehlas</span>

Probrali jsme si teorii jednoho tónu. Jenže jednobitová hudba dokáže hrát i víc tónů naráz, šílenci jako Tim Follin třeba i [tři](https://www.youtube.com/watch?v=j6MmtyN36bE), nebo [pět](https://www.youtube.com/watch?v=T42WuUpBuHE). Jak se to dělá?

Tak, buď můžete rychle měnit frekvence tónů a hrát chvilku C a chvilku E a takhle to střídat, podobně jako to třeba dělá Fuka v prvním Jonesovi. Anebo můžete oba tóny skládat dohromady &#8211; tj. ve smyčce máte dvě počítadla, každé pro jeden tón, takže vám vznikají dva průběhy, a výsledek je pak OR nebo AND těchto průběhů. (U střídy 1:N nemá AND smysl.)

Takhle jednoduché že by to bylo? No ano, je to tak. Teoreticky. V praxi narazíte na spoustu zádrhelů. Například na to, že když použijete generátor se střídou 1:N a zahrajete dva tóny, které jsou od sebe přesně o oktávu, tak vám impulsy toho vyššího splynou s impulsy toho nižšího a vy uslyšíte jen ten vyšší (tím trpí například hradlo [huby](https://shiru.untergrund.net/files/zx/huby.zip)). Řešit to lze &#8211; buď jeden tón decentně rozladíte, tj. posunete jeho frekvenci o kousíček jinam, např. přičtete k děliteli 1 (což dělejte s tím nižším tónem), nebo mu posunete fázi (tj. nebudete posílat log. 1 ve chvíli, kdy je počítadlo rovno 0, ale třeba 4). Tím sice docílíte toho, že budou znít oba, ale navíc si přidáte do výsledku parazitní frekvence. Na druhou stranu &#8211; v tom obdélníkovém šumu se to ztratí&#8230;

Nebo že u jiných stříd (1:1 například) zní dva tóny jako by měly poloviční hlasitost proti jednomu tónu. Mnojo&#8230;

Ještě takový detail &#8211; když u střídy 1:N měníme tu první hodnotu (tedy 2:N, 3:N, 4:N), tak do určité meze můžeme simulovat změnu hlasitosti. Pokud ale zvolíme první číslo příliš velké, tak se u vyšších frekvencí změní střída na něco blízké 1:1 &#8211; a zase máme jiný problém.

Když budou hrát dva tóny, jen mírně rozladěné, vzniknou ve výsledném průběhu takzvané [zázněje](https://cs.wikipedia.org/wiki/Z%C3%A1zn%C4%9Bj). Je to tím, že při hraní dvou frekvencí naráz vnímáme i třetí, která je rovna jejich rozdílu &#8211; toho lze taky využít při generování zvuků, ale to si nechme až na příště.

## <span id="Priste">Příště</span>

Co jsme ještě neprobrali? Třeba jak se dělají perkuse, tj. bicí (pomocí šumu a skluzů). Jak se dělají zvukové efekty (glissando, tremolo, ornamenty &#8211; arpeggia). Co je to detune a co fáze. K čemu je obálka. Jaká jsou k dispozici hradla, editory&#8230; a jak která fungují. A možná i spoustu dalších věcí &#8211; a protože vím, že mezi čtenáři jsou zkušení skladatelé jednobitových symfonií, tak je tentokrát poprosím: přispějte tématem, radou, poznámkou&#8230; Děkuju!

## <span id="Ke_hrani">Ke hraní</span>

No ano, mám pro vás hračku&#8230;



Co s tím? Funguje to samozřejmě zase jen v novějších Chrome a FF (takže mi nepište, že to ve _vašem_ prohlížeči nedělá nic, já to vím a je mi to jedno). Hrajeme si s tónem C. Nahoře je tlačítko MUTE, které vypne ten protivný tón. Vedle toho jsou tři základní průběhy: Sinusovka, trojúhelník, pila. Pod tím obdélník se střídou 1:1 &#8211; jeden tón, dva tóny (C a E) pomocí OR a totéž pomocí AND. Pak totéž pro obdélník se střídou 1:3. Pak následuje generátor se střídou 1:N a dvojtón (pomocí OR, AND nemá smysl). A pak pro srovnání je totéž se střídou 4:N &#8211; všimněte si, že tón zní hlasitěji. Pak jsou ukázky dvou tónů se střídou 4:N a kombinace tónu 1:N a 4:N

## <span id="Ke_cteni_a_inspiraci">Ke čtení a inspiraci</span>

  * <https://1bit.i-demo.pl/forum/2/zx-spectrum-48k-timex-2048-music/>
  * <https://shiru.untergrund.net/1bit/>
  * <https://sysel.webz.cz/sound/zx10.html>
  * <https://www.gurujoe.sk/2010/12/jeden-kanal-jeden-bit.html>