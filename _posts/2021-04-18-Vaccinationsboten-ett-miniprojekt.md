---
layout: post
title: 49. Vaccinationsboten - ett miniprojekt
---

Jag har länge velat göra något enklare programmeringsprojekt som inte är kopplat till mina studier. Såg en Twitter-bot som kvittrade ut en förloppsindikator för hur många procent av USA:s befolkning som hunnit vaccineras mot covid-19 och tänkte att det vore nice att göra något liknande för Sveriges vaccinationsstatistik. 

Jag kan inte längre hitta den ursprungliga boten men såg [den här](https://twitter.com/vaccination_bar) som gör något liknande för världens befolkning som helhet och [den här](https://twitter.com/uk_vaccination) för Storbritannien. För Sveriges del har jag hittills försökt hålla mig uppdaterad om statistiken genom att kika på SVT:s snygga [grafiska representation](https://www.svt.se/datajournalistik/corona-vaccin/) av andelen vaccinerade. Den bygger på data från Folkhälsomyndigheten, som i sin tur hämtar den från Nationella vaccinationsregistret.

## Skaffa Twitter Developer-konto 

Jag har aldrig skapat någon Twitter-bot tidigare och började med att skaffa ett [developer-konto](https://developer.twitter.com/en/apply-for-access) på Twitter. Man måste ge en kort beskrivning av sitt projekt och vilka funktioner man tänkte använda, men det går väldigt snabbt och verkar godkännas på en gång. Jag gjorde först misstaget att ansöka om ett developer-konto kopplat till mitt personliga Twitter-konto, men eftersom jag ville ha ett separat konto för boten skaffade jag det och ansökte sedan på nytt med just det kontot.  

## Hämta data från Folkhälsomyndigheten

Den första uppgiften för projektet är att hämta data från [Folkhälsomyndigheten](https://www.folkhalsomyndigheten.se/smittskydd-beredskap/utbrott/aktuella-utbrott/covid-19/statistik-och-analyser/statistik-over-registrerade-vaccinationer-covid-19/). På deras hemsida presenteras antalet och andelen personer (>=18 år) som vaccinerats så här:

<p align="center">
<img src="/images/fohm_tabell.PNG" alt="FOHM:s tabell över antalet vaccinerade" width="90%" height="auto" border="10" /><br>
<font size="2">Folkhälsomyndighetens tabell över antalet och andelen vaccinerade med 1:a och 2:a dosen bland personer 18 år eller äldre.</font>
</p>

Tyvärr finns det också mycket annat oväsentligt på sidan: sidomenyer, brödtext, länkar, andra tabeller, m.m. Jag vill bara hämta informationen från den här tabellen. Konsten att hämta ut information av intresse på nätet kallas data- eller webbskrapning (eng. *web scraping*). De tutorials jag hittat börjar alla med tipset att väja "Inspect"-alternativet när man högerklickar på sidan och undersöka hur datat är strukturerat. 

<p align="center">
<img src="/images/inspect_element.PNG" alt="Alternativet Inspect Element när man högerklickar på en sida" width="50%" height="auto" border="10" /><br>
<font size="2">I Firefox heter "Inspect"-alternativet i högerklicksmenyn "Inspect Element".</font>
</p>

Då dyker det upp en sån här Inspector-ruta som visar HTML- och CSS-koden för sidan. Efter att ha klickat mig runt i myllret av `<div>`-taggar hittade jag till slut tabellen:   

<p align="center">
<img src="/images/html_table.PNG" alt="HTML-koden för tabellen som visas i Inspector-fönstret" width="auto" height="auto" border="10" /><br>
<font size="2">Den nästlade HTLM-koden för tabellen av intresse i Inspector-fönstret.</font>
</p>

Med vetskapen om att en tabell i HTML inleds med en `<table>`-tag med en beskrivande *caption* som har kolumnrubriker inom `<th>`-taggar och det egentliga datat inom `<tr>`-taggar strider vi till verket och skriver ett Python-script för extrahera det. Jag följde [den här](https://towardsdatascience.com/web-scraping-scraping-table-data-1665b6b2271c) steg-för-steg-guiden med några smärre ändringar för hämta rätt tabell och parsa siffrorna så som jag ville ha dem. HTML-parsningen sker med [`BeautifulSoup`](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)-paketet och själva datat sparas i datamanipuleringsprogrammet pandas [`DataFrame`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)-struktur.  

<p align="center">
<img src="/images/vaccinerade_df.PNG" alt="Den extraherade datan i pandas DataFrame-format" width="auto" height="auto" border="10" /><br>
<font size="2">Det extraherade tabelldatat i pandas DataFrame-format.</font>
</p>

## Generera Twitter-meddelandets sträng

Jag försökte till en början få till förloppsindikatorerna med hjälp av `tqdm` precis som killen i [den här](https://blog.esciencecenter.nl/twitter-bots-for-science-1cf3f19dcda8) guiden hade gjort. Men när det inte blev bra valde jag istället en mer basic lösning, nämligen att själv skriva ut en indikator med hjälp av klammertecken, [blockelement](https://en.wikipedia.org/wiki/Block_Elements) i unicode och punkter. Hur stabil den här lösningen är återstår att se, men det verkar fungera hjälpligt. Nackdelen är att indikatorerna inte får samma storlek när de twittras ut:

<p align="center">
<img src="/images/progress_bar_terminal.PNG" alt="De handskrivna förloppsindikatorernas utseende i terminalen." width="auto" height="auto" border="10" /><br>
<font size="2">De handskrivna förloppsindikatorernas utseende i terminalen.</font>
</p>

<p align="center">
<img src="/images/progress_bar_twitter.PNG" alt="De handskrivna förloppsindikatorernas utseende på Twitter." width="70%" height="auto" border="10" /><br>
<font size="2">De handskrivna förloppsindikatorernas utseende på Twitter.</font>
</p>

*Uppdatering (20-04-2021):* Hade helt missat att inte bara längden på förloppsindikatorn blev skev, utan också procentenhetens längd. Indikatorn med 20,4 % ser ju ut att vara nästan 50 %! Detta beror på att blockelementets storlek är större än punkterna, vilket inte syns i terminalutskriften men däremot på Twitter. Har nu löst detta genom att ersätta både procentblocket och punkterna med blockelement av samma storlek:

<p align="center">
<img src="/images/block_progress_bar.jpeg" alt="Förloppsindiktarorer med blockelement av samma storlek" width="70%" height="auto" border="10" /><br>
<font size="2">Utseendet hos de nya förloppsindikatorerna med blockelement av samma storlek.</font>
</p>
     

## Automatisera boten med GitHub Actions

För automatiseringen av boten drog jag stor nytta av [den här guiden](https://blog.esciencecenter.nl/twitter-bots-for-science-1cf3f19dcda8) av Patrick Bos. Den introducerade mig till något som jag faktiskt var helt obekant med tidigare: [GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions). Med hjälp av en YAML-fil som man lägger in under `.github/workflows/` i repon kan man trigga GitHub att köra koden utifrån olika händelser (t.ex. att någon pushar till ett repo) eller schemalagda tidpunkter. Eftersom FOHM uppdaterar sin statistik varje dag från tisdag till fredag vid 14-tiden valde jag att köra programmet de dagarna vid halv 3. Notera att tidpunkten måste anges i [cron-syntax](https://crontab.guru/) och [UCT-tid](https://sv.wikipedia.org/wiki/Koordinerad_universell_tid). 

```yaml
name: Tweet latest vaccination statistics for Sweden
 
on:
  schedule:
    - cron:  '30 12 * * 2-5'  # Run every Tuesday-Friday at 12:30 UTC time (14:30 Swedish time)

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: '3.9.1'
      - name: install dependencies
        run: pip install -r requirements.txt
      - name: run script
        env:
          BOT_API: ${{ secrets.BOT_API }}
          BOT_API_SECRET: ${{ secrets.BOT_API_SECRET }}
          BOT_ACCESS: ${{ secrets.BOT_ACCESS }}
          BOT_ACCESS_SECRET: ${{ secrets.BOT_ACCESS_SECRET }}
        run: python run.py
```

I YAML-filen specificerar Bos också vilka andra program som koden är beroende av (genom att länka till en `requirements.txt`-fil) så att de kan installeras innan koden körs. Här initierar han också de miljövarialber, `env`, som behövs för Twitter-kontots API (api-nyckel, access-token, etc.) och som i sin tur göms undan i "Secrets"-delen av GitHub (en annan ny komponent för mig!). Den hittar man genom att gå till `Settings` --> `Secrets`.

I programkoden importeras en speciell argumentparsare - `configargparse` - som kan användas för att dels fånga upp miljövariablerna som anges i YAML-filen (och som sparas krypterade i `Secrets`), men även variabler som anges i en separat konfigureringsfil. Detta gör det enklare att testa koden både i molnet och lokalt eftersom man då enkelt kan komma åt API-nycklarna på hemmadatorn genom den separata filen (som man skyddar från att pusha med `.gitignore`).

En tredje och sista viktig komponent för att köra koden är [Tweepy](https://www.tweepy.org/), ett Python-bibliotek för att kommunicera med Twitters API. Det går att se hur Bos kombinerar den med `configargparse` [här](https://github.com/egpbos/covid_vaccine_progress_bot/blob/main/run.py). Och i [den här](https://realpython.com/twitter-bot-python-tweepy/#creating-twitter-api-authentication-credentials) guiden finns ytterligare information för hur man kan använda Tweepy för fler ändamål än att bara skriva tweets.    

<p align="center">
<img src="/images/configargparse.PNG" alt="Configargparse och Tweepy." width="70%" height="auto" border="10" /><br>
<font size="2">Configargparse-parametrarna samt autentisering med Tweepy för att skapa ett API-objekt.</font>
</p>

## Slutresultatet

Boten heter [@vaccination_SE](https://twitter.com/vaccination_SE) och källkoden går att hitta [här](https://github.com/datatjej/vaccination-bot-SE). Även om det inte var ett superavancerat projekt är jag ändå glad över att ha klarat av det och känner att jag lärt mig mycket på vägen. Det ska bli spännande att se hur väl GitHub Actions fungerar de närmaste dagarna och om det dyker upp några buggar.

<p align="center">
<img src="/images/vaccination_SE.PNG" alt="vaccination-bot-SE." width="70%" height="auto" border="10" /><br>
<font size="2">Vaccinationsbotens slutgiltiga utseende.</font>
</p>

*Uppdatering (20-04-2021):* Boten kördes på autopilot för första gången idag och det funkade! :D Men körningen skedde först 12 min efter utsatt tid, vilket tydligen är [ganska vanligt](https://stackoverflow.com/questions/65132563/why-is-github-actions-workflow-scheduled-with-cron-not-triggering-at-the-right-t). För att kompensera fördröjningen tänkte jag tidigarelägga schemat med 10 min.

*Uppdatering (11-08-2021):* Fyra månader senare fick jag mitt första runtime error när FHM plötsligt la till en punkt i slutet av tabellbeskrivningen, vilket gjorde att `find_parent()`-metoden i `BeautifulSoup`-paketet inte längre kunde hitta tabellen utifrån caption-taggen. Fixade det genom att lägga till den saknade punkten i koden, men det hela visar ändå på hur bräckligt webbskrapning kan vara inför ändringar på webbsidan.