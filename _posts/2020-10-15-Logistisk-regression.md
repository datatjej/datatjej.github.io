---
layout: post
title: 29. Logistisk regression
mathjax: true
---

Logistisk regression är ett statistiskt verktyg som ofta används som vägledd *baseline*-algoritm inom maskinlärning. Det används för att **klassificera** en observation i endera av **två klasser**: sant eller falskt, högt eller lågt, positivt eller negativt etc (värden i binärt format, d.v.s. **diskreta värden**). 

Logistisk regression är en sorts **diskriminativ klassificerare**, vilket innebär att syftet inte primärt är att lära sig förstå vad olika koncept egentligen innebär eller består av, utan främst att lära sig särskilja dem. 
Jurafsky & Martin ([2019: kap 5: sida 1](https://web.stanford.edu/~jurafsky/slp3/5.pdf)) skriver apropå diskriminativa klassifierare och data med katt- och hundbilder: *"So maybe all the dogs in the training data [of cats and dogs] are wearing collars and the cats aren't. If that one feature neatly separates the classes, the model is satisfied. If you ask such a model what it knows about cats all it can say is that they don't wear collars."* Detta ska ses i kontrast till de **generativa klassificerarna** av naiv Bayes-modell som också måste kunna återskapa liknande bildexempel på katter och hundar. 

Naiv Bayes-klassificerare kan lära sig att generera features givet en klass genom att ta hänsyn till sannolikhetstermen $ P(d\|k) $ (sannolikheten för dokument *d* givet klassen *k*) och klassannolikheten $ P(k) $:<br> 

$$ k = argmax(P(d\|k) * P(k)) $$

En diskriminativ klassificerare försöker istället att **direkt** beräkna sannolikheten för en klass *k* givet dokumentet *d*, $ P(k\|d) $, genom att lära sig tillskriva större vikt till dokumentfeatures som ger rätt klass. Såväl naive Bayes-modeller som logistisk regression är **probabilistiska modeller** som använder sig av **väglett lärande** (eng. *supervised learning*) med ett korpus av annoterade in- och utdatapar: *(x<sup>(i)</sup>,y<sup>(i)</sup>)* - där *(i)* avser individuella instanser i träningsdatat ([2019: kap 5: sida 2](https://web.stanford.edu/~jurafsky/slp3/5.pdf)).

Jurafsky & Martin (ibid) beskriver fyra komponenter hos ett maskininlärningssystem:<br>
1. en **feature-vektor**-representation av varje observation *x<sup>(i)</sup>* i indatat: *[x<sub>1</sub>, x<sub>2</sub>,...,x<sub>n</sub>]*. I en NER-labb som vi just nu håller på med i maskininlärningskursen (LT2316) var det många som använde features av typen POS (part of speech), ordlängd, ord-före-ordet, ord-efter-ordet, ordembedding m.m. Om *x* är en hel mening kan då featurevektorn bli lista av listor där varje ord representeras som en lista av features (i numerisk form) i den övergripande meningslistan.

<p align="center">
<img src="/images/sigmoid.svg" alt="Sigmoid-kurvan" border="10" /><br>By Qef (talk) - Created from scratch with gnuplot, Public Domain, https://commons.wikimedia.org/w/index.php?curid=4310325
</p> 
         
2. En **klassificeringsfunktion** som beräknar klassetiketten *y* via sannolikheten $ P(y\|x) $. I ett binärt klassificeringssammanhang är den antingen 0 eller 1. Den s-formade **sigmoid**-funktionen (σ), även kallad den **logistiska funktionen**, är den klassificeringsfunktion som definierar logistisk regression. Under träningen beräknas **vikten** (*weights*) av varje feature, dvs. hur betydelsefull den är för att bedöma klassen, samt en **bias term**, även kallad *intercept*, som läggs till det viktade indatat. När testdatat sedan utvärderas av klassificeraren tar den de framräknade vikterna från träningen och multiplicerar varje *x<sup>(i)</sup>* med dem (dvs. **dot product**: att multiplicera varje element i två vektorer med varandra) samt adderar bias-termen. Summan av de viktade värdena, *z*, kan ligga var som helst mellan -∞ och ∞. Värdet z matas därefter in i sigmoid-funktionen där det omvandlas till en "tillåten" sannolikhet mellan 0 och 1. **Beslutsgränsen** (eng. *decision boundary*) ligger där den ena klassen slutar och den andra börjar.

3. En **objektiv funktion** för inlärningen, vanligtvis för att mimimera felmarginalen i modellens utdata, dvs. hur mycket det uppskattade y:et skiljer sig från det egentliga y:et. Korsentropiförlustsfunktionen, **cross entropy loss function** är den förlustfunktion som vanligtvis används för logistisk regression. Vi väljer de vikter, *w*, och den bias, *b*, som **maximerar log-sannolikheten** för de "sanna" etiketterna *y* i träningsdatat för en viss observation *x*. Förlusten är alltså, i fallet med logistisk regression, en funktion av parametrarna *w* och *b* (vilka tillsammans kan skrivas som theta *θ*).  

4. En **algoritm för att optimera den objektiva funktionen**, dvs för att uppdatera vikterna och minska felmarginalen. Med **gradientmetoden** försöker vi hitta minimipunkten i förlustfunktionen genom att lokalisera den plats där förlustkurvan stiger som mest och gå i motsatt riktning. Med logistisk regression behöver man inte oroa sig för flera lokala minima då kurvan är **konvex**, vilket innebär att var man än börjar så kommer man till slut att hitta minimipunkten. Detsamma gäller inte för neurala nätverk med flera lager där optimeringsalgoritmen kan fastna i ett lokalt minimum. Den stokastiska gradientmetoden **stochastic gradient descent** (SDG) är en **iterativ gradientmetod som beräknar gradienten efter varje träningsexempel** och justerar *θ* i motsatt riktning. 

På frågan om man **när man bör använda logistisk regression och när naiv Bayes** skriver Jurafsky & Martin ([2019: kap 5: sida 6](https://web.stanford.edu/~jurafsky/slp3/5.pdf)) att logistisk regression lämpar sig bättre för **stora dokument- eller datasamlingar** eftersom det bättre uppskattar sannolikheten för **korrelerade features**. På mindre datasamlingar tenderar dock naiv Bayes att visserligen ge sämre uppskattning för feature-sannolikheten, men trots det klassificera korrekt. Detta i kombination med enkel implementering och snabb uppträning gör naiv Bayes till en lämplig lösning i vissa klassificeringsfall.

**Fun fact:** ett neuralt nätverk kan betraktas som en tårta av logistiska regressionsklassificerare staplade på varandra! :D
