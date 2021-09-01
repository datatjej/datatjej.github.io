---
layout: post
title: 38. Noisy channel-modellen för stavningskontroll
mathjax: true
---

**Noisy channel-modellen** (NCM) är ett formaliserat ramverk med många applikationsområden (bland annat maskinöversättning och taligenkänning), men här kommmer fokus att ligga på stavningskontroll. NCM betraktar det felstavade ordet som en **förvrängd** (eng. *distorted*) form av originalordet som skickats genom en "noisy" (brusig) kanal.  Den förvrängda formen kan innebära att enstaka bokstäver bytts ut, försvunnit eller tillkommit.

Genom att försöka bygga en modell av den brusiga kanalen, och skicka alla ord i språket genom den här modellen, kan vi då hitta det originalord vars förvrängda form motsvarar det felstavade ordet. Modellen kan förstås som en form av **bayesiansk inferens**, där det felstavade ordet är en observation *x* och stavningskontrollen ska hitta originalordet *w* som är mest troligt givet observationen: 

$$ ŵ = argmax P(w|x) $$

...där *ŵ* avser det uppskattade rätta ordet av alla möjliga ord i vokabuläret *V*.

Sätter man in den ekvationen i [Bayes teorem](https://datatjej.github.io/Bayes-teorem/):

$$ P(a|b) = \frac{P(b|a)*P(a)}{P(b)}$$

..får man:

$$ ŵ = argmax \frac{P(x|w)*P(w)}{P(x)}$$

...vilket kan förenklas till:

$$ ŵ = argmax P(x|w)*P(w) $$

...eftersom det hela tiden är samma observation $ x $ vi använder. Och istället för att undersöka varje ord i hela vokabuläret kan vi rikta in oss på några **troliga kandidater** och välja ut den mest troliga givet argmax. Listan över kandidater kan tas fram genom att applicera [minimum edit distance-algoritmen](https://datatjej.github.io/Minimum-Edit-Distance/). Den algoritmen kan hantera instättning, borttagning och ersättning av bokstäver. Men för att också hantera bokstäver som bytt plats med varandra finns en annan liknande algoritm: [Damerau-Levenshtein distance-algoritmen](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance).

När man väl har listan över tänkbara kandidater kan sannolikheten $ P(w) $ beräknas genom exempelvis det enskilda ordet frekvens i vokabuläret (unigram) eller deras frekvens givet föregående ord (bigram), osv. Sannolikheten $ P(x|w) $ (aka *kanalmodellen* eller *felmodellen*) kan beräknas genom att ta en rad olika aspekter i beaktning, t.ex. keyboard-layouten eller de typiska felen hos personen som skriver. Man kan också referera till en korpus med felstavade ord och i så fall beräkna sannolikheten för $ P(endå|ändå) $ genom att undersöka hur många gåner bokstaven *ä* ersatts med *e*. I så fall kan man ställa upp en **förvirringsmatris** (eng. *confusion matrix*) med samma storlek som alfabetet och räkna hur många gånger en viss bokstav ersatts med en annan.

Brill & Moore (2000) föreslår en mer sofistikerad version av noisy channel-modellen för stavningskontroll som inte bara tar enskilda bostäver i beaktning, utan också kombinationer och stavelser. Man poängterar att även om enskilda bokstavsfel står för en hög andel av alla stavfel, så är det ibland rimligare och mer effektivt att jämföra delar (eng. *partitions*) av en sträng, t.ex. $ P(ant|ent) $ istället för $ P(a|e) $ i felstavningen *confidant*. De tar även understrängens **position** i ordet i beaktning: *"[p]eople rarely mistype* antler *as* entler*, but often mistype* reluctant *as* reluctent*"* (Brill & Moore 2000:3).  

Referenser:<br>
- Eric Brill och Robert C. Moore. 2000.  *An Improved Error Model for Noisy Channel Spelling Correction*. I Proceedings of the 38th Annual Meeting of the Association for Computational Linguistics, sidorna 286–293, Hong Kong. Association for Computational Linguistics. https://www.aclweb.org/anthology/P00-1037/<br>
- Dan Jurafsky och James H. Martin. 2020. *Speech and Language Processing*. Utkast till 3:e upplagan. Kapitel B: https://web.stanford.edu/~jurafsky/slp3/B.pdf
