---
id: 1053
title: 'Alpha: Koncepce počítače s procesorem 8080/8085'
date: 2018-05-07T11:21:16+01:00
author: Martin Maly
layout: post
guid: https://retrocip.cz/?p=1053
permalink: /alpha-koncepce-pocitace-s-procesorem-8080-8085/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "6656666378"
image: https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-timing-748x198.png
categories:
  - Hardware
tags:
  - "8085"
  - OMEN Alpha
---
<div id="toc_container" class="toc_wrap_right no_bullets">
  <p class="toc_title">
    Obsah
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Vlastni_pocitac_Jak_by_mohl_vypadat"><span class="toc_number toc_depth_1">1</span> Vlastní počítač? Jak by mohl vypadat?</a>
    </li>
    <li>
      <a href="#Vyvody_8085"><span class="toc_number toc_depth_1">2</span> Vývody 8085</a>
    </li>
    <li>
      <a href="#Vnitrni_struktura_8085"><span class="toc_number toc_depth_1">3</span> Vnitřní struktura 8085</a>
    </li>
    <li>
      <a href="#Preruseni"><span class="toc_number toc_depth_1">4</span> Přerušení</a>
    </li>
    <li>
      <a href="#K_dalsimu_cteni"><span class="toc_number toc_depth_1">5</span> K dalšímu čtení</a>
    </li>
  </ul>
</div>

> <p data-key="125662">
>   Připravuju pokračování své knihy Hradla, volty, jednočipy. Tentokrát se budu zabývat konstrukcí osmibitových počítačů. Kniha by měla vyjít v roce 2019 opět v Edici CZ.NIC.
> </p>
> 
> <p data-key="125662">
>   Když jsem přemýšlel o tom, jak knihu pojmout, bylo mi jasné, že suchý výklad nebude stačit. Tedy, on by stačil, ale já nejsem zrovna příznivcem teoretických učebnic bez přesahu do praxe. Bylo mi tedy jasné, že nějakou konstrukci chci.
> </p>
> 
> Ano, můžu vzít nějaký reálný osmibit a ukazovat na něm, jak se věci mají, možná čtenáře i ponouknout ke stavbě repliky, ale touto cestou se mi moc jít nechtělo. Říkám si, že na vlastní konstrukci, která bude tak jednoduchá, jak jen to lze, půjde ilustrovat požadované věci mnohem líp.
> 
> Zvolil jsem tedy cestu konstrukce naprosto jednoduchých počítačů, které mají splňovat několik bodů:
> 
>   1. Budou používat součástky, které se dají běžně koupit.
>   2.  Nebudou se ortodoxně držet konstrukcí z 80. let &#8211; tedy když chci použít paměť, použiju moderní, třeba 128x8bit statickou RAM, což je jeden čip, a nebudu konstruovat zapojení z osmi čipů dynamické RAM a dalších tří IO kolem dokola.
>   3. Budou v duchu starých osmibitových časů, tedy jednoduché periferie &#8211; klávesnice, displej, reproduktor &#8211; a minimální softwarové vybavení.
>   4. Bude jednoduché psát pro ně software.
>   5. Klidně vynechám samotný starý procesor a potřebné funkce si naemuluju v moderním jednočipu. Nebo použiju jednočip místo periferií. No stress.
> 
> Takže jsem sednul a připravil několik konstrukcí, které jsou vhodné i pro začátečníky a na kterých se dají vysvětlit principy stavby systémů s osmibitovými mikroprocesory.
> 
> Teď vám nabízím první konstrukci a jako ukázku postupně vydám několik zkrácených kapitol z rukopisu. A pokud se chcete předzásobit, tak se mrkněte na [seznam součástek pro stavbu](https://retrocip.cz/alpha-seznam-soucastek/).

<h1 data-key="125662">
  Koncepce počítače s procesorem 8080/8085
</h1>

<p data-key="125662">
  <span data-key="125661">K tomu, abyste si postavili počítač, není potřeba nic moc extra. Stačí vám jen nepájivé kontaktní pole, propojovací vodiče, napájecí zařízení a pár součástek: procesor, paměť a nějaké periferie.</span>
</p>

<p data-key="125666">
  <span data-key="125663">Ovšem budu upřímný: v případě procesoru 8080 to tak moc neplatí. Tedy ne že by to nešlo, samozřejmě že to jde, ale problém je v tom, že <em>procesor 8080</em> vyžaduje tři napájecí napětí a také nějaké podpůrné obvody.</span>
</p>

<p data-key="125668">
  <span data-key="125667">Z různých technologických důvodů nemá procesor 8080 na čipu integrované některé důležité součásti, především řízení sběrnice a generování hodinových pulsů. Slouží k tomu dvojice externích obvodů 8224 a 8228, a ačkoli je možné se bez nich obejít a postavit si vlastní alternativy, tak takový postup nedoporučuju.</span>
</p>

<p data-key="125670">
  <span data-key="125669">Obvod 8224 slouží ke generování hodinového kmitočtu, ve správné fázi a se správnými poměry period, a k tomu synchronizuje RESET a některé další signály. Obvod bere připojený kmitočet a dělí ho devíti. To je pak pracovní kmitočet procesoru. Pokud má připojený generátor frekvenci 18 MHz, bude 8080 pracovat na frekvenci 2 MHz.</span>
</p>

<p data-key="125680">
  <span data-key="125671">Obvod 8228 generuje z takzvaného <em>stavového slova</em>, které procesor posílá po datové sběrnici, a několika <em>řídicích signálů</em> plné řídicí signály /MEMR, /MEMW, /IOR a /IOW. Navíc dokáže fungovat jako jednoduchý řadič přerušení &#8211; má pouze jednu úroveň a při vzniku požadavku pošle na sběrnici instrukci RST 7 (tedy skok na adresu 0038h, jak si povíme později).</span>
</p>

<p data-key="125680">
  <a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8080minimal.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1054" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8080minimal-650x422.png" alt="" width="650" height="422" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8080minimal-650x422.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8080minimal-768x499.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8080minimal-1024x665.png 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8080minimal.png 1391w" sizes="(max-width: 650px) 100vw, 650px" /></a>
</p>

<span data-key="125683">Teprve až tato <em>svatá trojice</em>, jak se těmto třem obvodům někdy přezdívá, tvoří samotný &#8222;mikroprocesor 8080&#8220;.</span>

<p data-key="125700">
  <span data-key="125687">V tom nejjednodušším systému, kde není zapotřebí řešit přerušení ani pozastavování činnosti či přebírání sběrnice, stačí nepoužité vstupy HOLD a INT připojit na log. 0, vstup RDYIN na log. 1 a výstupy INTE, WAIT, HLDA a INTA ignorovat. Celý procesor pak komunikuje se zbytkem systému pomocí čtyř řídicích signálů /MEMR, /MEMW, /IOR a /IOW, které říkají, že procesor chce číst z paměti (MEMory Read), zapisovat do paměti (MEMory Write), popřípadě číst nebo zapisovat z/do vstupně-výstupních obvodů (Input/Output Read, Input/Output Write).</span>
</p>

<div class="DocumentEditor-HeadingNode">
  <h2 data-key="125702">
    <span id="Vlastni_pocitac_Jak_by_mohl_vypadat"><span data-key="125701">Vlastní počítač? Jak by mohl vypadat?</span></span>
  </h2>
</div>

<p data-key="125724">
  <span data-key="125703">Ačkoli byl 8080 široce používaná &#8222;klasika&#8220;, pro novou konstrukci bych ho nedoporučil &#8211; minimálně kvůli třem napájecím napětím. Sáhl bych po lehce rychlejším a vylepšeném procesoru 8085, respektive jeho CMOS verzi 80C85 (vyrábělo hned několik výrobců). Procesor 8085 si vystačí s pouhým jedním napájecím napětím (standardních +5 voltů) a nepotřebuje speciální obvody pro generování hodin (stačí mu obyčejný hodinový puls, nebo dokonce jen připojit krystal s kondenzátory). Navíc má dva vývody pro sériový vstup a výstup a celkem pět přerušovacích vstupů &#8211; kromě INTR ještě vstup TRAP (nemaskovatelné přerušení) a tři vstupy RST (RST5.5, RST6.5 a RST7.5).</span>
</p>

<p data-key="125738">
  <span data-key="125725">Ani 8085 nedokáže pracovat úplně bez vnějších obvodů: návrháři totiž kvůli nutnosti vejít se do pouzdra se 40 vývody sáhli k takzvanému multiplexovanému adresování. Procesor má vyvedených osm vyšších bitů adresy (A8-A15) a nižších 8 bitů se vede po datové sběrnici (vývody AD0-AD7). Adresu posílá procesor nadvakrát. K řízení slouží signál ALE (Address Latch Enable). Pokud je v log. 1, znamená to, že procesor posílá po AD0-AD7 dolních 8 bitů adresy. Pokud je v log. 0, fungují tyto vývody jako datová sběrnice.</span>
</p>

<p data-key="125748">
  <span data-key="125739">Návrhář systému proto musí zajistit vnější obvod (osmibitový latch), který se postará o to, aby zachytil spodních 8 bitů adresy. Typicky lze použít třeba obvod 74573 (starší konstrukce používaly např. dva obvody 7475).</span>
</p>

<div class="DocumentEditor-HeadingNode">
  <h2 data-key="125750">
    <span id="Vyvody_8085"><span data-key="125749">Vývody 8085</span></span>
  </h2>
  
  <p>
    <a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-pinout.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1055" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-pinout-491x650.png" alt="" width="491" height="650" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-pinout-491x650.png 491w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-pinout.png 656w" sizes="(max-width: 491px) 100vw, 491px" /></a>
  </p>
</div>

<div class="DocumentEditor-HeadingNode">
  <h2 data-key="125755">
    <span id="Vnitrni_struktura_8085"><span data-key="125754">Vnitřní struktura 8085</span></span>
  </h2>
</div>

<p data-key="125768">
  <span data-key="125759">Na první pohled se od 8080A moc neliší &#8211; i 8085 má sadu registrů B, C, D, E, H, L, akumulátor A, Stack pointer SP, programový čítač PC, ALU, řídicí obvody, řadič&#8230; Navíc proti 8080 je rozšířena část řízení přerušení (o vstupy RST a TRAP), část sériového výstupu (SID, SOD) a zabudovaný generátor hodin.</span>
</p>

<p data-key="125768">
  <a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/80c85.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1056" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/80c85-650x475.png" alt="" width="650" height="475" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/80c85-650x475.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/80c85-768x562.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/80c85-1024x749.png 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/80c85.png 1169w" sizes="(max-width: 650px) 100vw, 650px" /></a>
</p>

<p data-key="125774">
  <span data-key="125769">Dole je vidět zmíněné rozdělení adresní sběrnice na dvě části &#8211; samostatných 8 horních bitů A15-A8, a multiplex pro spodních 8 bitů adresy / 8 bitů dat (AD7-AD0).</span>
</p>

<p data-key="125776">
  <span data-key="125775">Řídicí signály mají následující funkce:</span>
</p>

<div class="DocumentEditor-TableNode">
  <table>
    <tr data-key="125781">
      <td class="column-left" data-key="125778">
        <span data-key="125777">Signál</span>
      </td>
      
      <td class="column-left" data-key="125780">
        <span data-key="125779">Funkce</span>
      </td>
    </tr>
    
    <tr data-key="125789">
      <td class="column-left" data-key="125786">
        <span data-key="125782">A8-A15 (výstup, třístavový)</span>
      </td>
      
      <td class="column-left" data-key="125788">
        <span data-key="125787">Horní část adresy. Během stavů HOLD, HALT a při RESETu je ve stavu vysoké impedance</span>
      </td>
    </tr>
    
    <tr data-key="125797">
      <td class="column-left" data-key="125794">
        <span data-key="125790">AD0-AD7 (vstup/výstup, třístavový)</span>
      </td>
      
      <td class="column-left" data-key="125796">
        <span data-key="125795">Nižších osm bitů adresy, popřípadě satová sběrnice</span>
      </td>
    </tr>
    
    <tr data-key="125809">
      <td class="column-left" data-key="125802">
        <span data-key="125798">ALE (výstup)</span>
      </td>
      
      <td class="column-left" data-key="125808">
        <span data-key="125803">Address Latch Enable. Oznamuje, že na vývodech AD0-AD7 je nižší část adresy (1). Pokud je 0, znamená to, že AD0-AD7 funguje jako datová sběrnice</span>
      </td>
    </tr>
    
    <tr data-key="125817">
      <td class="column-left" data-key="125814">
        <span data-key="125810">S0, S1 (výstupy)</span>
      </td>
      
      <td class="column-left" data-key="125816">
        <span data-key="125815">Stavová informace. Spolu se signálem IO/M říká, jestli procesor čte instrukci, jestli čte / zapisuje data do paměti, jestli komunikuje s periferií, jestli obsluhuje přerušení nebo že je zastaven. V jednoduchých konstrukcích se nepoužívají.</span>
      </td>
    </tr>
    
    <tr data-key="125832">
      <td class="column-left" data-key="125822">
        <span data-key="125818">IO/M (výstup)</span>
      </td>
      
      <td class="column-left" data-key="125831">
        <span data-key="125823">Oznamuje, jestli procesor hodlá komunikovat s pamětí (0), nebo s periferiemi (1)</span>
      </td>
    </tr>
    
    <tr data-key="125844">
      <td class="column-left" data-key="125837">
        <span data-key="125833">/RD (výstup, třístavový)</span>
      </td>
      
      <td class="column-left" data-key="125843">
        <span data-key="125838">Pokud je 0, procesor čte data z vnějšího zařízení (paměti, portu). Vnější zařízení musí požadovaná data poslat na datovou sběrnici.</span>
      </td>
    </tr>
    
    <tr data-key="125852">
      <td class="column-left" data-key="125849">
        <span data-key="125845">/WR (výstup, třístavový)</span>
      </td>
      
      <td class="column-left" data-key="125851">
        <span data-key="125850">Pokud je 0, procesor chce zapsat data, co poslal na datovou sběrnici, do portu nebo do paměti.</span>
      </td>
    </tr>
    
    <tr data-key="125860">
      <td class="column-left" data-key="125857">
        <span data-key="125853">READY (vstup)</span>
      </td>
      
      <td class="column-left" data-key="125859">
        <span data-key="125858">V dobách, kdy paměti a periferie byly pomalejší než procesor, mohly poslat během cyklu čtení nebo zápisu signál &#8222;počkej, než budou data připravena&#8220;. Slouží k tomu právě vstup READY. Pokud je 1, procesor jede na plný výkon. Pokud je 0, procesor po nastavení signálu /RD či /WR počká, dokud nebude READY zase 1, pak teprve pokračuje dál. My ho klidně připojíme na 1 a budeme věřit, že moderní paměti jsou dost rychlé&#8230;</span>
      </td>
    </tr>
    
    <tr data-key="125876">
      <td class="column-left" data-key="125865">
        <span data-key="125861">HOLD (vstup)</span>
      </td>
      
      <td class="column-left" data-key="125875">
        <span data-key="125866">Normálně 0. Pokud je nastaven na 1, procesor dokončí nejnutnější operaci (cyklus čtení nebo zápisu) a odpojí svoji datovou sběrnici, adresní sběrnici a signály /RD, /WR a IO/M. Odpojením mám na mysli přepnutí do stavu Z (vysoké impedance). Jakmile je procesor odpojen, potvrdí, že je vše hotovo, signálem HLDA. Jednoduché systémy tento signál nepoužívají, můžete jej připojit na log. 0</span>
      </td>
    </tr>
    
    <tr data-key="125884">
      <td class="column-left" data-key="125881">
        <span data-key="125877">HLDA (výstup)</span>
      </td>
      
      <td class="column-left" data-key="125883">
        <span data-key="125882">Za normálního provozu 0. Pokud byl dán požadavek HOLD, tak se po dokončení nezbytných operacáí a odpojení sběrnic tento signál nastaví do log. 1.</span>
      </td>
    </tr>
    
    <tr data-key="125896">
      <td class="column-left" data-key="125889">
        <span data-key="125885">INTR (vstup)</span>
      </td>
      
      <td class="column-left" data-key="125895">
        <span data-key="125890">Interrupt request, tedy Požadavek na přerušení. Procesor testuje stav tohoto signálu na konci každé instrukce a také ve stavu HALT (po instrukci HLT) a HOLD. Pokud je 0, nic se neděje, pokud je 1, začne proces obsluhy přerušení. Procesor přečte z datové sběrnice hodnotu a považuje ji za instrukci, kterou vykoná. Nejčastěji se posílají jednobytové instrukce RST, ale je možné poslat i tříbytovou instrukci CALL. O poslání instrukce se musí postarat vnější obvody, takzvané řadiče přerušení. Programem je možné zakázat vyvolání přerušení tímto signálem.</span>
      </td>
    </tr>
    
    <tr data-key="125907">
      <td class="column-left" data-key="125901">
        <span data-key="125897">/INTA (výstup)</span>
      </td>
      
      <td class="column-left" data-key="125906">
        <span data-key="125902">Interrupt Acknowledge. Po příchodu požadavku přerušení INTR použije procesor výstup /INTA k načtení výše zmíněné instrukce (namísto signálu /RD)</span>
      </td>
    </tr>
    
    <tr data-key="125918">
      <td class="column-left" data-key="125912">
        <span data-key="125908">RST5.5, RST 6.5, RST 7.5 (vstupy)</span>
      </td>
      
      <td class="column-left" data-key="125917">
        <span data-key="125913">Přímé vstupy přerušení. Fungují podobně jako signál INTR, ale nevyžadují celý ten opruz s načítáním instrukce pomocí /INTA. Místo toho si samy interně zařídí skok na patřičná místa. Stejně jako INTR dokážou &#8222;probudit&#8220; procesor ze stavu HALT a HOLD. Programátor může tyto signály ignorovat pomocí speciální instrukce (&#8222;maskování přerušení&#8220;)</span>
      </td>
    </tr>
    
    <tr data-key="125926">
      <td class="column-left" data-key="125923">
        <span data-key="125919">TRAP (vstup)</span>
      </td>
      
      <td class="column-left" data-key="125925">
        <span data-key="125924">TRAP je podobný signálům RST x.5 s tím rozdílem, že toto přerušení nemůže programátor zamaskovat.</span>
      </td>
    </tr>
    
    <tr data-key="125934">
      <td class="column-left" data-key="125931">
        <span data-key="125927">/RESET IN (vstup, Schmitt KO)</span>
      </td>
      
      <td class="column-left" data-key="125933">
        <span data-key="125932">Vstup nulování. Po startu systému nebo při havárii by měly vnější obvody tento signál, který je normálně v log. 1, přepnout do log. 0. Tím započne interní proces inicializace: procesor odpojí sběrnice, nastaví programový čítač na adresu 0000h, ukončí všechny případné čekací cykly a začne pracovat &#8222;od začátku&#8220;. Tento vstup je vybavený Schmittovým klopným obvodem, to znamená, že je možné k němu připojit přímo oblíbený obvod s rezistorem a kondenzátorem, který se postará o RESET při zapnutí napájecího napětí.</span>
      </td>
    </tr>
    
    <tr data-key="125942">
      <td class="column-left" data-key="125939">
        <span data-key="125935">RESET OUT (výstup)</span>
      </td>
      
      <td class="column-left" data-key="125941">
        <span data-key="125940">Indikuje, že je procesor ve stavu RESET. Tento signál je synchronizovaný s hodinami a můžete ho použít jako systémový RESET pro zbytek systému.</span>
      </td>
    </tr>
    
    <tr data-key="125950">
      <td class="column-left" data-key="125947">
        <span data-key="125943">X1, X2 (vstupy)</span>
      </td>
      
      <td class="column-left" data-key="125949">
        <span data-key="125948">Slouží k připojení hodinového krystalu. Jeho frekvence je interně vydělena dvěmi a výsledek dává systémový takt. Vstup X1 může být použit i jako vstup hodinového signálu z externího obvodu.</span>
      </td>
    </tr>
    
    <tr data-key="125961">
      <td class="column-left" data-key="125955">
        <span data-key="125951">CLK (výstup)</span>
      </td>
      
      <td class="column-left" data-key="125960">
        <span data-key="125956">Systémové hodiny. Jejich frekvence je poloviční proti frekvenci na vstupu X1 (tedy třeba proti frekvenci připojeného krystalu)</span>
      </td>
    </tr>
    
    <tr data-key="125969">
      <td class="column-left" data-key="125966">
        <span data-key="125962">SID (vstup)</span>
      </td>
      
      <td class="column-left" data-key="125968">
        <span data-key="125967">Sériový vstup. Hodnota na tomto vývodu je načtena do nejvyššího bitu akumulátoru pomocí instrukce RIM.</span>
      </td>
    </tr>
    
    <tr data-key="125977">
      <td class="column-left" data-key="125974">
        <span data-key="125970">SOD (výstup)</span>
      </td>
      
      <td class="column-left" data-key="125976">
        <span data-key="125975">Sériový výstup. Hodnota na tomto vývodu je nastavena nebo nulována pomocí instrukce SIM.</span>
      </td>
    </tr>
    
    <tr data-key="125982">
      <td class="column-left" data-key="125979">
        <span data-key="125978">Vcc</span>
      </td>
      
      <td class="column-left" data-key="125981">
        <span data-key="125980">Napájecí napětí. Nominálně 5 voltů, u CMOS verze může být někde mezi 3V a 6V.</span>
      </td>
    </tr>
    
    <tr data-key="125987">
      <td class="column-left" data-key="125984">
        <span data-key="125983">GND</span>
      </td>
      
      <td class="column-left" data-key="125986">
        <span data-key="125985">Zem</span>
      </td>
    </tr>
  </table>
</div>

<p data-key="125994">
  <span data-key="125989">V datasheetu (je jich spousta různých od různých výrobců, já bych doporučil datasheet od OKI, jejich čip nesl název MSM80C85AH) najdete i velmi důležitý graf, který ukazuje průběhy signálů na sběrnici. Podívejme se na něj spolu:</span>
</p>

<p data-key="125994">
  <a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-timing.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1057" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-timing-650x498.png" alt="" width="650" height="498" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-timing-650x498.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/8085-timing.png 748w" sizes="(max-width: 650px) 100vw, 650px" /></a>
</p>

<p data-key="125999">
  <span data-key="125998">První řádek ukazuje hodinový signál. M1 je první strojový cyklus, T1 až T4 jsou jeho hodinové takty. Při vyzvedávání instrukce tedy procesor řídí výstupy takto:</span>
</p>

<li data-key="126005">
  <span data-key="126000">T1: Nastaví horní část adresy instrukce na výstupy A8-A15, dolní část na výstupy AD0-AD7, a zároveň aktivuje signál ALE (Address Latch Enable). Tento signál řídí už zmíněný buffer mimo procesor, jehož úkolem je zapamatovat si právě spodní část adresy. Zároveň nastavuje signál IO/M do hodnoty 0, tedy &#8222;komunikace s pamětí&#8220;. Signály /RD a /WR jsou neaktivní, stavové signály nás nemusí zajímat. Se sestupnou hranou hodin v čase T1 se deaktivuje signál ALE a dolní část adresy zůstává zachycena v bufferu.</span>
</li>
<li data-key="126015">
  <span data-key="126006">T2: Procesor přepíná signály AD0-AD7 na vstup dat a aktivuje signál /RD, což informuje okolní systém, že procesor hodlá číst (/RD) z paměti (IO/M je v log. 0). Paměť je připojena ke sběrnici a vybrána adresa.</span>
</li>
<li data-key="126017">
  <span data-key="126016">T3: S náběžnou hranou hodin přečte procesor stav na datové sběrnici a začne jej vyhodnocovat coby instrukci. Poté deaktivuje signál /RD, takže se paměť může opět odpojit.</span>
</li>
<li data-key="126019">
  <span data-key="126018">T4: Procesor dekóduje instrukci.</span>
</li>

<p data-key="126021">
  <span data-key="126020">Dejme tomu, že instrukce potřebuje jeden osmibitový parametr. Ten je uložen, jak bývá zvykem, na následující adrese. Následuje tedy strojový cyklus M2, během něhož jsou čtena data z paměti. Když se podíváte zase na schéma, vidíte, že je téměř totožný s cyklem M1, až na to, že trvá pouhé tři takty &#8211; odpadá &#8222;dekódovací&#8220; takt.</span>
</p>

<div class="DocumentEditor-HeadingNode">
  <h2 data-key="126023">
    <span id="Preruseni"><span data-key="126022">Přerušení</span></span>
  </h2>
</div>

<p data-key="126029">
  <span data-key="126024">Pokud v systému nastane nějaká událost, která vyžaduje, aby se jí procesor bezodkladně věnoval (propukne požár, uživatel stiskne tlačítko, přijdou nová data, &#8230;), vyvolá takzvané přerušení. U 8085 k tomu slouží hned tři mechanismy.</span>
</p>

<p data-key="126035">
  <span data-key="126030">První je identický jako u procesoru 8080 &#8211; vnější zařízení nastaví signál INTR, procesor si přečte z řadiče přerušení jednu instrukci a vykoná ji. Nepřekvapivě jde o instrukce skoku do podprogramu (pro programátory, zvyklé na vyšší jazyky: něco jako vyvolání handleru, volání funkce atd.) O správné poslání instrukce se musí postarat vnější obvody, většinou specializované obvody, nazývané řadiče přerušení.</span>
</p>

<p data-key="126045">
  <span data-key="126036">U 8080 šlo tuto složitost zjednodušit pomocí obvodu 8228 &#8211; připojením jednoho z vývodů (INTA) přes rezistor 1K k napětí 12 voltů se z tohoto obvodu stal jednoduchý &#8222;řadič přerušení&#8220;, který po příchodu požadavku na přerušení poslal na sběrnici hodnotu FFh, což je hodnota, kterou si procesor dekóduje jako instrukci RST 7. (RST 7 uloží aktuální adresu na zásobník a udělá odskok na adresu 0038h, ale o tom víc později). 8085 tuto vymoženost nemá, takže buď použijete řadič přerušení, nebo si vystačíte s interními vstupy RSTx a TRAP.</span>
</p>

<p data-key="126047">
  <span data-key="126046">Druhý způsob je použití vstupů RSTx. Jejich funkce je podobná té, co jsem popisoval u obvodu 8228 o odstavec výš &#8211; vygenerují si skokovou instrukci samy. Ono to není tak úplně doslova, protože mechanismus je lehce jiný, ale princip je stejný.</span>
</p>

<p data-key="126053">
  <span data-key="126048">Vstupy RST 5.5 a RST 6.5 (asi klidně můžeme říkat pět a půl a šest a půl) jsou, podobně jako vstup INTR, pravidelně kontrolovány, a jsou-li ve stavu 1, procesor vyvolá přerušení.</span>
</p>

<p data-key="126055">
  <span data-key="126054">Vstup RST 7.5 má, na rozdíl od předchozích, zabudovaný vnitřní klopný obvod, který reaguje na vzestupnou hranu. Výhoda je, že stačí jen krátký signál a procesor si jej inteně &#8222;podrží&#8220; až do doby, kdy testuje přerušovací vstupy.</span>
</p>

<p data-key="126071">
  <span data-key="126056">Třetí způsob je signál TRAP. Všechny předchozí způsoby přerušení může programátor zablokovat, &#8222;zamaskovat&#8220; pomocí instrukce DI (Disable Interrupt) a povolit pomocí EI (Enable Interrupt). Pokud jsou přerušení zakázána (DI), procesor signály INTR a RST ignoruje. TRAP je výjimka, tento signál vyvolá přerušení i tehdy, když jsou zakázána. Proto se mu také říká <em>nemaskovatelné přerušení</em>.</span>
</p>

<p data-key="126073">
  <span data-key="126072">Co se stane, když přijde naráz víc požadavků na přerušení? Procesor má zabudovaný jednoduchý mechanismus priorit, viz následující tabulka, a vykoná obslužnou rutinu pro požadavek s nejvyšší prioritou.</span>
</p>

<div class="DocumentEditor-TableNode">
  <table>
    <tr data-key="126082">
      <td class="column-left" data-key="126075">
        <span data-key="126074">Signál</span>
      </td>
      
      <td class="column-left" data-key="126077">
        <span data-key="126076">Priorita</span>
      </td>
      
      <td class="column-left" data-key="126079">
        <span data-key="126078">Adresa, kde je uložena obslužná rutina</span>
      </td>
      
      <td class="column-left" data-key="126081">
        <span data-key="126080">Vyvoláno pomocí&#8230;</span>
      </td>
    </tr>
    
    <tr data-key="126094">
      <td class="column-left" data-key="126084">
        <span data-key="126083">TRAP</span>
      </td>
      
      <td class="column-left" data-key="126089">
        <span data-key="126085">Nejvyšší (1)</span>
      </td>
      
      <td class="column-left" data-key="126091">
        <span data-key="126090">0024h</span>
      </td>
      
      <td class="column-left" data-key="126093">
        <span data-key="126092">Vzestupná hrana nebo log. 1 během testování</span>
      </td>
    </tr>
    
    <tr data-key="126106">
      <td class="column-left" data-key="126096">
        <span data-key="126095">RST 7.5</span>
      </td>
      
      <td class="column-left" data-key="126098">
        <span data-key="126097">2</span>
      </td>
      
      <td class="column-left" data-key="126100">
        <span data-key="126099">003Ch</span>
      </td>
      
      <td class="column-left" data-key="126105">
        <span data-key="126101">Vzestupná hrana (pozdržená)</span>
      </td>
    </tr>
    
    <tr data-key="126115">
      <td class="column-left" data-key="126108">
        <span data-key="126107">RST 6.5</span>
      </td>
      
      <td class="column-left" data-key="126110">
        <span data-key="126109">3</span>
      </td>
      
      <td class="column-left" data-key="126112">
        <span data-key="126111">0034h</span>
      </td>
      
      <td class="column-left" data-key="126114">
        <span data-key="126113">Log. 1 během testování</span>
      </td>
    </tr>
    
    <tr data-key="126124">
      <td class="column-left" data-key="126117">
        <span data-key="126116">RST 5.5</span>
      </td>
      
      <td class="column-left" data-key="126119">
        <span data-key="126118">4</span>
      </td>
      
      <td class="column-left" data-key="126121">
        <span data-key="126120">002Ch</span>
      </td>
      
      <td class="column-left" data-key="126123">
        <span data-key="126122">Log. 1 během testování</span>
      </td>
    </tr>
    
    <tr data-key="126136">
      <td class="column-left" data-key="126126">
        <span data-key="126125">INTR</span>
      </td>
      
      <td class="column-left" data-key="126131">
        <span data-key="126127">Nejnižší (5)</span>
      </td>
      
      <td class="column-left" data-key="126133">
        <span data-key="126132">Podle poslané instrukce</span>
      </td>
      
      <td class="column-left" data-key="126135">
        <span data-key="126134">Log. 1 během testování</span>
      </td>
    </tr>
  </table>
</div>

<h2 data-key="126140">
  <span id="K_dalsimu_cteni">K dalšímu čtení</span>
</h2>

<p data-key="126140">
  <a class="DocumentEditor-LinkNode tooltipped" href="#" aria-label="https://saundby.com/electronics/8085/" data-key="126139"><span data-key="126138">https://saundby.com/electronics/8085/</span></a>
</p>

<p data-key="126143">
  <a class="DocumentEditor-LinkNode tooltipped" href="#" aria-label="https://www.nostalcomp.cz/cvicny8080.php" data-key="126142"><span data-key="126141">https://www.nostalcomp.cz/cvicny8080.php</span></a>
</p>

<h1 data-key="126143">
  Zapojení procesoru 8085
</h1>

<a href="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-cpu.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-1058" src="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-cpu-650x363.png" alt="" width="650" height="363" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-cpu-650x363.png 650w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-cpu-768x429.png 768w, https://retrocip.cz/wp-content/uploads/sites/6/2018/05/alpha85-cpu-1024x573.png 1024w" sizes="(max-width: 650px) 100vw, 650px" /></a>

Takto bude vypadat srdce celého počítače. Dovolte jen pár poznámek:

V roli adresového bufferu je obvod IC2 typu 74573 (ať už ve verzi HC, HCT nebo ALS). Funkčně je shodný s obvodem 74373, až na jeden rozdíl: 373 má vývody přeházené, 573 má hezky všechny vstupy na jedné straně pouzdra a výstupy na druhé. Hodinový vstup je připojen na vývod ALE, povolovací vstup OE jsem připojil natvrdo k zemi. Mohl bych ho připojit na vývod HLDA a zajistit tak odpojení adresní sběrnice ve stavu HOLD, ale tento stav nebude náš počítač používat.

Nebudeme používat ani HOLD, ani READY, ani přerušení, proto jsou tyto vstupy připojeny na neutrální úroveň (READY je 1 &#8211; stále připraveno, HOLD a přerušení na 0). Z výstupů jsem nepoužil stavové S0, S1, výstup RESET OUT, a logicky ani HLDA a INTA. Stejně tak nezapojené zůstalo i sériové rozhraní &#8211; vstup SID je připojen k zemi, výstup SOD je volný. Můžete si k němu připojit LEDku přes rezistor a zkoušet si třeba bliknout&#8230;

Vlevo od procesoru jsou dva obvody. Níž je generátor hodinových pulsů s krystalem a dvěma kondenzátory. Krystal jsem zvolil o frekvenci 3,6864 MHz. Proč? Proč ne 4 nebo, cojávím, 27?

Zaprvé &#8211; frekvence nesmí být nižší než 1 MHz. Píšou to v datasheetu. Zadruhé &#8211; neměla by být vyšší než 10 MHz. To tam taky píšou. Ideální je někde okolo 4 MHz, protože procesor pak běží na příjemně pomalé frekvenci okolo 2 MHz, tedy zhruba tak, jak běžela originální 8080A.

Proč ale 3,6864 MHz? No protože pak bude procesor běžet na frekvenci 1,8432 MHz, tedy na poloviční.

Dobře, ale proč ne třeba 4 MHz?

No protože 1843200 Hz / 96 = 19200 a 1843200 Hz / 192 = 9600. Už svítá? Správně, 19200 a 9600 jsou standardní frekvence pro sériovou komunikaci. Z frekvence 1,8432 MHz je odvodíme bez problémů pomocí dělení celým číslem. Kdybychom chtěli tyto frekvence získat třeba z 4 MHz, museli bychom hodiny dělit koeficientem např. 208,3333 nebo 416,6666, a v přenosu by byly chyby. Takže tady raději oželím trochu výkonu, když vím, že mi to usnadní další zapojování.

<span data-slate-fragment="JTdCJTIyZGF0YSUyMiUzQSU3QiU3RCUyQyUyMmtpbmQlMjIlM0ElMjJkb2N1bWVudCUyMiUyQyUyMm5vZGVzJTIyJTNBJTVCJTdCJTIyZGF0YSUyMiUzQSU3QiU3RCUyQyUyMmtpbmQlMjIlM0ElMjJibG9jayUyMiUyQyUyMmlzVm9pZCUyMiUzQWZhbHNlJTJDJTIydHlwZSUyMiUzQSUyMnBhcmFncmFwaCUyMiUyQyUyMm5vZGVzJTIyJTNBJTVCJTdCJTIya2luZCUyMiUzQSUyMnRleHQlMjIlMkMlMjJyYW5nZXMlMjIlM0ElNUIlN0IlMjJraW5kJTIyJTNBJTIycmFuZ2UlMjIlMkMlMjJ0ZXh0JTIyJTNBJTIyQSUyMHBvc2xlZG4lQzMlQUQlMjB2JUM0JTlCYyUzQSUyMGtvbmRlbnolQzMlQTF0b3J5JTIwdSUyMGtyeXN0YWx1ISUyMEpzb3UlMjBkJUM1JUFGbGUlQzUlQkVpdCVDMyVBOSUyMGslMjB0b211JTJDJTIwYWJ5JTIwa3J5c3RhbCUyMGttaXRhbCUyMG5hJTIwc3ByJUMzJUExdm4lQzMlQTklMjBmcmVrdmVuY2kuJTIwSmVqaWNoJTIwa2FwYWNpdHUlMjBtJUM1JUFGJUM1JUJFZXRlJTIwYnUlQzQlOEYlMjBzcG8lQzQlOEQlQzMlQUR0YXQlMkMlMjBuZWJvJTIwdnklQzQlOEQlQzMlQURzdCUyMHolMjBkYXRhc2hlZXR1LiUyMCUyMiUyQyUyMm1hcmtzJTIyJTNBJTVCJTVEJTdEJTJDJTdCJTIya2luZCUyMiUzQSUyMnJhbmdlJTIyJTJDJTIydGV4dCUyMiUzQSUyMkolQzMlQTElMjB6JTIwZGF0YXNoZWV0dSUyMHZ5JUM0JThEZXRsJTIwNTYlMjBwRiUyQyUyMGNvJUM1JUJFJTIwc2UlMjBtaSUyMHRlZHklMjB6ZCVDMyVBMWxvJTIwamFrbyUyMG9icm92c2slQzMlQTElMjBrYXBhY2l0YS4lMjBOYXNhZGlsJTIwanNlbSUyMDMzJTIwcEYlMkMlMjBhJTIwaSUyMHRvJTIwYnlsbyUyMG1vYy4lMjBOYWtvbmVjJTIwanNlbSUyMG5lY2hhbCUyMGplbiUyMGplZGVuJTIwa29uZGVueiVDMyVBMXRvciUyMDMzJTIwcEYlMjB1JTIwdnN0dXB1JTIwWDIuJTIwUGFrJTIwc2UlMjB1ayVDMyVBMXphbG8lMkMlMjAlQzUlQkVlJTIwNTYlMjBwRiUyMGJ5bG8lMjB6JTIwZGF0YXNoZWV0dSUyMENNT1MlMjB2ZXJ6ZSUyMGElMjBqJUMzJUExJTIwbSVDNCU5QmwlMjBwb3UlQzUlQkVpdG91JTIwTk1PUyUyQyUyMGtkZSUyMHNlJTIwZG9wb3J1JUM0JThEdWolQzMlQUQlMjBwJUM1JTk5aSUyMHQlQzQlOUJjaHRvJTIwZnJla3ZlbmMlQzMlQURjaCUyMGthcGFjaXR5JTIwb2tvbG8lMjA3JTIwcEYuJTIyJTJDJTIybWFya3MlMjIlM0ElNUIlN0IlMjJkYXRhJTIyJTNBJTdCJTdEJTJDJTIya2luZCUyMiUzQSUyMm1hcmslMjIlMkMlMjJ0eXBlJTIyJTNBJTIySVRBTElDJTIyJTdEJTVEJTdEJTVEJTdEJTVEJTdEJTVEJTdE">A</span> <span data-offset-key="132683-0">poslední věc: kondenzátory u krystalu! Jsou důležité k tomu, aby krystal kmital na správné frekvenci. Jejich kapacitu můžete buď spočítat, nebo vyčíst z datasheetu. </span><span data-offset-key="132683-1"><em>Já z datasheetu vyčetl 56 pF, což se mi tedy zdálo jako obrovská kapacita. Nasadil jsem 33 pF, a i to bylo moc. Nakonec jsem nechal jen jeden kondenzátor 33 pF u vstupu X2. Pak se ukázalo, že 56 pF bylo z datasheetu CMOS verze a já měl použitou NMOS, kde se doporučují při těchto frekvencích kapacity okolo 7 pF.</em></span>

Nad generátorem hodin je obvod pro /RESET. Pomocí kondenzátoru k zemi a rezistoru k napájecímu napětí je po zapnutí napájení na vstupu /RESET IN logická 0, dokud se kondenzátor pomalu nenabije. Vhodný poměr odporu a kapacity si můžete buď spočítat, nebo experimentálně vyzkoušet. Myslím, že rezistorem 10K a kondenzátorem 10M nic nezkazíte. Dejte systému trochu času na start, protože po zapnutí napájení ještě chvíli napětí kolísá. Díky tomuto triku je v tu dobu procesor stále ve stavu RESET, a když nastartuje, je napětí už ustálené.

Ještě víc vlevo je tlačítko, které připojuje vstup na zem. Je to magické tlačítko RESET, které náš počítač přivede k rozumu, pokud se náhodou někdy zapomene a skončí třeba v nekonečné smyčce. Díky kondenzátoru jsou odfiltrované i zákmity&#8230;

Procesor tedy máme _pořešený_. S okolním světem komunikuje pomocí adresní sběrnice, datové sběrnice, signálů IO/M, /RD, /WR a hodinového signálu CLK. To je opravdu všechno, víc není pro jednoduchý počítač potřeba.