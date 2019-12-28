---
layout: post
title: 18. "I am sorry to hear you are depressed"
---

Det finns två typer av dialogsystem: 1) **uppgiftorienterade**, som hjälper till med saker som att boka biljetter, hitta närliggande restauranger eller ringa upp någon åt dig, och 2) **chatbotar**, vars syfte är att så naturligt som möjligt simulera mänskliga konversationer, både för nytta (i uppgiftorienterade dialogsystem) och rent nöje (Jurafsky & Martin 2019).  

Man talar om tre olika typer av chatbotsarkitektur:

1) **regelstyrda system** 

2) **informationshämtningssystem** (korpus-baserad)

3) **maskininlärningsbaserade system** (korpus-baserad)

Den första typen - de rent **regelstyrda systemen** - återfinnar man främst i de tidigaste chatbotarna. Det kändaste exemplet är antagligen ELIZA, en chatbot från 60-talet som i enlighet med den Rogerianska grenen av kliniskt psykologi programmerats för att upprepa patientens yttranden, samt ställa földjfrågor utifrån **nyckelord** och klassiska frågor av typen "Hur får det dig att känna?". Nyckelorden får då olika rankningar utifrån hur generella eller specifika de är. Tack vare **minnesstacken** kan programmet också återuppta ämnen som nämnts tidigare i samtalet (*"Earlier you said that you..."*).

<p align="center">
<img src="/images/eliza_transkription.PNG" alt="Samtal med Eliza" width="100%" height="auto" border="10" /><br></p>

Exempel på samtal med ELIZA ([Weizenbaumm 1966](https://dl-acm-org.ezp.sub.su.se/citation.cfm?id=357991)).

Den andra två typerna av chatbotsarkitektur förlitar sig istället för regler på **korpusar** med transkriptioner av andra samtal. De **informationshämtningsbaserade systemen** (eller IR-systemen, från eng. *information retrieval*) använder sig av olika algoritmer för att hitta ett lämpligt svar i korpusen, givet användarens input. Antingen brukar de försöka hitta det yttrande i korpusen som har högst cosinus-likhet med användarens senaste input, och ge det svar som följer på det yttrandet, eller så jämförs input med alla tänkabara svar i korpusen rakt av, utifrån tanken att ett passande svar borde dela många ord med själva inputsträngen. Intressant nog, påpekar Jurafsky & Martin (2019), verkar den senare strategin fungera bättre.

Den andra korpusbaserade ansatsen, den med **maskininlärningsbaserade system**, kallas hos Jurafsky & Martin **encoder decoder chatbots**: *"An alternate way to use a corpus to generate dialogue is to think of response generation as a task of *transducing* from the user’s prior turn to the system’s turn. This is basically the machine learning version of Eliza;  the system learns from a corpus to transduce a question to an answer"* (2019: kaptiel 26: sida 11). Jag har ännu inte läst på tillräckligt mycket om maskininlärning för att sätta mig in i hur det fungerar, men Jurafsky & Martin (2019) nämner att tanken på den här typen av system väcktes inom maskinöversättningsforskningen, men att fraserna mellan språk tenderar att stämma mycket bättre överens med varandra än yttrande och tillhörande svar i en dialog.

<p align="center">
<img src="/images/mmi.PNG" alt="Jämförelse mellan seq2seq och MMI" width="100%" height="auto" border="10" /><br></p>  

Jämförelse mellan traditionell seq2seq och Maximum Mutual Information-metoden ([Li et al. 2016](https://arxiv.org/pdf/1510.03055.pdf)).

Lösningen var alltså encoder decoder-modellen. För att undvika repetetiva och tråkiga svar av typen "Jag vet inte" - som den här så kallade [seq2seq](https://en.wikipedia.org/wiki/Seq2seq)-ansatsen annars tenderar att generera - har forskarna bland annat lanserat **Maximum Mutual Information**-funktionen, se ovan [(Li et al. 2016)](https://arxiv.org/pdf/1510.03055.pdf). Ett annat problem - med såväl informationshämtningsbaserada och maskinlärningsbaserade system - är att de i sin enklaste form inte tar hänsyn till den större kontexten när de genererar tänkbara svar. Li et al. ([2017](https://arxiv.org/pdf/1701.06547.pdf)) har använt sig av något som kallas **adversarial training** för att förbättra den övergripande relevansen hos svaren:

>"A good dialogue model should generate utterances indistinguishable from human dialogues. Such a goal suggests a training objective resembling the idea of the Turing test (Turing, 1950). We borrow the idea of adversarial training (Goodfellow et al., 2014; Denton et al., 2015) in computer vision, in which we jointly train two models, a generator (a neural SEQ2SEQ model) that defines the probability of generating a dialogue sequence, and a discriminator that labels dialogues as human-generated or machine-generated. This discriminator is analogous to the evaluator in the Turing test. We cast the task as a reinforcement learning problem, in which the quality of machine-generated utterances is measured by its ability to fool the discriminator into believing that it is a human-generated one. The output from the discriminator is used as a reward to the generator, pushing it to generate utterances indistinguishable from human-generated dialogues.
"

När det kommer till **uppgiftsorienterade system** nämner Jurafsky & Martin (2019) två stora arkitekturer: 1) **GUS-** och 2) **dialoge state**-aritekturen. Båda baserar sig på konceptet med **frames** (ramar?): *"A frame is a kind of knowledge frame structure representing the kinds of intentions the system can extract from user sentences, and consists of a collection of **slots**, each of which can take a set of possible **values**. Together this set of frames is sometimes called a **domain ontology**."* (2019: kapitel 26: sida 12). Beroende av typen av domän man rör sig i kan en slot då motsvara en maträtt (t.ex. take-away-dialogsystem) eller ett datum, klockslag eller en destination (biljettbokningssystem). Många system arbetar efter dessa ramar och *slots*, och fyller i ett inre formulär allteftersom användaren uppger mer information. De kvarstående formulärfönstren dikterar vilka följdfrågor som ställs.    

Eftersom många system tillhandahåller flera olika ramar, t.ex. ett biljettbokningssystem som också erbjuder hotellreservation, innebär det att systemet måste ha en villkorsbaserad **kontrollfunktion** för att veta när den ska hoppa vidare till frågor gällande en annan ram, och ha koll på vilka frågor som fortfarande är relevanta att ställa. Men innan några formulärfönster (*slots*) ens fylls i måste först domänet som användarens yttranden berör klassificeras, och därefter användarens avsikt. I de ursprungliga modellerna av GUS-arkitekturen användes otaliga handskrivna regler med reguljära uttryck för att matcha yttranden mot avsikt och formulärifyllning. Det moderna system är, som så mycket annat inom dagens språkteknologi, ofta maskininlärningsbaserade snarare än regelstyrda, även om den underliggande arkitekturen med forumlär och ramar fortfarande är densamma. 

Den andra typen av uppgiftsorienterade system, **dialogue-state-arkitekturen**, beskrivs av Jurafsky & Martin (2019) som en mer sofistikerad version av *frame*-arkitekturen. Begreppet *dialogue state* (som jag väljer att kalla *dialogtillstånd*) verkar användarens yttrande i form av en talakt, dvs vilket syfte yttrandet har där och då i dialogen (är det att INFORMERA? BEKRÄFTA? UNDERSÖKA?), i kombination med de tidigare nämnda formulärrutorna (*slots*). Eftersom dialoge-state-arkituren ofta grundar sig i maskininlärning måste systemen tränas i att korrekt identifiera dialogtillståden (med hänsyn på *slots*, domän, avsikt), exempelvis genom BERT (som får bli föremål för ett annat inlägg senare). 

Den här typen av system användar sig av **dialogue-state trackers** för att hålla koll på användarens senaste dialogtillstånd och ramen i sin nuvarande helhet (dvs vilka rutor som är ifyllda). Ofta tränas trackern på annoterad dialogdata, men i sin allra enklaste form kan den använda utdatat från formulärifyllningen för att bestämma dialogtillståndet.    

<p align="center">
<img src="/images/dialogue_state_tracker.PNG" alt="Exempel på dialogue-state-tracking" width="100%" height="auto" border="10" /><br></p>

Exempel på dialogue-state-tracking ([Jurafsky & Martin 2019](https://web.stanford.edu/~jurafsky/slp3/26.pdf)).

Om användaren säger något fel och därför vill korrigera sig själv, måste systemet även kunna identifiera korrigeringsakten, vilket är lättare sagt än gjort. Människor som korrigerar sig själva tenderar att **hyperartikulera** (*"Jag sa STO-CK-HOLMMMM"*), något som ironiskt nog kan göra det svårare för språkigenkänningskomponenten i systemet att förstå. Men i kombination med andra vanligt förekommande korrigeringsmönster (tex svärord!) kan systemet ändå tränas till att bättre uppfatta korrigeringar som just rättning av äldre information, och inte som helt ny information. 

Andra viktiga delar i dialogtillståndssystemet är **dialogue policy**, vars syfte är att sätta rätt handling som svar på andvändarens input. Detta genom att exempelvis beräkna sannolikheten för att man uppfattat användarens indata rätt, vilket kan ske genom att BEKRÄFTA, eller helt sonika AVSLÅ begäran med *"Ursäkta, jag uppfattade inte vad du sa"*. Den muntliga eller skriftliga realiseringen av detta kallas *natural language generation* (**NLG**).
