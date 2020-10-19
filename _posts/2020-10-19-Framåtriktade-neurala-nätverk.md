---
layout: post
title: 31. Framåtriktade neurala nätverk
---

Framåtriktade neurala nätverk, på engelska **feed-forward neural networks**, är den enklaste typen av neuralt nätverk. Det kallas framåtriktat eftersom beräkningen sker iterativt från ett neuronlager till ett annat utan att något skickas tillbaka ([Jurafsky & Martin 2019: kap 7: sida 1](https://web.stanford.edu/~jurafsky/slp3/7.pdf)). Som [tidigare nämnts](https://datatjej.github.io/Logistisk-regression/) är neurala nätverk i grund och botten uppbyggda på logistisk regression, men med några viktiga undantag.

Logistisk regression innebär att ta den **inre produkten** (eng. *dot product*) av en **indatavektor** och en **viktmatris** samt applicera en **normaliserad Sigmoid-funktion** (softmax) för att generera en sannolikhetsdistribution som summerar till 1. Ett neuralt nätverk lägger till **ett eller flera gömda lager** mellan inputvektorerna och softmax-utdatat. Detta gör det möjligt för nätverket att i princip lära sig vilken funktion som helst. 

Med logistiska regressionsklassificerare beestämmer man dessutom själv vilka features som ska användas genom kunskap om domänet. När man arbetar med neurala nätverk är det istället vanligare att låta nätverket själv avgöra vilka features som är relevanta för klassificieringen, så kallad **representationsinlärning**. **Djupa nätverk** (eng. *deep nets*), dvs. nätverk med flera lager, lämpar sig särskilt väl för representationsinlärning. 

Den här arkitekturen innebär att utdatat i varje lager går igenom en **aktiveringsfunktion** som omvandlar den till indata för ett annat lager. Genom icke-linjära aktiveringsfunktioner kan man fånga upp komplexa mönster i datat som inte låter sig uttryckas linjärt ([Jain 2019](https://towardsdatascience.com/everything-you-need-to-know-about-activation-functions-in-deep-learning-models-84ba9f82c253)). Det förhindrar också att utdatavärdet blir för högt genom att hålla det inom en viss kravspecificerad gräns (ibid). 

<p align="center">
<img src="/images/aktiveringsfunktioner.jpeg" alt="Tre populära aktiveringsfunktioner" width="100%" height="auto" border="10" /><br>
</p>

Tre populära icke-linjära aktiveringsfunktioner är **sigmoid**, **tanh** och den stegvis linjära aktiveringsfunktionen **ReLU**. Sigmoid används i praktiken inte som aktiveringsfunktion utan lämpar sig bättre för binära klassificeringsuppgifter (ibid). Tanh är en variant av sigmoid – "very similar but always better" ([Jurafsky & Martin 2019: kap 7: sida 2](https://web.stanford.edu/~jurafsky/slp3/7.pdf)) – som går från -1 till +1. Men den enklaste och kanske mest använda aktiveringsfunktionen är ReLU, som är samma som inputsvektorn *x* när *x* är positiv, annars 0 ([Jurafsky & Martin 2019: kap 7: sida 3](https://web.stanford.edu/~jurafsky/slp3/7.pdf)). Rao & Brian skriver i "Natural Language Processing with PyTorch" (2019:43): *"In fact, one could venture as far as say to say that many of the recent innovations in deep learning would've been impossible without the use of ReLU." 

När neurala nätverk uppdaterar sina vikter under upplärningen skickas **felgradienten** tillbaka genom nätet, från utdatalagret till indatalagret (s.k. *backpropagation*). Problemet är bara att med många lager så minskas gradienten markant när den skickas tillbaka, vilket kan kan göra det svårt att veta hur vikterna i neuronlagren närmast inmatningslagret ska regleras ([Brownlee 2019](https://machinelearningmastery.com/how-to-fix-vanishing-gradients-using-the-rectified-linear-activation-function/)). Detta kallas **vanishing gradient descent**-problemet och ReLU är just en av de lösningar som finns för att råda bot på detta. 

Med sigmoid och tanh riskerar man att få värden av *w·x+b* som är **mättade** (eng. *saturated*), dvs. väldigt nära 1, men *"[r]ectifiers don’t have this problem, since the output of values close to 1 also approaches 1 in a nice gentle linear way"* ([Jurafsky & Martin 2019: kap 7: sida 4](https://web.stanford.edu/~jurafsky/slp3/7.pdf)). Nackdelen är bara det s.k. **"dying ReLU"**-problemet i och med att alla negativa indatavärden sätts till 0: "[...] over time certain inputs in the network can simply become zero and never revive again" (Rao & Brian 2019:44).
