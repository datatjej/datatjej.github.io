---
layout: post
title: 17. Vad kännetecknar mänskliga dialoger?
---

Häromveckan gick NLP-kursen in på det som mer än något annat symboliserar språkteknologins reella framsteg och baksteg: **dialogsystemen**. Även om virtuella assistenter som Alexa och Google Home blivit allt vanligare år 2019, har den röststyrda tekniken ännu inte helt ersatt tangenterna och musklicken i våra datorer och mobiler.               
 
Vari ligger då svårigheterna i att utveckla system som kan hantera dialoger med användaren på ett så övertygande sätt att det klarar [Turing-testet](https://en.wikipedia.org/wiki/Turing_test)? Mänskliga dialoger är ett komplext maskineri som kännetecknas av **talarbyte**, **talakter**, **grounding**, **subdialoger**, **initiativ** samt **implikatur** (Jurafsky & Martin 2019). 
 
**Talarbyte** (eng. *turns*) omfattar två problem: dels att systemet måste kunna avgöra när användaren talat klart, s.k. *endpoint detection* (vilket kan vara svårt pga störande ljud eller pauser mitt i mening), och dels att det måste kunna avgöra när det själv ska sluta tala p.g.a. av avbrott från användaren för exempelvis rättning eller precisering.  

**Talakter** avser teorin om att alla yttranden i en dialog kan betraktas som handlingar, framförallt då konstateranden, direktiv, förpliktelser och bekräftanden. En fråga är till exempel ofta ett gömt direktiv, om den frågande förväntar sig ett svar. Genom att sätta korrekt talakt på ett yttrande kan systemet lättare avgöra hur det ska besvara det.

Dialoger är ofta strukturerade kring talakater i par, s.k. *adjacency pairs*, som kan vara av typen FRÅGA/SVAR, FÖRSLAG/ACCEPTERANDE, KOMPLIMANG/BORTVIFTANDE, osv. Men ibland förekommer **subdialoger** eller **sidosekvenser** mellan första och andra delen av paren, vilket ett system måste vara utrustat för att hantera. Exempel på det är rättningssekvenser, där en person som bett om biljetter för ett visst datum plötsligt ändrar sig och frågar om ett annat datum. 

Begreppet **grounding** handlar om att talare måste hitta *common grounds*, på svenska "gemensamma ståndpunkter" eller en "gemensam plattform". Men läser man Jurafsky & Martin (2019) verkar det snarare handla om att bekräfta att man förstår vad den andra säger, vilket kan ske genom att upprepa vad den andra just har sagt, eller bara genom ett enkelt "okej".

Dialoger kännetecknas även av **initiativ**, vilket avser intiativet - och därmed kontrollen - som en eller flera talare har i dialogen och den riktningen den tar. I naturligt förekommande dialoger brukar intitiativet vara mixat - man turas om att introducera nya samtalsämnen. Men i dialoger med system är det inte ovanligt att systemet har allt eller inget intiativ alls. Jämför exempelvis digitala kundserviceassistenter ("Uppge personnummer", "Uppge vilken typ av ärende samtalet gäller"...) och passiva hemmaassistenter såsom Alexa. 

Dialogsystemen måste slutligen ockå kunna hantera det som Grice (1975) kallar **konversationell implikatur**. Människor utgår ofta ifrån att den information man får från motparten i ett dialogutbyte har relevans för det ämne som för närvarande avhandlas. Om någon på frågan "När vill du resa till Paris?" svarar med att konstatera att hen måste vara på konferens där mellan 29 februari till 3 mars, så kan man anta att detta har relevans för önskat in- och utresedatum. Detta är uppenbart för en människa, men svårare att få ett dialogsystem att förstå.

Utmaningarna är därför många, men fördelarna med välfungerande dialogsystem är fler än de uppenbart bekvämlighetsrelaterade. I videon nedan argumentar den välrenommerade datavetaren Raj Reddy för vinsterna med att **tillgängliggöra röststyrd teknik för jordens fattiga och analfabeta befolkningar**.

<p align="center">
<a href="https://www.youtube.com/watch?v=m81KPZVqB70"><img src="/images/rajreddy.PNG" 
alt="4th HLF – Hot Topic: Artificial Intelligence – Presentation Raj Reddy" width="100%" height="auto" border="10" /></a></p>
