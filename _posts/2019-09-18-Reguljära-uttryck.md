---
layout: post
title: 4. Reguljära uttryck
---

**Reguljära uttryck** är något som dykt upp i vår programmeringskurs på MLT-programmet redan andra veckan. Det är en algebraisk standardnotation för att definiera textsöksträngar i diverse applikationer. Strängen kan bestå av vilka alfanumeriska tecken som helst (inklusive kommatering och mellanslag: ␣). Det reguljära uttrycket kräver ett **mönster**, som i sin enklaste form kan vara ett enskilt tecken eller ord, t.ex. **/?/** eller **/ekorre/**. De är också **skiftlägeskänsliga**; **/a/** ger inte samma resultat som **/A/**.         

Om man vill hitta alla strängar som innehåller ordet "ekorre" oavsett gemen eller versal i början kan man skriva (inom hakparenteser) **/[eE]korre/**. Hakparenteser används också för att matcha strängar innehållande *något* av flera valfria tecken. Mönstret **[åäö]** matchar exempelvis såväl strängen "ödla" som "åska". Mönstret **[A-Z]**, med bindeestreck, matchar alla strängar som innehåller en versal. Insättningstecknet **^** (eng. *caret* /ˈkærɪt/) används för att negera reguljära uttryck, så att mönstret **[^d]** matchar alla strängar som *inte* innehåller bokstaven d (gäller dock endast om ^ är det första tecknet inom hakparentes). 

Ett mönster innehållande frågetecken, t.ex. **[klor?]**, matchar strängar innehållande "klo" och "klor" eftersom frågetecknet signalerar "föregående tecken eller inget alls". **Asteriksen** * (eng. **Kleene**, "cleany star") betyder "inga eller flera förekomster av föregående tecken", vilket innebär att mönstret **/bää*!/** kan matcha "bä!", "bää!" eller "bääääääääääää!". Om man istället vill uttrycka "en eller flera av föregående tecken" kan man använda **plustecknet** +, t.ex. **/bää+!/**. Punkten . är ett jokertecken (eng. **wildcard** expression) matchar vilket tecken som som helst; **/bä.a/** som matchar både "bära" och "bäva".
        
**Ankare** (eng. *anchors*) är specialtecken som kopplar reguljära uttryck till särskilda platser i en sträng. Om insättningstecknet sätts i början av ett sökmönster (inte inom hakparenteser), t.ex. **/\*Katt/**, så matchar det bara motsvarande strängar i början på en rad. **Dollartecknet $** matchar slutet av en rad, \b matchar en ordgräns (t.ex. **/\bork\b/** som matchar strängen "ork" men inte "orkar". Disjunktionen eller **pipe**-symbolen \| (som skrivs med AltGr + <>-tangenten på nordiska Windows-tangentbord) anger ett OR-uttryck: **/hund\|katt/** matchar antingen strängen "hund" eller "katt". Om man vill applicera pipe på en ändelse av ett ord, t.ex. hitta alla förekomster av orden "kvinna" och "kvinnor", skriver man mönstret så här: **/kvinn(a\|or)/**.

Vissa tecken har **företräde** (eng. *precedence*) framför andra. Hierarkin (med högst till lägst prioritet) redovisas i tabellen nedan: 

|Benämning      | Tecken       |
|---------------|--------------|
|Parentes       | ()           |
|Räknetecken    | * + ? {}     |
|Ankare         | ^my end$     |
|Disjunktion    |    &#124;    |

Reguljära uttryck är **giriga**, vilket innebär att de försöker matcha så stor del av en substräng som möjligt. Mönstret **/[a-z]\*/** matchar till exempel hela substrängen "blötdjur", och inte bara "b" eller "blöt" (även om det skulle kunna det).

*Uppdatering (19-09-2019)*: Medan jag gjorde en övning i boken hittade jag en praktisk grej som jag hade missat tidigare: **nummeroperatorn \1**. Den används för att refera tilbaka till ett tidigare uttryck. Så om man vill sätta vinkelparenteser runt varje förekomst av ett nummer i en text, kan man helt enkelt anropa **s/([0-9]+)/<\1>/**, där där s/ avser mellanslag och plustecknet uttrycker "en eller flera av föregående tecken". Det kan också användas för att säga att en viss sträng måste förekomma två gånger i texten (t.ex. "får får [får]"): **([a-zA-Z]+)\s+\1** 
