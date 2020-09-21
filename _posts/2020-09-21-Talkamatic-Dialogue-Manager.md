---
layout: post
title: 28. Talkamatic Dialogue Manager
---

Idag läser jag [Domain-specific and General Syntax and Semantics in the Talkamatic Dialogue Manager](http://www.cssp.cnrs.fr/eiss11/eiss11_larsson-and-berman.pdf) av Larsson & Berman (2016) som beskriver designfilosofin bakom dialogsystemet TDM (Talkamatic Dialogue Manager). I kort går det ut på att **skilja domän-specifik kunskap från den allmänna dialoghanteringen**. 

Men först några ord om dialogsystem. Det finns fyra generella typer:<br>
1) ändliga automater<br>
2) formulärbaserade<br>
3) planbaserade<br>
4) informationstillståndsbaserade<br>

De **ändliga automaterna** följer en strikt frågeföljd där användarens svar triggar nästa steg i flödesschemat. Systemet har allt dialoginitiativ och tenderar att ignorera eller missförstå svar som inte direkt besvarar en specific systemfråga. De **formulärbaserade dialogsystemen** består av fält (*slots*) och värden (*values*) som ofta motsvarar specifika systemfrågor och användarsvar, t.ex. fältet ANKOMSTSTAD och frågan *"Vart vill du resa?"*. Den här typen av system kan tillåta att användaren själv tar visst talinitiativ genom att låta hen ge mer information än vad som för tillfället efterfrågas:<br>
- Vart vill du resa?<br>
- Till Malmö den 22 september

Så gott som alla de stora systemen på marknaden är formulärbaserade, exempelvis Siri och Google Assistant. 

De **planbaserade dialogsystemen** var populära på 80- och 90-talet och betraktar dialoger som centrerade kring *planering* - vad man ska säga och göra härnäst. Systemet antar att användarna är rationella varelser som har ett bestämt syfte med sina yttranden. Om någon på en tågstation frågar en tågvärd när nästa tåg till Stockholm avgår kan man anta att personen faktiskt tänkte resa med det tåget och eventuellt skulle vilja ha ytterligare upplysningar, exempelvis om vilken plattform det avgår ifrån (se (Allen & Perrault 1980)[https://nlp.stanford.edu/acvogel/allenperrault.pdf]). De planbaserade systemen kan i teorin hantera väldigt komplexa dialoger, men i praktiken är de dyra att utveckla och går lätt sönder (Larsson 2020-09-01, förläsningsslides).

De **informationstillståndsbaserade dialogsystemen** (eng. *information state approach*) är en blandning av de ändliga automat- och formulärbaserade ansatserna samt den planbaserade. Informationstillståndet visar vilket tillstånd dialogen befinner sig i vid ett givet tillfälle: *"[it] represents the information necessary to distinguish it from other dialogues, representing the cumulative additions from previous actions in the dialogue, and motivating future action"* (Traum & Larsson 2000:3). 

Man skiljer den här ansatsen från det man kallar dialogtillståndsbaserade ansatser (eng. *dialogue state approaches*): *"These latter approaches conceive a 'legal' dialogue as behaving according to some grammar, with the states representing the results of performing a dialogue move in some previous state, and each state licensing a set of allowable next dialogue moves"* (Traum & Larsson 2000:5). 

En viktig princip för de informationstillståndsbaserade systemen är **"separation of concerns"**: man ser till att hålla **domänkunskapen** (t.ex. telefoni eller vägbeskrivning), den **lingvistiska kunskapen* och själva **dialoghanteraren** åtskilda. Den här åtskiljnigen gör det enklare att lägga till nya domäner, att anpassa systemet till nya språk och att uppdatera dialoghanteraren med ny funktionalitet. TDM är ett informationstillståndssystem som följer den här principen. 

TDM består förutom av en **frontend** (tal-till-text och text-till-tal), **backend** (dialogue move engine) och en **session manager** (som ger varje frontend tillgång till backend) också av följande komponenter:<br>
- en **ontologi**: definierar koncept, entiteter och handlingar (*actions*) som användaren eller systemet kan referera till<br> 
- **domänkunskap**: dialogplaner som beskriver hur handlingar utförs, frågor ska besvaras samt vilken information som krävs för detta<br>  
- en **språkmodell** (LM): beskriver ord och yttranden som systemet eller användaren kan använda<br>
- en **service interface**: beskriver hur de tjänster (t.ex. API:er eller annan funktionalitet) som domänen behöver ska nås och användas<br>

En av styrkorna med TDM gentemot andra dialogsystem är att det lättare **kan hoppa mellan olika ämnen** och **inte glömmer bort** tidigare påbörjade handlingar. Det är också bättre på att hantera de **dialogtypiska fenomen** som beskrevs i det här [inlägget](https://datatjej.github.io/Larsson-(2015)-och-Larsson-(2017)/): over-answering, other-answering och answer revision. Även **feedbackfunktionaliteten** är genomarbetad och användaren kan få återkoppling kring huvuvida systemet överhuvudtaget uppfattade frågan samt om den förstod innebörden och intentionen.

I ontologin finns bland annat **predikat** som definierar propositioner och frågor. Dessa motsvaras av predikat av första ordningens logik, men i lite nedskalad form. Yttrandet "Jag vill åka till Paris" kan semantiskt representeras som *vill(användare, åka-till(användare, paris)* eller *vill(a, åka-till(a,p)) & namn(p, paris) & användare(a)*, men i TDM skippar man de överflödiga *vill*- och *användare*-värdena och skriver *dest-city(paris)* (Larsson & Berman 2016:98). 

TDM:s **dialogplaner** ger information om hur dialogen med användaren bör fortlöpa. Högst upp deklareras en övordnad plan (action = "top") som definierar vad systemet ska göra i början av varje interaktion (Larsson & Berman 2016:100). Planerna definieras generellt av sitt tillhörande *goal* som beskriver vad planan ska ha uppnått när den är färdig. De finns två slags planer i TDM:<br>
- **resolve**(q) där q:Question<br>
- **perform**(a) där a:Action

Planerna består av ett visst antal steg som krävs för att uppnå målet. Ibland kan det vara en handling utanför själva dialogen som måste genomföras, som att ringa den av användaren valda kontakten. Dessa steg har taggen *dev_perform*, eller *dev_query* om det istället handlar om en fråga skickad till service interfacet.

**TDM-språkmodellen** (eller *grammatiken*) definieras av domän-specifik och generell grammatik. Den generella grammatiken beskriver utformingen av *dialogue moves* (*ask*, *answer*, *request*, *confirm*, *greet*) och olika typer av återkoppling, medan den domän-specifika bland annat beskriver vilket eller vilka nyckelord som ska användas som kommando för en viss uppgift.

Artikeln av Larsson & Berman (2016) avslutas bland annat med en diskussion om vikten av **semantisk koordinering** inom dialogsystem. Som systemen ser ut idag måste användaren ofta formulera sig på ett visst sätt för att bli förstådd, vilket kan kan resultera i en överbelastad tankeverksamhet vid fokuskrävande moment som bilkörning. Det bästa vore därför om systemen kunde lära sig förstå och hantera alternativa benämningar för individer och predikat: *"However, semantic coordination will be more useful when meaning representations have more structure and when more reasoning is performed"* (Larsson & Berman 2016:107).
