---
layout: post
title: 54. Systemhantering i Linux
---

Linux är ett populärt operativsystem för serveradministration eftersom det är 1) säkert, 2) stabilt och 3) flexibelt [[1](https://www.youtube.com/watch?v=WMy3OzvBWc0)]. 

## Init
Init (förkortning av *initialization*) är den första processen som startas upp när man bootar ett Unix-baserat (t.ex. Linux, Mac OS) datorsystem [[2](https://en.wikipedia.org/wiki/Init)]. Den är ursprunget till alla efterföljande processer. Det är operativsystemskärnan (eng. *kernel*) som initierar init under uppstarten och ett misslyckande att köra init kan leda till något som kallas [kernel panic](https://sv.wikipedia.org/wiki/Kernel_panic).

## Daemon
Init är en s.k. daemon (uttalas "di'mon" eller "dejmon")- ett program som körs som en bakgrundsprocess istället för att kontrolleras direkt av användaren. Init är en form av daemon som själv kan skapa andra daemoner eller adoptera en daemon som kommit till genom ett [fork](https://en.wikipedia.org/wiki/Fork_(system_call))-systemanrop (när en process skapar en annan process som är en kopia av sig själv och sedan upphör). Ett annat exempel på en daemon-process är systemprogrammet cron med vilket man kan exekvera cron-jobb (script som ska köras automatiskt vid vissa tidpunkter).

## Systemd
Systemd (sammanslagning av *system* + *d\[aemon\]*) är ett Linux-specifikt init-system och tillika systemhanterare som innehåller många olika systemkomponenter. Det började utvecklas av ingenjörer på Red Hat (ett amerikanskt open source-mjukvaruföretag bl.a. känt för sin kommersiella företagsdistribution av Linux: *Red Hat Enterprise Linux*) 2010 och lanserades i Linux-distributionen Fedora för första gången 2011.

Systemd är organiserat kring enheter (eng. *units*) av olika typer: tjänster (*.service*), monteringspunkter (*.mount*), enhetsfiler (*.device*), socklar (*.socket*), m.m. [Den här sidan](https://www.computernetworkingnotes.com/linux-tutorials/systemd-units-explained-with-types-and-states.html) beskriver dessa enheter som delar på en buss som sköter olika uppgifter (blinkers, bromsar, backspegel...) och kontrolleras av busschauffören, i det här fallet systemd. 

## Unit-filer
Unit-filerna kan finnas på lite olika platser, men systemets egen kopia av dem hamnar automatiskt i `/lib/systemd/system`-katalogen under installation. Vill man modifiera dem är det bäst att göra det genom att manipulera en kopia av filen i mappen `/etc/systemd/system`, vars filer har företräde över motsvarande filer på alla andra platser i systemet [[3](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files )]. Om man bara vill ändra en liten del av en unit-fil kan man också skapa en undermapp där filen man vll ändra ligger som man ger samma namn som själva filen samt tillägget `.d` (ex. `cron.service.d`) och däri lägga en fil med ändelsen `.conf` [[3](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files )].

Tjänsterna (`.service`-filerna) kan vara inbyggda daemoner som cron eller syslog, men även externa program av olika slag, t.ex. `solr.service` om man har sökmotorn Solr installerad. 

Enhetsfilerna (`.device`) erbjuder ett gränssnitt mellan datorn och drivrutinerna på annan hårdvara (t.ex. en skrivare) men även till diskpartioneringar  och systemresurser utan koppling till egentliga enheter.

Monteringsfilerna (`.mount`) har som syfte att koppla externa filer och enheter till det egna systemets filhierarki (genom definiera en monteringspunkt) och därigenom göra det tillgängligt för användaren.

Socklar (`.socket`) är kommunikationsändpunkter för data på ett nätverk eller mellan olika processer på operativsystemet [c]. Dessa kan aktivera en tillhörande `.service`-fil när de tar emot inkommande trafik [[4](https://www.computernetworkingnotes.com/linux-tutorials/systemd-units-explained-with-types-and-states.html)]. 

Target-filer (`.target`) används för att gruppera unit-filer via beroende programvaror (eng. *dependencies*) och etablera standardiserade namn för så kallade synkroniseringspunkter i beroendena mellan unit-filer [[5](https://man7.org/linux/man-pages/man5/systemd.target.5.html)].

​Swap-filerna (`.swap`) har med Linux-systemets virtuella minneshantering att göra. När RAM-minnet är fullt kan delar av arbetsminnet som inte behövs där och då kopieras över till en så kallad swap-partition på en hårddisk eller till en swap-fil [[6](https://www.linux.com/news/all-about-linux-swap-space/)]. [Här](https://www.linux.com/news/all-about-linux-swap-space/) kan man läsa mer om swap-utrymmeshantering.

Snapshot-filer (`.snapshot`) skapas när man kör kommandot `systemctl snapshot` och är en sparad bild av systemets tillstånd vid en given tidpunkt. Det erbjuder ett sätt att gå tillbaka till tidigare systemtillstånd om något händer som kräver det, men ska inte ses som en ersättning till vanlig backup-kopiering av filer.

## Systemctl
Systemctl är det centrala verktyget för att kontrollera init-systemet. Service-applikationerna kan exempelvis startas (om), stopppas eller laddas om (dvs ladda om konfig-filerna utan att starta om tjänsten, om den möjligheten finns) med systemctl-kommandot `sudo systemctl <start/restart/stop/reload> <applikation>.service`. Suffixet `.service` kan utelämnas helt, t.ex. `sudo systemctl stop solr`.

## Referenser

[1] https://www.youtube.com/watch?v=WMy3OzvBWc0
[2] https://en.wikipedia.org/wiki/Init
[3] https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files 
[4] https://www.computernetworkingnotes.com/linux-tutorials/systemd-units-explained-with-types-and-states.html
[5] https://man7.org/linux/man-pages/man5/systemd.target.5.html
[6] https://www.linux.com/news/all-about-linux-swap-space/
