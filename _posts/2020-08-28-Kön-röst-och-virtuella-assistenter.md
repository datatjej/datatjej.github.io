---
layout: post
title: 24. Kön, röst och virtuella assistenter
---

När jag arbetade med en röststörd virtuell assistent under praktikjobbet i somras började jag fundera över valet av kvinnlig eller manlig röst i sådana assistenter (tänk Siri, Google Assistant, Cortana och Alexa som alla har kvinnlig röst som default-läge) och vad som överhuvudtaget får en röst att låta karaktäristiskt kvinnlig eller manlig. 

På den första punkten visade det sig att människor generellt tycker att kvinnliga röster är lättare att gilla [[1](#fotnot1)], men att manliga röster framstår som mer kompetenta [[2](#fotnot2)] - vilket kanske förklarar valet av manlig röst för IBM:s allvetande Watson. 

Intressant nog hade Apples första försök till röststyrd assistent, Casper, en ganska androgyn röst. Kolla in den här härliga videon från 1992!

<p align="center">
<a href="https://www.youtube.com/watch?v=8De_KxYt1pQ" target="_blank"><img src="/images/casper.PNG" 
alt="Casper: Apple’s initial Voice First system from 1992" width="480" height="360" border="10" /></a></p>

Ett annat mer nutida exempel på ett projekt där det uttalade syftet var att ta fram en könsneutral röst för dialogsystem är [röstassistenten Q](https://www.genderlessvoice.com/). En av dess skapare, Nils Nørgaard, beskriver i TEDx-föreläsningen nedan om arbetet med att ta fram rösten och vad som egentligen får en röst att låta manlig eller kvinnlig. Han pratar om **grundfrekvensen** (eng. *fundamental frequency*) i mänskliga röster, som ofta ligger på 80-175 Hz för män och 145-250 Hz för kvinnor. Det spann på 145-175 Hz där både kvinnliga och manliga röstlägen kan ligga kallar han **"the neutral zone"**. Genom att sätta en inspelad röst som inte urpsrungligen låg i det frekvensbandet till 153 Hz, och ändra hur vokalerna formas genom ett så kallat **formantfilter**, lyckades han syntetisera en röst som varken låter typiskt kvinnlig eller manlig. 

<p align="center">
<a href="https://www.youtube.com/watch?v=qH6KB7MrOPw" target="_blank"><img src="/images/q_tedx.PNG" 
alt="How to create a genderless voice | Nis Nørgaard | TEDxUniversityofNicosia" width="520" height="360" border="10" /></a></p>

Jag grävde vidare lite i Google Scholar och hittade [den här artikeln](http://icphs2011.hk.lt.cityu.edu.hk/resources/OnlineProceedings/RegularSession/Poon/Poon.pdf) av Poon & Ng (2011) som undersöker vikten av grundfrekevens och formantfrekvenser när det kommer till att identifiera en talares biologiska kön. Man nämner **källa-filter-teorin** (som jag går in på lite kort i slutet av [det här](https://datatjej.github.io/Fonetik-fononologi-och-taligenk%C3%A4nning/) inlägget), enligt vilken mänskligt tal är att betrakta som ett resultat av av **stämbandens vibrationer** och den efterföljande **resonansen i ansatsröret** som förstärker vissa vibrationer och minskar andra (ibid).     

Poon & Ng fann att grundfrekvensen, precis som Nørgaard var inne på, är den främsta ledtråden för sådan identifiering. Grundfrekvensen, alltså röstläget eller tonhöjden (eng. *pitch*), beror på stämbandens fysiska vibrationsfrekvens och påverkas av storleken hos stämbanden, som generellt är större hos män. Även ansatsröret, som tillsammans med tung- och läpprörelserna i munnen skapar formanterna, är generellt sett större hos män, vilket ger män och kvinnor lite olika formantvärden. Videon nedan ger en superbra och mer detaljerad förklaring för hur det här med formanter funkar:  

<p align="center">
<a href="https://www.youtube.com/watch?v=jl4zGRSYqkE" target="_blank"><img src="/images/formanter.PNG" 
alt="How Do We Change Our Mouths to Shape Waves? Formants" width="520" height="360" border="10" /></a></p>

För att runda av och komma tillbaka till de mer språkteknologiska aspekterna av det här med röst och kön: Det går förstås att skapa program som utifrån parametrarna ovan och annoterad data kan gissa talarens biologiska kön - men är det något vi *borde* göra? Vad skulle ett sådant verktyg kunna användas till? Ge mer könsriktad reklam? Kanske ge vissa användare ett helt annat bemötande när de interagerar med en virtuell assistent? Det hela blir lätt problematiskt om det beräkande könet inte stämmer överens med det upplevda (t.ex. trans, icke-binär) eller om informationen används för förstärka förlegade könsnormer. [Den här artikeln](https://edition.cnn.com/2019/11/21/tech/ai-gender-recognition-problem/index.html) beskriver mer ingående farorna med AI som förutspår kön.

**Referenser**<br>
<a name="fotnot1">[1]</a>: [The impact of gender stereotyping on the perceived likability of virtual assistants](https://aisel.aisnet.org/amcis2020/cognitive_in_is/cognitive_in_is/4/]) - Ernst & Herm-Stapelberg 2020. <br>
<a name="fotnot2">[2]</a>: [Gender stereotyping’s influence on the perceived competence of Siri and co.](https://scholarspace.hmanoa.hawaii.edu/handle/10125/64286), Ernst & Herm-Stapelberg 2020.
