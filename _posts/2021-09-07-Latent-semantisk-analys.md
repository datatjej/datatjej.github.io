---
layout: post
title: 53. Latent semantisk analys
---

Latent semantisk analys (eng. *latent semantic analysis*) (LSA) är en oövervakad metod för att analysera dolda ("latenta") relationer mellan dokument utifrån de ord som förekommer i dem - även känt som ämnesigenkänning (eng. *topic recognition*). 

På en term-dokument-matris med tillhörande frekvenser (dvs. hur många gånger varje ord i vokabuläret förekommer i varje dokument) utför man en [dimensionsreducering](https://datatjej.github.io/Dimensionsreduktion/) med hjälp av en teknik som kallas singulärvärdesuppdelning (eng. *singular value decomposition*) (SVD).

<p align="center">
<a href="https://www.youtube.com/watch?v=Fy0bF7u6W20" target="_blank"><img src="/images/latent_semantic_analysis.PNG" 
alt="A Trivial Implementation of LSA using Scikit Learn (2/5)" width="100%" height="auto" border="10" /></a><br>
Klicka på bilden för att komma till en videotutorial för hur man kan implementera LSA med scikit-learn.</p> 

En förklaring om exakt hur SVD fungerar får vänta tills jag hunnit läsa någon kurs i linjär algebra, men värt att notera är att de resulterande "ämnena" ibland kan vara svåra att tolka (Wikipedia-artikeln om LSA har ett bra exempel under [Limitations](https://en.wikipedia.org/wiki/Latent_semantic_analysis#Limitations)) och dessutom skilja sig lite från gång till gång. SVD/LSA är också beräkningsmässigt en ganska tung operation som kräver mycket minne. 

**Fun fact:** När man använder LSA i informationshämtningssammanhang brukar det istället kallas latent semantisk indexering (eng. *latent semantic indexing*) (LSI). 

**Referenser** <br>
- [Latent Semantic Analysis](https://en.wikipedia.org/wiki/Latent_semantic_analysis), Wikipedia, 07-09-2021.
- [A Trivial Implementation of LSA using Scikit Learn (2/5)](https://www.youtube.com/watch?v=Fy0bF7u6W20), YouTube, 07-09-2021.
- Föreläsningsslides från andra föreläsningen i kursen *Machine Learning for Statistical NLP* (LT2222), VT 2021.  