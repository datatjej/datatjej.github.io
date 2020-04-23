---
layout: post
title: 20. Lambdakalkyl med lite curry
---

Inom kursen **komputationell semantik** (LT2213) konfronterades vi i förra veckan med något nytt och läskigt: **lambdakalkyl** (eng. *lambda calculus*). Sedan dess har jag skjutit upp att sätta mig in i det på allvar, men eftersom vi har en inlämningsuppgift på ämnet som strax måste in tänkte jag ta kvällen i akt och läsa på så mycket som möjligt innan jag går och lägger mig. Nu kör vi!

Vi börjar med lite historia. Lambdakalkyl definieras på svenska [Wikipedia](https://sv.wikipedia.org/wiki/Lambdakalkyl) som ett "formellt system som skapades för att undersöka funktioner och rekursion". Som upphovsman räkans den amerikanske matematikern och logikern [**Alonzo Church** (1903-1995)](https://sv.wikipedia.org/wiki/Alonzo_Church) som arbetade fram systemet på 1930-talet. Han har också gjort sig känd för [**Church-Turings hypotes**](https://sv.wikipedia.org/wiki/Church-Turings_hypotes), som säger att en matematisk funktion är **effektivt beräkningsbar** om och endast om den kan beräknas med hjälp av en algoritm på en Turing-maskin. 

Tidigare hade man inte haft en bra beskrivning av effektiv beräkningsbarhet (mer än om den kunde utföras med penna och papper), men framväxten av tre olika formella beräkningssystem -- 1) Gödels och Herbrands generella rekursiva funktioner, 2) Churchs lamdakalkyl och 3) Turings Turingmaskin -- gjorde att man kunde fastställa sambandet i Church-Turings hypotes. Det visade sig nämligen att dessa tre system hänger ihop; en funktion som är lambda-beräkningsbar är också alltid Turing-kompatibel och även generellt rekursiv. 

Men tillbaka till där vi började. Vad ska lambdakalkyl överhuvudtaget vara bra för inom lingvistiken? Videon nedan ger en lysande och koncis förklaring, men i kort så verkar det handla om att enklare kunna utvärdera sanningsvärden i meningar (vilket är ju kärnan av semantiken som vetenskapsdisciplin) genom att ta bryta upp stora, komplicerade semantiska uttryck/funktioner i mindre sådana, så kallad **currying**. Detta är exempelvis hjälpsamt när man har ett transitivt verb i formen av ett predikat inom första ordningens logik (FOL), t.ex. *gilla(Anna, äpplen)* (för "Anna gillar äpplen"), och vill kunna sätta in objektet *äpplen* i funktionen innan man vet vem subjektet är.
          
<p align="center">
<a href="https://www.youtube.com/watch?v=BwWQDzXBuwg"><img src="/images/lambdakalkyl.PNG" 
alt="How Can One Greek Letter Help Us Understand Language? Lambda Calculus" width="100%" height="100%" border="10" /></a></p>

 
*23/4: fortsätter skriva lite senare*