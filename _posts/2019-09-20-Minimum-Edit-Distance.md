---
layout: post
title: 8. Minimum Edit Distance-algoritmen  
---

Algoritmen för **minsta editeringsavstånd** är en **icke-probibalistisk algoritm** (alltså inte beroende av sannolikhetslära) som används för att beräkna **stränglikhet** utifrån antalet **editeringsoperationer** (**insättning** / eng. *insertion*: i, **borttagning** / eng. *deletion*: d och **ersättning** / eng. *substitution*: s) som behövs för att omvandla en sträng till en annan. Den är framför allt användbar i stavningskontrollssammanhang, där man vill korrigera isolerade ord som blivit felstavade genom att undersöka vilken sträng som ligger närmast till hands att byta ut den till. Jurafsky & Martin (2009:72-73) ger exemplet GRAFFE, som mer troligt är en felstavning av ordet GIRAFFE än GRAIL, eftersom **strängavståndet** (eng. *string distance*) mellan GRAFFE och GIRAFFE är mindre än det mellan GRAFFE och GRAIL.  

<p align="center">
<img src="/images/editdistance1.PNG" alt="strängar i alignment" width="240" height="180" border="10" /> <br>
.</p>

Genom rada upp orden bredvid varandra parallellt (eng. **alignment**) kan man se hur de skiljer sig från varandra genom att mappa tecken mot tecken. I bilden nedan, snodd från Jurafsky & Martin (2009), ser man hur orden INTENTION och EXECUTION skiljer sig åt genom fem editeringsoperationer. Det första i:et i INTENTION finns exempelvis inte alls representerat i EXECUTION-strängen, vilket får etiketten d (**deletion**). Man kan tillskriva olika kostnad eller vikt för varje editeringsoperation om man vill. För att beräkna **Levenshtein-distansen** mellan två strängar kan man helt enkelt ge varje operation kostanden 1, vilket skulle sätta distansen mellan strängarna i bilden ovan till 5. 

Minsta editeringsavstånd beräknas genom **dynamisk programmering** och distansmatriser. Det är en generell metod för lösning av optimeringsproblem och består av en grupp algoritmer som först introducerades av matematikern Richard Bellman på 50-talet. De använder tabellering för att kombinera lösningar på delproblem och därigenom finna lösningen på det större problemet. Andra algoritmer som grundar sig i dymanisk programmering är **Viterbi** (en utökning av minimum edit distance-algoritmen, som använder probibalistiska definitioner av operationerna), **CKY** och **Early**. I **Unix** kan man använda kommandot **diff** för jämföra två filer och skillnader i deras innehåll rad för rad.     