---
layout: post
title: 7. Porter-stemmern 
---

I kapitel 3 om finita transduktorer i *Speech and language processing* -- mer specifikt i avsnittet om **lexikonlösa finita transduktorer** -- nämns **Porter-stemmern** (eng. *The Porter Stemmer*). Det är inte första gången jag ser den figurera i en språkteknologisk text, så jag tänkte att det vore värt att undersöka lite närmare i en separat post. Men vad är då en stemmer? 

En stemmer är "ett program eller en algoritm som avgör den morfologiska roten till ett ord" [[1](https://sv.wikipedia.org/wiki/Stemmer)]. Enligt Wikipedia kallas de också **trunkerare** eller **trunkeringsalgoritm** på svenska, eftersom just trunkering av böjda ord till rotformen är dess huvuduppgift. Stemmrar är speciellt användbara i **informationshämtningssammanhang**, där sökningen efter ett visst begrepp förenklas om olika former av ett ord sammanflätas (eng. *conflate*) till ett. Porter ([[1]](https://cl.lingfil.uu.se/~marie/undervisning/textanalys16/porter.pdf)) ger i sin välciterade artikel [*An algorithm for suffix stripping*]  från 1980 ett exempel med orden CONNTECT, CONNECTED, CONNECTING, CONNECTION och CONNENCTIONS, där man genom att skala bort suffixen -ED, -ING, -ION och -S kan sammanfläta dem till formen CONNECT. Det här fungerar enligt Jurafsky & Martin (2009) bättre om informationshämtningen sker i mindre dokument där risken att sökordet förekommer i just det formatet i dokumentet.

<p align="center">
<img src="/images/porter1.png" alt="rigraf" width="480" height="360" border="10" /> <br>
Abstract till Martin Porters artikel från 1980.</p>

Eftersom stemmerns uppgift inom informationshämtning är att underlätta sökningen efter begrepp, är det inte alltid självklart när ett suffix bör klippas bort. Porter ger exemplet med RELATE och RELATIVITY, som är besläktade och har samma rotform, men som betydelsemässigt skiljer sig åt ganska mycket och används i olika sammanhang. Ett annat problem, kopplat till frånvaron av lexikon, är att Porter-stemmern inte kan avgöra om ändelsen -ER i WANDER ("vandra") är ett suffix eller en del av stammen, men ändå väljer att klippa bort -ER, vilket kan leda till orelevanta sökresultat kopplade till WAND ("[troll]stav"). Man skulle kunna lösa det genom att skriva till fler regler och undantag, men Porter (1980:213) skriver: "[t]here comes a stage in the development of a suffix stripping program, where the addition of more rules to increase the performance in one area of the vocabulary causes an equal degradation of performance elsewhere". 

Porters algoritm beskrivs på ett ingående men lättbegripligt sätt i artikeln från 1980. Orden genomgår olika regelkontroller kopplade till villor. Om exempelvis ordet CONFLATED har kapats ner till CONFLAT i första kontrollen (eftersom en av reglerna säger att ord som innehåller en vokal i stammen och slutar på -ED får en nulländelse: (*v*) ED --> ), så säger en annan regel i nästa kontroll att ord vars stammar slutar på -AT får ändelsen -ATE, så CONFLAT(ED) --> CONFLATE. Algoritmen ser också till att inte ta bort suffix när stammen anses vara för kort. Det innebär att ord som DERIVE inte förlorar sitt suffix, medan ordet DERIVATE däremot gör det. 

<p align="center">
<img src="/images/porter2.png" alt="rigraf" width="480" height="360" border="10" /> <br>
Ett urval av trunkeringsregler presenterade i Porters artikel.</p>

Porter visar också att hans algoritm presterar bättre än ett annat mer komplicerat system som vid tillfället användes för att söka en känd textsamling, *Cranfield 200 Collection*. Bara marginellt bättre, men det är ändå imponerande när man tar i beaktning att Porters algoritm skrevs med färre än 400 rader kod i prorammeringsspråket ([BCPL](https://en.wikipedia.org/wiki/BCPL)) (som för övrigt var ett språk som introducerade användingen av måsvingar för att separarera kodblock från varandra, och vars variantspråk B utgjorde grunden för programmeringsspråket C). Kanske är det också därför som Porters stemmer blev något av en standardalgoritm för trunkering i det engelska språket.   