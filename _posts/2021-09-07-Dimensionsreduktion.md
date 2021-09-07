---
layout: post
title: 52. Dimensionsreduktion
---

Dimensionsreduktion (eng. *dimensionality reduction*) handlar om att reducera hög-dimensionella vektorrum till låg-dimensionella diton så att dolda relationer mellan dimensionerna -- eller snarare dolda relationer mellan de lingvistiska egenskaperna/attributen (eng. *features*) som varje dimension består av -- blir mer framträdande. 

Om man har ett vektorrumsmodell för dokument av typen *bag of words* så får varje dokumentvektor lika många dimensioner som antalet ord i det samlade vokabuläret. Men eftersom det bara är dimensionerna vars ord faktiskt förekommer i dokumentet som får ett värde (t.ex. råfrekvens, binärt värde eller tf idf-värde) blir varje dokumentvektor väldigt gles (eng. *sparse*).

Glesa, hög-dimensionella vektorer ligger till grund för något som kallas **dimensionalitetsförbannelsen** (eng. *curse of dimensionality*) och som bland annat innebär att det blir svårare att gruppera objekt utifrån särskiljande egenskaper eftersom dessa blir mer svårhittade. 

Dimensionalitetsförbannelsen gör det också svårare att applicera analysmetoder som kräver statistisk signifikans, eftersom *"[i]n order to obtain a statistically sound and reliable result, the amount of data needed to support the result often grows exponentially with the dimensionality"* ([Wikipedia](https://en.wikipedia.org/wiki/Curse_of_dimensionality)).

Det finns flera sätt att generera låg-dimensionella vektorrum:<br>
- genom valet av *features* (t.ex. testa sig fram och se vad som ger bäst resultat)
- genom tensorfaktorisering 
    - vektorrummet faktoriseras först i flera låg-dimensionella matriser
    - rader och kolumner väljs ut på basis av någon viktighetsheuristik
    - Ex: latent semantisk indexering, principalkomponentanalys
- diskriminativa tränings-/maskininlärningsansatser
    - iterativt uppdatera låg-dimensionella vektorer
    - uppdatera dem på basis av att kunna rekonstruera "objektiv" data
    - Ex: autoencoders, djupinlärning
  
Nackdelen med dimensionsreduktion är att de resulterande vektorerna oftast blir svårbegripliga för människor att läsa och tolka rakt av.

**Referenser:**<br>
- [Dimensionality Reduction](https://en.wikipedia.org/wiki/Curse_of_dimensionality), Wikipedia, 07-09-2021.
- [Curse of Dimensionality](https://en.wikipedia.org/wiki/Curse_of_dimensionality), Wikipedia, 07-09-2021.
- Föreläsningsslides från andra föreläsningen i kursen *Machine Learning for Statistical NLP* (LT2222), VT 2021.   