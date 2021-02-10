---
layout: post
title: 44. Manjavacas et al. (2019)
mathjax: True
---

Jag upptäckte ganska sent i mina uppsatsefterforskningar att lemmatisering (när man mappar en äldre stavningsvariant till en standardiserad ordboksform utan böjningar) behandlades lite separat från "vanlig" normalisering (när man går från en äldre stavningsvariant till en modern form med bibehållen böjning) inom forskningen. Idag detaljläser jag därför en artikel på temat lemmatisering och hitoriska språk som verkar utgöra SOTA: *Improving lemmatization of non-standard languages with joint learning* av Manjavacas et al. (2019).

Författarna har implementerat en kodare-avkodare-modell som kan fånga upp meningskontexten med hjälp av en hierarkisk meningskodare. De tränar den på två uppgifter -- lemmatisering och språkmodellering (se mitt tidigare inlägg om [fleruppgiftsinlärning och historisk textnormalisering](https://datatjej.github.io/Fleruppgiftsinl%C3%A4rning-f%C3%B6r-historisk-textnormalisering/) -- och når därigenom bättre resultat än enklare kodare-avkodare-modeller och så kallade edit tree-baserade metoder på flera dataset. 

Man beskriver svårighetsgraden av lemmatisering som beroende av två faktorer: 1) morfologisk komplexitet, och 2) token-lemma-ambiguitet. På den senare punkten har man som exempel ordet *living* som kan referara bådet till adjektivlemmat *living* och verblemmat *live* - något som meningskontext kan lösa.  
  
De edit tree-baserade metoderna behandlar lemmatisering som en klassificeringsuppgift där klasserna är binära redigeringsträd som skapas utifrån träningsdata: *"[given] a token-lemma pair, its binary edit-tree is induced by computing the prefix and suffix around the longest common subsequence, and recursively building a tree until no common character can be found"* (Manjavacas et al. 2019:2). Den här metoden är speciellt användbar för språk med regelbundna böjningar med suffix.

Författarna beskriver tre olika kodare-avkodar-arkitekturer (ED) som de jämför mot varandra: 1) en klassisk ED (`Plain`), 2) en ED med meningskontext (`Sent`), och 3) en ED med meningskontext tränad på en språkmodell (`Sent-LM`). Den **klassiska ED:n**, som också verkar ligga till grund för de övriga två, tar ett löpord $x_{t}$ som indata och läser av det tecken för tecken. Syftet är att avkoda ett mållemma $l_{t}$ -- som är grundat i den mellanliggande representationen av $ x_{t} $ -- ett tecken i taget. För varje löpord $ x_{t} $ extraherar man en sekvens av teckeninbäddningar (eng. *token character embeddings*):

$$ c_{1}^{x} \text{,...,} c_{n}^{x} $$ 

från en inbäddningsmatris:

$$ W_{\text{enc}} \in \mathcal{R}^{\|C\| \times d} $$

...där $ \|C\| $ är storleken hos teckenvokauläret och $ d $ inbäddningsdimensionaliteten.

Teckeninbäddningarna skickas in i en bidirektionell RNN-kodare som beräknar en framåtriktad och bakåtriktad sekvens av gömda tillstånd (eng. *hidden states*):       
   
$$ \overrightarrow{h_{1}^{\text{enc}}} \text{,...,} \overrightarrow{h_{n}^{\text{enc}}} \text{ och } \overleftarrow{h_{1}^{\text{enc}}} \text{,...,} \overleftarrow{h_{n}^{\text{enc}}}  $$

Den slutgiltiga representationen för varje tecken $ i $ är en konkaktenering av de framåtriktade och bakåtriktade tillstånden: 

$$ h_{i}^{\text{enc}} = [ \overrightarrow{h_{i}^{\text{enc}}} ; \overleftarrow{h_{i}^{\text{enc}}} ] $$

Vid varje avkodningssteg $ j $ genererar en RNN-avkodare ett gömt tillstånd $ h_{j}^{\text{dec}} $ utifrån: 
1) den lemma-baserade teckeninbäddningen $ c_{j}^{l} $ från inbäddningsmatrisen $ W_{\text{dec}} \in \mathcal{R}^{\|L\| \times d} $, 
2) det föregående gömda tillståndet $ h_{j-1}^{\text{dec}} $, och 
3) ytterligare kontext. 

Den här ytterligare kontexten består av en summeringsvektor $ r_{j} $ som skapats via en uppmärksamhetsmekanism (Bahdanau et al. 2014) som tar det tidigare avkodartillståndet $ h_{j-1}^{dec} $ och sekvensen av kodar-aktiveringar $ h_{1}^{enc} \text{,....,} h_{n}^{enc} $. 

Slutligen beräknas utdatavärdena för tecknet $ j $ genom en linjär projektion på det nuvarande avkodartillståndet $ h_{j}^{\text{enc}} $ med parametrarna $ O \in \mathcal{R}^{\|H \times \|L} $, som normaliseras med ett *softmax*-lager. Modellen tränas för att maximera sannolikheten för målteckensekvensen genom något som kallas *teacher forcing*, som i kort verkar gå ut på att mata modellen med facit pö om pö så att inte hela teckensekvensen blir fel bara för att något i början blev knasigt och tilläts vara kvar [3].

$$ P(l_t|x_t) =  \prod^{m}_{j=1} P(c_{j}^{l} | c_{\lt j }^{l}, r_{j}, \theta_{\text{enc}}, \theta_{\text{dec}}) $$

I ekvationen ovan beskrivs den enkla den grundläggande, klassiska ED:n. Här avser $ \theta $ parametrarna/vikterna hos kodaren och avkodaren och $ r_{j} $ teckenkontexten, genererad som en summeringsvektor via uppmärksamhetsmekanismer.  

Den andra **ED:n med meningskontext** (`Sent`) tar den globala meningskontexten i beaktning genom att för varje löpord $ x_{t} $ extrahera egenskaper på ordnivå via det sista gömda tillståndet från den teckenbaserade bidirektionella avkodaren beskriven ovan ($ w_{t} = [ \overrightarrow{h_{i}^{\text{enc}}} ; \overleftarrow{h_{i}^{\text{enc}}} ] $). De extraherade orden kan också berikas med information från en ordinbäddningsmatris, men den informationen visade sig inte gynna lemmatiseringsresultaten nämnvärt för de historiska språken i studien. Mönstern på meningsnivå (eng. *sentence-level features*) beräknas genom att konkatenera de framåtriktade och bakåtriktade aktiveringarna hos ett extra-RNN som arbetar bidirektionellt på meningarna. Den här modellen får följande ekvation:

$$ P(l_t|x_t) =  \prod^{m}_{j=1} P(c_{j}^{l} | c_{\lt j }^{l}, r_{j}, s_{t}; \theta_{\text{enc}}, \theta_{\text{dec}}) $$

...där $ s_{t} $ är mönstren på meningsnivå. 

Den trejde och sista ED-modellen är den som förbättrar mönsterigenkänningen på meningsnivå genom att även tränas på språkmodellering, dvs att förutspå nästa ord givet en eller flera tidigare ord. Språkmodellsförlusten tas sedan i beaktning när man försöker lemmatisera utifrån meningskontexten som beskrevs ovan: *"Given the forward and backward subvectors of the sentence encoding $ s^{t} = [ \overrightarrow{s^{t}} ; \overleftarrow{s^{t}} ] $, we train two additional softmax classifiers to predict token $ x^{t+1} $ given $ \overrightarrow{s^{t} $ and $ x^_{t-1} $ given $ \overleftarrow{s^{t}} $ with parameters $ O_{\text{LM fwd}} $ and $ O_{\text{LM bwd} \in \mathcal{R}^{\|S \times \|V \|} $ "* (Manjavacas et al. 2019:4).

Referenser
* [1] Enrique Manjavacas, Ákos Kádár och Mike Kestemont. 2019. Improving Lemmatization of Non-Standard Languages with Joint Learning. NAACL-HLT.
* [2] Dzmitry Bahdanau, Kyunghyun Cho och Yoshua Bengio. 2014. Neural machine translation by jointly learning to align and translate. arXiv preprint arXiv:1409.0473.  
* [3] Wanshun Wong. 2019. [What is teacher forcing?](https://towardsdatascience.com/what-is-teacher-forcing-3da6217fed1c) towardsdatascience.com (Hämtad: 09-02-2021)
