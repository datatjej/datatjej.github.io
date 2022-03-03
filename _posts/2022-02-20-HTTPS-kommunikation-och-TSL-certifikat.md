---
layout: post
title: 55. HTTP-kommunikation och TSL-certifikat
---

Datakommunikation över internet regleras under kommunikationdprotokollet HTTP (*Hypertext Transfer Protocol*) som började utvecklas av Tim Berners Lee vid partikelfysiklaboratoriet CERN i Genève 1989 och introduceras till en bredare allmänhet under början av 90-talet. Med den allra första versionen, som kommit att kallas HTML 0.9. kunde man bara skicka HTML-sidor från webbservrar till webbklienter, och det som returnerades var antingen den önskade webbsidan eller en specialgenererad HTML-sida som beskrev problem i kommunikationen. Status- eller felkoder fanns inte [[1](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)].

## Klient-server-kommunikation
HTTP är i grund och botten en klient-server-kommunikation som innebär att en klient - vanligtvis en webbläsare - skickar en begäran till en server och får tillbaka ett svar. Mellan klienten och servern finns det oftast flera lager av proxyservrar som agerar mellanhand i hämtningen av webbsidor [[2](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)]. Proxyserverar används dels för att surfa anonymt i och med att det inte är klientens egna publika IP-address som når målservern, utan proxyns [[3](https://www.youtube.com/watch?v=5cPIukqXe5w&t=190s)]. De används också för att snabba på hämtningen av information när man som företag exempelvis har en proxyserver som cachar webbsidor som användarna hämtar i en databas [[3](https://www.youtube.com/watch?v=5cPIukqXe5w&t=190s)]. Företag kan också använda proxyservrar för att logga de anställdas aktivitet på internet eller blockera vissa webbsidor [[3](https://www.youtube.com/watch?v=5cPIukqXe5w&t=190s)].

 
## HTTPS
År 1995 introduceras kryptering via HTTPS, det vill säga HTTP i kombination av säkerhetsprotokollet SLL (*Secure Socket Layer*) från företaget Netscape. Syftet var att kryptera meddelandena som sänds mellan server och klient och samtidigt garantera autenticeringen mellan dem. Detta var framför allt viktigt för e-handlare och deras kunder som ville kunna skicka kortuppgifter utan att få dem avlästa av utomstående. SSL kom så småningom att ersättas av TSL (*Transport Layer Security*) som även fick spridning utanför e-handeln [[4](https://www.wired.com/2017/01/half-web-now-encrypted-makes-everyone-safer/)]. Med tiden har HTTPS blivit den dominerande kommunikationsprotokollet hos webbsidor på internet [[5](https://w3techs.com/technologies/details/ce-httpsdefault))]. 

## TSL-certifikat
TSL-protokollet kräver att webbisidor kan legitimera sig gentemot webbklienter med ett digitalt certifikat. Det digitala certifikatet visar att webbservern är den som den utger sig för att vara inom PKI (*Public Key Infrastructure*) - infrastrukturen för krypteringsnycklar. Certifikaten ufärdas av en certifikatutfärdare (CA) som kan vara av antingen kommersiell och icke-kommersiell natur (som vid exempelvis stora institutioner och organisationer som har sina egna utfärdare).

## Java-kryptering
Applikationer som ska kommunicera över TSL i sin nätverkstrafik behöver också kunna identifiera sig via TSL-certifikat. Detta görs i form av lösenordsskyddade certifikatfiler (för Java-applikationer antingen det äldre Java-specifika formatet JKS eller det nyare och språkneutrala PKCS12) som läggs in i samma filsystem som applikationen samt en certifikathanterare (`keytool` i Java JDK). 

### Key store
I Javas key store lagras de egna certifikaten med tillhörande privata nycklar som användas vid autenticeringsprocedurer via HTTPS. När applikationen kommunicerar med en annan enhet letar den upp sin privata nyckel i key store:n och presenterar motsvarande publika nyckel tillsammans med certifikatet [[6](https://www.baeldung.com/java-keystore-truststore-difference)].

### Trust store
I trust store:n lagras andra enheters certifikat som man litar på. I Java kallas den `cacerts` och den skapas i mappen `$JAVA_HOME/jre/lib/security` [[6](https://www.baeldung.com/java-keystore-truststore-difference)]. 

Man kan lista innehållet i trust store-nyckelringen med följande kommando:
`keytool -list -keystore <namn på trust store:n>.jks` 

## Teckenkodning
URL:er som skickas över internet teckenkodas med [ASCII](https://sv.wikipedia.org/wiki/ASCII) (*American Standard Code for Information Interchange*). När URL:er innehåller tecken som inte finns i ASCII-uppsättningen måste dessa konverteras, vilket görs med en kombination av `%`-tecknet följt av två hexadecimaler [[6](https://www.w3schools.com/tags/ref_urlencode.asp)]. I programmeringsprojekt kan just kodningen av URL-strängar medföra problem när den görs felaktigt. Man måste bland annat se till att bevara URI-schemat (`https://`) och snedstrecken i URL-path:en [[7](https://stackoverflow.com/questions/51292397/which-parts-of-a-url-must-be-encoded)].

## Referenser

[1] [Evolution of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP). Mozilla.org (20-11-2021).<br>
[2] [An Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview). Mozilla.org(26-02-2022).
[3] [What is a proxy server?](https://www.youtube.com/watch?v=5cPIukqXe5w&t=190s). PowerCert Animated Videos (26-02-2022). 
[4] [Half of the web is now encrypted. That makes everyone safer.](https://www.wired.com/2017/01/half-web-now-encrypted-makes-everyone-safer/) Wired.com (30-01-2017).<br>
[5] [Usage statistics of Default protocol https for websites](https://w3techs.com/technologies/details/ce-httpsdefault). W3techs.com - Web Technology Surveys (20-02-2022).<br>
[5] [Difference Between a Java Keystore and a Truststore](https://www.baeldung.com/java-keystore-truststore-difference). Baeldung.com (30-09-2021).
[6] [HTML URL Encoding Reference](https://www.w3schools.com/tags/ref_urlencode.asp). W3Schools.com (26-02-2022). 
[7] [Which parts of a URL must be encoded?](https://stackoverflow.com/questions/51292397/which-parts-of-a-url-must-be-encoded). Stackoverflow.com (03-03-2022). 

