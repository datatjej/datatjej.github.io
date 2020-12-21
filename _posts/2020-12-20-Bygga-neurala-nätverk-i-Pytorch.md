---
layout: post
title: 41. Bygga neurala nätverk i PyTorch
mathjax: true
---

Neurala nätverk är grundbulten inom maskininlärning och består av lager på lager av artificiella neuroner som utför matematiska funktioner på indata och ger ett eller flera utdatavärden som prediktion. När vi bygger neurala nätverk i MLT-kurserna är [PyTorch](https://pytorch.org/) det maskininlärningsbibliotek som dominerar. Det är, som namnet antyder, baserat på ramverket Torch och programmeringsspråket Python. Ett annat känt maskininlärningsbibliotek för Python är [Keras](https://keras.io/), baserat på TensorFlow.

<p align="center">
<a title="PyTorch, BSD &lt;http://opensource.org/licenses/bsd-license.php&gt;, via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:PyTorch_logo_black.svg"><img width="256" alt="PyTorch logo black" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c6/PyTorch_logo_black.svg/256px-PyTorch_logo_black.svg.png"></a>
</p>


All data som bearbetas i PyTorch måste representeras som **tensorer**. En tensor är -- precis som NumPys np.array -- en n-dimensionell array, men till skillnad från NumPy-arrayer kan tensorer föras över till en GPU för att snabba på beräkningarna [[1](https://pytorch.org/tutorials/beginner/pytorch_with_examples.html)]. Något som hjälpte mig förstå användandet av tensorer och matriser bättre var när vi fick till uppgift att själva ta ut betydelsefulla egenskaper (eng. *features*) från textdata som förarbete inför skapandet av ett neuralt nätverk för identifiering av läkemedelsnamn i text. 

Den processen krävde dels lite reflektion över vilka egenskaper som faktiskt är betydelsefulla (POS-tagg, ordet innan, ordet efter, ordinbäddningar eller något helt annat?) och dels att dessa egenskaper sparades som **listor av listor** för varje mening i textdatat. En mening med tre ord skulle då kunna se ut så här: 

*[ [f1, f2, f3, ... , fn],  [f1, f2, f3, ... , fn] , [f1, f2, f3, ... , fn] ]*

...där de inre listorna motsvarar varje ord i meningen och *f* betecknar en av flera utvalda egenskaper hos varje ord, representerad i numerisk form (den ursprungliga formen, t.ex. POS-taggen "JJ" för adjektiv, samt ordet som egenskaperna avser, sparas i någon annan datastruktur för senare konsultering). Den här listor-av-listor-strukturen omvandlas sedan till en tensor genom metoden `torch.Tensor()` (där `torch.FloatTensor()` är defaultvarianten, även om `long`, `double` mfl. också är möjliga).  

Själva nätverket kan implementeras som en klass som ärver från PyTorch-modulen `nn.Module`. Rao & McMahan (2019:41) beskriver i *Natural Language Processing with PyTorch* hur den enklaste formen av neuralt nätverk, en **perceptron**, skulle kunna implementeras med den här klassen:

{% highlight python %}
import torch
import torch.nn as nn

class Perceptron(nn.Module):
    def __init__(self, input_dim):
        super(Perceptron, self).__init__()
        self.fc1 = nn.Linear(input_dim, 1)

    def forward(self, x_in):
        return torch.sigmoid(self.fc1(x_in)).squeeze()
{% endhighlight %}

En perceptron kan definieras matematiskt som:

$$ y = f(wx + b) $$ 

...där *w* är vikterna, *x* är indatat, *b* är *bias*-vikten och *f* vanligtvis en icke-linjär funktion (Rao & McMahan 2019). I exemplet ovan definieras *wx + b*-delen av ekvationen med den linjära funktionen `Linear` i den obligatoriska `init`-konstruktorn. Som [aktiveringsfunktion](https://datatjej.github.io/Fram%C3%A5triktade-neurala-n%C3%A4tverk/) *f()* används `sigmoid`-funktionen i den lika obligatoriska **forward**-funktionen. Forward definierar hur indatat ska skickas fram i nätverket genom bearbetning från aktiveringsfunktionerna.

För att **spara och ladda** PyTorch-modellerna man tränar upp behöver man tre saker [[3](https://pytorch.org/tutorials/beginner/saving_loading_models.html)]:<br>
- `torch.save`: sparar objekt (modeller, tensorer, ordboksstrukturer) till disk.
- `torch.load`: laddar sparade objekt till minnet. 
- `torch.nn.Module.load_state_dict`: laddar modellens parameterar (bl.a. vikter och bias) som ordboksstruktur m.h.a. `state_dict` (en Python-ordbok som mappar varje lager i modellen till motsvarande parametertensor). Även optimerare (`torch.optim`) har en `state_dict` som lagrar optimerarens tillstånd och de hyperparametrar som används.

*OBS: Inte helt färdigskrivet, men jag fyller på allteftersom jag lär mig mer. ¯\\\_(ツ)\_/¯* 

