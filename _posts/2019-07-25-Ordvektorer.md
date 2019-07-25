---
layout: post
title: Ordvektorer, AKA "You shall know a word by the company it keeps"[^*]
---

Häromdagen stötte på jag begreppet **word2vec** och var tvungen att luska i vad det innebär. Enligt den engelskspråkiga [Wikipedia-artikeln](https://en.wikipedia.org/wiki/Word2vec) refererar det till en grupp datamodeller som används för att generera s.k. **"word embeddings"**, d.v.s. ord som vektorer av reella tal, utifrån träningsdata. Vektorer är matematiska storheter som har såväl storlek som riktning (till skillnad från skalära storheter som enbart har en storlek, t.ex. temperatur eller ljusstyrka)[[1](https://sv.wikipedia.org/wiki/Vektor)]. 

Det som är bra med ord i vektorformat är att det gör det enklare att identifiera och rekonstruera deras lingvistiska kontext, d.v.s. de ord som de tenderar att förekomma tillsammans med. På så sätt kan man också kvantifiera och kategorisera semantiska likheter mellan ord och fraser, vilket är just vad den distributionella semantiken sysslar med. Det är till stor nytta i en rad olika NLP-tillämpningar, som sökningar och maskinöversättning, men även i områden helt utanför språket där man vill kunna hitta mönster i data[[2](https://skymind.ai/wiki/word2vec)].  

Word2vec är utvecklat och patenterat av den tjeckiske datorforskaren Tomáš Mikolov och hans kollegor vid Google. Det inbegriper två modeller: continuous bag of words (CBOW) - som används för att idenfitiera ett visst ord utifrån den givna kontexten - och skip-gram-modellen (SGNS), där man istället utfrån i ett givet ord försöker utröja kontexten [[3](https://www.quora.com/Are-n-gram-models-one-hot-encoding-and-word2vec-different-types-of-word-representations-and-word-vectors)]. 

I en artikel från 2013 beskriver upphovsmännen hur deras modeller kan tränas upp för att urskilja bland annat semantiska mönster av typen: a är för b vad c är för d [[4](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/rvecs.pdf)]. Om man vet att Madrid är huvudstaden i Spanien och vill veta vad huvudstaden i Frankrike är, kan man alltså utifrån ekvationen x = "Madrid"-"Spanien"+"Frankrike" få ut "Paris", eftersom cosinus-avståndet mellan vektorerna "Madrid" och "Spanien" är ungefär detsamma som mellan "Paris" och "Frankrike".   

Här är en inspelning från Mikolovs presentation om neuronnätverk och NLP vid Brnos tekniska universitet (2016): 

<p align="center">
<a href="https://www.superlectures.com/vgs-it/neural-networks-for-natural-language-processing"><img src="/images/wordvectors.PNG" 
alt="Tomáš Mikolov: Neural Networks for Natural Language Processingy" width="480" height="360" border="10" /></a></p>

Och [här](http://vectors.nlpl.eu/explore/embeddings/en/) är ett roligt verktyg att hitta semantiska relationer mellan ord på engelska och norska. 


[^*]: Citat av brittiske lingvisten John Rupert Firth (1890-1960). 
