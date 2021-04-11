---
layout: post
title: 48. Tokens och SSH-nycklar (GitHub)
---

Fick ett mejl från GitHub om att de [snart inte längre kommer att tillåta lösenordsbaserad autentisering](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/). Tog en djup suck och insåg att det nog är dags att sätta sig in i hur tokens och SSH-nycklar egentligen fungerar.

## Två sätt att klona

För att veta när och hur man ska använda tokens eller SSH-nycklar behöver man känna till att det finns flera sätt att klona ett GitHub-repo på: HTTPS och SSH.

<p align="center">
<img src="/images/https_ssh.PNG" alt="Olika alternativ för att clonea ett GitHub-repo" width="70%" height="auto" border="10" /><br>
<font size="2">HTTPS och SSH. Två av tre möjliga sätt att klona ett GitHub-repo.</font>
</p>
 
Både HTTPS och SSH är protokoll för kryptering av data. **HTTPS** ([*Hypertext Transfer Protocol Secure*](https://en.wikipedia.org/wiki/HTTPS)) används på internet för att autentisera en webbsida och skydda data som skickas mellan användaren och webbsidan. För närvarande tillåter GitHub antingen kontolösenord eller specialgenererade tokens när man utför Git-operationer på ett HTTPS-klonat repo. **SSH** ([*Secure Shell Protocoll*](https://en.wikipedia.org/wiki/Secure_Shell_Protocol)) används för att säkert ansluta sig till andra datorer, något vi t.ex. gör i skolan när vi kopplar upp oss mot MLT-programmets egen server. För att klona ett GitHub-repo via SSH krävs det att man har en SSH-nyckel sparad på datorn.    

Jag är fortfarande inte helt säker om den ena sättet att klona har någon fördel gentemot den andra när det kommer till GitHub. Någonstans läste jag att vissa brandväggar inte nödvändigtvis accepterar SSH-nycklar och att HTTPS-baserade tokenlösningar då kan vara bättre. Tokens kan också upplevas som smidigare att fixa i ordning än SSH-nycklar. Trots det verkar många på nätet föredra SSH framför tokens, och fördelen med det kanske klarnar med tiden.

## Tokens

**Personliga åtkomsttoken** (PAT, eng. *personal access tokens*) är ett alternativ till lösenord som innebär att systemet du vill använda genererar en lösenordstoken åt dig. Fördelen med PAT är att de är [[1](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)]: <br>

1) *unika* och kan genereras för en enda användning eller enhet, <br>
2) *återkallbara* utifall att man vill stoppa åtkomst som inte längre behövs, <br> 
3) *begränsade* på så sätt att man kan anpassa åtkomsten till vissa funktioner, <br>
4) *slumpmässigt genererade* och därför svåra att knäcka. <br>

Ett GitHub-konto kan ha flera olika tokens kopplade till sig för olika enheter och ändamål.

Det finns [tydliga instruktioner](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) i GitHubs dokumentation för hur man genererar en token under "Settings" i sin GitHub-profil och sedan använder den. En token kan -- precis som ett lösenord -- sparas i cachen på datorn så att man inte behöver ange det varje gång man vill utföra en Git-operation. Läs mer om det [här](https://docs.github.com/en/github/getting-started-with-github/caching-your-github-credentials-in-git).   

## Windows-specifika cache-problem  

Jag hade sedan tidigare en token sparad på datorn som jag ville ta bort från min GitHub-profil och generera på nytt (detta görs under `Settings` --> `Developer Settings` --> `Personal Access Tokens`). Jag fick då problemet att det här fönstret började dyka upp varje gång jag försökte pusha något:

<p align="center">
<img src="/images/github_login.PNG" alt="GitHub inloggningsfönster via Git Credential Manager for Windows" width="70%" height="auto" border="10" /><br>
<font size="2">Inloggningsfönstret från Git Credential Manager for Windows.</font>
</p>

Det verkade vara kopplat till [Git Credential Manager for Windows](http://microsoft.github.io/Git-Credential-Manager-for-Windows/Docs/CredentialManager.html) som är en autentiseringshanterare för Git som antagligen installerades i samband med att jag skaffade [Git for Windows](https://gitforwindows.org/) (en Git-verktygslåda för Windows som kommer med de användbara applikationerna **Git BASH** och **Git GUI**). Det finns säkert flera fördelar med att använda en Git-specifik autentiseringshanterare för att spara lösenord och tokens, men eftersom den inte verkade oumbärlig så valde jag att ta bort den och istället använda **Windows inbyggda autentiseringshanterare** (`wincred`)

Det går att **avaktivera eller avinstallera Git Credential Manager for Windows** genom att följa tipsen i [den här](https://stackoverflow.com/questions/37182847/how-do-i-disable-git-credential-manager-for-windows) Stackoverflow-tråden. Jag gjorde det och öppnade sedan ett Git BASH-fönster där jag skrev in kommandot `git config --global credential.helper wincred` för att koppla Windows egen autentiseringshanterare till Git. Därefter pushade jag ändringar i ett befintligt projekt och skrev in min nygenererade token istället för mitt GitHub-lösenord i lösenordsfältet. Det sparades då på platsen i **Kontrollpanelen** som ses i bilden nedan och behövde inte anges på nytt senare.

<p align="center">
<img src="/images/autentiseringshanteraren.PNG" alt="Windows egna autentiseringshanterare i Kontrollpanelen" width="auto" height="auto" border="10" /><br>
<font size="2">Autentiseringsuppgifter för bl.a. Github sparade via Windows egen autentiseringshanterare.</font>
</p>

## SSH-nycklar

**SSH-nycklar** används alltså när man vill klona och utföra andra Git-operationer på en GitHub-repo med SSH. Precis som för tokens finns det också [tydliga instruktioner](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) i GitHubs dokumentation för hur man går till väga för att skapa dem. Den väsentliga skillnaden i proceduren är att tokens genereras på GitHub-sidan och sedan används lokalt medan SSH-nycklar först genereras lokalt och sedan läggs till manuellt på GitHub-kontot. Innan man klonar med SSH måste SSH-nyckeln finnas på plats både i datorn och på GitHub-kontot, och vid kloningen anger man lösenordet (eng. *passphrase*) man valt för sin SSH-nyckel. 

Om man har en massa en GitHub-repos som man redan klonat med HTTPS men av någon anledning vill ändra till SSH så går det lätt att ordna. Det enda man behöver göra är att öppna sin git-terminal i mappen för projektet man vill **ändra autentisering** för och antingen skriva:

`git remote set-url origin https://github.com/ANVÄNDARNAMN/REPONAMN.git` 

...om man vill ändra ett SSH-klonat repo till HTTPS (notera `https`-prefixet i början av URL:en). Eller:

`git remote set-url origin git@github.com:ANVÄNDARNAMN/REPONAMN.git`

...om man vill ändra ett HTTPS-klonat repo till SSH (notera `git@`-prefixet i URL:en). För mer info, klicka dig [hit](https://docs.github.com/en/github/getting-started-with-github/managing-remote-repositories#changing-a-remote-repositorys-url).

 <p align="center">
<img src="/images/ssh2https.PNG" alt="Kommando för att ändra från SSH till HTTPS" width="80%" height="auto" border="10" /><br>
<font size="2">Exempel på hur man går till väga för att ändra ett SSH-klonat repo till HTTPS.</font>  
</p>