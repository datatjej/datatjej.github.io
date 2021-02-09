---
layout: post
title: 43. Manjavacas et al. (2019)
mathjax: True
---

Jag upptäckte ganska sent i mina uppsatsefterforskningar att lemmatisering (när man mappar en äldre stavningsvariant till en standardiserad ordboksform utan böjningar) behandlades lite separat från "vanlig" normalisering (när man går från en äldre stavningsvariant till en modern form med bibehållen böjning) inom forskningen. Idag detaljläser jag därför en artikel på temat lemmatisering och hitoriska språk som verkar utgöra SOTA: *Improving lemmatization of non-standard languages with joint learning* av Manjavacas et al. (2019).

Författarna har implementerat en kodare-avkodare-modell som kan fånga upp meningskontexten med hjälp av en hierarkisk meningskodare. De tränar den på två uppgifter -- lemmatisering och språkmodellering (se mitt tidigare inlägg om [fleruppgiftsinlärning och historisk textnormalisering](https://datatjej.github.io/Fleruppgiftsinl%C3%A4rning-f%C3%B6r-historisk-textnormalisering/) -- och når därigenom bättre resultat än enklare kodare-avkodare-modeller och så kallade edit tree-baserade metoder på flera dataset. 

Man beskriver svårighetsgraden av lemmatisering som beroende av två faktorer: 1) morfologisk komplexitet, och 2) token-lemma-ambiguitet. På den senare punkten har man som exempel ordet *living* som kan referara bådet till adjektivlemmat *living* och verblemmat *live* - något som meningskontext kan lösa.  
  
De edit tree-baserade metoderna behandlar lemmatisering som en klassificeringsuppgift där klasserna är binära redigeringsträd som skapas utifrån träningsdata: *"[given] a token-lemma pair, its binary edit-tree is induced by computing the prefix and suffix around the longest common subsequence, and recursively building a tree until no common character can be found"* (Manjavacas et al. 2019:2). Den här metoden är speciellt användbar för språk med regelbundna böjningar med suffix.

Kodare-avkodar-arkitekturen som Manjavacas et al. (2019) föreslår tar ett löpord *x<sub>t</sub>* som indata och läser av det tecken för tecken. Syftet är att avkoda ett mållemma *l<sub>t</sub> -- som är grundat i den mellanliggande representationen av *x<sub>t</sub>* -- ett tecken i taget. // För varje löpord *x<sub>t</sub>* extraherar man en sekvens av teckeninbäddningar (eng. *token character embeddings*) *c<sup>x</sup><sub>1</sub>,...,c<sup>x</sup><sub>n</sub>* från en inbäddningsmatris *W<sub>enc</sub> ∈ R<sup>|C|×d</sup>, där *|C|* är storleken hos teckenvokauläret och *d* inbäddningsdimensionaliteten.

Teckeninbäddningarna skickas in i en bidirektionell RNN-kodare som beräknar en framåtriktad och bakåtriktad sekvens av gömda tillstånd (eng. *hidden states*):       
   
$$ h_1^enc^\rightarrow,...,h_n^enc^\rightarrow och h_1^enc^\leftarrow,...,h_n^enc^\leftarrow $$

Den slutgiltiga representationen för varje tecken *i* är en konkaktenering av de framåtriktade och bakåtriktade tillstånden: 

$$ h_i^enc = [h_1^enc^\rightarrow;h_1^enc^\leftarrow] $$

Vid varje avkodningssteg *j* genererar en RNN-avkodare ett gömt tillstånd *h<sub>j</sub><sup>dec</sup>* utifrån den lemma-baserade teckeninbäddningen *c<sub>j</sub><sup>l</sup>* från inbäddningsmatrisen *W<sub>dec</sub> ∈ R<sup>|L|×d</sup>, det föregående gömda tillståndet *h<sub>j-1</sub><sup>dec</sup>* och ytterligare kontext. Den här extrakontexten består av en summeringsvektor *r<sub>j</sub> som skapats via en uppmärksamhetsmekanism (Bahdanau et al. 2014) som tar det tidigare avkodartillståndet h<sub>j-1</sub><sup>dec</sup>* och sekvensen av kodaraktiveringar *h<sub>1</sub><sup>enc</sup>....,h<sub>n</sub><sup>enc</sup>*.

Slutligen beräknas utdatavärdena för tecknet *j* genom en linjär projektion på det nuvarande avkodartillståndet *h<sub>j</sub><sup>enc</sup>* med parametrarna *O ∈ R<sup>|H×|L</sup>*, som normaliseras med ett *softmax*-lager. Modellen tränas för att maximera sannolikheten för målteckensekvensen genom något som kallas *teacher forcing*, vilket jag nog får gå in på mer grundligt i ett annat inlägg, men som i kort verkar gå ut på att mata modellen med facit pö om pö så att inte hela teckensekvensen blir fel bara för att något i början blev knasigt och tilläts vara kvar [2].

$$ P(l_t|x_t) =  \Prod^{m}_{j=1} P(c_{j}^{l} | c_{\leq j }^{l}, r_{j}, \theta_{\text{enc}}, \theta_{\text{dec}} $$

\overrightarrow

 


Referenser
* [1] 
* [2] Wanshun Wong. [What is teacher forcing?](https://towardsdatascience.com/what-is-teacher-forcing-3da6217fed1c) towardsdatascience.com