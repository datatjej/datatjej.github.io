---
layout: post
title: 27. Larsson et al. (2000) och Larsson et al. (2019)
---

Idag tänkte jag försöka sammanfatta två dialogsystemsartiklar som skrivits med nästan 20 års mellanrum. Den ena är [*Using the process of distilling dialogues to understand dialogue systems*](https://www.ida.liu.se/~arnjo82/papers/icslp-sllsaj-00.pdf) av Larsson, Santamarta och Jönsson (2000) och den andra *Towards negotiative dialogue for the Talkamatic Dialogue Manager* av Larsson, Berman och Hjelm (2019). Den senare presenterades under [Swedish Dialogue Workshop](https://sites.google.com/view/swedish-dialogue-workshop-2019/home) i Göteborg förra året. 

Den gemensamma nämnaren för artiklarna är Staffan Larsson, professor i språkteknologi här vid Göteborgs universitet och grundare till [Talkamatic](http://talkamatic.se/), vars dialoghanterare TDM vi kommer att använda i kursen Dialogsystem II (LT2319).

Den första artikeln behandlar **destillering av dialoger**, d.v.s. att skriva om annoterade, mänskliga dialoger från talkorpora för att simulera människa-dator-interaktioner och använda dem i dialogsystem. Detta presenteras som ett komplement till dialoger insamlade från **autentiska interaktioner mellan människor** i liknande sammanhang, samt till **Wizard of Oz-metoden** för dialoginsamling, där två personer pratar och den ena personen helt eller delvis simulerar ett datorsystem (vilket kan påverka den som spelar användarparten att formulera sig på ett visst sätt). Författarna menar att dialogdestillering också kan användas för att bättre förstå sig på människa-dator-interaktioner. 

Man fortsätter med att beskriva destilleringsprocessen i två delar: 1) utveckling av riktlinjer, 2) applicering av riktlinjer. De generella riktlinjerna man ger är att **inte ändra användarens yttranden**, att **låta dialogen förbli sammanhängande** och att **bestämma vilka deltagare dialogen består av** (ta bort yttranden från eller riktade till icke-deltagare). Principen att inte ändra användaryttranden har att göra med att man inte vill begränsa användarens naturliga uttryckssätt. För systemets del presenterar man däremot en lista över **språkliga, funktionella och etiska egenskaper** som man anser borde tas i beaktning när man anpassar den mänskliga service-aktörens yttranden till ett dialogsystem.  

Bland de **språkliga egenskaperna** nämner man bland annat att systemet inte ska avbryta användaren, att det ska omformulera sig om användaren inte förstår och att de ska erbjuda feedback när användaren talar. Till de **funktionella egenskaperna** räknas bland annat att systemet ska kunna minnas information, inte upprepa sig, samt kunna motivera sina handlingar om det ombeds att göra det. De **etiska egenskaperna** omfattar exempelvis att systemet ska vara artigt och inte tvinga på sina egna "åsikter" eller på annat sätt övertyga användaren. Några av de etiska egenskaperna som listas känns dock lite föråldrade, t.ex. att systemet inte ska vara ironiskt eller skämtsamt.

När riktlinjerna väl appliceras kan det ske genom att hela dialogen granskas i olika omgångar utifrån kategori, eller att varje yttrande som kan tillskrivas systemaktören granskas ett och ett utifrån samtliga kategorier. Det viktiga är att dialogen förblir sammanhängande även om yttranden modiferas eller tas bort. Den här nära granskningen av yttranden och dialoger samt utformandet och appliceringen av riktlinjer tvingar granskaren att djupare reflektera över människa-dator-interaktion och vad som krävs för ett bra dialogsystem.

Den andra artikeln handlar om **"negotiative dialogue"** (*förhandlande dialog?*), d.v.s. att användaren ska kunna **undersöka och jämföra olika träffar** i en databassökning tillsammans med dialogsystemet med dialogutbyten av den här typen (inspirerat från dialogen som presenteras i artikeln):<br>
- Finns det någon Anna på Dr Forselius Backe?<br>
- Det finns två stycken, en Anna Larsson och en Anna Holtz.<br>
- Hur gammal är de?<br>
- Anna Larsson är 42 och Anna Holtz 37.<br>
- Jag tror den jag letar efter är över 40. Vad är 42-åringens nummer?<br>
- Okej, Anna Larssons nummer är NNN-NNN NN NN.<br>
- Toppen. Men skulle jag kunna få den andras nummer också?<br>
- Visst. Anna Holtz nummer är NNN-NNN NN NN.<br> 
- Tack!<br>

Ovanstående dialog skiljer sig lite från en traditionell databassökning i ett dialogsystem som enbart presenterar alla möjliga träffar, eller ett som inkrementellt smalnar av och presenterar sökspannet på nytt allteftersom användaren ger mer information. I den förhandlande dialogen kan användaren **be om mer information kring en eller flera sökträffar** (*"Hur gamla är de?"*). Författarnas egna experiment där en av deltagarna agerade system visade också att frågor av typen 
*"**Vet du** vilken gata personen bor på?"* är vanligare än frågor som förutsätter visst vetande (*"Vilken gata bor personen på?"*), något som borde tas i beaktning vid utvecklande av den här typen av dialogsystem.