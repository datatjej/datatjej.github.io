---
layout: post
title: 26. Synka en fork (GitHub)
---

Det finns två sätt att synka en fork så att den uppdateras med de ändringar som gjorts i den ursprungliga repon (AKA "uppströmsrepon") efter forkningen.

# Synka i terminalen 

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
$ git remote add upstream https://github.com/ORIGINAL_ÄGARE/ORIGINAL_REPOSITORY.git
{% endhighlight %}

(I mitt exempelfall är uppströmsrepon: https://github.com/GrammaticalFramework/comp-syntax-2020.git)

4. Säkerställ att uppströmsrepon verkligen laggts till:

{% highlight git %}
$ git remote -v
> origin  https://github.com/datatjej/comp-syntax-2020.git (fetch)
> origin  https://github.com/datatjej/comp-syntax-2020.git (push)
> upstream https://github.com/GrammaticalFramework/comp-syntax-2020.git (fetch)
> upstream https://github.com/GrammaticalFramework/comp-syntax-2020.git (push)
{% endhighlight %}

################SLUT PÅ KONFIGURERA REMOTE-REPO################


