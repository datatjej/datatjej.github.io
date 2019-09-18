---
layout: post
title: Reguljära uttryck
---

Reguljära uttryck är något som dykt upp i vår programmeringskurs på MLT-programmet redan andra veckan. Det är en algebraisk standardnotation för att definiera textsöksträngar i diverse applikationer. Strängen kan bestå av vilka alfanumeriska tecken som helst (inklusive kommatering och mellanslag: ␣). Det reguljära uttrycket kräver ett **mönster**, som i sin enklaste form kan vara ett enskilt tecken eller ord, t.ex. /?/ eller /ekorre/. De är också **skiftlägeskänsliga**; /a/ ger inte samma resultat som /A/.         

Om man vill hitta alla strängar som innehåller ordet "ekorre" oavsett gemen eller versal i början kan man skriva (inom hakparenteser) /[eE]korre/. Hakparenteser används också för att matcha strängar innehållande <u>något</u> av flera valfria tecken. Mönstret [åäö] matchar exempelvis såväl strängen "ödla" som "åska". Mönstret [A-Z], med bindeestreck, matchar alla strängar som innehåller en versal. Insättningstecknet ^ (en. caret /ˈkærɪt/) används för att negera reguljära uttryck, så att mönstret [^d] matchar alla strängar som *inte* innehåller bokstaven d (gäller dock endast om ^ är det första tecknet inom hakparentes). 

Ett mönster innehållande frågetecken, t.ex. [klor?], matchar strängar innehållande "klo" och "klor" eftersom frågetecknet signalerar "föregående tecken eller inget alls". Asteriksen * (en. **Kleene**, "cleany star") betyder "inga eller flera förekomster av föregående tecken", vilket innebär att mönstret /bää*!/ kan matcha "bä!", "bää!" eller "bääääääääääää!". Om man istället vill uttrycka "en eller flera av föregående tecken" kan man använda plustecknet +, t.ex. /bää+!/. Punkten . är ett jokertecken (en. **wildcard** expression) matchar vilket tecken som som helst; /bä.a/ som matchar både "bära" och "bäva".
        
Ankare (en. **anchors**) är specialtecken som kopplar reguljära uttryck till särskilda platser i en sträng. Om insättningstecknet sätts i början av ett sökmönster (inte inom hakparenteser), t.ex. /*Katt/, så matchar det bara motsvarande strängar i början på en rad. Dollartecknet $ matchar slutet av en rad, \b matchar en ordgräns (t.ex. /\bork\b/ som matchar strängen "ork" men inte "orkar". Disjunktionen eller **pipe**-symbolen | (som skrivs med AltGr + <>-tangenten på nordiska Windows-tangentbord) anger ett OR-uttryck: /hund|katt/ matchar antingen strängen "hund" eller "katt". Om man vill applicera pipe på en ändelse av ett ord, t.ex. hitta alla förekomster av orden "kvinna" och "kvinnor", skriver man mönstret så här: /kvinn(a|or)/.

Vissa tecken har **företräde** (en. precedence) framför andra. Hierarkin (med högst till lägst prioritet) redovisas i tabellen nedan: 

| Parenetes        | ()          |
| Räknetecken      | * + ? {}    |
| Sekvenser/ankare | the ^my end$|
| Disjunktion      |      |      |
