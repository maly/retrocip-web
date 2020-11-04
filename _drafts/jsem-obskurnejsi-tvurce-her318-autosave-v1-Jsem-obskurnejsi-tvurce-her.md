---
id: 321
title: Jsem obskurnější tvůrce her
date: 2014-06-06T07:53:57+01:00
author: Martin Maly
layout: revision
guid: http://retrocip.uelectronics.info/318-autosave-v1/
permalink: /318-autosave-v1/
---
Heč! A navíc vám je tu ukážu&#8230;

<!--more-->

To jsem takhle tuhle hledal nějakou zmínku o Piškworks. Kdysi dávno je pro ZX Spectrum udělal Patrik Rak aka Raxoft. A jak jsem googlil, tak jsem narazil na disertační práci mgr. Švelcha s názvem [Osmibitové &#8222;poblouznění&#8220;: Počátky kultury počítačových her v Československu](https://is.cuni.cz/webapps/zzp/download/140030658).

Práce je to zajímavá, určitě si ji přečtěte. Já se do ní začetl, čtu, čtu, až jsem došel k textovkám, které mě tehdy zajímaly, pročítám si to, a najednou:

<img loading="lazy" class="aligncenter size-medium wp-image-319" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2014/06/obskurni-650x108.jpg" alt="obskurni" width="650" height="108" srcset="https://retrocip.cz/wp-content/uploads/sites/6/2014/06/obskurni-650x108.jpg 650w, https://retrocip.cz/wp-content/uploads/sites/6/2014/06/obskurni-1024x171.jpg 1024w, https://retrocip.cz/wp-content/uploads/sites/6/2014/06/obskurni.jpg 1130w" sizes="(max-width: 650px) 100vw, 650px" /> 

Tak samozřejmě že mě to potěšilo. Člověka vždycky potěší, když si na něj v akademické práci vzpomenou, a když ho navíc označí za &#8222;obskurnějšího tvůrce&#8220;, cítí se polichocen&#8230; Tvůrce her, to si na vizitku může napsat kdejaký hejhula, ale _obskurnější_, to zní, pane! (Jako když v novinové recenzi napsali, že &#8222;výkon Joeyho Tribbianiho byl obskurní&#8220;.)

Nemůžu se na autora zlobit, protože vím, že mé tehdejší hry byly opravdu [velmi obskurní textovky](http://www.oldplayer.cz/hrichy-mladi/). A tak to beru s nadhledem.

Druhé věc je, že se mi konečně podařilo sehnat kazetový přehrávač, a tak jsem jen tak z dlouhé chvíle začal digitalizovat svoje staré kazety. Až mě překvapilo, že to šlo docela slušně. Vzal jsem tedy &#8222;kazetu 22&#8220;, na které bylo napsáno, že tam jsou nějaké moje textovky &#8211; a opravdu, podařilo se mi zdigitalizovat tři kousky, na které jsem si ani nevzpomněl, že jsem je napsal! (A jsou neuvěřitelně obskurní!)

Dneska večer jsem pokračoval, tentokrát s kazetou 28. Ta byla ještě záhadnější, protože k ní nemám vůbec obal, takže jsem naprosto netušil, co na ní je. Tušil jsem jen, že tam budou asi nejlepší díla, protože to byla poslední číslovaná kazeta, kterou jsem používal, než jsem Spectrum odložil na skříň. O to větší bylo moje překvapení, když jsem našel hru Q-Bert.

Q-Berta určitě znáte. Pyramida, hopsá se a přebarvují čtverečky. Nějak [takhle](https://www.youtube.com/watch?v=PdnYB9o3IWU). Pamatuju se, že to byla jedna z posledních věcí, co jsem na Spectru dělal, jen tak pro radost, abych si zaprogramoval v Pikasmu. Ale doteď jsem měl zato, že jsem skončil někde ve stádiu přeložitelného zdrojáku, co můžu loadnout, zkompilovat a tajemným &#8222;RANDOMIZE USR bůhvíco&#8220; spustit.

Jenže teď jsem koukal, a já to tehdy dodělal. Opravdu. Ta hra má:

  * BASICový preloader. A ne ledajaký &#8211; je to nějaký ultrakrutý &#8222;protection system&#8220;, Spectristi budou vědět, o co šlo. Tenhle používal nějaké hodně divoké triky s chybou při tisku CHR$ 9, navíc je protkán nějakými veselými vzkazy pro ty, co to rozkódovávají. Taková byla tehdy móda, tak se nešklebte!
  * Obrazovku. Nic moc teda, ale má ji.
  * Loader. Počítal čas do nahrání a blikal kusem obrazovky. Jojo, loadery jsem měl rád. Někdy bych o nich mohl napsat článek, dělám si mentální poznámku&#8230;
  * Komprimovanou binárku. Pressor 3 se to, tuším, jmenovalo.
  * Úvodní obrazovku se scrollovacím textem, kde jsem pozdravoval kdekoho, od Patrika Raka po Busysofta.
  * Divoké efekty při pauze (WTF? Fakt jsem to tam udělal?)
  * Ve zdrojáku zase nějaké pozdravy pro crackery (ne, to nejsou ty sušenky, ale lidi, co rozbíjeli ochrany u programů).
  * Standa TELSOFT Telipský k té hře udělal nějaké sprity. S ním jsme vytvořili autorské sdružení podle vzoru Golden Triangle, ale nazvali jsme se &#8222;Invalid Coin&#8220;.
  * Hudbu pro AY (uslyšíte, pokud nahrajete na 128k verzi). Ano, je taky hodně obskurní.
  * Nějaké animace a efekty, u nichž si můžete tipnout, jaké přesně jsou&#8230;

A protože veřejný tlak naléhá, abych tyhle perly ducha zveřejnil, tak mu vyjdu vstříc. Ne, textovky budu ještě chvilku vstřebávat, ale Q-Berta vám pustím!

Prosím, tady je ke stažení jako [qbert.tap](http://retrocip.cz/zxs/games/qbert.tap).

(Samozřejmě to kvůli té ochraně vyžaduje hodně věrný emulátor &#8211; já použil [Zer0](http://ramtop.wordpress.com/), tam to šlape. Pokod vám to ve FUSE nefunguje, vypněte si všechny &#8222;fast load&#8220;, &#8222;accelerate loaders&#8220; a podobné vymož)