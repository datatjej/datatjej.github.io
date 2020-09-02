---
layout: post
title: 26. Synka en fork (GitHub)
---

Det finns två sätt att synka en fork så att den uppdateras med de ändringar som gjorts i den ursprungliga repon (AKA "uppströmsrepon") efter forkningen.

# Synka via terminalen 
Det här ser kanske lite invecklat ut men går förvånansvärt smidigt. Har än så länge inte stött på några problem med den här metoden. Instruktionerna är hämtade från [GitHubs egna dokumentation](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/syncing-a-fork). 

**1. Öppna Git Bash på forkmappen.**

###########KONFIGURERA REMOTE-REPO##############

*Börja med att konfigurera en remote-repo som pekar på den ursprungliga repon enligt följande:*

**2. Lista de nuvarande konfigurerade remote-reporna för forken:**

{% highlight git %}
$ git remote -v
> origin  https://github.com/datatjej/comp-syntax-2020.git (fetch)
> origin  https://github.com/datatjej/comp-syntax-2020.git (push)
{% endhighlight %}

**3. Lägg till uppströmsrepon som ytterligare remote-repo:** 

{% highlight git %}
$ git remote add upstream https://github.com/ORIGINALÄGARE/ORIGINALREPO.git
{% endhighlight %}

(I mitt exempelfall är uppströmsrepon: https://github.com/GrammaticalFramework/comp-syntax-2020.git)

**4. Säkerställ att uppströmsrepon verkligen lagts till:**

{% highlight git %}
$ git remote -v
> origin  https://github.com/datatjej/comp-syntax-2020.git (fetch)
> origin  https://github.com/datatjej/comp-syntax-2020.git (push)
> upstream https://github.com/GrammaticalFramework/comp-syntax-2020.git (fetch)
> upstream https://github.com/GrammaticalFramework/comp-syntax-2020.git (push)
{% endhighlight %}

##########SLUT PÅ KONFIGURERA REMOTE-REPO###########

**5. Hämta uppströmsrepons grenar och tillhörande commits. Commits som gjorts i master-branchen förvaras i en lokal branch med namnet upstream/master.** 

{% highlight git %}
$ git fetch upstream
> remote: Enumerating objects: 142, done.
> remote: Counting objects: 100% (142/142), done.
> remote: Compressing objects: 100% (112/112), done.
> remote: Total 1808 (delta 72), reused 82 (delta 30), pack-reused 1666
> Receiving objects: 100% (1808/1808), 69.63 MiB | 185.00 KiB/s, done.
> Resolving deltas: 100% (1025/1025), completed with 2 local objects.
> From https://github.com/GrammaticalFramework/comp-syntax-2020
> * [new branch]      master                -> upstream/master
{% endhighlight %}

**6. Checka ut din forks lokala masterbranch:**

{% highlight git %}
$ git checkout master
{% endhighlight %}

**7. Mergea ändringarna i upstream/master med din lokala masterbranch. Detta påverkar inte dina lokala ändringar på forken.**

{% highlight git %}
$ git merge upstream/master
{% endhighlight %}

**8. Kom ihåg att pusha de uppdaterade ändringarna på din lokala fork till din remote-fork på GitHub också.** 

Klaaaart! :D

# Synka via GitHub
Har aldrig följt den här metoden tidgiare, så jag testar att göra det medan jag skriver. Intstruktioner hittade jag [här](https://www.earthdatascience.org/courses/intro-to-earth-data-science/git-github/github-collaboration/update-github-repositories-with-changes-by-others/). I kort går det ut på att göra en slags omvänd pull request.

**1. Inspektera dina remote-fork-vy. Där kan du se hur många commits den ligger efter uppströmsrepon:**

<p align="center">
<img src="/images/github_fork.PNG" alt="Remote-fork-vy på GitHub" width="100%" height="auto" border="10" /><br>
  I det här fallet ligger min fork 308 commits bakom GrammaticalFrameworks repo.
</p>

**2. Klicka på antingen "Pull request" eller "Compare":**

<p align="center">
<img src="/images/github_fork_compare.PNG" alt="Pull request- och Compare-knapparna" width="100%" height="auto" border="10" /><br>
</p>

**...så att du hamnar i den här vyn:**

<p align="center">
<img src="/images/github_fork_comparing_changes.PNG" alt="Comparing changes-vyn" width="100%" height="auto" border="10" /><br>
</p>

**3. Klicka sedan på "compare across forks":**  

<p align="center">
<img src="/images/github_fork_compare_across_forks.PNG" alt="Compare across forks-knappen" width="100%" height="auto" border="10" /><br>
</p>

**4. Välj den egna forken som base-fork och uppströmsrepon som head-fork:**

<p align="center">
<img src="/images/github_fork_base_head.PNG" alt="Välj base- och head-fork." width="100%" height="auto" border="10" /><br>
</p>

**5. Klicka på "Create pull request":** 

<p align="center">
<img src="/images/github_fork_create_pull_request.PNG" alt="Create pull request-knappen." width="100%" height="auto" border="10" /><br>
</p>

**6. Namnge pull request-commiten med något informativt och klicka på "Create pull request" igen:**

<p align="center">
<img src="/images/github_fork_namnge_commiten.PNG" alt="Namnge pull request-commiten." width="100%" height="auto" border="10" /><br>
</p> 

**7. Mergea pull requesten genom att scrolla ner i pull requesten och klicka på "Merge pull request":** 

<p align="center">
<img src="/images/github_fork_merge_pull_request.PNG" alt="Merge pull request-knappen." width="100%" height="auto" border="10" /><br>
</p> 

**8. Klicka på "Confirm merge":**

<p align="center">
<img src="/images/github_fork_confirm_merge.PNG" alt="Merge pull request-knappen." width="100%" height="auto" border="10" /><br>
</p> 

**9. Nu är din fork uppdaterad med det senaste! Kom ihåg att hämta ner ändringarna lokalt också med git pull. :)

