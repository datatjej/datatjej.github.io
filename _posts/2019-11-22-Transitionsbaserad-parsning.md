---
layout: post
title: 13. Transitionsbaserad dependensparsning
---
**Transitionsbaserad dependensparsning** (eng. *transition-based parsing*) har sitt ursprung i något som kallas **shift-reduce-parsning**. Inom shift-reduce använder man sig av en kontextfri grammatik, en stack, samt den ordföljd som ska parsas som input. Orden flyttas (genom "shift") en efter en över till stacken, och de två översta orden jämförs mot högersidan i grammatikens regler. När en matchning hittats tas orden bort ("reduceras") från stacken och ersätts av den icke-terminala symbolen i vänsterledet av regeln man matchat mot. 

Inom dependensparsning frångår man användandet av en grammatik till förmån för en **guide** eller ett **orakel** som tränats upp genom maskininlärning på annoterade dependensträd. Och istället för att ersätta orden med en icke-terminal symbol tillskriver man dem dependesrelation. **Arc-standard** är den transitionsbaserade parsningsalgoritm som ligger närmast shift-reduce-ansatsen. Där erätts reduce-steget av två andra möjliga steg, **left-arc** (vänsterbåge) och **right-arc** (högerbåge). 

Om oraklet bedömmer att de två ord som för närvarande ligger nägst högst upp i stacken har en dependensrelation till varandra så tillskriver det ordet som ligger näst högst upp i stacken en vänster- eller högerbåge. Vid vänsterbåge tas huvudet (ordet näst högst upp i stacken) bort, och vid högerbåge försvinner istället dependenten. Därefter matas ytterligare ett ord över till stacken från **bufferten** (inputlistan med ord) och jämförs mot det som nu ligger näst högst upp. Om oraklet bedömer att ingen dependensrelation föreligger, matar den in ytterligare ett ord från bufferten via **shift** och letar matchningar tills bufferten och stacken är tömd.

Det som är fördelaktigt med arc-standard-algoritmen är att den är snabb och enkel att implementera, vilket beror på att den är **girig** (eng. *greedy*) och väljer första bästa dependensrelation den hittar istället för att testa en massa olika parsningsvarianter eller gå tillbaka (backtracking) om den gör fel. Nackdelen är dock att högerbågarna tar längre tid att deklarera om det ligger några andra ord mellan huvudet och dependenten som först måste hanteras. I lite längre meningar kan detta leda till felaktiga parsningar, något som delvis kan motverkas genom att använda **arc-eager-algoritmen** istället. 

Arc-eager tillåter ord med högerledsrelation att kopplas ihop med sin dependent tidigare genom att parsern bara fokuserar på ett ord i stacken åt gången och jämför det mot det ord som ligger högst upp i bufferten. Om högerledsrelation föreligger flyttar den över dependenten från bufferten till stacken istället för att ta bort den. I slutet används en **reduce**-operator för att ta bort ord högst upp i stacken. 

På nätet finner man en hel del presentationer som i tabellform förklarar arc-standard och arc-eager-algoritmerna steg för steg. Men jag rekommenderar videon nedan för en begriplig redogörelse av den närbesläktade shift-reduce-modellen:

<p align="center">
<a href="https://www.youtube.com/watch?v=f5-hTA9hA3s"><img src="/images/shiftreduce.PNG" 
alt="Dependency Parsing: Shift-Reduce Models" width="600" height="360" border="10" /></a></p>
