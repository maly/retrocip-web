---
id: 822
title: Soutěž v programování mikrořadičů
date: 2016-06-26T23:15:59+01:00
author: Martin Maly
layout: post
guid: http://retrocip.uelectronics.info/?p=822
permalink: /soutez-v-programovani-mikroradicu/
xyz_lnap:
  - "1"
dsq_thread_id:
  - "4940874690"
categories:
  - Software
tags:
  - arduino
  - Atmel
  - shield
  - Soutěž
---
O víkendu proběhlo celorepublikové kolo soutěže mládeže v programování. Se Štěpánem Bechynským jsme byli porotci kategorie &#8222;mikrořadiče&#8220;.

Účastníci dostali Arduino a výukový kit, který používáne na workshopu Arduino101. Na vypracování úlohy měli čtyři hodiny&#8230; Chcete s nimi zmřit síly?

\[sc:ebay item=&#8220;arduino uno USB&#8220;\] \[sc:ebay item=&#8220;arduino clock shield TM1636&#8243;\]

## Zadání úlohy pro celostátní kolo kategorie mikrořadiče

#### Univerzální hlídač lednice

Nové lednice mají velmi často zabudovaný teploměr, takže víte, jestli fungují správně, a když necháte dveře dlouho otevřené, tak začnou pískat. Co ale udělat s lednicí, která takto vybavená není? To je úkol pro Univerzální hlídač lednice (UHL). Vaším úkolem je takovéto zařízení vytvořit. Hardware dostanete připravený a vaším úkolem bude vše naprogramovat.

Funkce UHL je následující: 1. Pokud je lednice otevřená déle než 30 vteřin, tak začne UHL přerušovaně (5 Hz) pískat. Otevření lednice detekujte pomocí fotorezistoru.

2. Při otevření lednice zobrazuje UHL teplotu ve stupních Celsia nebo Fahrenheita, podle toho, co si uživatel naposled zvolil, viz bod 3. Teplota je zaokrouhlena na celé číslo a za teplotou se na displeji zobrazí jednotky, tedy znak C nebo F.

3. Pokud je lednice zavřena, tak se na displeji nic nezobrazuje, aby se šetřila energie.

4. Tlačítkem Menu UHL přepíná mezi zobrazením teploty ve stupních Celsia, Fahrenheita.

5. UHL ukládá maximální a minimální naměřenou teplotu. Tyto hodnoty zůstanou uloženy, i když UHL přijde o napájení, např. při výměně baterie.

6. Ke každému extrému se zároveň uloží i datum a čas. Nezapomeňte proto správně nastavit hodiny.

7. Pomocí tlačítka Up zobrazí UHL maximální naměřenou teplotu a po 10 vteřinách se vrátí na naposled zvolené zobrazení, viz funkce č. 2.

8. Pomocí tlačítka Down zobrazí UHL minimální naměřenou teplotu a po 10 vteřinách se vrátí na naposled zvolené zobrazení, viz funkce č. 2.

9. UHL umí hlídat překročení mezních hodnot teploty. Při překročení minimální nebo maximální teploty se rozblikají všechny LED a na displeji začne blikat hodnota teploty, která je mimo rozsah. Frekvence blikání diod je 10 Hz, frekvence blikání displeje je 1 Hz. Nastavení minimální a maximální teploty se provádí přes sériovou linku. Tyto hodnoty se uchovávají i při odpojení napájení.

10. Po připojení UHL k počítači si mohu některé informace zobrazit nebo nastavit pomocí následujících příkazů přes sériovou linku:

&nbsp;

<table>
  <tr>
    <th>
      Příkaz (nezáleží na velikosti písmen)
    </th>
    
    <th>
      Funkce
    </th>
    
    <th>
      Ukázka výstupu
    </th>
  </tr>
  
  <tr>
    <td>
      Maxc
    </td>
    
    <td>
      Maximální hodnota ve °C
    </td>
    
    <td>
      2016-01-14 10:32:14 10C
    </td>
  </tr>
  
  <tr>
    <td>
      Maxf
    </td>
    
    <td>
      Maximální hodnota ve °F
    </td>
    
    <td>
      2016-01-14 10:32:14 50F
    </td>
  </tr>
  
  <tr>
    <td>
      Minc
    </td>
    
    <td>
      Minimální hodnota ve °C
    </td>
    
    <td>
      2016-01-14 12:02:14 7C
    </td>
  </tr>
  
  <tr>
    <td>
      Minf
    </td>
    
    <td>
      Minimální hodnota ve °F
    </td>
    
    <td>
      2016-01-14 12:02:14 44F
    </td>
  </tr>
  
  <tr>
    <td>
      Delete
    </td>
    
    <td>
      Smaže uložené informace
    </td>
    
    <td>
      OK nebo ERROR
    </td>
  </tr>
  
  <tr>
    <td>
      Allc
    </td>
    
    <td>
      Zobrazí minimální a maximální teplotu ve °C
    </td>
    
    <td>
      2016-01-14 12:02:14 7C<br /> 2016-01-14 10:32:14 10C
    </td>
  </tr>
  
  <tr>
    <td>
      Allf
    </td>
    
    <td>
      Zobrazí minimální a maximální teplotu ve °F
    </td>
    
    <td>
      2016-01-14 12:02:14 44F<br /> 2016-01-14 10:32:14 50F
    </td>
  </tr>
  
  <tr>
    <td>
      Setlimitmaxc <hodnota>
    </td>
    
    <td>
      Nastaví poplach při překročení maximální teploty
    </td>
    
    <td>
      OK nebo ERROR
    </td>
  </tr>
  
  <tr>
    <td>
      Setlimitminc <hodnota>
    </td>
    
    <td>
      Nastaví poplach při překročení minumální teploty
    </td>
    
    <td>
      OK nebo ERROR
    </td>
  </tr>
  
  <tr>
    <td>
      Deletelimitmaxc Vypne hlídání
    </td>
    
    <td>
      maximální teploty
    </td>
    
    <td>
      OK nebo ERROR
    </td>
  </tr>
  
  <tr>
    <td>
      Deletelimitminc Vypne hlídání
    </td>
    
    <td>
      minimální teploty
    </td>
    
    <td>
      OK nebo ERROR
    </td>
  </tr>
</table>

11. Pokud uživatel provede neplatnou operaci, třeba zadá nesmyslnou hodnotu, tak se na displeji objeví text ERROR, který bude po dobu 10 vteřin po displeji rolovat, a zároveň se pošle chybové hlášení po sériové lince. Pak se UHL vrátí do režimu zobrazení teploty.

Pravidla

1. Pokud něčemu nerozumíte, tak se ihned zeptejte.

2. Musí se použít jen dodaný jednotný hardware.

3. Smí se použít Standard Libraries, které jsou dostupné v čisté instalaci Arduino IDE viz. https://www.arduino.cc/en/Reference/Libraries.

4. Smí se použít knihovna TM1636 pro ovládání displeje, **kterou dodá pořadatel. **_(Odstranili jsme znaky A-F, abychom si ověřili, že soutěžící chápou cizí kód  dokážou ho upravit)_

5. Knihovny se mohou libovolně upravovat.

6. Můžete vytvořit vlastní knihovnu.

7. Jiné knihovny jsou zakázané.

8. Zdrojový kód musí být pečlivě okomentován, aby bylo zřejmé, že mu autor rozumí. I když vydělíte dvě čísla, tak musíte do komentáře napsat proč je dělíte.

9. Zdrojové kódy aplikace, včetně knihoven, uložte na plochu do adresáře, jehož jméno bude vaše startovní číslo. Adresář je třeba si vytvořit.

10. V případě stejného počtu bodů rozhoduje čas odevzdání úlohy. (Kdo dřív přijde, ten dřív mele. First come, first serve.)

Co máte k dispozici

1. Schéma zapojení použitého shieldu.

2. Seznam použitých součástek.

3. Katalogový list hodin reálného času.

4. Knihovnu TM1636.

5. Katalogový list použitého termistoru.

6. Vzorec pro přepočet °C na °F.

<p style="padding-left: 30px;">
  t[C°]= 5/9.(t[F°]−32)
</p>

7. Vzorec pro výpočet teploty.

t [°C] – výsledná teplota

r [Ω] – odpor termistoru

B – konstanta z katalogového listu

t[°C]=1 / (log(r[Ω] / 10000) / (B+1) / 298.15)−273.15

* * *

Tak co, troufnete si?

Soutěžící (12-19 let) se s tím popasovali všelijak. Dva byli výrazně lepší než zbytek, vítěz dokone používal i DDRD a PORTD, nastavil si přerušení,&#8230;

Na druhé straně konstrukce:

<pre class="">switch (flag) {
    case true:
        flag = false;
        break;
    case false:
        flag = true;
        break;
}</pre>

byla velmi neotřelá a ukazovala, že autor možná udělá líp, když se bude věnovat jiným aktivitám 😉

Troufnete si vy? Alespoň koncept&#8230;