---
layout: post
title: 5. Finite-state machines och ändliga automater
---

**Finite-state machines**, **finite-state automata** eller **ändlig/finita automater** på svenska är (tydligen!) en viktig del i datalingvistiken. Det refererar till matematiska beräkningsmodeller som bland annat används för att implementera reguljära uttryck. Men omvänt uttryckt så kan reguljära uttryck även användas för att beskriva en finit automat: *"any finite-state automation can be described with a regular expression"* (Jurafsky & Martin 2009:26). Förvirrande? Vi har bara börjat!

En finit automat kan representeras som en **riktad graf** (eng. *directed graph), som enligt Wikipedia är en graf där vars **bågar** (eng. *arcs*) "...har en definierad riktning mellan de två noder [eng. *vertice*/*node*] som bågen förbinder, bågen är så att säga enkelriktad"[[1]](https://sv.wikipedia.org/wiki/Riktad_graf). Inte helt lättförstått, men här är i alla fall en fin bild: 

<p align="center">
<img src="/images/directed.svg" alt="rigraf" width="480" height="360" border="10" /> <br>
Illustration av enkelriktad graf (Wikipedia 2019).</p>'

Automaten består av ett antal **tillstånd** (eng. *states*) som representeras av noderna i grafen, och där tillstånd 0 är **startillståndet**. Tanken är att maskinen börjar från nod 0 (starttillståndet) och sedan kollar av varje tecken i en sträng. Om nästa tecken i strängen stämmer överens med den symbol som bågen till nästa tillstånd representerar, så korsar den bågen över till nästa nod. Om stränginputen tagit slut när vi kommit till **slutnoden** i grafen (eng. *final state*/*accepting state*) så har den lyckats matcha den eftersökta strängen. Om ett givet tecken inte accepteras av maskinen hamnar den automatiskt i ett **tomt tillstånd**, på engelska *fail state* eller *sink state*.

Finita automater delas in i **deterministiska** - där beteendet hos automaten beror på vilket tillstånd den befinner sig i och vilken teckeninput den har vid ett givet ögonblick - och **icke-deterministiska**. Den icke-determinstiska finita automataten har en eller flera noder där den själv måste välja väg. Nackdelen med det är förstås att maskinen kan välja fel båge och neka (eng. *reject*) en viss input som den borde ha accepterat. 

Det finns **tre lösningar** på det problemet:
* **backup**: sätter en markör på flervalsnoden där det kan gå fel så att man kan gå tillbaka och välja rätt båge om det senare visade sig bli fel. 
* **look-ahead**: kikar på nästa input i förväg för att avgöra vilken väg som är rätt. 
* **parallellism**: kika på en alternativ bana varje gång vi kommer till en flervalsnod. 

Värt att notera är at vilken icke-deterministisk finit automat som helst kan omvandlas till en deterministisk finit automat.       