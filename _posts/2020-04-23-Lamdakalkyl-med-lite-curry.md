---
layout: post
title: 20. Lambdakalkyl med lite curry
---

Inom kursen **komputationell semantik** (LT2213) konfronterades vi i förra veckan med något nytt och läskigt: **lambdakalkyl** (eng. *lambda calculus*). Sedan dess har jag skjutit upp att sätta mig in i det på allvar, men eftersom vi har en inlämningsuppgift på ämnet som strax måste in tänkte jag ta kvällen i akt och läsa på så mycket som möjligt innan jag går och lägger mig. Nu kör vi!

<p align="center">
<img src="/images/gillar.png" alt="Exempel på lambdauttryck" width="100%" height="auto" border="10" /><br>
</p>

Vi börjar med lite historia. Lambdakalkyl definieras på svenska [Wikipedia](https://sv.wikipedia.org/wiki/Lambdakalkyl) som ett "formellt system som skapades för att undersöka funktioner och rekursion". Som upphovsman räkans den amerikanske matematikern och logikern [Alonzo Church (1903-1995)](https://sv.wikipedia.org/wiki/Alonzo_Church) som arbetade fram systemet på 1930-talet. Han har också gjort sig känd för [Church-Turings hypotes](https://sv.wikipedia.org/wiki/Church-Turings_hypotes), som säger att en matematisk funktion är **effektivt beräkningsbar** om och endast om den kan beräknas med hjälp av en algoritm på en Turing-maskin.<sup>[1](#fotnot1)</sup> Men det var en annan amerikan, [Richard Montague (1930-1971)](https://en.wikipedia.org/wiki/Richard_Montague), som introducerade lambdakalkylen till lingvistiken.  

Så vad ska lambdakalkyl överhuvudtaget vara bra för inom språkteknologin? Videon nedan ger en bra introduktion, men i kort så handlar det om att enklare kunna utvärdera sanningsvärden i meningar (vilket är ju kärnan av semantiken) genom att ta bryta upp stora semantiska uttryck/funktioner i mindre sådana, så kallad **currying**. 

När man har en mening i naturligt språk, t.ex. "Anna gillar äpplen" så kan den delas upp i ett syntaktiskt träd där nominalfrasen (NP) har en undernod, *Anna*, och verbfrasen (VP) "gillar äpplen" två stycken: *gillar* och *äpplen*. När vi sedan vill konstruera vår semantiska representation av meningen börjar vi från undernoderna i VP:t och märker på en gång att predikatet *gillar* kräver två variabler, en som gillar och något som gillas: gillar(x,y). 

För att lösa detta beräkningsmässigt bryter vi ut x:et och y:et till ett lambdauttryck, så att vi kan hålla till godo innan vi når objektet *äpplen* och nominalfrasen *Anna*: λy λx.(gillar(x,y). Placeringen av lambda-y innan lambda-x innebär att y:et är den första variabeln som pluggas in i funktionen, som då får formen λx.gillar(x,äpplen), innan vi går vidare och pluggar in subjektvariabeln x.    
                   
<p align="center">
<a href="https://www.youtube.com/watch?v=BwWQDzXBuwg"><img src="/images/lambdakalkyl.PNG" 
alt="How Can One Greek Letter Help Us Understand Language? Lambda Calculus" width="100%" height="100%" border="10" /></a></p>

<a name="fotnot1">1</a>:<font size="2">Tidigare hade man inte haft en bra beskrivning av effektiv beräkningsbarhet (mer än om den kunde utföras med penna och papper), men framväxten av tre olika formella beräkningssystem -- 1) Gödels och Herbrands generella rekursiva funktioner, 2) Churchs lamdakalkyl och 3) Turings Turingmaskin -- gjorde att man kunde fastställa sambandet i Church-Turings hypotes. Det visade sig nämligen att dessa tre system hänger ihop; en funktion som är lambda-beräkningsbar är också alltid Turing-kompatibel och även generellt rekursiv.</font>