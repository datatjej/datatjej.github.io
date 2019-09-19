---
layout: post
title: 6. Finite-state transducers och finita transduktorer
---

**Finita transduktorer** (FT) är något som behandlas i kapitel 3 av *Speech and language processing*, i anslutning till **morfologisk parsning**: "[p]arsing means taking an input and producing some sort of linguistic structure for it" (Jurafsky & Martin 2009:45). För att bygga en morfologisk parser krävs:

- **Lexikon**: en lista över stammar och affix och grundläggande information om dem.
- **Morfotaktik**: information om vilka morfem som kan följa andra morfem i ett ord.
- **Stavningsregler**: till exempel kunskapen att engelska ord som slutar på -y får -ies i plural.

FT är ett slags finita automater som kan mappa mellan två uppsättningar av symboler och därför käner igen strängar i **par**. De har en mer generell funktion än finita automater: "...where an FSA defines a formal language by defining a set of strings, an FST defines a *relation* between sets of strings" (Jurafsky & Martin 2009:57). Finita transduktorer får därigenom flera roller: 1) **igenkännare** (den accepterar bara strängpar som uppfyller en viss form av avvisar andra) 2) **generator** (matar ut strängpar), 3) **översättare** (läser in en sträng och matar ut en annan), 4) **uppsättningsrelaterare** (kan beräkna relationer mellan stränguppsättningar).

**Tre egenskaper** karaktäriserar FT:

- **Union**
- **Inversion**: transduktorinversion innebär att etiketterna på input och output byter plats, så istället för att mappa från input A till output B, blir det från input B till output A. 
- **Komposition**: om en transduktor T1 mappar från A1 till B1, och en transduktor T2 från A2 till B2, då mappar T1 o T2 från A1 till B2. Komposition gör det möjligt att ersätta två transduktorer med en mer kraffull transduktor. 

Ett annat begrepp som verkar värt att känna till är **projektion**, som avser den finita automat som skapas genom att extrahera enbart en sida av relationen hos en FT. 

Finita tranduktorer är **ofta icke-deterministiska** genom att en given input kan ge upphov till många olika output, något som gör dem långsamma. En typ av transduktor är dock **deterministisk** i sin input, nämligen **sekventiella transduktorer**. Varje tillstånd i en sekventiell transduktor leder som mest till en båge ut från det tillståndet. En generaliserad variant av den sen sekventiella transduktorn är den **subsekventiella transduktorn**, som genererar ytterligare en output-sträng i sluttillståndet, som sedan konkateneras till den hittills producerade outputen. Såväl sekventiella som subsekventiella transduktorer är **snabba** (processeringstiden är proportionell till antalet symboler i inputen, och inte till antalet tillstånd) och det finns **effektiva algoritmer** för dem. 

Varken sekventiella eller subsekventiella transduktorer kan dock hantera **tvetydighet** (eng. *ambiguity*), eftersom varje input bara kan omvandla till en output. Därför finns det en utökning av subsekventiella transduktorer som kan hantera tvetydighet utan att tappa i effektivitet, nämligen den **p-subsekventiella transduktorn**. Dessa gör sig lämpliga för att representera ordböcker, morfologiska och fonologiska regler liksom syntaktiska begränsningar. 

Morfologisk parsning med FT sker genom **finit morfologi** (eng. *finite-state morphology*), där man representerar ord som en länk mellan den **lexiala nivån** (eng. *lexical level*) -- som representerar konkatenering av morfem som bygger upp ordet -- och **ytnivån** (eng. *surface level*), som representerar konkateneringen av bokstäver som ordet består av. Jämför med exemplet *cats* i bilden nedan (som leder till en introduktionsvideo om FT om du klickar på den). 

 <p align="center">
<a href="https://www.youtube.com/watch?v=SkGM5vujvls" target="_blank"><img src="/images/fst.PNG" 
alt="NLP: Finite State Transducer for Morphological Parsing" width="480" height="360" border="10" /></a></p>

Som framgår ur bilden så motsvaras karaktärerna i det övre, lexikala bandet ofta av exakt samma karaktär i det undre ytbandet: "c" svarar mot "c", "a" mot "a" och "t" mot "t". Den engelska beteckningen för detta är *default pairs*, men på svenska kanske man skulle kunna prata om **baspar** eller **standardpar**. Ofta vill man kombinera två transduktorer med varandra (eng. **cascade**), så att output från en transduktor blir input till den andra, t.ex. när man kombinerar en lexikal FT med en ortografisk FT. Det fungerar särskilt väl vid generering av strängar, men vid parsning av befintliga strängar måste **disambiguering** ske genom att ta kringliggande ord i beaktning.  