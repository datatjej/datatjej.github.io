---
layout: post
title: 46. Maskininlärning, djupinlärning och AI
---

I den här texten tänkte jag försöka reda ut skillnaden mellan maskininlärning (ML), djupinlärning (DL) och artificiell intelligens (AI).

**Maskininlärning** är ett stort forskningsområde (som även innefattar djupinlärning) och som i kort går ut på att lära datorer att hitta mönster i data som sedan kan generaliseras till osedd data. ML kan delas in i *övervakad* och *icke-övervakad inlärning*, där den förra använder data som annoterats med de kategorier eller klasser man vill kunna förutspå, medan den senare använder ouppmärkt data. Övervakad inlärning är vanligast både inom ML och DL, men icke-övervakad inlärning används för bland annat klusteranalys och inte minst för att skapa ordinbäddningar.

Konventionell maskininlärning har länge använt sig av handplockade *features* (ofta utvalda av en domänexpert) för att träna datamodeller. Inom t.ex. sentimentanalys kan det innebära att man förlitar sig på närvaron av specifika nyckelord, emojis, punktuation, negation m.m. för att träna upp en modell att klassificera en recension som positiv, negativ eller neutral [[1](https://www.researchgate.net/publication/288013084_Coooolll_A_Deep_Learning_System_for_Twitter_Sentiment_Classification)]. Inom datorseendefältet kan handplockade features för att upptäcka cancerceller i tredimensionella röntgenbilder vara exempelvis bildintensitet, topologisk struktur och textur [[2](https://www.nature.com/articles/s41598-020-77264-y)]. Den här typen av ML dominerade länge och handplockade features kan än idag vara värdefulla när man inte har så mycket annoterad data att tillgå [[2](https://www.nature.com/articles/s41598-020-77264-y)]. Men de kan vara tidskrävande att ta fram och kan dessutom missa mer komplexa mönster i datat. 
  
**Djupinlärning** är en gren av ML där datorn själv får undersöka rådatat för att upptäcka värdefulla mönster som kan användas som prediktorer i klassificeringen. DL-metoder är "djupa" efter som de består av flera icke-linjära neuronlager som successivt bidrar till en mer komplex representation [[3](https://www.nature.com/articles/nature14539)]:

> "An image, for example, comes in the form of an array of pixel values, and the learned features in the first layer of representation typically represent the presence of absence of edges at particular orientations and locations in the image. The second layer typically detects motifs by spotting particular arrangements of edges, regardless of small variations in the edge positions" (LeCun et al. 2015).  

DL drar alltså nytta av det faktum att features på högre nivå ofta är uppbyggda av features på lägre nivå. Precis som objekt i en bild är uppbyggda av kanter, streck och konturer i specifika kombinationer, så är exempelvis meningar i naturligt språk uppbyggda av ord, stavelser, bokstavstecken, fonem och foner.    

**Artificiell intelligens** beskrivs i den engelskspråkiga [Wikipedia-artikeln](https://en.wikipedia.org/wiki/Artificial_intelligence) som intelligens uppvisad av maskiner. Men i vår AI-kurs i MLT-programmet har AI snarare handlat om s.k. "situated agents", dvs program eller robotar som genom sensorer tillåts interagera med sin omgivning. Ta till exempel en objektsklassificerare som lär sig nya objekt genom en videobild, eller en robot som med hjälp av människors instruktioner lär sig hitta nya färdvägar eller skapa inre represenationer av en kontorsbyggnad och dess olika rum. 

Ofta brukar skillnaden mellan AI, ML och DL illustreras som tre överlappande cirklar där AI är den yttersta cirkeln, med ML som inre cirkel och underfält samt DL som allra innersta cirkel och underfält till ML. 

<p align="center">
<img src="/images/ml_som_underfält_till_ai.jpg" alt="Förhållandet mellan AI, ML och DL" width="auto" height="auto" border="10" /><br>
<font size="2"><a href="https://commons.wikimedia.org/wiki/File:Fig-X_All_ML_as_a_subfield_of_AI.jpg">Yakoove</a>, <a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>, via Wikimedia Commons</font>  
</p>


Detta skulle innebära att all form av ML (och DL) också är AI. Men utifrån definitionen av AI som jag tar med mig från programmet skulle deras inbördes ordning snarare se ut så här:  

<p align="center">
<img src="/images/ai_ml_dl.jpg" alt="Förhållandet mellan AI, ML och DL" width="auto" height="auto" border="10" /><br>
<font size="2"><a href="https://commons.wikimedia.org/wiki/File:Fig-y_Part_of_ML_as_subfield_of_AI_or_AI_as_subfield_of_ML.jpg">Yakoove</a>, <a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>, via Wikimedia Commons</font>  
</p>