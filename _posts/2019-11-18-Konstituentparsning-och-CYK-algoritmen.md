---
layout: post
title: 11. Konstituentparsning och CYK-algoritmen
---

Tillbringar måndagskvällen med att läsa kapitel 13 om **konstituentparsning** (eng. *constituency parsing*) i Jurafsky & Martin (2009). Man nämner två ambiguitetsproblem kopplade till den här typen av parsning: 1) attachment ambiguity, och 2) coordination ambiguity. 

**Attachment ambiguity** avser de tillfällen då en konstituent kan kopplas samman till ett parsträd på fler än ett ställe. ETT vanligt exempel på detta är meningar av typen "Mary såg mannen med teleskopet", där *med teleskopet* kan tillskrivas antingen verbfrasen *såg* eller substantivfrasen *mannen*. **Coordination ambiguity** avser istället tvetydigheten i fraser sammanbundna av konjunktioner som *och*, t.ex. *blinda katter och hundar*, som antingen kan parsas som *[blinda [katter och hundar]]* eller *[blinda katter] och [hundar]*. 

Den här typen av tvetydighet löses med hjälp av **syntaktisk disambiguering**, som kräver "[...] statistical, semantic and contextual knowledge sources that vary in how well they can be integreated into parsing algorithms" (Jurafsky & Martin 2009: KÄLLA!). 

En känd parsningsalgoritm är **CYK** (förkortning för upphovsmännen Cocke-Younger-Kasami). Den är en del av den dynamiska progammeringen, som i kort går ut på att angripa problem genom att bryta ner dem i underproblem vars lösningar sparas i tabellform. 

Grammtiken som används med CYK måste vara i **Chomskys normalform**, vilket innebär en binär grammatik av typen *A --> B C* (där *A,B,C* är icke-terminala symboler som representerar fraser, t.ex. NP, VP, PP...) eller *A --> w* (där *w* är en terminal symbol, dvs ett ord). Vilken kontextfri grammatik som helst kan omvandlas till Chomskys normalform utan att något går förlorat. 

**Antalet tänkbara parsningar** för en given sträng *w* ökar exponentiellt med längden av *w* [(Har-Peled & Parthasarathy 2009)](https://courses.grainger.illinois.edu/cs373/sp2009/lectures/lect_15.pdf). Tanken med CYK är att man istället för att undersöka alla möjliga parsträd av en sträng enbart fokuserar på understrängarna. En sträng *w* med längden *n* har då <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\sum_{k=1}^{n}&space;(n-k&plus;1)&space;=&space;\frac{n(n&plus;1)}{2}&space;=&space;\binom{n&plus;1}{2}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\sum_{k=1}^{n}&space;(n-k&plus;1)&space;=&space;\frac{n(n&plus;1)}{2}&space;=&space;\binom{n&plus;1}{2}" title="\sum_{k=1}^{n} (n-k+1) = \frac{n(n+1)}{2} = \binom{n+1}{2}" /></a> understrängar, vars möjliga parsningar summeras i en tabell ur vilken vi sen kan extrahera en slutgiltig parsning. 

<p align="center">
<a title="Trougnouf [CC BY-SA 4.0 (https://creativecommons.org/licenses/by-sa/4.0)], via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:CYK_algorithm_animation_showing_every_step_of_a_sentence_parsing.gif"><img width="512" alt="CYK algorithm animation showing every step of a sentence parsing" src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f5/CYK_algorithm_animation_showing_every_step_of_a_sentence_parsing.gif/512px-CYK_algorithm_animation_showing_every_step_of_a_sentence_parsing.gif"></a><br>Trougnouf [CC BY-SA 4.0 (https://creativecommons.org/licenses/by-sa/4.0)]</p>

Algoritmen arbetar nerifrån och upp (eng. **bottom-up**) genom att först undersöka giltigheten hos var och en av de allra minsta understrängarna (len 1), vilket kan motsvara enskilda ord eller tecken. Den går därefter vidare till att undersöka giltigheten hos varje understräng av len 2, sedan len 3, och så vidare. En giltig sträng är en sträng vars olika komponenter på något plan uppfyllerna reglerna i grammatiken som algoritmen jobbar utifrån, och kännetecknas av startsymbolen S i toppen av tabellen.     

Källor:
    
