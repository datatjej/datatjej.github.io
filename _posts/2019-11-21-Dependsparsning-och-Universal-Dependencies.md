---
layout: post
title: 12. Dependensparsning och Universal Dependencies
---

**Dependensparsning**, som behandlas i kapitel 15 i Jurafsky & Martin (2009), skiljer sig från konstituentparsning genom att den syntaktiska strukturen i språket betraktas på ord- och inte frasnivå. Mellan orden råder **riktade, binära, grammatiska relationer** som representeras av bågar från de så kallade **huvud**-orden till **dependenterna**. Exempel på relationer är *subjekt* (NSUBJ), *direkt objekt* (DOBJ) och *indirekt objekt* (IOBJ), men även olika former av attribut (eng. *modifier*, MOD) och *kasus* (CASE) genom exempelvis prepositioner och postpositioner.

<p align="center">
<a title="Tjo3ya [CC BY-SA 4.0 (https://creativecommons.org/licenses/by-sa/4.0)], via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:UD_picture_1.1.jpg"><img width="128" alt="UD picture 1.1" src="https://upload.wikimedia.org/wikipedia/commons/e/e9/UD_picture_1.1.jpg"></a>
</p>

Dessa relationer är **riktade grafer** som matematiskt kan skrivas *G = (V,A)*, där *V* är en uppsättning noder som motsvarar morfem och/eller satstecken i en mening, och *A* är en uppsättning ordnade nodpar som också kallas **bågar**. Bågarna motsvarar relationerna mellan elementen i *V*. Vidare måste grafen uppfylla följande kriterier:
1) ...att det finns en enskild **rotnod** som inte har några inkommande bågar.  
2) ...att alla noder, med undantag för rotnoden, har en och enbart en inkommande båge.
3) ... att det finns en unik väg från rotnoden till varje nod i *V*.        

De riktade graferna karaktäriseras också av något som kallas **projektivitet**. Bågen mellan huvud och dependent är att beteckna som projektiv om det finns en väg från huvudet till varje ord som befinner sig mellan och huvudordet och dependenten. Många av de stora dependensträbankerna för engelska är genererade genom huvudletande i frasträdbanker, vilket innebär att alla sådana meningar är projektiva av naturen. Projektivitet är också en förutsättning för många av de vitt användna parsningsalgoritmerna, vilket innebär att icke-projektiva meningar genererade av sådana algoritmer kommer att innehålla en del fel. 

Vad är då **fördelarna** med att använda dependensparsning framför konstituentparsning? Under föreläsningen i tisdags nämndes flera. På ett lingvistiskt plan fungerar dependensparsning bättre för **språk med fri ordföljd**, och den visar också bättre den **semantiska relationen** mellan ord genom huvud-dedepent-relationen. På ett beräkningsmässigt plan leder dependensparsningen till **effektivare algoritmer**, eftersom bara ett beslut per ord måste tas och det inte finns några gömda icke-terminaler. Dependensannoteringen har också genomgått en viss **standardisering**, vilket tyvärr saknas för konstituentparsningen och dess trädbanker, skapade innan internet-eran. 

Ett känt ramverk för dependensannotering är [**Universal Dependencies**](https://universaldependencies.org/) (UD), som också tillhandahåller ett stort antal öppna träbanker för i dagsläget (nov 2019) 90 språk. UD har stark representation från Uppsala universitet genom arbetet och koordineringen från professor Joakim Nivre.

På Youtube finns den här 10 minuter långa introduktionsvideon till ämnet:

<p align="center">
<a href="https://www.youtube.com/watch?v=1_LQscB4Wso"><img src="/images/dependency_parsing.PNG" 
alt="Dependency Parsing Introduction" width="480" height="360" border="10" /></a></p>
