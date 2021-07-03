---
layout: post
title: 35. Använda en API
---

För det egna projektarbetet i Dialogsystem II-kursen blev vi instruerade att leta upp en API för en tjänst och integrera den i en chattbot. En **API** (eng. *application programming interface*) är ett mjukvarulager som tillåter dig att kommunicera med en extern webbsidas server, exempelvis för att logga in på en sida via ditt Google-konto (sidan måste då kommunicera med Googles API) eller för att, som i mitt fall, kunna få mitt dialogsystem att hämta information från mitt konto på [Todoist](https://todoist.com/app/). 

För att använda en API ordentligt krävs ofta lite kunskap om hur informationen som API:n kan returnera ser ut, och hur man gör själva anropen. I sin enklaste form kan ett API-anrop göras via en URL-sträng som inkluderar vissa parameterar. I en av våra tidigare labbar använde vi exempelvis en URL av det här formatet:<br> `http://api.openweathermap.org/data/2.5/weather?q={city},{country}&units={unit}&APPID={key}`<br> 
...för att hämta väderdata från webbsidan [OpenWeather](https://openweathermap.org/). Parametern "key" avser här en användares egna **API-nyckel**, som är unik för användaren och fungerar som ett slags ID.

API-datan returneras oftast i **JSON-format** (eng. *JavaScript Object Notation*), även om andra format som XML och YAML också är möjliga. Den är organiserad i nyckel-värde-par (eng. *key-value pairs*) i en ordboksliknande datastruktur som i bildan nedan. Där syns hur nyckelvärdet *weather* tar en ny ordbok av nyckel-värde-par som värde, innehållandets bland annat *"description":"scattered clouds"*. I bilden ses även nyckel-värde-paret *"temp[eratur]":"12.19 [grader celsius]* och *"name":"Gothenburg"*.  

<p align="center">
<img src="/images/openweather_json.PNG" alt="JSON-data från OpenWeather" width="60%" height="auto" border="10" /><br>
Väderdata för Göteborg i JSON-format från OpenWeather</p> 

Om man vill manipulera den här datan i ett eget program kan det vara till hjälp att använda sig av färdiga **bibliotek** (sammanställda av webbsidan eller företaget vars API man vill använda sig av) som redan innehåller en del funktioner som man kan tänkas behöva. Todoist erbjuder till exempel ett [Python-bibliotek](https://github.com/Doist/todoist-python) som går att `pip`-installera i terminalen (`pip3 install todoist-python`) och sedan importera (`from todoist.api import TodoistAPI`) till egna Python-filer. En [väldokumenterad API](https://developer.todoist.com/sync/v8/?python#overview) och ett bra Python-bibliotek underlättar rejält.

Det finns olika typer av **API-requests**. Den vanligaste är kanske `GET`-metoden som används för att hämta data. De andra är `POST` (används för att skapa ny data på server-sidan), `PUT`/`PATCH` (används för att uppdatera befintlig data på server-sidan) och `DELETE` (för att radera data från server-sidan). För att kunna använda dessa metoder i ett Python-script behöver man importera HTTP-biblioteket **requests**, vilket redan görs Todoists eget Python-bibliotek.

Jag önskar att jag lärt mig använda API:er lite tidigare i min progammeringsbana, för det är verkligen magiskt när man lyckas hämta data från sitt egna konto på en webbsida genom några rader kod i terminalen eller genom ett eget script. Det öppnar också upp för möjligheten att skriva enkla program för att **automatisera vardagssysslor** och låta datorn sköta det tråkiga.

*Uppdatering (21-04-2021):* Jag har nu testat på att använda Twitters API för att skapa en Twitter-bot som kvittrar ut den senaste covid 19-vaccinationssatistiken för Sverige! Läs mer om det [här](https://datatjej.github.io/Vaccinationsboten-ett-miniprojekt/).