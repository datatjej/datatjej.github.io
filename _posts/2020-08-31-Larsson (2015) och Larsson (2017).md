---
layout: post
title: 25. Larsson (2015) och Larsson (2017)
---

Så här i början av höstterminen när man fortfarande ligger i fas och inte vill halka efter tänkte jag använde det här utrymmet så som jag från början avsåg att använda det - för rena kursanteckningar. För första lektion i dialogsystem II som börjar den här veckan har vi två läsanvisngar: [Larsson (2015)](https://flov.gu.se/digitalAssets/1536/1536925_semdial2015_godial_proceedings.pdf#page=197) och [Larsson (2017)](https://www.aclweb.org/anthology/W17-5503.pdf). Den första artikeln har vi redan haft som läsning i NLP-kursen, men jag minns inte om jag tog några anteckningar då, så here we go!

**Larsson (2015)** undersöker hur dialogsystemen hos Apple, Google och Microsoft hanterar några vanliga fenomen i mänskliga dialoger: 1) *over-answering*, 2) *other-answering* och 3) *answer revision*. **1) Over-answering** innebär att användaren **ger mer information än vad som förväntas**, t.ex.<br>

- Ring ett samtal.<br>
- Okej, vem vill du ringa?<br>
- Miras mobil. <-- over-answering<br>
- Okej, ringer Miras mobil.<br>

Dialogen ovan är inte alls problematisk för en människa, men för ett dialogsystem som förväntar sig en bit information i taget (- Vem vill du ringa? - Mira - På vilket nummer, hem eller mobil? - Mobil) kan det bli förvirrande. Larssons undersökning visar också att dåvarande Siri och Cortana kunde hantera over-answering, men däremot inte Google Now (nuvarande Google Assistant).

**2) Other-answering** innebär att användaren **ger ett svar på en annan fråga** än den som systemet precis ställt, t.ex:<br>

- Ring ett samtal.<br>
- Okej, vem vill du ringa?<br>
- Mobil. <-- other-answering <br>

Larssons undersökning visade att såväl Google Now som Cortana förväntade sig rätt svar vid rätt tillfälle från användaren (även om systemet tänkt fråga om vilket nummer som avses -- hem, arbete eller mobil -- redan i nästa fråga), medan Siri kunde hantera other-answering.

Den tredje och sista användarinputen som tas upp, **3) answer revision**, avser de tillfällen då användaren **rättar ett tidigare givet svar**, t.ex:<br>

- Ring Mira. <br>
- Ok, arbete eller mobil? <br>
- Nej förresten, ring Peter. <-- answer revision <br>

I idealfallet skulle systemet svara något i stil med: "Okej, ringer Peter istället. Arbete eller mobil?". Så som de undersökta systemen såg ut 2015 kunde ingen av dem hantera svarskorrigeringar. Siri ignorerade dem antingen helt eller avbröt hela processen vid nyckelord som "nej". Och man skulle vilja tro att mycket förbättrats på den här fronten de senaste åren, men en labb i NLP-kursen förra hösten visade att åtminstone Google Assistant inte kunde hantera over-answering, other-answering eller answer revision på svenska särskilt väl, se figur 1. 

<p align="center">
<img src="/images/google_assistant_labb.PNG" alt="Resultat av labb med Google Assistant" width="100%" height="auto" border="10" /><br>
Figur 1. Resultatet av min egen labbundersökning med Google Assistant på svenska år 2019. 
</p>

**Larsson (2017)** behandlar **användarinitierade underdialoger**, *"i.e. interactions where a system question is responded to with a question or request from the user, who thus unitiates a sub-dialogue"* (Larsson 2017:1). Han citerar [Łupkowski & Ginzburg (2013)](https://hal.archives-ouvertes.fr/hal-01138036/document) som lyfter fram att frågor som svar på frågor utgör så mycket som 20 % av alla frågosvar i British National Corpus - det är alltså inget marginellt fenomen. 

Larsson testar hur några dialogsystem -- Siri, API.AI, Houndify, Cortana och Alexa -- reagerar på sådana underdialoger utifrån **Trindi Tick-listan** från [Bos et al. (1999)](https://gup.ub.gu.se/file/207630). Det är en lista över önskvärda beteenden hos ett dialogsystem, uttryckta i ja-och-nej-frågor som *"Is utterance interpretation sensitive to context?"*, t.ex:<br>

- När vill du anlända?<br>
- Imorgon<br>
- Du vill anlända fredag den 3 januari<br>

Han delar in de undersökte dialogsystemen i fyra olika kategorier:<br>
- **Stängda system:** har en fast uppsättning icke-konfigurerbara dialogapplikationer, t.ex. Siri.<br>
- **Konfigurerbara tjänsteplattformar:** erbjuder dialoghantering och domänimplentering genom att utvecklare kan välja domäner och koppla ihop deras färdiga implementeringar med specifika tjänster, t.ex. SiriKit och Houndify.<br>
- **Domänutvecklingsplattformar:** erbjuder generisk dialoghantering genom att låta utvecklare implementera sina egna domäner eller välja färdiga från en lista, t.ex. API.AI och Houndify.
- **Dialogskal:** erbjuder inbyggd taligenkänning (ASR), talförståelse (NLU) och text-till-tal-funtionalitet (TTS) - utvecklare implementerar dialoghanterare och domäner, t.ex. Cortana, Alexa.<br>        

Undersökningen av de valda systemen är uppdelad i två delar. En del som testar hur systemen reagerar när användare svarar på en fråga relaterad till en viss uppgift (t.ex. *ringa ett samtal*) med att fråga om en annan uppgift (t.ex. *status för missade samtal*), t.ex:<br>

- Ring ett samtal<br>
- Vem vill du ringa?<br>
- Har jag några missade samtal?<br>
- Nej, du har inga missade samtal. Tillbaka till samtalsläge. Vem vill du ringa? <br>

Resultaten av Larssons undersökning visar att ingen av de testade systemen (med utantag för Houndify som testades eftersom inget av dess domän hade fler än en uppgift som inkluderade frågor till användaren) kunde acceptera en sidouppgift och sedan gå tillbaka till den ursprungliga. 

Den andra delen av undersökningen tittade på hur väl systemen kunde hantera frågor kopplade till sidouppgifter från andra domäner än den ursprungliga uppgiftens domän (med avslutande återgång till den ursprungliga uppgiften) t.ex:<br>

- Ring ett samtal<br>
- Vem vill du ringa?<br>
- Vad är klockan?<br>
- 20:00. Tillbaka till samtalsläge. Vem vill du ringa?<br>

Resultaten av den här delen visade att Siri och Alexa kunde hantera den nya fråga-om-klockan-uppgiften, men utan återgång till ursprungsuppgiften. API.AI och Cortana ignorerade istället den nya uppgiften och upprepade frågan kopplad till den ursprungliga uppgiften: Vem vill du ringa?

I diskussionsdelen nämner Larsson att man skulle kunna testa fler dialogsystem, som Microsofts Luis eller IBM:s Watson. Han föreslår även att koppla samman dialoghanteringen med kvalitets-, användarbarhets- och attraktivitetsmått från exempelvis PARADISE-ramverket. Han poängterar också att användaren kanske inte alltid vill gå tillbaka till ursprungsfrågan, något som måste utvärderas från fall till fall. Avslutningsvis nämner han två system, Indigo från Artificial Solutions och Talmatatic Dialogue Manager (TDM) från Talkamatic, som faktiskt kan hantera underdialoger.
