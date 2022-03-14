---
layout: post
title: 57. Java-meddelandehantering med Apache ActiveMQ
---

I distribuerade system (system uppdelade på flera applikationer/datorer sammankopplade via internet) behöver olika tjänster och servrar kunna kommunicera med varandra, något som ofta sker med hjälp av en meddelandemellanhand (MOM, *message-oriented middleware*). Det möjliggör för en klient att göra ett API-anrop till en destination som sköts av mellanhanden. Mellanhanden behåller meddelandet tills det att en mottagare plockat upp det, vilket innebär att kommunikationen kan ske asynkront. 

## JM
Ett Java-baserat API för meddelandehantering är JM (*Jakarta Messaging*, tidigare JMS: *Java Message Service*) [[1]()] som stödjer så kallad punkt-till-punkt-kommunikation samt publicera/prenumerera-modeller. I punkt-till-punkt-kommunikationen skickas meddelandena av en producent (*producer*) till en meddelandekö enligt FIFO (*first in, first out*) som behåller meddelanden till dess att en konsument (*consumer*) plockat upp dem eller meddelandenas hållbarhet gått ut. I publicera/prenumerera-modellen publicerar en aktör meddelanden kring ett särskilt ämne som en eller flera användare (okända för publiceraren) kan välja att prenumerera på.     

## Apache ActiveMQ
Apache ActiveMQ beskrivs på den [egna hemsidan](https://activemq.apache.org/) som den mest populära open source-lösningen för Java-baserad meddelandehantering [[2]()]. Den följer JM-standardens specifikationer och varje meddelande som skickas med ActiveMQ består av en *header* (metadata om meddelandet, exempelvis sista förbrukningsdatum, och även persistens, dvs eventuell lagring på disk utifall att meddelandehanteraren kraschar), *properties* (frivillig metadata kring meddelandet, exempelvis för att tillåta konsumenten att filtrera meddelanden utifrån specifika properties-värden) och en *body* (meddelandekroppen, exempelvis text eller binär data) [[3](https://www.datadoghq.com/blog/activemq-architecture-and-metrics/)]. Apache ActiveMQ finns i två olika upplagor: en "klassisk" version och en nyare version som kallas Artemis.   

## Referenser
[1] [Jakarta Messaging](https://projects.eclipse.org/projects/ee4j.messaging). Oracle Java Documentation (14-03-2022).<br>
[2] [Apache ActiveMQ](https://activemq.apache.org/). activeMQ.apache.org (14-03-2022).<br>
[3] [ActiveMQ architecture and key metrics](https://www.datadoghq.com/blog/activemq-architecture-and-metrics/). David M. Lentz (13-09-2021).<br>

