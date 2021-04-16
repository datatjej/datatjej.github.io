---
layout: post
title: 21. Cooper storage
---

I semantikkursen har vi den senaste veckan stiftat bekantskap med en teknik för att bryta ner och tolka meningar som innehåller kvantifikatorer: **Cooper storage**. Eftersom den är utvecklad av professor emeritus Robin Cooper från Göteborgs universitet (se [Quantifications and Syntactic Theory](https://link-springer-com.ezp.sub.su.se/book/10.1007/978-94-015-6932-3#about) från 1983) kändes det givet att ägna den lite extra uppmärksamhet här.

Så varför behöver vi en särskild metod för att **hantera kvantifikatorer**? Jo, för att kvantifikatorer lätt blir **tvetydiga** så fort man har fler än en. Ett klassiskt exempel inom logiken är *Alla beundrar någon*, som innehåller allkvantifikatorn ∀ ("alla") och existenskvantifikatorn ∃ ("någon"). Den meningen antingen kan tolkas som att alla beundrar en och samma person (t.ex. Beyoncé) eller – och kanske det mest intuitiva – att alla beundrar någon, men att denna någon inte är densamma för alla. Detta kallas på engelska **quantifier scope ambiguity** (kvantifikatorisk omfångstvetydighet? ¯\ _(ヅ)_/¯).  

Antalet möjliga tolkningar växer med [fakultetsfunktionen](https://sv.wikipedia.org/wiki/Fakultet_(matematik)) (som skrivs med utropstecken), vilket innebär att tre kvantifikatorer i en mening ger 3! möjliga tolkningar, d.v.s. 1 * 2 * 3 = 6 stycken. När man parsar en mening i naturligt språk vill man kunna ta hänsyn till alla möjliga sådana tolkningar utan att låsa fast sig vid någon specifik. Man söker därför en **underspecificerad betydelserepresentation** (eng. *underspecified meaning represenation*) av meningen utifrån vilken man sedan kan generera mer specifika betydelser. 

Det är här Cooper-förrådet kommer in i bilden. Om man tänker sig att meningen ovan, "Alla beundrar någon", kan tolkas på två sätt:<br> 
(1) all x.(person(x) -> exists y.(person(y) & admire(x,y)))<br>
(2) exists y.(person(y) & all x.(person(x) -> admire(x,y)))<br>
...där (1) är den konventionella tolkningen och (2) är Beyoncé-tolkningen, så kan man oavsett tolkningsval ändå konstatera att den finns en underliggande betydelse i meningen – nämligen att någon beundras. Detta illusteras i bilden nedan, som jag har ritat med inspiration av ett annat exempel från [kapitel 10:4.5 i NLTK-boken](http://www.nltk.org/book_1ed/ch10.html) (Bird, Klein, Loper 2009).

<p align="center">
<img src="/images/beundra.jpg" alt="Strukturen hos meningen Alla beundrar någon" border="10" /> <br>
Olika läsningar av meningen "Alla beundrar någon". De grekiska bokstäverna Φ [fi] och ψ [psi] står som platshållare för det kvantifikatorn avser.</p>

"Förrådet" i Cooper storage-metoden innehåller just den här nedskalade betydelsen, **kärnan** (eng. *core*), av meningen, *beundra (x,y)* eller *x beundrar y*, samt en lista över **operatorer** (eng. *binding operators*) som motsvarar de kvantifierade nominalfraser som står ovanför *beundra(x,y)* i bilden ovan, fast i lambda-format (så *∀x.(person(x) -> Φ* blir exempelvis *λQ.∀x.(person(x) -> Q(x)*). Beroende på i vilken ordning vi sedan väljer att plugga in endera operator i kärnan får vi olika betydelse av meningen (antingen att alla beundrar olika personer eller en och samma person). Detta kallas **s-retrieval**. 

Jurafsky och Martin (2009: 595) nämner **två begränsningar** med Cooper storage-metoden. Den ena är att det finns en mängd andra sätt som meningar kan vara tvetydiga på, men Cooper storage löser enbart problemet med kvantifierade nominalfraser. Den andra är att även om vi skulle kunna utöka metoden för att hantera andra former av tvetydighet, så kvarstår det faktum den inte tillåter oss att specificera vilka möjliga tolkningar av en mening som är mer troliga än andra genom att sätta villkor: *"This is an ability that is crucial if we wish to be able to apply specific lexical, syntactic, and pragmatic knowledge to narrow down the range of possibilities for any given expression"* (2009: 595-596).
