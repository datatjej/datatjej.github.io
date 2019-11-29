---
layout: post
title: 15. Bayes teorem
---

Eftersom jag har tappat räkningen över hur många gånger jag hört namnet "Bayes" nämnas under MLT-programmets gång, var jag tvungen att dubbekolla om jag inte skrivit något om **Bayes teorem** tidigare. Men det verkar inte som det. Teoremet har fått sin namn efter den engelske prästen Thomas Bayes (1702-1761) som först beskrev det, och utgör en grundbult inom sannolikhetsläran. Det formulerar sannolikheten för att något ska inträffa utifrån något annat, t.ex. sannolikheten för att solen ska stiga upp även imorgon ifall man har sett den stiga upp x antal gånger tidigare. 

<p align="center">
<a href="https://www.youtube.com/watch?v=XQoLVl31ZfQ&t=202s"><img src="/images/bayesteorem.PNG" 
alt="Bayes' Theorem - The Simplest Case" width="480" height="360" border="10" /></a></p>

I femminutersvideon ovan ges uppställningen för teoremet i blått och jämförs hur det förhåller sig gentemot de enkla beräkningarna av villkorsbaserad sannolikhet (i rött och gult). Fördelen som lyfts fram med Bayes teorem är att man enkelt kan anpassa det utifrån den fakta man redan har, t.ex. om man vet sannolikheten för A givet B men inte tvärtom.

Bayes gav också upphov till det som kallas **bayesiansk statistik** eller **bayesiansk inferens**, vilket är en metodik som utgår ifrån att sannolikheten för ett fenomen hela tiden förändras i takt med att man får mer empirisk kunskap om det. Ju fler gånger man ser solen gå upp, desto säkrare slutsats kan man dra att den också går upp imorgon. 

I videon nedan (väldigt sevärd!) förklaras hur man kan applicera Bayes teorem på sammanhang där man intuitivt kanske tror att sannolikheten är större än den egentligen är, t.ex. sannolikheten för att ha en ovanlig sjukdom om ett test med 99-procentig säkerhet visar positivt. Man nämner också användandet av Bayes teorem inom ett språkteknologiskt användningsområde, nämligen **skräpposthantering**. Genom att applicera ett **bayesiskt filter** kan man utifrån de ord som används i ett mejl beräkna sannolikheten för att det rör sig om just skräppost. 

<p align="center">
<a href="https://www.youtube.com/watch?v=R13BD8qKeTg"><img src="/images/spam.PNG" 
alt="The Bayesian Trap" width="480" height="360" border="10" /></a></p> 

Videon tar också upp något intressant som man väljer att kalla den **den bayesiska fällan** (eng. *Bayesian trap*). Om man enbart utgår ifrån den kunskap man har sedan tidigare för att beräkna sannolikheten för att något ska hända, så finns kanske risken att man blir blind för andra möjliga utfall. Överfört till andra sammanhang än solens upp- och nedgång skulle det kunna innebära att man vänjer sig vid resultatet av vissa händelser och handlingar för att man aldrig sett hur saker skulle kunna gå annorlunda. Kanske upplever man att sannolikheten för att lyckas på uppkörningen är närmast obefintlig om man misslyckats alla tidigare gånger, vilket riskerar att bli en självuppfyllande profetia.