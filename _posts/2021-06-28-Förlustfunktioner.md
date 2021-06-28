---
layout: post
title: 50. Förlustfunktioner, AKA när liten förlust är stor vinst
mathjax: true
---

Förlustfunktioner (eng. *loss function*) används inom maskin- och djupinlärning för att undersöka hur mycket en modells prediktioner skiljer sig från facit. Man söker att **optimera förlustfunktionen**, vilket innebär att hitta det globala minimumet i funktionen -- där paramterarna (som kan uppgå till miljontals) ger minst förlust -- vilket kan göras med [stokastisk gradientnedstining](https://datatjej.github.io/Stokastisk-gradientnedstigning/). Men hur tar man då fram själva förlustfunktionen?

Det finns två olika sorters förlustfunktioner beroende på vilken typ av inlärning man sysslar med: 1) regressionsförlust och 2) klassificeringsförlust [[1]](https://towardsdatascience.com/common-loss-functions-in-machine-learning-46af0ffc4d23). En klassificeringsmodell har som utdata ett begränsat antal etiketter, medan regression matar ut ett värde på en kontinuerlig skala givet ett visst antal förklarande variabler, exempelvis priset på en lägenhet i Göteborg givet läge, byggår och antal kvadratmeter. 

Oavsett vilken typ av inlärning man arbetar med måste den [aktiveringsfunktion](https://datatjej.github.io/Fram%C3%A5triktade-neurala-n%C3%A4tverk/) man använder i utdatalagret av nätverket vara kompatibel med valet av förlustfunktion [[2]](https://machinelearningmastery.com/loss-and-loss-functions-for-training-deep-learning-neural-networks/). 

## Regressionsförlust

#### Mean Squared Error (MSE)

För regressionsuppgifter är *mean squared error* (`nn.MSELoss()` i PyTorch) en vanlig förlustfunktion:

$$ MSE = (\frac{1}{n})\sum_{i=1}^{n}(y_{i} - ŷ_{i})^{2} $$

...som ger medelvärdet av skillnaden mellan prediktionen (*ŷ*) och facitvärdet (*y*) i kvadrat utifrån ett dataset av storlek *n* och *n* prediktioner. Den appliceras under antagandet att målvariabeln är normalfördelad (en [Gausskurva](https://sv.wikipedia.org/wiki/Normalf%C3%B6rdelning#/media/Fil:Standard_deviation_diagram.svg) till utseendet) [[3](https://machinelearningmastery.com/how-to-choose-loss-functions-when-training-deep-learning-neural-networks/)] och att aktiveringsfunktionen i utdatalagret är linjär [[2](https://machinelearningmastery.com/loss-and-loss-functions-for-training-deep-learning-neural-networks/)]. I och med kvadreringen straffar den i högre grad fel som skiljer sig mycket från facit.

#### Mean Squared Logarithmic Error (MSLE)

En variant av MSE är *mean squared logarithmic error loss* (MSLE). Den används med fördel när målvariablen har större spridning i sina värden och vi därför inte vill hårdstraffa felen som skiljer sig mycket från facitvärdet. Den betraktar istället *den procentuella skillnanden* mellan prediktion och målvärde och beräknas genom att ta ut medelvärdet av felet i kvadrat på den naturliga logartimen av prediktionsvärdet [[4](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/mean-squared-logarithmic-error-(msle))]:

$$ MSLE = (\frac{1}{n})\sum_{i=0}^{n}(\log{y_{i} + 1} - \log{ŷ_{i} + 1}^{2} $$

Kodblocket nedan visar hur man kan implementera MSLE i PyTorch [[5](https://discuss.pytorch.org/t/rmsle-loss-function/67281)]:

`class RMSLELoss(nn.Module):`
    `def __init__(self):`
        `super().__init__()`
        `self.mse = nn.MSELoss()`
        
    `def forward(self, pred, actual):`
        `return torch.sqrt(self.mse(torch.log(pred + 1), torch.log(actual + 1)))`


#### Mean Absolute Error (MAE)

Om man har att göra med värden som i huvudsak följer en Gausskurva men ändå har en del extremvärden som ligger långt ifrån medelvärdet kan man använda *mean absolute error* (MAE) som förlustfunktion [[3](https://machinelearningmastery.com/how-to-choose-loss-functions-when-training-deep-learning-neural-networks/)]. MAE beräknas genom att ta medelvärdet av summan av absolutvärdet mellan alla prediktioner och deras facitvärden [[4](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/mean-absolute-error)]:

$$ MAE = (\frac{1}{n})\sum_{i=0}^{n}(\lvert y - ŷ_{i} \rvert $$

## Klassificeringsförlust

Valet av förlustfunktion vid klassificeringsuppgifter beror först och främst på huruvida man sysslar med klassificering med två eller flera klasser. 

### Binär klassificering

#### Binary Cross Entropy (BCE)

Vid binär klassificering är *binary cross entropy loss* (`nn.BCELoss` i Pytorch) en vanlig förlustfunktion. Den beräknas genom att ta fram följande medelvärde:

$$ BCE = -\frac{1}{utdatastorlek}\sum_{i=1}^{utdatastorlek} y_{i} \cdot \log{ŷ_{i}} + (1-y_{i}) \cdot  \log{(1 - ŷ_{i})} $$

...där $ ŷ_{i} $ är det $ i $:e skalärvärdet i modellens utdata (där $ utdatastorlek $ är den totala mängden utdatavärden) och $ y_{i} $ det motsvarande facitvärdet [[5](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/binary-crossentropy
)]. Även om man jobbar med binär klassificering så kan antalet kan antalet etiketter/klasser variera i exempelvis en receptklassificerar som klassificerar recept utifrån om de är nötfria, glutenfria, veganvänliga, etc. Med BCE används enbart sigmoid som aktiveringsfunktion, och den måste sättas in innan sista utdatalagret [[6](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/binary-crossentropy
)]. 

#### Hinge Loss (HL)

En annan förlustfunktion vid binär klassificering är *hinge loss* (`nn.HingeEmbeddingLoss` i PyTorch). Det känns som att jag måste läsa in mig lite mer på stödvektormaskiner (eng. *support vector machines*) - för vilka den här typen av förlustfunktion huvudsakligen utvecklades [[3](https://machinelearningmastery.com/how-to-choose-loss-functions-when-training-deep-learning-neural-networks/)] - för att få en bättre förståelse för HL. En källa nämner i alla fall att HL straffar prediktioner som inte har rätt tecken (+/-) [[3](https://machinelearningmastery.com/how-to-choose-loss-functions-when-training-deep-learning-neural-networks/)], och en annan att dess variant *squared hinge loss* kan användas när man *inte* är så intresserad av hur säker klassificeraren är i sina beräkningar [[7](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/squared-hinge)].  


### Flerklassklassificering

Vid flerklassklassificering vill man kunna tillskriva ett objekt enbart en av flera möjliga klasser.

#### Categorical Cross Entropy Loss (CCEL)

*Categorical cross entropy loss* (`nn.CrossEntropyLoss` i PyTorch) är en vanligt förekommande förlustfunktion vid flessklassklassificering och definieras enligt följande [[?](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/categorical-crossentropy)]:

$$ CCEL = -\sum_{i=1}^{utdatastorlek} y_{i} \cdot \log{ŷ_{i}} $$

...där $ ŷ_{i} $ är det $ i $:e skalärvärdet i modellens utdata och $ y_{i} $ det motsvarande facitvärdet. De olika klasserna måste representeras med [*one-hot encoding*](https://en.wikipedia.org/wiki/One-hot)-vektor [[8](https://www.section.io/engineering-education/understanding-loss-functions-in-machine-learning/#loss-functions-for-classification)] och aktiveringsfunktionen vid CCEL måste vara softmax. 


### Referenser
[1] - [Common Loss Functions in Machine Learning](https://towardsdatascience.com/common-loss-functions-in-machine-learning-46af0ffc4d23)), av Ravindra Parmar (2018). towardsdatascience.com <br>
[2] - [Loss and Loss Functions for Training Deep Learning Neural Networks](https://machinelearningmastery.com/loss-and-loss-functions-for-training-deep-learning-neural-networks/), av Jason Brownlee (2019). machinelarningmastery.com <br>
[3] - [How to Choose Loss Functions When Training Deep Learning Neural Networks](https://machinelearningmastery.com/how-to-choose-loss-functions-when-training-deep-learning-neural-networks/), av Jason Brownlee (2019). machinelarningmastery.com <br>
[4] - [Mean Squared Logarithmic Error](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/mean-squared-logarithmic-error-(msle)). peltarion.com <br>
[5] - [RMSLE loss function (diskussionforumsinlägg)](https://discuss.pytorch.org/t/rmsle-loss-function/67281), 2020. discuss.pytorch.org <br>
[6] - [Binary Crossentropy](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/binary-crossentropy
). peltarion.com <br>
[7] - [Squared Hinge](https://peltarion.com/knowledge-center/documentation/modeling-view/build-an-ai-model/loss-functions/squared-hinge). peltarion.com <br>
[8] - [Understanding Loss Functions in Machine Learning](https://www.section.io/engineering-education/understanding-loss-functions-in-machine-learning/#loss-functions-for-classification), av Prashanth Saravanan (2021). section.io