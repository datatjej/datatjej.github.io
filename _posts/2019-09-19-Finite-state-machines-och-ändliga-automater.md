---
layout: post
title: 5. Finite-state machines och ändliga automater
---

**Finite-state machines**, **finite-state automata** eller **ändliga automater** på svenska refererar till en typ av matematisk flödesmodell för att modellera *sekventiell logik* ([Lancelot Bors 2018](https://medium.com/@mlbors/what-is-a-finite-state-machine-6d8dec727e2c)). Det kan användas inom en rad olika omården för att beskriva regelbundna processer av olika slag ([freeCodeCamp](https://www.freecodecamp.org/news/finite-state-machines/) har ett exempel med en kaffemaskin), och även inom språkteknologin kan det användas för att modellera olika typer av text- och talprocessering (t.ex. tokenisering). 

Om man är bekant med [Chomsky-hierarki](https://datatjej.github.io/Generativ-grammatik-och-Chomsky-hierarki/) vet man kanske att den enklaste formen av grammatik är ett regelbundet språk som kan beskrivas med [reguljära uttryck](https://datatjej.github.io/Regulj%C3%A4ra-uttryck/). Ändliga automater kan användas för att implementera reguljära uttryck, och reguljära uttryck kan i sin tur användas för att beskriva en ändlig automat: *"any finite-state automation can be described with a regular expression"* (Jurafsky & Martin 2009:26). 

En ändlig automat representeras som en **riktad graf** (eng. *directed graph*), som enligt Wikipedia är en graf vars **bågar** (eng. *arcs*) *"...har en definierad riktning mellan de två **noder** (eng. *vertice*/*nodes*) som bågen förbinder, bågen är så att säga enkelriktad"*[[1]](https://sv.wikipedia.org/wiki/Riktad_graf). Inte helt lättbegripligt, men här är i alla fall en fin bild: 

<p align="center">
<img src="/images/directed.svg" alt="rigraf" width="480" height="360" border="10" /> <br>
Illustration av enkelriktad graf (Wikipedia 2019).</p>

Automaten består av ett antal **tillstånd** (eng. *states*) som representeras av noderna i grafen, och där tillstånd 0 är **startillståndet**. Tanken är att maskinen börjar från nod 0 (starttillståndet) och sedan kollar av varje tecken i en sträng. Om nästa tecken i strängen stämmer överens med den symbol som bågen till nästa tillstånd representerar, korsar den bågen över till nästa nod. Om stränginputen tagit slut när vi kommit till **slutnoden** i grafen (eng. *final state*/*accepting state*) har den lyckats matcha den eftersökta strängen. Om ett givet tecken inte accepteras av maskinen hamnar den automatiskt i ett **tomt tillstånd**, på engelska *fail state* eller *sink state*.

Ändliga automater delas in i **deterministiska** - där beteendet hos automaten beror på vilket tillstånd den befinner sig i och vilken teckeninput den har vid ett givet ögonblick - och **icke-deterministiska**. Den icke-determinstiska finita automataten har en eller flera noder där den själv måste välja väg. Nackdelen med det är att maskinen kan välja fel båge och neka (eng. *reject*) en viss input som den borde ha accepterat. 

Det finns **tre lösningar** på det problemet:
* **backup**: sätter en markör på flervalsnoden där det kan gå fel så att man kan gå tillbaka och välja rätt båge om det senare visade sig bli fel. 
* **look-ahead**: kikar på nästa input i förväg för att avgöra vilken väg som är rätt. 
* **parallellism**: kika på en alternativ bana varje gång vi kommer till en flervalsnod. 

Värt att notera är att vilken icke-deterministisk finit automat som helst kan omvandlas till en deterministisk finit automat.       