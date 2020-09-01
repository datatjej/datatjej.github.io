---
layout: post
title: 26. Synka en fork (GitHub)
---

Det finns två sätt att synka en fork så att den uppdateras med de ändringar som gjorts i den ursprungliga repon (AKA "uppströmsrepon") efter forkningen.

# Synka i terminalen 
Det här ser kanske komplicerat ut men går förvånansvärt smidigt. Har än så länge inte stött på några problem med den här metoden. 

1. Öppna Git Bash på forkmappen.

###################KONFIGURERA REMOTE-REPO######################

*Börja med att konfigurera en remote-repo som pekar på den ursprungliga repon enligt följande:*

2. Lista de nuvarande konfigurerade remote-reporna för forken:

{% highlight git %}
$ git remote -v
> origin  https://github.com/datatjej/comp-syntax-2020.git (fetch)
> origin  https://github.com/datatjej/comp-syntax-2020.git (push)
{% endhighlight %}

3. Lägg till uppströmsrepon som ytterligare remote-repo: 

{% highlight git %}
$ git remote add upstream https://github.com/ORIGINALÄGARE/ORIGINALREPO.git
{% endhighlight %}

(I mitt exempelfall är uppströmsrepon: https://github.com/GrammaticalFramework/comp-syntax-2020.git)

4. Säkerställ att uppströmsrepon verkligen lagts till:

{% highlight git %}
$ git remote -v
> origin  https://github.com/datatjej/comp-syntax-2020.git (fetch)
> origin  https://github.com/datatjej/comp-syntax-2020.git (push)
> upstream https://github.com/GrammaticalFramework/comp-syntax-2020.git (fetch)
> upstream https://github.com/GrammaticalFramework/comp-syntax-2020.git (push)
{% endhighlight %}

################SLUT PÅ KONFIGURERA REMOTE-REPO################

5. Hämta uppströmsrepons grenar och tillhörande commits. Commits som gjorts i master-branchen förvaras i en lokal branch med namnet upstream/master. 

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

6. Checka ut din forks lokala masterbranch:

{% highlight git %}
$ git checkout master
{% endhighlight %}

7. Mergea ändringarna i upstream/master med din lokala masterbranch. Detta påverkar inte dina lokala ändringar på forken.

{% highlight git %}
$ git merge upstream/master
{% endhighlight %}

